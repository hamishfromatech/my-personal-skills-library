---
name: local-first-web-architecture-2026
description: Build production-grade local-first web applications using client-side SQLite, CRDT sync, and privacy-by-design data architecture. Covers the spectrum from offline-first to full local-first, tool selection (PowerSync, ElectricSQL, Yjs, Automerge), conflict resolution, schema migrations, and auth patterns. Use when designing apps where data privacy, offline resilience, or instant UI responsiveness are core requirements. NOT for server-generated analytics dashboards or systems requiring strong transactional consistency.
---

# Local-First Web Architecture 2026

## Overview

Local-first is a data architecture, not a performance optimization. The user's device holds the primary copy of their data. The app reads and writes to a local database. Sync with servers or peers happens in the background. The server, when it exists, is a sync peer — not the gatekeeper.

By 2026, SQLite running in the browser via WebAssembly (wa-sqlite, PGlite), persisted to the Origin Private File System (OPFS), makes this practical for production. The payoff is instant UI, offline resilience, and genuine data sovereignty. The cost is sync complexity, conflict resolution, and client-side schema migrations.

This skill synthesizes production experience from three shipped local-first apps and two abandoned attempts, providing an honest assessment of where local-first wins, where it fails, and how to start incrementally.

## When to Use

- Building note-taking, document editing, project management, or field apps with unreliable connectivity
- Designing products where "your data never leaves your device" is a core selling point
- Adding offline resilience to specific features within otherwise traditional apps (offline drafts, collaborative notes)
- Evaluating whether local-first fits a new product vs. traditional request/response architecture
- Architecting privacy-first tools where E2EE and user data ownership are competitive moats

NOT for:
- Server-generated analytics dashboards (the server produces the data; client ownership adds nothing)
- Banking, payment processing, or inventory with strong transactional consistency requirements
- Simple CRUD admin panels used by five people with reliable internet
- Massive datasets that won't fit on client devices

## Core Principles

| Principle | What It Means | Traditional Equivalent |
|-----------|---------------|----------------------|
| **Client as data node** | The device runs its own database; reads/writes are local | Client as thin view requesting permission from server |
| **Server as sync peer** | Server authenticates, backs up, and reconciles — but does not gatekeep every read/write | Server as sole source of truth |
| **Instant reads/writes** | UI updates immediately; sync is background and invisible | Click → POST → wait → response → optimistic update → rollback on failure |
| **Eventual consistency** | Conflicts resolve asynchronously; most merges are automatic | Strong consistency via single database |
| **User data ownership** | Data survives the service shutting down; user can export, migrate, fork | Data trapped in SaaS database |

## The Spectrum of Local-First

You do not have to go all-in. Start with one feature:

| Level | Pattern | Effort | Payoff |
|-------|---------|--------|--------|
| **0 — Offline-first** | Service worker caches API responses; server wins on reconnect | Low | Graceful degradation |
| **1 — Local writes** | Writes go to local store first; sync queue pushes to server | Medium | Instant perceived writes |
| **2 — Full replica** | Client holds complete or partial database replica; server syncs bidirectionally | High | Offline functionality, instant queries |
| **3 — Collaborative CRDT** | Multiple clients edit concurrently; CRDTs merge without conflicts | Very high | Real-time collaboration without locks |
| **4 — E2E encrypted** | Data encrypted on client before sync; server sees only ciphertext | Very high | Maximum privacy, zero-trust server |

## Client-Side Data Layer

### Storage Options

| Technology | Good For | Trade-offs |
|------------|----------|------------|
| **IndexedDB** | Broad compatibility, moderate data, unstructured data | Terrible DX, no SQL, verbose API |
| **OPFS + SQLite WASM** | Relational data, complex queries, serious apps | Safari quirks (~400KB bundle), memory limits on mobile |
| **PGlite** | Full Postgres compatibility, identical SQL on client and server | Newer, larger bundle, still maturing |

**Recommended starting stack:** wa-sqlite with OPFS persistence, WAL mode, serialized writes through a queue.

```javascript
import { SQLiteAPI } from 'wa-sqlite';
import { OPFSCoopSyncVFS } from 'wa-sqlite/src/examples/OPFSCoopSyncVFS.js';

async function initDatabase() {
  const module = await SQLiteAPI.initialize();
  const vfs = new OPFSCoopSyncVFS('app-db');
  await vfs.initialize(module);
  const db = await module.open_v2('data.db');
  await module.exec(db, 'PRAGMA journal_mode=WAL');
  return db;
}
```

### Safari Gotchas
- `createSyncAccessHandle()` may silently fail in iframe contexts (Safari 18)
- Fallback to IndexedDB-backed persistence on Safari if OPFS fails
- Test on real devices; emulator behavior differs

## Sync Engines

| Engine | Model | Maturity | Best For |
|--------|-------|----------|----------|
| **Yjs** | CRDT (document-oriented) | Production-ready | Real-time collaborative text editing |
| **Automerge** | CRDT (document-oriented, Rust-backed) | Production-ready | Document-centric apps, Rust ecosystems |
| **PowerSync** | Database replication (Postgres ↔ SQLite) | Production-ready | Existing Postgres backends, one-way replication with write-back |
| **ElectricSQL** | Active-active replication (Postgres ↔ SQLite) | Maturing | True active-active; powerful but rougher edges |
| **Triplit** | Full-stack database with built-in sync | Early, promising | Greenfield projects where you own the full stack |
| **Zero (Rocicorp)** | Query-based sync | Early | Worth watching; query-subscription model |

**Recommendation:** For teams with an existing Postgres backend, start with PowerSync. For real-time collaborative text, use Yjs. For greenfield apps where you can choose the full stack, evaluate Triplit.

## Conflict Resolution

### The Naive Approach (Don't Do This)
```javascript
// Silent remote wins — user loses local changes without knowing
function resolveConflict(local, remote) {
  return remote;
}
```

### Field-Level Last-Write-Wins (LWW)
Works for ~95% of app data conflicts:

```typescript
interface FieldValue {
  value: any;
  updatedAt: string; // ISO timestamp with sub-ms precision
  clientId: string;   // Tiebreaker when timestamps match
}

function pickWinner(a: FieldValue, b: FieldValue): FieldValue {
  const tA = new Date(a.updatedAt).getTime();
  const tB = new Date(b.updatedAt).getTime();
  if (tA !== tB) return tA > tB ? a : b;
  return a.clientId > b.clientId ? a : b;
}
```

### Semantic Conflicts
When field-level merge produces logically impossible results (double-booked meeting room):

1. **Accept the write** — rejecting it creates ghost records on the client
2. **Flag the violation** — sync the violation object back to the client
3. **Surface non-blocking resolution UI** — let the user decide

```typescript
interface SyncViolation {
  type: 'scheduling_conflict' | 'capacity_exceeded' | 'stale_assignment';
  recordId: string;
  description: string;
  conflictingRecords: string[];
  detectedAt: string;
}
```

### When to Surface Git-Style Conflicts
Almost never for typical app data. Users don't want to resolve merge conflicts. Only for high-stakes content: legal documents, medical records, financial ledgers.

## Authentication and Authorization

- **Auth works the same:** JWT tokens, OAuth, session management — but the token authenticates the *sync connection*, not every request
- **Authorization at sync boundary:** The server enforces row-level access via sync rules (PowerSync) or shapes (ElectricSQL). Never rely on client-side hiding — DevTools exposes the local SQLite file
- **Write validation:** Server validates mutations during write-back against authorization rules before applying to Postgres

## Schema Migrations on a Thousand Devices

On the server: run migration against one database.  
On the client: every user has their own database that may be running any schema version.

```javascript
const MIGRATIONS = [
  { version: 1, sql: 'CREATE TABLE IF NOT EXISTS tasks (...)' },
  { version: 2, sql: 'ALTER TABLE tasks ADD COLUMN priority INTEGER DEFAULT 0;' },
];

async function runMigrations(db) {
  await db.execute('CREATE TABLE IF NOT EXISTS _schema_version (version INTEGER)');
  const rows = await db.execute('SELECT version FROM _schema_version');
  const current = rows.length > 0 ? rows[0].version : 0;

  for (const mig of MIGRATIONS) {
    if (mig.version > current) {
      await db.execute('BEGIN');
      try {
        await db.execute(mig.sql);
        await db.execute('INSERT OR REPLACE INTO _schema_version (rowid, version) VALUES (1, ?)', [mig.version]);
        await db.execute('COMMIT');
      } catch (err) {
        await db.execute('ROLLBACK');
        throw err;
      }
    }
  }
}
```

**Rules:**
- Design migrations to be additive (new columns with defaults, new tables)
- Avoid renames and drops unless absolutely necessary
- Old clients may still write to dropped columns during sync — handle server-side

## End-to-End Encryption

Since data lives on the client, encrypt before sync:

- Client encrypts data with user-managed keys
- Server stores and relays ciphertext it cannot read
- Enables zero-trust server architecture

Products like Anytype implement this. For A-Tech products handling sensitive data, E2EE pairs naturally with local-first.

## Performance Reality

| Metric | Local-First | Traditional (REST) |
|--------|-------------|-------------------|
| Read 500 rows | <2 ms (MBP), ~8 ms (mid-range Android) | Network RTT + server query + serialization |
| Write | Instant (local INSERT); sync is background | POST → wait → response → optimistic update |
| Initial sync (5K tasks) | ~1.2s broadband, ~4–5s slow mobile | N/A (reads happen on demand) |
| Bundle size | +~400KB gzipped (SQLite WASM) | Baseline |

**Mitigations:**
- Partial sync (sync only active projects)
- Lazy-load the database module with dynamic `import()`
- Prune old data aggressively on mobile
- Show one-time "Setting up workspace" screen during initial sync

## Testing Local-First Apps

| Layer | Technique |
|-------|-----------|
| Merge logic | Unit tests — pure functions, deterministic |
| Convergence | Integration tests — two client instances, concurrent edits, assert identical state |
| Offline/online transitions | Playwright E2E with `context.setOffline(true)` |
| CRDT correctness | Property-based testing (fast-check) — random operation sequences, assert convergence |

## A-Tech Applications

### A-Coder (IDE)
- Local SQLite for project metadata, file index, and search indices
- Code itself stays on disk; sync via Git or peer-to-peer
- Offline mode: full project browsing and editing without connectivity
- E2EE option for enterprise teams

### Be Practical (Playbooks)
- Playbook annotations, highlights, and personal templates stored locally
- Optional federated sync for community learning patterns
- Cross-device sync via encrypted peer-to-peer or self-hosted server

### Builder's Club
- Open-source reference implementation of local-first toolkit
- CRDT-based collaborative playbook editing
- "Your data never leaves your device" as brand differentiator

## Anti-Patterns

| Anti-Pattern | Why It Fails |
|--------------|--------------|
| LocalStorage as database | 5–10 MB cap, synchronous, string-only |
| Treating offline-first as local-first | Server is still the source of truth; you haven't changed data ownership |
| Syncing entire database to every client | Authorization failure; performance disaster |
| Client-side authorization only | DevTools bypasses all "hidden" data |
| Rejecting conflicting writes server-side | Creates ghost records on client that diverge from server state |
| Event sourcing for simple CRUD | Reconstructing state from logs adds complexity most apps don't need |

## Tool Selection Decision Tree

```
Need real-time collaborative text editing?
  YES → Yjs
  NO → Continue

Have an existing Postgres backend?
  YES → PowerSync (today) or ElectricSQL (6–12 months)
  NO → Continue

Greenfield, own the full stack?
  YES → Evaluate Triplit
  NO → Continue

Just need offline + instant reads, no collaboration?
  YES → Custom sync layer (REST push/pull) + local SQLite
```

## References

- See [references/smashing-magazine-2026-deep-dive.md](references/smashing-magazine-2026-deep-dive.md) for the full Smashing Magazine article extraction, tool comparison matrix, and additional Safari/OPFS troubleshooting.
- See [references/ink-and-switch-paper.md](references/ink-and-switch-paper.md) for the original 2019 "Local-First Software" paper summary and seven ideals.

## Sources
- Durgesh Pawar — "The Architecture Of Local-First Web Development" (Smashing Magazine, May 6, 2026)
- Ink & Switch — "Local-First Software" (2019)
- Martin Kleppmann — "CRDTs: The hard parts" and *Designing Data-Intensive Applications*
- PowerSync, ElectricSQL, Yjs, Automerge, Triplit, Zero, TinyBase, PGlite documentation (2026)

## Date Researched
2026-06-19 | Daily Research Process | A-Tech Research Division

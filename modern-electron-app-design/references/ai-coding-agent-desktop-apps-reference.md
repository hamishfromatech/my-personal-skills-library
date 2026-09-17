# AI Coding Agent Desktop Apps — Design Reference

This reference covers the design patterns, architecture decisions, and UI approaches used by three major AI coding agent desktop apps: **Claude Code Desktop**, **OpenCode Desktop**, and **Hermes Desktop**. These apps represent the state of the art in Electron-based AI coding tool design.

---

## 1. Claude Code Desktop

**Source:** https://code.claude.com/docs/en/desktop
**Developer:** Anthropic
**Stack:** Electron, React, TypeScript

### Overview
Claude Code Desktop is Anthropic's official desktop application for AI-assisted software development. It provides a graphical interface for the Claude Code CLI engine with parallel sessions, drag-and-drop pane layout, integrated terminal, file editor, side chats, computer use, visual diff review, app previews, and PR monitoring.

### Key Design Patterns

#### Three-Tab Architecture
The app has three main tabs:
- **Chat** — General conversations with Claude
- **Cowork** — Dispatch and longer agentic work
- **Code** — Software development (the primary surface)

#### Session-Based Architecture
- Each conversation is a **session** with its own chat history, project folder, and code changes
- Sessions are **independent** — changes in one don't affect others
- **Git worktrees** provide isolation: each session gets its own copy of the project
- Sessions can run in **parallel** — click + New Session or press Cmd+N
- Sessions can be viewed side-by-side (hold Cmd/Ctrl and click a session)

#### Pane Layout System
The Code tab is built around **draggable, resizable panes**:
- Chat pane — main conversation
- Diff pane — file-by-file code review
- Preview pane — embedded browser for app preview
- Terminal pane — integrated terminal
- File pane — inline file editor
- Plan pane — task planning
- Tasks pane — background work monitoring
- Subagent pane — agent output

Users can drag panes by their headers to reposition, or drag edges to resize.

#### Permission Mode System
Five permission modes control Claude's autonomy:
| Mode | Behavior |
|---|---|
| Ask permissions | Claude asks before editing files or running commands |
| Auto accept edits | Auto-accepts file edits, asks before terminal commands |
| Plan mode | Reads and explores, proposes a plan without editing |
| Auto | Executes all actions with background safety checks |
| Bypass permissions | Runs without permission prompts (sandboxed only) |

#### Environment Configuration
Three environment types for where Claude executes:
- **Local** — runs on your machine
- **Remote** — runs on Anthropic's cloud infrastructure (continues even if you close the app)
- **SSH** — runs on a remote machine over SSH

#### UI Features
- **Side chat** (Cmd+;) — ask a question using session context without derailing the main conversation
- **Diff view** with inline commenting — click any line to add a comment, submit all at once
- **Auto-verify** — Claude automatically verifies code changes after editing (screenshots, DOM inspection, form filling)
- **View modes** — Normal (tool calls collapsed), Verbose (every step), Summary (final responses only)
- **@mention files** — type @ followed by a filename to add context
- **File attachments** — images, PDFs, drag-and-drop
- **Keyboard shortcuts** — comprehensive set (Cmd+N new session, Cmd+W close, Ctrl+Tab cycle, etc.)
- **Computer use** — Claude can control your screen, open apps, click and type
- **Connectors** — MCP-based integrations (GitHub, Slack, Linear, Notion, Google Calendar)
- **Skills** — slash commands that extend Claude's capabilities
- **Plugins** — reusable packages adding skills, agents, hooks, MCP servers

#### Enterprise Features
- Admin console controls (disable Code, bypass permissions, etc.)
- Managed settings files pushed via MDM
- SSH host allowlisting
- Device management policies (macOS MDM, Windows group policy)
- SSO authentication

#### Design Philosophy
- **Desktop runs the same engine as CLI** — shared config, project memory, MCP servers
- **CLI-to-Desktop migration** — run `/desktop` in terminal to move a session
- **Desktop for visual work** (parallel sessions, panes, diff review), **CLI for scripting/automation**
- Auto-updates, OS notifications for completed tasks

---

## 2. OpenCode Desktop

**Source:** https://dev.to/brendonovich/moving-opencode-desktop-to-electron-4hip
**Developer:** OpenCode (anomalyco)
**Stack:** Electron, TypeScript, React (migrated from Tauri)

### Overview
OpenCode is an open-source AI coding agent. The desktop app provides a graphical interface for the OpenCode server, which handles agent loops, LLM communication, and SQLite database interactions.

### Migration from Tauri to Electron

OpenCode Desktop was originally built with **Tauri** but migrated to **Electron**. Key reasons:

1. **WebKit rendering inconsistencies** — Tauri uses WebKit on macOS and Linux, which has worse performance than Chromium and minor CSS/style inconsistencies across platforms. This directly impacted the ability to ship a consistent experience.

2. **CLI bundling issues** — Running the bundled CLI impacted startup time and occasionally failed on Windows.

3. **Node.js integration** — Moving from Bun to Node.js meant the server code could run directly within Electron's built-in Node process, eliminating the need to bundle and manage a separate CLI process.

### Architecture
- **Client-server model** — everything written in TypeScript
- Clients (TUI, web UI, desktop) talk to a server that handles agent loops, LLM communication, and SQLite database
- The desktop app bundles the server and runs it within Electron's Node process
- The web UI connects to servers over HTTP

### Design Approach
- **Portable UI** — designed to work across TUI, web, and desktop surfaces
- **Minimal setup** — download and run, no CLI wrestling
- **Cross-platform** — macOS, Windows, Linux

### Related: nanasi-apps/opencode-desktop
**Source:** https://github.com/nanasi-apps/opencode-desktop
**Stack:** Electron, Vue 3, TypeScript
**Status:** Archived (Jun 2026)

A community macOS Electron app for OpenCode Web setup:
- Guided setup for OpenCode + Oh My OpenCode
- Built-in OpenCode Web in a native desktop window
- Optional Cloudflare Tunnel for remote access
- Tray/menu-bar mode for always-ready workflow
- Auto-installs Homebrew, OpenCode, and Oh My OpenCode

### Related: marmotz-dev/opencode-ui
**Source:** https://github.com/marmotz-dev/opencode-ui
A simple desktop client for OpenCode providing a user-friendly interface to interact with OpenCode, offering control and freedom to use any provider, any model, and any editor.

---

## 3. Hermes Desktop

**Source:** https://hermes-agent.nousresearch.com/docs/user-guide/desktop
**Developer:** Nous Research (official) / fathah (community, 12.8k stars)
**Stack:** Electron, React, TypeScript (fathah/hermes-desktop)

### Overview
Hermes Desktop is a native desktop app built around the same Hermes Agent core as the CLI and gateway — same config, same API keys, same sessions, same skills, same memory. It is not a separate product or lightweight clone; it uses the same Hermes Agent core and drives it through a modern, thoughtfully designed UI.

### Key Design Patterns

#### Chat-First Window with Left Sidebar
- The app is organized as a **chat-first window** with a left sidebar for navigation
- Designed for managing multiple simultaneous agent conversations
- Configure messaging providers, create artifacts, browse project folder structures
- Work on multiple projects at once

#### Streaming Chat UI
- **Streaming responses** with live tool activity and structured tool-call summaries
- Same conversation history as every other Hermes surface — sessions started here resume in CLI/TUI and vice versa
- **Drag-and-drop files** anywhere in the chat area to attach them
- **Right-hand preview rail** — render web pages, files, and tool outputs side by side while chatting
- **Composer history** — up/down arrow keys in empty composer to recall and reuse previous prompts
- **Queue editing** — edit messages queued up before they're sent

#### Status Bar
- Bottom bar shows live session state
- **Per-session YOLO toggle** — bypasses dangerous-command approval prompts for just this session

#### Model Picker
- Lives in the composer, just left of the microphone
- Switch model, reasoning effort, and fast mode from one dropdown
- **Sticky UI state** — remembered locally per device, follows across new chats and restarts
- Per-model effort/fast presets — each model remembers its own reasoning effort and fast-mode choice

#### File Browser
- Explore and preview the working directory without leaving the app
- Useful for following along as the agent reads, writes, and edits files
- Set initial project directory with `hermes desktop --cwd <path>`

#### Voice Mode
- Talk to Hermes and hear it back
- On macOS, the OS prompts once for microphone access

#### Settings & Onboarding
- Manage providers, models, tools, and credentials from a real UI instead of editing YAML
- **First-run onboarding** — gets users to their first message in seconds
- Settings panes cover: providers/keys, model selection, toolset configuration, MCP servers, gateway, session management
- **Providers settings pane** — dedicated place with Accounts/API-keys UX for signing in and storing credentials per provider
- Every provider and model in the menus — full catalog, not a curated subset
- **Tool-backend installs from GUI** — run post-setup install steps directly from the app

#### Management Panes
- **Skills** — browse, install, and manage skills
- **Cron** — view and manage scheduled jobs
- **Profiles** — switch between Hermes profiles (isolated config/skills/sessions)
- **Messaging** — set up gateway channels
- **Agents and Command Center** — orchestration surfaces for multi-agent work

#### Keyboard & Navigation
- **Command palette** (Cmd+K) — jump to actions and navigate from keyboard
- **Rebindable shortcuts** — remap keyboard shortcuts in Settings
- **Custom zoom shortcuts** — zoom in half-step increments
- **UI language switcher** — change interface language in-app

#### Remote Backend Support
- Connect to a Hermes backend running on another machine (VPS, home server, Mini behind Tailscale)
- Two auth methods: OAuth (Nous Portal) for public-facing, username/password for trusted networks
- Per-profile remote hosts — each profile can point at its own remote backend

#### Uninstall Options
Three levels of removal:
1. **Uninstall Chat GUI only** — removes desktop app and its data; agent, config, and chats stay
2. **Uninstall GUI + agent, keep my data** — removes app and agent but keeps config, chats, secrets
3. **Uninstall everything** — removes app, agent, and all user data

### fathah/hermes-desktop (Community Edition)
**Source:** https://github.com/fathah/hermes-desktop
**Stars:** 12.8k | **Forks:** 1.5k | **License:** MIT

The most popular community Hermes desktop app with extensive features:
- **22 slash commands** — /new, /clear, /fast, /web, /image, /browse, /code, /shell, /usage, /help, /tools, /skills, /model, /memory, /persona, /version, /compact, /compress, /undo, /retry, /debug, /status
- **14 toolsets** — web, browser, terminal, file, code execution, vision, image gen, TTS, skills, memory, session search, clarify, delegation, MoA, task planning
- **16 messaging gateways** — Telegram, Discord, Slack, WhatsApp, Signal, Matrix, Mattermost, Email, SMS, iMessage, DingTalk, Feishu/Lark, WeCom, WeChat, Webhooks, Home Assistant
- **Memory system** — view/edit memory entries, user profile memory, capacity tracking, discoverable memory providers
- **Persona editor** — edit SOUL.md personality
- **Scheduled tasks** — cron job builder with 15 delivery targets
- **Token usage tracking** — live prompt/completion token counts and cost display
- **Session management** — full-text search (SQLite FTS5), date-grouped history, resume and search
- **Profile switching** — create, delete, switch between separate Hermes environments
- **Backup, import & debug dump** — full data backup/restore and system diagnostics
- **Log viewer** — view gateway and agent logs from Settings
- **Auto-updater** — via electron-updater
- **i18n ready** — internationalization framework
- **Test suite** — SSE parser, IPC handlers, preload API surface, installer utilities, constants validation

---

## Common Design Patterns Across All Three Apps

### 1. Chat-First Interface
All three apps center around a chat interface as the primary interaction mode. The chat is where users send prompts, see streaming responses, and interact with the AI agent.

### 2. Session Management
- **Claude Code** — parallel sessions with Git worktree isolation, sidebar session list, session filtering
- **Hermes** — session list with archiving, full-text search, date-grouped history, cross-profile sessions
- **OpenCode** — persistent chat history per project

### 3. Sidebar Navigation
All three use a left sidebar for navigation between different views (sessions, settings, projects, etc.).

### 4. Streaming Responses
Real-time streaming of AI responses with live indicators for tool activity, code changes, and agent actions.

### 5. File System Integration
- **Claude Code** — file pane, @mention files, drag-and-drop attachments, file browser
- **Hermes** — file browser, drag-and-drop files into chat, preview rail
- **OpenCode** — project-based directory context

### 6. Multi-Provider Support
All three support multiple AI model providers (Anthropic, OpenAI, Google, local models, etc.) with easy switching.

### 7. Dark Theme
All three default to developer-friendly dark themes with Claude-inspired or custom color schemes.

### 8. Electron + React/Vue + TypeScript
The dominant stack is Electron with React (or Vue) and TypeScript, using Vite for build tooling.

### 9. CLI Integration
All three apps wrap or integrate with a CLI tool — they're graphical frontends for command-line AI coding agents.

### 10. Security-First Architecture
- Context isolation enabled
- Node integration disabled in renderer
- Content Security Policy enforced
- Preload scripts for secure IPC

---

## Design Lessons for Building AI Coding Agent Desktop Apps

### What Makes a Great AI Coding Desktop App

1. **Session isolation** — Each conversation should be independent with its own context, file changes, and Git state. Git worktrees (Claude Code) are the gold standard.

2. **Parallel workflows** — Users need to work on multiple tasks simultaneously. Side-by-side session viewing and easy switching are critical.

3. **Visual diff review** — Code changes should be reviewable file-by-file with inline commenting before committing.

4. **Integrated preview** — An embedded browser for previewing app changes, with auto-verification (Claude Code's auto-verify is a standout feature).

5. **Permission granularity** — Users need fine-grained control over what the AI can do (read files, edit files, run commands, access the web).

6. **Pane-based layout** — Draggable, resizable panes let users arrange their workspace (chat, diff, preview, terminal, files) however they want.

7. **Side chats** — The ability to ask a question without derailing the main conversation is a killer feature (Claude Code's Cmd+; side chat).

8. **Environment flexibility** — Support for local, remote/cloud, and SSH execution environments.

9. **Rich settings UI** — Instead of editing YAML/JSON config files, provide a graphical settings interface for providers, models, tools, and credentials.

10. **First-run onboarding** — Guided setup that gets users from download to first message in seconds.

### Tech Stack Recommendations for AI Coding Desktop Apps

| Component | Recommendation |
|---|---|
| Framework | Electron (for Chromium rendering consistency) |
| UI Library | React 18+ or Vue 3 |
| Language | TypeScript |
| Build Tool | Vite / Electron-Vite |
| Styling | Tailwind CSS |
| Components | ShadCN UI or custom |
| IPC | contextBridge + ipcRenderer/ipcMain |
| State | Zustand, Jotai, or React Context |
| Streaming | Server-Sent Events (SSE) |
| Database | SQLite (via better-sqlite3) |
| Packaging | Electron Builder or Electron Forge |

### Why Electron Wins for AI Coding Apps
- **Chromium rendering** — consistent CSS and performance across platforms (vs. WebKit in Tauri)
- **Node.js integration** — run server/agent code directly in the same process
- **Mature ecosystem** — extensive tooling, plugins, and community support
- **Cross-platform** — macOS, Windows, Linux from a single codebase
- **Auto-update** — mature update infrastructure (electron-updater, Squirrel)

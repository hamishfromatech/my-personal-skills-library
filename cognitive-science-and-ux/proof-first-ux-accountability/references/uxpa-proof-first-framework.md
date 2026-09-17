# Proof-First UX Framework: Stop Prompting for Luck and Design for Accountability

**Source:** UXPA Magazine, April 8, 2026  
**Author:** Isabel Novais (Design Director at Usercentrics)  
**URL:** https://uxpamagazine.org/proof-first-ux-framework-stop-prompting-for-luck-and-design-for-accountability/

## Core Argument
Current AI interfaces force users to "prompt for luck" — throwing requests into opaque parameters and hoping for quality outputs. The Proof-First Framework reorients AI UX around accountability, where users stop fighting invisible parameters and can correct the system's understanding of their intent.

## Key Principles

### 1. Users Stop Fighting Invisible Parameters
Most AI tools hide the context, constraints, and model settings that shape outputs. Proof-first design makes these visible and editable.

### 2. Users Can Correct the System's Understanding
Instead of re-prompting from scratch when outputs miss the mark, users should be able to adjust the AI's retrieved context, constraints, or reasoning path directly.

### 3. Accountability Over Autonomy
The framework distinguishes between "autonomous AI" (which makes decisions users can't trace) and "accountable AI" (which shows its work and invites correction).

## Industry Context
The NN/g State of UX 2026 report identifies that available roles will demand "breadth and judgment, not just artifacts." Proof-first UX is the interface layer that cultivates that judgment by making AI reasoning legible to users.

## Technical Implementation Notes
- Parameter transparency requires exposing model selection, temperature, top-p, and context retrieval methods
- Context editing requires allowing users to modify retrieved documents or knowledge base entries before generation
- Reasoning traces can be generated through chain-of-thought prompting or explicit step-by-step architecture
- Accountability signatures can use content-addressable hashing to tie outputs to specific input configurations

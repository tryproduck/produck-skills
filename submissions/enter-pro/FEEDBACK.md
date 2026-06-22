# Enter Pro — Captured User Friction

> **Captured via:** Produck feedback extension  
> **Product tested:** Enter Pro (https://enterpro.dev)  
> **Session type:** Real developer workflow — building a full-stack web app (YourJam monorepo)  
> **Date:** June 2026  

---

## FP1 — No Rollback UI (State Destruction)

**What I was trying to do:**  
I asked Enter Pro to refactor `frontend/src/pages/Home.jsx` to improve performance. It made changes across 3 files simultaneously. One of the changes was incorrect and broke the layout.

**What got in the way:**  
Enter Pro had no built-in undo or rollback mechanism. The only option was `Ctrl+Z` which doesn't help when the agent has made 80+ edits across multiple files. I had to manually `git diff` each file to understand what happened and restore manually.

**Specific friction:**  
- No atomic rollback for a session of agent edits
- No "undo this agent session" button
- I lost 35 minutes recovering manually from a bad agent run

---

## FP2 — Memory Desync (Context Drift)

**What I was trying to do:**  
Between sessions, I manually refactored the imports in `Home.jsx` to clean up dead code. Then I started a new Enter Pro session and asked it to add a new feature to the same file.

**What got in the way:**  
Enter Pro had no knowledge of my manual changes. It re-introduced the exact imports I had deleted, referencing components that no longer existed. The agent's context was completely stale.

**Specific friction:**  
- Agent has zero awareness of manual developer edits between sessions
- No mechanism to "re-sync" the agent's mental model of the codebase
- Agent confidently wrote code based on stale file content

---

## FP3 — Forced Commits / No Staging (Destructive Merges)

**What I was trying to do:**  
I asked Enter Pro to implement a set of 5 features. It completed all 5, but 2 of the implementations were wrong. When it committed, it committed everything together.

**What got in the way:**  
There was no per-file review mechanism. I couldn't accept the 3 good changes and reject the 2 bad ones without manually cherry-picking through git. The agent treats its output as an atomic bundle.

**Specific friction:**  
- No per-file accept/reject before committing
- Agent bundles all changes into one commit
- Difficult to selectively keep good changes and discard bad ones

---

## FP4 — Blind Workspace Trust (Environmental Decay)

**What I was trying to do:**  
I opened a new session in a monorepo project. Enter Pro started writing code immediately without checking the environment.

**What got in the way:**  
The monorepo had no root `package.json` and the frontend node_modules was partially corrupted. Enter Pro wrote code importing packages that weren't installed, generating runtime errors immediately.

**Specific friction:**  
- No pre-flight check on workspace health before coding
- Agent doesn't know if `node_modules` are installed
- Agent doesn't check for missing `.env` variables or security vulnerabilities before starting

---

## FP5 — No Long-term Memory (Context Amnesia)

**What I was trying to do:**  
Every new Enter Pro session required re-explaining the project structure, coding conventions (no Tailwind, use HSL colors, always use arrow functions), and architectural decisions.

**What got in the way:**  
Enter Pro starts completely fresh every session. There is no persistent memory of project rules or team conventions. Re-teaching the agent takes 5-10 minutes at the start of every session.

**Specific friction:**  
- Every session starts from zero context
- Project-specific coding rules must be re-stated every time
- No persistent memory store for agent directives or style guides

---

## Summary Table

| ID  | Friction Point           | Severity | Time Lost |
|-----|--------------------------|----------|-----------|
| FP1 | No Rollback UI           | Critical | 35 min    |
| FP2 | Memory Desync            | High     | 20 min    |
| FP3 | No Staged Review         | High     | 15 min    |
| FP4 | Blind Workspace Trust    | Medium   | 10 min    |
| FP5 | No Long-term Memory      | High     | 8 min/session |

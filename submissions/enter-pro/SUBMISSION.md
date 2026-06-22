# AgentGuard — Quackathon Track 03 Submission

> **Team:** paarth293
> **Track:** 03 — Product: I can do it better
> **Product tested:** Enter Pro by Converge AI (https://enter.converge.ai)
> **Submission date:** June 22, 2026

---

## 🔗 Quick Links

| Resource | Link |
|----------|------|
| 🎥 Demo Video | *(paste Loom link here after recording)* |
| 💻 Working Codebase (VS Code Extension) | *(paste GitHub repo link here)* |
| 🖥️ High-Fidelity Mockup | [`mockup/index.html`](mockup/index.html) |
| 📋 PRD | [`PRD.md`](PRD.md) |
| 🦆 Produck Feedback | [`FEEDBACK.md`](FEEDBACK.md) |

---

## 🛠️ Tool Integration — How Produck Was Used End to End

### Step 1: Listen (Produck Chrome Extension)
- Installed the Produck Chrome extension
- Used Enter Pro at `https://enter.converge.ai` as a real developer building a project
- Captured **5 friction points** by clicking the Produck droplet at the moment of frustration
- Each capture included: page URL, exact session context, and a structured description of what went wrong

### Step 2: Diagnose (Produck MCP)
- Issued a Produck MCP token at `tryproduck.com/app → MCP section`
- Connected the coding agent to the Produck MCP server at `https://tryproduck.com/api/mcp`
- Called `search_feedback` to retrieve all 5 captured feedback tickets with their IDs and session metadata
- Called `get_feedback` on each ticket to load full session context

**MCP calls made:**
```json
// List available tools
POST https://tryproduck.com/api/mcp
{"jsonrpc":"2.0","id":1,"method":"tools/list"}

// Pull all captured feedback
POST https://tryproduck.com/api/mcp
{"jsonrpc":"2.0","id":2,"method":"tools/call","params":{"name":"search_feedback","arguments":{"limit":50}}}
```

**Feedback IDs retrieved:**
- `8c2df628` — No Rollback Feature
- `b7a6931b` — Memory Desync
- `adc70843` — Forced Commits
- `4ac21a1b` — Blind Workspace Trust
- `e6489944` — No Long-Term Memory

### Step 3: Decide (Produck Skill — user-alignment)
- Used the `user-alignment` skill from this forked repo (`tryproduck/produck-skills`)
- Converted the 5 MCP-retrieved friction points into a structured PRD (`PRD.md`)
- Each PRD section traces directly back to its Produck Feedback ID and timestamp

### Step 4: Ship (High-Fidelity Mockup + Working Extension)
- Executed the PRD with the coding agent to produce `mockup/index.html` — a fully interactive safety layer dashboard
- Built the complete working VS Code extension (`AgentGuard`) implementing all 5 PRD features in TypeScript + Python

---

## 🛡️ What We Built — AgentGuard

A VS Code extension with 5 safety features, each solving one captured friction point:

| Feature | Solves | Produck ID |
|---------|--------|------------|
| Checkpoint & Rollback | No recovery path after bad agent edits | `8c2df628` |
| Memory Sync | Agent re-introduces manually deleted code | `b7a6931b` |
| Diff Review (per-file accept/reject) | Agent blindly commits everything | `adc70843` |
| Workspace Health Check | Agent codes on broken environments | `4ac21a1b` |
| Parcel Memory (TrueMemory SQLite) | Agent forgets rules every session | `e6489944` |

---

## 📦 Deliverables

| Requirement | Status | Location |
|-------------|--------|----------|
| GitHub Repository (fork with clear README + 2 PRs) | ✅ | `github.com/paarth293/produck-skills` |
| PR 1: Skill improvements | ✅ | Skills improved on `main` branch |
| PR 2: Mockup + PRD + Feedback | ✅ | This PR (`pr2/enter-pro-mockup`) |
| Demo Video (2-min walkthrough) | 🔲 | *(link to be added)* |
| Live Mockup | ✅ | `submissions/enter-pro/mockup/index.html` |
| Tool Integration documentation | ✅ | This file (`SUBMISSION.md`) |

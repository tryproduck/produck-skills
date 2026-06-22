# 🦆 Produck Skills

**High-quality, user-experience-centered skills for your coding agents.**

---

## 🏆 Quackathon Track 03 Submission — AgentGuard

This repository is our official submission for **Track 03: Product — I can do it better** at the Quackathon. 

### 🛡️ What is AgentGuard?
**AgentGuard** is a developer safety and control layer for AI-driven development. It addresses key trust and blast-radius issues when using coding agents like **Enter Pro** by providing:
1. **Checkpointing & Atomic Rollback (FP1):** Save and restore workspace snapshots in < 5 seconds.
2. **Memory Sync (FP2):** Auto-detect manual file changes made by developers between sessions to sync the agent's mental model.
3. **Diff Staging Review (FP3):** Per-file accept/reject panel to avoid destructive, blind code commits.
4. **Workspace Health Auditor (FP4):** Pre-flight health score before starting agent sessions.
5. **Persistent Project Memory (FP5):** A local SQLite database (backed by **TrueMemory**) to persist coding guidelines and rulebooks across chat sessions.

---

### 📂 Submission Deliverables

We have submitted **two pull requests** back to the upstream repository:

#### 1. Pull Request 1: Skill Enhancements (Telemetry & Memory Guidelines)
* **Status:** Pushed to `main` branch.
* **Goal:** Enhance agent capability to ingest workspace session replays/telemetry and leverage persistent preferences files.
* **Files:**
  * [`skills/user-alignment/SKILL.md`](skills/user-alignment/SKILL.md) — Added guidelines for telemetry ingestion (replays, console stack traces, network payloads) and persistent memory sync.
  * [`skills/user-alignment/references/prd-template.md`](skills/user-alignment/references/prd-template.md) — Added schema and placeholders for session evidence and persistent rules configuration.

#### 2. Pull Request 2: Enter Pro Feedback, PRD & Interactive Mockup
* **Status:** Pushed to branch [`pr2/enter-pro-mockup`](https://github.com/paarth293/produck-skills/tree/pr2/enter-pro-mockup).
* **Goal:** Prove the feedback loop on Enter Pro and present the interactive safety layer solution design.
* **Files:**
  * [`submissions/enter-pro/FEEDBACK.md`](submissions/enter-pro/FEEDBACK.md) — 5 specific friction points captured via the Produck Chrome extension.
  * [`submissions/enter-pro/PRD.md`](submissions/enter-pro/PRD.md) — Full product requirements for the AgentGuard safety layer.
  * [`submissions/enter-pro/mockup/index.html`](submissions/enter-pro/mockup/index.html) — High-fidelity, interactive HTML mockup showcasing the dashboard and safety tabs.

---

### 💻 The Working Project Codebase
The full, working implementation code for the **AgentGuard VS Code Extension** is located in the companion directory:
* 📁 Codebase Directory: [`final_quack`](file:///C:/Users/paart/OneDrive/Desktop/final_quack)
* ⚙️ Features: A fully compiled and functional TypeScript VS Code extension that communicates via a Python subprocess bridge to a real SQLite database (`~/.truememory/memories.db`), managing persistent agent directive rules and checkpoint logs.

---

## Install

One line installs every skill in this repo into your coding agent:

```bash
npx skills add tryproduck/produck-skills
```

That's it. It uses the open [`skills`](https://github.com/vercel-labs/skills) CLI (no global install,
just `npx`) and works across 70+ agents — Claude Code, Cursor, Codex, Copilot, and more. The skills
are available the next time your agent starts.

---

## Skills Catalog

| Skill | What it does |
| --- | --- |
| [`user-alignment`](skills/user-alignment/SKILL.md) | Turn vague or messy user requests into aligned, agent-executable PRDs — with scope, bounded phases, testable acceptance criteria, and explicit do-not-do boundaries. |

More user-experience skills are on the way.

---

## Contributing

Want to add a skill? See [CONTRIBUTING.md](CONTRIBUTING.md) for the skill-authoring contract
(folder layout, required frontmatter, and the PR checklist). The bar is high-quality, UX-centered
skills only.

---

## About Produck

These skills come from the team building [Produck](https://tryproduck.com) — **Build products your users love.** 

Produck enables apps to capture in-context feedback from their users and rapidly ship features and fixes that improve product-market fit.

- 🌐 Website: [tryproduck.com](https://tryproduck.com)
- 📚 Docs: [docs.tryproduck.com](https://docs.tryproduck.com)

## License

[Apache-2.0](LICENSE)

# 🦆 Produck Skills

**High-quality, user-experience-centered skills for your coding agents.**

This repo is our open-source, documentation-driven collection of [agent skills](https://code.claude.com/docs/en/skills) to help you build products users love.
Each skill is a folder of markdown that teaches a coding agent (Claude Code, Cursor, Codex, Copilot, and others) how to ship features and fixes that your users want to improve product-market fit — starting with turning messy user requests into context-rich PRDs your agents can actually execute.

![Star History Chart](https://api.star-history.com/svg?repos=tryproduck%2Fproduck-skills&type=timeline&legend=top-left)

> ⭐ **If these skills are useful, [star the repo](https://github.com/tryproduck/produck-skills)** — it helps other builders find it.

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
| [`produck-feedback`](skills/produck-feedback/SKILL.md) | Act on real in-context user feedback: pull a feedback ticket's full context through the Produck MCP and drive a feedback → aligned PRD → build loop. Pairs with `user-alignment`. |

More user-experience skills are on the way.

---

## Produck MCP & plugin

The `produck-feedback` skill drives the **Produck MCP** — a hosted server that gives your agent two
tools over your own feedback:

- **`search_feedback`** — find relevant feedback tickets (filter by domain).
- **`get_feedback`** — load one ticket's full context: the user's written/spoken feedback,
  reconstructed page HTML, annotation markers, session activity, and a generated design doc.

Both installs need a Produck MCP token — mint one at
[tryproduck.com/app](https://tryproduck.com/app) → **MCP** (looks like `mcp_live_…`). The plugin reads
it at runtime; set it **before** the MCP tools will work (the skills work without it).

### Claude Code — plugin (skills + MCP)

```bash
# 1) in your shell: save your token
mkdir -p ~/.produck && echo "mcp_live_YOUR_TOKEN" > ~/.produck/mcp_token
```
```
# 2) in Claude Code:
/plugin marketplace add tryproduck/produck-skills
/plugin install produck@produck
```

### Codex — plugin (skills + MCP)

```bash
# 1) in your shell: export your token
export PRODUCK_MCP_TOKEN=mcp_live_YOUR_TOKEN

# 2) add the marketplace, then enable "produck" from the /plugins browser
codex plugin marketplace add tryproduck/produck-skills
```

### Any other agent

Install the skills with `npx skills add tryproduck/produck-skills`, then add the MCP server using your
agent's native config — see
[`skills/produck-feedback/references/connect-mcp.md`](skills/produck-feedback/references/connect-mcp.md)
for Cursor, opencode, manual Codex, and manual Claude Code snippets.

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
- 🛠️ Maintained by the Produck team, including [Xander Minzenmay](https://github.com/LucaXav)

## License

[Apache-2.0](LICENSE)

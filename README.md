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

## Skills catalog

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

[MIT](LICENSE)

---
name: produck-feedback
description: Use when acting on real user feedback — you have a Produck feedback ticket or URL, or the user wants to fix something users complained about or build a feature users asked for. Pulls full feedback context through the Produck MCP and drives a feedback → aligned PRD → build loop. Triggers on "fix what users reported", a feedback ticket id, "use my produck feedback", or building from a feedback queue.
license: Apache-2.0
metadata:
  author: produck
  version: "1.0.0"
---

# Building from Produck feedback

**Purpose:** Turn real, in-context user feedback into shipped fixes. This skill drives the
[Produck](https://tryproduck.com) MCP — it tells the agent how to find the right feedback, load the
full context behind it, and convert that context into an aligned change instead of a guess.

> Requires the Produck MCP server connected. If `search_feedback` / `get_feedback` are not available,
> see [`references/connect-mcp.md`](references/connect-mcp.md) to connect it and mint a token first.

---

## 1. What this connects to

Produck captures feedback **in the user's own session**, so a ticket is far richer than a text
complaint. Each ticket can carry:

- **Written and spoken feedback** — the user's typed note plus any voice transcription.
- **Reconstructed page HTML** — a static snapshot of the exact page the user was looking at.
- **Annotation markers** — the specific elements/regions the user pointed at.
- **Session activity** — a timeline of what the user did leading up to the feedback.
- **A generated design doc** — Produck's own first-pass analysis of the issue, when present.

The job of this skill is to pull that context and use **all** of it, not just the title.

## 2. The two MCP tools

The server is named `produck` (tools may appear prefixed, e.g. `mcp__produck__search_feedback`).

### `search_feedback` — find relevant tickets
Returns lightweight summaries; use it to locate tickets, then load full context with `get_feedback`.

| Input | Type | Notes |
| --- | --- | --- |
| `domain` | string, optional | Filter to one website domain. |
| `limit` | int 1–50, optional | Defaults to 20. |
| `cursor` | string, optional | Pagination cursor from a previous call. |

Returns `{ items, nextCursor, count }` — each item a summary (id, domain, page URL, created-at,
annotation count, source). Page with `cursor` until `nextCursor` is null when you need the full set.

### `get_feedback` — load one ticket in full
| Input | Type | Notes |
| --- | --- | --- |
| `feedbackId` | string, required | The ticket id (from a `search_feedback` item, a URL, or the user). |

Returns the full ticket: written/spoken feedback, reconstructed HTML, annotations, session activity,
and the design doc when present. **Always `get_feedback` before building** — never act on a summary alone.

## 3. The operating loop

```text
search_feedback   → find the relevant ticket(s) (filter by domain, page as needed)
get_feedback      → load ONE ticket's full context
user-alignment    → turn the ticket into an agent-executable PRD (do not skip)
build             → implement the fix / high-fidelity mockup
verify            → check the change against the ticket's annotations + design doc
```

- **Do not jump from a raw ticket straight to code.** A ticket is a user *request*, not a spec. Hand
  it to the **`user-alignment`** skill first to produce an aligned PRD — interpreted intent, scope,
  non-goals, acceptance criteria — then build from that. The two skills are designed to compose.
- **Ground every decision in the captured context.** Use the reconstructed HTML and annotation markers
  to locate the exact elements involved; use the session activity to understand what the user was
  doing; use the design doc as a starting hypothesis, not the final answer.
- **Close the loop.** When you finish, verify the result against the annotations and the stated
  outcome — the same ticket is your acceptance check.

## 4. Setup

To connect the MCP, choose the install path for your agent and mint a token — see
[`references/connect-mcp.md`](references/connect-mcp.md).

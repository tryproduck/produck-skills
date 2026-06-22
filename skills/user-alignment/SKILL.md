---
name: user-alignment
description: Use when turning a vague or messy user request into an aligned, agent-executable PRD. Converts raw requests into testable specs with scope, phases, acceptance criteria, and explicit do-not-do boundaries for coding agents. Triggers on planning a feature from a rough ask, writing a PRD/spec, or aligning on intent before code.
license: Apache-2.0
metadata:
  author: produck
  version: "1.0.0"
---

# User Alignment & Agent-Executable PRDs

**Purpose:** A practical guide for turning messy user requests into aligned, testable Product Requirements Documents (PRDs) that autonomous or semi-autonomous agents can execute without drifting.

> Two supporting files ship with this skill:
> - [`references/prd-template.md`](references/prd-template.md) — the full copy-paste agent-executable PRD template.
> - [`references/reading-list.md`](references/reading-list.md) — the research and reference map this guide is built on.

---

## 1. Core thesis

A good agent PRD is not just a product document. It is a **shared operating contract** between the user, the product owner, and the implementation agent.

It must do three jobs at once:

1. **Align on user intent** — what the user actually wants, why it matters, and what outcome would make them say “yes, that’s it.”
2. **Remove ambiguity before execution** — especially around scope, constraints, priorities, edge cases, and trade-offs.
3. **Translate intent into executable work** — sequenced phases, explicit files/systems, acceptance criteria, tests, and “do not do” boundaries.

For human teams, ambiguity can be resolved in meetings. Agents often resolve ambiguity by guessing. The PRD’s job is to make guessing unnecessary.

---

## 2. Research-backed principles

### 2.1 Start with the user problem, not the implementation

AI/product PRDs should begin with the user pain and the cost of the status quo, not with “we will use AI/model/tool X.” The model, framework, or agent is an implementation detail unless the user explicitly constrained it.

A strong problem statement should include:

- Who is affected.
- What they are trying to accomplish.
- What blocks them today.
- Why existing/manual/deterministic solutions are insufficient.
- What measurable improvement would matter.

**Bad:** “Build an AI assistant for support.”  
**Better:** “Support agents spend 8 minutes triaging each ticket, and 23% are misrouted. We need ticket classification under 2 seconds with at least 92% routing accuracy, while escalating low-confidence cases.”

### 2.2 Treat the PRD as an executable artifact

Spec-driven development treats the spec as a durable source of truth, not disposable planning scaffolding. The spec should be checked into the repo and referenced by agents across sessions.

The PRD should answer:

- What should be built?
- Why does it matter?
- What is explicitly out of scope?
- What order should work happen in?
- How will each phase be verified?
- What should the agent never touch?

### 2.3 Write for two audiences: humans first, agents second

The top of the PRD should explain the product and user context in human language. The lower sections should become increasingly operational and literal for the agent.

Recommended split:

| Section type | Audience | Style |
| --- | --- | --- |
| Problem, users, goals, success metrics | Humans + agents | Clear prose, rationale, trade-offs |
| Scope, constraints, edge cases | Humans + agents | Structured bullets/tables |
| Phases, tasks, tests, commands | Agents | Explicit, sequential, verifiable |
| “Do not do” instructions | Agents | Direct prohibitions, no nuance |

### 2.4 Break work into bounded phases

Traditional PRDs often describe the whole product and leave sequencing to engineers. Agent PRDs should define implementation phases with dependencies and testable outputs.

Each phase needs:

- **Dependency:** what must already exist.
- **Scope:** what this phase covers.
- **Out of scope:** adjacent work the agent must not do yet.
- **Tasks:** concrete implementation actions.
- **Verification:** commands, tests, screenshots, or manual checks that prove completion.

### 2.5 Define evaluation before implementation

For AI or agentic features, “works” is rarely binary. Define launch thresholds, target thresholds, and aspirational thresholds before building.

Include three metric classes:

1. **Outcome metrics** — user/business result, e.g. completion rate, time saved, conversion lift.
2. **Quality metrics** — correctness, relevance, tone, completeness, usefulness.
3. **Operational metrics** — latency, cost, reliability, throughput, escalation rate.

For agent-executed software projects, also define:

- Unit/integration/e2e tests required.
- Manual QA checks.
- Review criteria.
- Regression risks.
- Expected screenshots or artifacts.

### 2.6 Make constraints and prohibitions explicit

Agents overbuild when boundaries are unclear. Every PRD should include an explicit “do not do” section.

Examples:

- Do not change authentication.
- Do not modify database schema outside the listed migration.
- Do not install new dependencies without approval.
- Do not touch deployment files.
- Do not redesign unrelated UI.
- Do not commit secrets.
- Do not treat P1/P2 items as required for MVP.

### 2.7 Use semi-formal requirement language when precision matters

For behavioral requirements and acceptance criteria, use a lightweight syntax such as EARS or Given/When/Then.

Useful EARS patterns:

- `WHEN [trigger] THEN [system] SHALL [response]`
- `IF [condition] THEN [system] SHALL [response]`
- `WHILE [state] [system] SHALL [continuous behavior]`
- `WHERE [context] [system] SHALL [contextual behavior]`

Useful BDD pattern:

```gherkin
Given [context]
When [user action or system event]
Then [observable result]
And [additional observable result]
```

Avoid vague terms like “fast,” “simple,” “intuitive,” “robust,” or “user-friendly” unless they are backed by measurable thresholds.

### 2.8 Ingest User Session Replays & Droplet Context

When debugging or implementing fixes from user feedback, agents must not rely solely on text descriptions. They should actively parse available session recordings, screenshots, console error logs, and network payloads captured during the user session. 

An agent-executable PRD must instruct the agent on how to:
- Correlate screenshots to specific UI components and coordinates.
- Parse stack traces and console errors to identify the exact files and lines of code responsible.
- Use network request/response payloads to trace API mismatches or data corruptions.

### 2.9 Leverage Persistent Memory & Preferences

To prevent context drift and avoid re-teaching the agent style preferences (e.g. using arrow functions, specific hook patterns) or workspace constraints across different chat sessions, the project must maintain a persistent memory file (e.g. `.agentguard/parcel-memory.json`). 

The PRD should define:
- How the agent reads this file before every execution step.
- How the agent updates the history/state log in the memory file after completing a phase.
- An explicit rule requiring the agent to align all generated code with these persistent preferences.

---

## 3. User alignment workflow

Use this workflow before writing the PRD.

### Step 1: Capture the raw request

Write the request down as-is. Do not immediately translate it into features.

```markdown
## Raw Request
> [Paste the user’s exact words]
```

### Step 2: Extract the intent

Summarize what you believe the user wants in one paragraph.

```markdown
## Interpreted Intent
The user wants [outcome] for [user/persona] because [problem]. The desired result is [observable success state].
```

### Step 3: Separate facts, assumptions, and unknowns

```markdown
## Facts
- [Directly stated by user]

## Assumptions
- [Reasonable inference, but not confirmed]

## Unknowns
- [Information needed before implementation]
```
## Risks
- Technical risks that could affect implementation
- Data privacy or security concerns
- Timeline or resource constraints
- External dependencies that may block progress

Identify high-risk items before implementation begins and surface them during alignment.
A good agent should act on obvious defaults, but should not hallucinate material requirements. If an unknown changes architecture, cost, privacy, or scope, ask before execution.

### Step 4: Ask only high-leverage clarification questions

Do not interrogate the user with twenty questions. Ask the few questions that materially change what gets built.

Use this priority order:

1. **Outcome:** What does success look like?
2. **User:** Who is this for?
3. **Scope:** What is in/out for v1?
4. **Constraints:** What systems, stack, data, deadline, or policy constraints apply?
5. **Failure tolerance:** What happens if the agent/system is wrong?
6. **Approval:** Who needs to sign off?

### Step 5: Read back the aligned understanding

Before writing an executable PRD, produce a short alignment read-back:

```markdown
My understanding:
- We are solving: [problem]
- For: [users]
- Success means: [metrics / user-visible outcome]
- MVP includes: [scope]
- MVP excludes: [non-goals]
- Key constraints: [constraints]
- Open questions: [remaining unknowns]
```

This catches mismatches early, before the agent turns them into code.

---

## 4. Agent-executable PRD template

The full copy-paste template lives in **[`references/prd-template.md`](references/prd-template.md)**.

Copy it into `docs/prds/[feature-name].md` and fill it in. It covers: raw request, aligned
understanding, goals/non-goals/success metrics, scope, user stories with acceptance criteria, UX
requirements, technical context, agent instructions and prohibitions, implementation phases,
testing/evaluation plan, rollout/monitoring/fallback, risks and open questions, and a readiness
checklist.

---

## 5. PRD review rubric for agents

Before handing a PRD to an implementation agent, score it against this rubric.

| Area | Pass condition | Red flag |
| --- | --- | --- |
| User alignment | A third party can explain who the user is and what success means | “User-friendly,” “better,” or “AI-powered” without concrete outcome |
| Scope | In-scope and out-of-scope are both explicit | Only lists features to build, not what to avoid |
| Priority | P0/P1/P2 or phase ordering exists | Everything appears equally important |
| Requirements | Each requirement is atomic and testable | Multiple behaviors packed into one sentence |
| Acceptance criteria | Observable Given/When/Then or EARS statements | Subjective criteria like “works well” |
| Technical context | Stack, files, APIs, data, and constraints are listed | Agent must infer architecture from scratch |
| Evaluation | Metrics and thresholds exist | “We’ll know it when we see it” |
| Failure modes | Edge cases, fallback, rollback are defined | Happy path only |
| Agent boundaries | Explicit “do not do” list exists | Agent can modify adjacent systems freely |
| Execution | Phases have dependencies and verification | One giant undifferentiated task |

A PRD is not ready for agent execution if two reviewers can reasonably disagree about what should be built.

---

## 6. Common failure modes and fixes

### Failure mode: The request is converted into features too early

**Symptom:** The PRD lists screens/buttons/models but does not explain the user outcome.

**Fix:** Add the raw request, interpreted intent, user/persona, problem statement, and desired outcome before feature requirements.

### Failure mode: Scope creep through adjacent fixes

**Symptom:** The agent “helpfully” redesigns or refactors unrelated areas.

**Fix:** Add explicit non-goals and file/system boundaries.

### Failure mode: Acceptance criteria are not testable

**Symptom:** Requirements use terms like fast, clean, intuitive, robust, seamless.

**Fix:** Replace adjectives with thresholds, observable behavior, screenshots, commands, or examples.

### Failure mode: Agent implements before alignment

**Symptom:** Code appears before the user has confirmed the approach.

**Fix:** Require Phase 0 discovery and a plan-only step before production changes.

### Failure mode: The PRD is too big for useful agent context

**Symptom:** The agent follows early sections but ignores later constraints.

**Fix:** Keep a canonical PRD, then feed only the relevant phase plus global constraints into each execution step.

### Failure mode: AI quality is treated like deterministic QA

**Symptom:** “Output should be correct” with no eval set, thresholds, or fallback behavior.

**Fix:** Define evaluation scenarios, launch thresholds, target quality, confidence thresholds, and human escalation rules.

---

## 7. Useful prompts

### 7.1 Turn a rough request into an alignment read-back

```text
Read the user request below. Do not write a PRD yet.

Return:
1. Raw request summary
2. Interpreted intent
3. User/persona
4. Desired outcome
5. In-scope assumptions
6. Out-of-scope assumptions
7. Top 3 clarification questions that materially affect implementation

Request:
[PASTE REQUEST]
```

### 7.2 Ask an agent to draft a PRD without coding

```text
You are in planning mode. Do not modify files or write code.

Using the aligned understanding below, draft an agent-executable PRD.
The PRD must include:
- Problem statement
- Goals and non-goals
- P0/P1/P2 scope
- User stories with acceptance criteria
- Technical constraints
- Agent prohibitions
- Implementation phases
- Verification commands
- Open questions

Aligned understanding:
[PASTE]
```

### 7.3 Ask an agent to review a PRD

```text
Review this PRD for agent execution readiness.

Score it against:
- User alignment
- Scope clarity
- Requirement testability
- Technical context
- Evaluation plan
- Agent boundaries
- Phase sequencing

Return:
1. Pass/fail summary
2. Top 10 ambiguities
3. Missing constraints
4. Requirements that are not testable
5. Suggested rewrites
6. Whether an implementation agent can safely start Phase 1

PRD:
[PASTE]
```

### 7.4 Ask an implementation agent to execute one phase

```text
Read the PRD and execute only Phase [N].

Rules:
- Do not implement other phases.
- Do not modify files outside the phase scope unless you explain why.
- Run the listed verification commands.
- If a requirement is ambiguous, stop and ask.
- At the end, report files changed, tests run, and remaining risks.

PRD:
[LINK OR PASTE RELEVANT SECTIONS]
```

---

## 8. Autoresearch loop synthesis

This section distills the deeper research pass into a reusable operating loop.

### 8.1 The agent PRD pipeline

Use this pipeline when a request is vague, multi-step, or expensive to execute incorrectly.

```text
Raw ask
  → alignment read-back
  → discovery research
  → problem / outcome framing
  → requirements quality pass
  → agent-executable PRD
  → implementation plan
  → phase-by-phase execution
  → verification and review
```

**Rule:** do not let the implementation agent skip directly from raw ask to code. The first agent loop should produce understanding and a plan, not implementation.

### 8.2 Layered artifact model

Keep durable repo rules separate from feature-specific PRDs.

| Artifact | Purpose | Examples |
| --- | --- | --- |
| Repo instructions | Always-on conventions and constraints | `AGENTS.md`, `CLAUDE.md`, `.github/copilot-instructions.md`, Cursor rules |
| PRD / agent spec | Feature-specific user intent, scope, constraints, acceptance criteria | `docs/prds/export-flow.md` |
| Implementation plan | Exact technical approach and task breakdown | `docs/plans/export-flow-plan.md` |
| Task prompt | One phase or task at a time | “Execute Phase 1 only” |
| Verification evidence | Proof that implementation matches spec | tests, screenshots, logs, diff summary |

### 8.3 Vague ask → clear requirement loop

1. **Capture the ask verbatim** — preserve the user’s words before interpreting.
2. **Extract the problem** — what pain, cost, delay, risk, or missed opportunity is implied?
3. **Identify the user and stakeholder** — who experiences the pain, who decides, who operates/supports it?
4. **Separate problem from proposed solution** — is the requested feature the only way to solve it?
5. **Gather evidence** — past behavior, metrics, support tickets, user quotes, logs, screenshots.
6. **Map assumptions** — user need, feasibility, data availability, compliance, cost, performance, adoption.
7. **Define success** — outcome metric + quality metric + guardrail metric.
8. **Slice MVP scope** — P0 must ship, P1 should ship, P2 explicitly later.
9. **Write acceptance criteria** — testable Given/When/Then or EARS statements.
10. **Read back alignment** — confirm the interpretation before build.

### 8.4 Research frameworks to apply

| Framework | Use when | Output |
| --- | --- | --- |
| Impact Mapping | Stakeholder asks for a feature but business outcome is unclear | Goal → actors → behavior changes → deliverables |
| Opportunity Solution Tree | There are multiple possible solutions or unclear product bet | Outcome → opportunities → solutions → assumption tests |
| User Story Mapping | Workflow is broad or overloaded | Backbone activities → tasks → release slices |
| 5 Whys | Request describes a symptom | Root cause and measurable desired outcome |
| The Mom Test | Need user evidence | Past-behavior evidence, not speculative opinions |
| SMART criteria | Goal or NFR is vague | Specific, measurable, achievable, relevant, time-bound target |

### 8.5 Requirements quality checklist

Use this as a lightweight PRD linter before handing work to an agent.

```markdown
## Requirements Quality Checklist

### Problem and evidence
- [ ] The problem is specific and user-centered.
- [ ] Claims are backed by evidence or marked `[VERIFY]`.
- [ ] Target users/personas are explicit.
- [ ] Business/user goal is measurable.

### Scope
- [ ] In-scope behavior is listed.
- [ ] Out-of-scope behavior is listed.
- [ ] Priority is clear: P0 / P1 / P2.
- [ ] Dependencies and owners are named.
- [ ] Assumptions are explicit.

### Requirement quality
- [ ] Each requirement is singular: one actor, one behavior.
- [ ] Each requirement is testable.
- [ ] Vague terms are replaced with thresholds or examples.
- [ ] Functional and non-functional requirements are separated.
- [ ] Non-functional requirements have measurable thresholds.

### Flows and edge cases
- [ ] Main happy path is step-by-step.
- [ ] Empty, loading, and error states are covered.
- [ ] Permissions/roles are covered.
- [ ] Invalid input and boundary cases are covered.
- [ ] Fallback/rollback behavior is covered.

### Agent execution
- [ ] Implementation phases are bounded.
- [ ] Each phase has exact verification steps.
- [ ] “Do not touch” areas are explicit.
- [ ] The agent must report files changed, tests run, and remaining risks.
```

### 8.6 Requirements smell checklist

Flag and rewrite requirements containing:

| Smell | Examples | Rewrite move |
| --- | --- | --- |
| Ambiguous adjectives | fast, easy, robust, scalable, intuitive | Replace with metric or observable behavior |
| Loopholes | where possible, as needed, if feasible | State exact condition or remove |
| Open-ended verbs | support, handle, improve, optimize | Specify actor, input, output, threshold |
| Vague pronouns | it, this, they | Name the system/entity |
| Baseless comparisons | better, faster, best | Add baseline and target |
| Negative-only requirement | shall not fail silently | Define required fallback behavior |
| Compound requirement | A and B and C | Split into atomic requirements |
| Missing verification | no test, metric, example | Add acceptance criteria |

### 8.7 EARS requirement patterns

Use one behavior per requirement.

```text
Ubiquitous:
The <system> shall <required behavior>.

Event-driven:
When <trigger>, the <system> shall <response>.

State-driven:
While <state>, the <system> shall <behavior>.

Unwanted behavior:
If <error condition>, then the <system> shall <mitigation or fallback>.

Optional feature:
Where <feature/config exists>, the <system> shall <behavior>.

Complex:
While <state>, when <trigger>, the <system> shall <response>.
```

### 8.8 Agent execution gate

Before letting an agent implement, require this gate to pass:

```markdown
## Agent Execution Gate

The agent may start implementation only if:

- [ ] The raw request and interpreted intent are documented.
- [ ] The MVP scope is explicit.
- [ ] Non-goals and do-not-touch areas are explicit.
- [ ] Each P0 requirement has acceptance criteria.
- [ ] Each phase has verification commands or manual checks.
- [ ] Open questions are either resolved or marked non-blocking.
- [ ] The first execution prompt asks for one phase only.
```

---

## 9. Further reading

The expanded reference map and full citation list are in
**[`references/reading-list.md`](references/reading-list.md)** — grouped by topic (agent-executable
specs and coding-agent workflows, user alignment and requirements discovery, requirements quality
and acceptance criteria).

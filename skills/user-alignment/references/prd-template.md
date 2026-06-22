# Agent-executable PRD template

Copy this template into `docs/prds/[feature-name].md`. It is referenced from
[`../SKILL.md`](../SKILL.md) §4.

````markdown
# [Feature / Product Name] PRD

**Status:** Draft | Review | Approved | In Progress | Complete  
**Owner:** [Name]  
**Date:** [YYYY-MM-DD]  
**Source request:** [Link, ticket, Slack thread, email, etc.]  
**Implementing agent:** [Hermes / Claude Code / Codex / Cursor / other]

---

## 1. Raw Request

> [Paste the user’s exact request here]

### 1.1 Session Replay & Telemetry Evidence

- **User Session Replay Link:** `[Link to Produck session replay]`
- **Console Log / Stack Trace:**
  ```
  [Paste raw console output or error traces here]
  ```
- **Relevant Network Payload:**
  ```json
  // [Paste request/response JSON here]
  ```
- **Screenshot / Artifact references:** `[Link to screenshot images]`

---

## 2. Aligned Understanding

### 2.1 Interpreted Intent

[One paragraph explaining what the user wants, in plain language.]

### 2.2 User / Persona

| Persona | Goal | Pain point | Current workaround |
| --- | --- | --- | --- |
| [Persona] | [Goal] | [Pain] | [Current workaround] |

### 2.3 Problem Statement

[What is broken, missing, slow, confusing, or expensive today? Include evidence if available.]

### 2.4 Desired Outcome

[What should be true after this ships?]

---

## 3. Goals, Non-Goals, and Success Metrics

### 3.1 Goals

- G1: [Specific goal]
- G2: [Specific goal]
- G3: [Specific goal]

### 3.2 Non-Goals

- NG1: [Explicitly not building]
- NG2: [Explicitly not building]
- NG3: [Explicitly deferred]

### 3.3 Success Metrics

| Metric | Launch threshold | Target | Measurement method |
| --- | ---: | ---: | --- |
| [Metric] | [Minimum] | [Target] | [How measured] |

---

## 4. Scope

### 4.1 In Scope

| Priority | Requirement | Rationale |
| --- | --- | --- |
| P0 | [Must-have] | [Why] |
| P1 | [Should-have] | [Why] |
| P2 | [Nice-to-have] | [Why] |

### 4.2 Out of Scope

- [Adjacent capability the agent must not build]
- [Deferred feature]
- [Risky area that requires separate approval]

### 4.3 Dependencies

| Dependency | Owner | Status | Notes |
| --- | --- | --- | --- |
| [API/design/data/etc.] | [Owner] | [Ready/TBD/Blocked] | [Notes] |

---

## 5. User Stories & Acceptance Criteria

### Story 1: [Title]

**As a** [persona],  
**I want** [action/capability],  
**so that** [benefit].

**Acceptance criteria:**

- GIVEN [context] WHEN [action] THEN [observable outcome].
- WHEN [trigger] THEN [system] SHALL [specific response].
- IF [condition/error] THEN [system] SHALL [fallback behavior].

### Story 2: [Title]

[Repeat as needed.]

---

## 6. UX / Interaction Requirements

### 6.1 Primary Flow

1. [Step]
2. [Step]
3. [Step]

### 6.2 Empty, Error, and Edge States

| State | Expected behavior | Acceptance test |
| --- | --- | --- |
| Empty state | [Behavior] | [How to verify] |
| Error state | [Behavior] | [How to verify] |
| Permission denied | [Behavior] | [How to verify] |

### 6.3 Copy / Tone Requirements

- [Tone, language, naming, messaging]

---

## 7. Technical Context for the Agent

### 7.1 Stack

- Language/framework: [e.g. Next.js 15, TypeScript]
- Styling: [e.g. Tailwind, design system path]
- Backend/data: [e.g. Supabase, Postgres]
- Test framework: [e.g. pytest, vitest, playwright]

### 7.2 Relevant Existing Files / Patterns

| Area | Path | Notes |
| --- | --- | --- |
| [Area] | `[path]` | [Pattern to follow] |

### 7.3 APIs / Data Contracts

```ts
// Example request/response shape, schema, event, or interface
```

### 7.4 Permissions, Privacy, and Security

- [Data classification]
- [PII/secrets handling]
- [Auth/authorization constraints]
- [Compliance requirements]

### 7.5 Persistent Memory & Preference Sync

The agent must read and respect rules specified in `.agentguard/parcel-memory.json` (if present) before generating code. 
Example structure:
```json
{
  "rules": [
    "Always use arrow functions",
    "No traditional function declarations"
  ],
  "sessionHistory": [
    {
      "timestamp": "2026-06-22T06:00:00Z",
      "summary": "Completed health checks and diff views"
    }
  ]
}
```

---

## 8. Agent Instructions & Prohibitions

### 8.1 Required Behavior

- Follow existing code style and patterns.
- Prefer minimal, focused changes.
- Keep each implementation phase independently testable.
- Update tests and docs with code changes.
- Stop and ask if a requirement conflicts with existing architecture.

### 8.2 Do Not Do

- Do not modify files outside the listed scope unless explicitly required and explained.
- Do not install new dependencies without approval.
- Do not change authentication, billing, deployment, or secrets handling unless this PRD explicitly says so.
- Do not implement P1/P2 items during a P0-only phase.
- Do not silently change public APIs or database schemas.

---

## 9. Implementation Phases

### Phase 0: Discovery / Repo Inspection

**Dependencies:** None  
**Scope:** Understand current codebase and confirm implementation path.  
**Out of scope:** Writing production code.

**Tasks:**
1. Inspect relevant files and existing patterns.
2. Identify test commands and local verification steps.
3. Produce implementation plan for Phase 1.

**Testable output:**
- Agent produces a short plan listing exact files to modify and tests to run.

---

### Phase 1: [Name]

**Dependencies:** Phase 0  
**Scope:** [Bounded scope]  
**Out of scope:** [Explicit exclusions]

**Tasks:**
1. [Task]
2. [Task]
3. [Task]

**Verification:**
- Run: `[command]`
- Expected: `[expected result]`
- Manual check: `[screenshot / UI / API behavior]`

---

### Phase 2: [Name]

[Repeat phase structure.]

---

## 10. Testing & Evaluation Plan

### 10.1 Automated Tests

| Test type | Command | Required result |
| --- | --- | --- |
| Unit | `[command]` | [Expected] |
| Integration | `[command]` | [Expected] |
| E2E | `[command]` | [Expected] |

### 10.2 Manual QA

- [ ] [Manual check]
- [ ] [Manual check]
- [ ] [Manual check]

### 10.3 AI / Agent Quality Evaluation, if applicable

| Scenario | Expected behavior | Failure threshold | Escalation/fallback |
| --- | --- | --- | --- |
| [Scenario] | [Expected] | [Threshold] | [Fallback] |

---

## 11. Rollout, Monitoring, and Fallback

### 11.1 Rollout Plan

- [Alpha/internal]
- [Beta/limited users]
- [Full launch]

### 11.2 Monitoring

- [Metric/event/log/dashboard]

### 11.3 Fallback / Rollback

- [How to disable, revert, or degrade gracefully]

---

## 12. Risks and Open Questions

### 12.1 Risks

| Risk | Likelihood | Impact | Mitigation |
| --- | --- | --- | --- |
| [Risk] | [Low/Med/High] | [Low/Med/High] | [Mitigation] |

### 12.2 Open Questions

| Question | Owner | Needed by | Decision |
| --- | --- | --- | --- |
| [Question] | [Owner] | [Date/phase] | [TBD] |

---

## 13. PRD Readiness Checklist

- [ ] Raw request is preserved.
- [ ] Interpreted intent has been read back to the user/stakeholder.
- [ ] Goals and non-goals are explicit.
- [ ] P0/P1/P2 priorities are clear.
- [ ] Each user story has testable acceptance criteria.
- [ ] Edge cases and failure modes are documented.
- [ ] Technical constraints and existing code patterns are listed.
- [ ] Agent prohibitions are explicit.
- [ ] Implementation is broken into bounded phases.
- [ ] Each phase has a testable output.
- [ ] Test commands and expected results are listed.
- [ ] Open questions have owners.
````

# AGENTS.md - Codex Operating Instructions

This project uses Codex repo-scoped skills and custom agents.

- Repo skills live in `.agents/skills/`.
- Shared checklists live in `.agents/references/`.
- Codex custom agents live in `.codex/agents/`.

Follow this file exactly. It is the durable project contract that Codex reads before work starts.

## 0. Project Context And Source-Of-Truth Files

Read on demand. Do not load all three eagerly.

| File | Holds | Read when |
|------|-------|-----------|
| [SPEC.md](SPEC.md) | what and why: features, stack, conventions, acceptance criteria | starting a feature; making a design choice; checking acceptance criteria |
| [tasks/plan.md](tasks/plan.md) | how and order: architecture decisions, task entries, dependencies, verification steps | beginning a task; checking dependency order |
| [tasks/todo.md](tasks/todo.md) | live progress checklist | session start; after completing a task |

Session start:
1. Read `tasks/todo.md`.
2. Find the next unchecked task.
3. Glance at the status line in `SPEC.md` if present.
4. Stop and wait for the user's explicit request.

Working a task:
1. Read `tasks/todo.md` and identify the task requested by the user, or the first unchecked task if the user asks for the next task.
2. Read only that task's entry in `tasks/plan.md` plus cited `SPEC.md` sections.
3. Use the relevant skill from `.agents/skills`.
4. Implement incrementally with tests.
5. Run the verification steps and show evidence.
6. Stop at the phase checkpoint. Do not roll into the next task.
7. Tick `tasks/todo.md`; update `SPEC.md` or `tasks/plan.md` if scope changed.

## 1. Skill-First Rule

Before writing code or a plan, identify which Codex skill applies. Use explicit `$skill-name` invocation when the user asks for a workflow by name, and allow implicit skill matching when the task description clearly fits a skill.

Intent map:

```text
Task arrives
    |
    |-- Requirements are vague              -> $interview-me
    |-- Rough concept needs variants         -> $idea-refine
    |-- New feature or greenfield project    -> $vibe-spec
    |-- Existing SPEC.md needs tasks         -> $vibe-plan
    |-- Implementing code                    -> $vibe-build
    |   |-- UI work                          -> $frontend-ui-engineering
    |   |-- API/interface work               -> $api-and-interface-design
    |   |-- High-stakes decision             -> $doubt-driven-development
    |   |-- Needs official docs              -> $source-driven-development
    |   |-- Security-sensitive surface        -> $security-and-hardening
    |-- Writing or running tests             -> $vibe-test
    |-- Something broke                      -> $debugging-and-error-recovery
    |-- Reviewing code                       -> $vibe-review
    |-- Simplifying recent changes           -> $vibe-simplify
    |-- Committing or branching              -> $git-workflow-and-versioning
    |-- Deploying or launch readiness         -> $vibe-ship
```

## 2. Codex Entry Points

Codex custom prompts are deprecated; this template uses repo-scoped skills as workflow entry points.

| User intent | Codex skill |
|-------------|-------------|
| Make a spec | `$vibe-spec` |
| Break spec into tasks | `$vibe-plan` |
| Build a task | `$vibe-build` |
| Run TDD workflow | `$vibe-test` |
| Review changes | `$vibe-review` |
| Simplify code | `$vibe-simplify` |
| Pre-launch go/no-go | `$vibe-ship` |
| Browser E2E execution | `$vibe-e2e` |

If the user types old slash-command language such as `/spec`, `/plan`, or `/build`, treat it as the matching `$vibe-*` skill unless they explicitly mean a different tool.

## 3. Typical Feature Lifecycle

For non-trivial features, follow this sequence unless the user narrows the request:

```text
1. interview-me              - clarify vague requirements
2. idea-refine               - compare possible approaches
3. vibe-spec                 - define what to build and acceptance criteria
4. vibe-plan                 - break into small, verifiable tasks
5. context-engineering       - load only the right context
6. source-driven-development - verify framework/API facts against current docs
7. vibe-build                - implement one thin slice
8. doubt-driven-development  - challenge non-trivial decisions early
9. vibe-test                 - write/execute focused tests
10. vibe-review              - review before merge
11. vibe-simplify            - reduce complexity without behavior changes
12. git-workflow-and-versioning - prepare atomic commits
13. documentation-and-adrs   - document important decisions
14. vibe-ship                - launch checklist and rollback plan
```

A bug fix can use a shorter path:

```text
debugging-and-error-recovery -> vibe-test -> vibe-review
```

## 4. Non-Negotiable Behaviors

1. Surface assumptions before non-trivial implementation.
2. Stop on real contradictions; name the tradeoff and wait for resolution.
3. Push back when a requested path is risky; propose a safer alternative.
4. Prefer boring, obvious code over clever abstractions.
5. Touch only what the task requires.
6. Verify with real evidence: test output, build output, runtime checks, or review findings.
7. Never claim completion without verification.
8. Keep `tasks/todo.md` and `tasks/plan.md` current after task work.
9. Ask before committing unless the user explicitly requested a commit.

## 5. Codex Custom Agents

Codex only spawns subagents when the user explicitly asks for subagents or parallel agent work. When requested, use these project agents:

| Agent | File | When to use |
|-------|------|-------------|
| `code-reviewer` | `.codex/agents/code-reviewer.toml` | Pre-merge review across correctness, readability, architecture, security, performance |
| `security-auditor` | `.codex/agents/security-auditor.toml` | Security-focused review, threat modeling, OWASP hardening |
| `test-engineer` | `.codex/agents/test-engineer.toml` | Test strategy, missing coverage, Prove-It regression tests |

For launch readiness, ask Codex to spawn one agent per perspective and wait for all results:

```text
Use $vibe-ship. Spawn code-reviewer, security-auditor, and test-engineer in parallel, wait for all three, then synthesize a go/no-go decision.
```

Subagents should not spawn other subagents. The main session owns orchestration and synthesis.

## 6. Failure Modes To Avoid

1. Skipping skill selection and diving straight into code.
2. Filling ambiguous requirements silently.
3. Agreeing to unsafe or overcomplicated approaches without caveats.
4. Broad refactors unrelated to the task.
5. Reading the entire repository when the task needs a narrow slice.
6. Running build/test commands repeatedly without changing anything or learning from the failure.
7. Leaving stale status in `tasks/todo.md`, `tasks/plan.md`, or `SPEC.md`.

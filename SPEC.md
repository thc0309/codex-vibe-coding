# SPEC.md - Codex Vibe Coding Template

Status: active template

## Objective

Provide a token-efficient Codex project template with durable instructions, repo-scoped skills, focused references, task tracking, and verification scaffolds for frontend, backend, review, and E2E work.

## Source Of Truth

- `AGENTS.md`: short runtime rules loaded often.
- `.agents/skills/*/SKILL.md`: workflow entry points.
- `.agents/references/*.md`: long checklists read on demand.
- `tasks/plan.md`: task details and verification.
- `tasks/todo.md`: live checklist.
- `tasks/test-plan.md`: E2E/browser cases.
- `tasks/test-result.md`: E2E failures and blockers.

## Token Requirements

- Keep always-loaded instructions short and specific.
- Split work into `spec -> plan -> build -> review -> ship/e2e`.
- Prefer deterministic shell/CLI data gathering when LLM reasoning is unnecessary.
- Prefer `gh` CLI or predownloaded GitHub artifacts over repeated MCP calls for routine GitHub data.
- Load frontend, backend, testing, security, performance, and accessibility references only when the task needs them.

## Frontend Requirements

- Choose Vite/React/Tailwind, Next.js, or another frontend stack only when `SPEC.md` records the reason.
- Use `.agents/references/frontend-profile.md` for UI task acceptance.
- Do not create a component library or design tokens until 2-3 real use cases justify them.
- Browser/E2E coverage for critical UI flows belongs in `tasks/test-plan.md`.

## Backend Requirements

- Choose backend runtime, framework, database, queue, and deployment target only when the spec records the reason.
- Use `.agents/references/backend-profile.md` for API/server task acceptance.
- Backend work must define contracts, validation boundaries, authz, error shape, persistence behavior, observability, and relevant performance limits.

## Verification

Documentation-only changes are verified by Markdown/file inspection and git diff. Runtime app changes must add appropriate tests and command evidence.

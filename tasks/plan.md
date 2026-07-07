# Task Plan

Status: T01 complete

## T01 - Apply Token, Frontend, Backend, And E2E Template Optimizations

### Goal

Apply the requested token-efficiency, frontend, backend, and E2E scaffold improvements to this Codex template.

### Acceptance Criteria

- `AGENTS.md` is shortened to always-needed runtime rules.
- Long guidance is moved or linked through `.agents/references/`.
- Token budget mode covers concise output, narrow file reads, phase splitting, MCP/tool overhead, and GitHub CLI preference.
- Frontend profile template covers stack selection, acceptance checklist, no premature component library, and thin UI slices.
- Backend profile template covers stack selection, API/server acceptance checklist, contracts, validation, security, performance, and thin backend slices.
- `tasks/test-plan.md` exists for `vibe-e2e`.
- `tasks/test-result.md` exists for non-pass E2E results.
- `SPEC.md`, `tasks/plan.md`, and `tasks/todo.md` are no longer empty.

### Files Touched

- `AGENTS.md`
- `README.md`
- `SPEC.md`
- `.agents/references/token-optimization.md`
- `.agents/references/frontend-profile.md`
- `.agents/references/backend-profile.md`
- `.agents/skills/frontend-ui-engineering/SKILL.md`
- `.agents/skills/api-and-interface-design/SKILL.md`
- `.agents/skills/vibe-plan/SKILL.md`
- `tasks/plan.md`
- `tasks/todo.md`
- `tasks/test-plan.md`
- `tasks/test-result.md`

### Verification

- Inspect Markdown content for the requested rules and scaffolds.
- Confirm expected files exist and are non-empty.
- Review git diff for scope.

### Verification Evidence

- `rtk proxy git diff --check`: pass, no whitespace errors.
- `rtk proxy find AGENTS.md SPEC.md tasks .agents/references -type f -size 0 -print`: pass, no empty source/checklist files.
- `index_repository(mode="fast")`: graph updated to 802 nodes and 784 edges.
- Graph search found `Frontend Profile Template`, `Backend Profile Template`, and `Test Plan`.

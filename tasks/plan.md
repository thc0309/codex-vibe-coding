# Task Plan

Status: T02 complete

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

## T02 - Integrate Skill Intake Into `$vibe-plan`

### Goal

Make `$vibe-plan` automatically map project scope to existing repo skills and record missing skill gaps during planning.

### Acceptance Criteria

- `.agents/skills/vibe-plan/SKILL.md` instructs Codex to inspect stack/work domains and map tasks to skills.
- `$vibe-plan` records a `Skill Intake Summary` in `tasks/plan.md`.
- `$vibe-plan` recommends missing skill installation/creation without doing it automatically.
- README explains the new skill intake behavior.

### Verification

- Inspect `vibe-plan` skill instructions.
- Inspect README usage docs.
- Run `git diff --check`.

### Verification Evidence

- `rtk proxy git diff --check`: pass, no whitespace errors.
- Inspected `.agents/skills/vibe-plan/SKILL.md`: skill intake is plan-only and records recommended/missing skills.
- Inspected `README.md`: documents `Skill Intake Trong $vibe-plan`.

## T03 - Add Caveman And Ponytail Installation Guidance

### Goal

Document how to install and activate Caveman and Ponytail for users of this template.

### Acceptance Criteria

- README includes Caveman install commands and activation/deactivation examples.
- README includes Ponytail Codex marketplace install flow, hooks review step, and mode commands.
- README notes relevant safety step: review install script or lifecycle hooks before trusting.

### Verification

- Inspect README installation section.
- Run `git diff --check`.

### Verification Evidence

- `rtk proxy git diff --check`: pass, no whitespace errors.
- Inspected README install section: Caveman and Ponytail commands, activation, deactivation, and hooks/script review notes are present.
- `index_repository(mode="fast")`: graph updated to 811 nodes and 793 edges.

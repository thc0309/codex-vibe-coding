---
name: vibe-build
description: Implement one planned task incrementally with tests, verification, task status updates, and no automatic commit.
---

Use `incremental-implementation` and `test-driven-development`.

For each task:

1. Identify the requested task or the next unchecked task in `tasks/todo.md`.
2. Read that task's acceptance criteria in `tasks/plan.md` plus relevant `SPEC.md` sections.
3. Load only the needed source context.
4. If the task touches user input, authentication, authorization, data storage, secrets, permissions, or external integrations, also use `security-and-hardening`.
5. Write a failing test for the expected behavior when practical.
6. Implement the minimum code to pass.
7. Run focused tests, then broader regression checks appropriate to the change.
8. Run the build or typecheck when the project provides one.
9. Mark the task complete in `tasks/todo.md` and update `tasks/plan.md` if scope changed.
10. Report changes and verification evidence.
11. Suggest a commit message, but do not commit unless the user explicitly asks.

If a step fails, use `debugging-and-error-recovery` and report the blocker with evidence.

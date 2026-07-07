# codex-vibe-code

Template chuẩn cho dự án AI Vibe Code chạy trên Codex. Bản này chuyển template Claude gốc sang các convention hiện tại của Codex: `AGENTS.md` cho project instructions, `.agents/skills` cho repo-scoped skills, và `.codex/agents` cho custom subagents.

## Vì sao dùng template này?

- Tối ưu token bằng RTK cho shell output và skill `caveman` khi cần giao tiếp siêu ngắn.
- Ép quy trình có cấu trúc: `interview -> refine -> spec -> plan -> build -> test -> review -> simplify -> ship`.
- Dùng Codex skills thay cho prompt dài lặp lại mỗi lần.
- Giữ source of truth rõ ràng qua `SPEC.md`, `tasks/plan.md`, và `tasks/todo.md`.
- Có custom agents cho review, security audit, và test strategy khi cần chạy subagents song song.
- Bắt buộc verify bằng evidence thật trước khi coi một task là xong.

## Cấu trúc

```text
.
├── AGENTS.md                     # Durable project instructions loaded by Codex
├── SPEC.md                       # What and why: features, stack, acceptance criteria
├── tasks/
│   ├── plan.md                   # How and order: task plan, AC, verification
│   ├── todo.md                   # Live progress checklist
│   ├── test-plan.md              # Browser/E2E suites for vibe-e2e
│   └── test-result.md            # Non-pass E2E evidence
├── .agents/
│   ├── skills/                   # Repo-scoped Codex skills
│   └── references/               # Reusable checklists
└── .codex/
    └── agents/                   # Project custom subagents
```

## Yêu cầu cài đặt

### Codex

Cài Codex CLI hoặc dùng Codex app/IDE extension theo tài liệu OpenAI. Sau khi mở repo này trong Codex, project instructions sẽ được đọc từ `AGENTS.md`.

Kiểm tra nhanh:

```bash
codex --version
codex --ask-for-approval never "Summarize the current project instructions."
```

### RTK

[RTK](https://github.com/rtk-ai/rtk) là CLI proxy giúp nén output command.

```bash
curl -fsSL https://rtk.ai/install.sh | sh
rtk --version
rtk gain
```

## Bắt đầu dự án mới

Clone hoặc copy template:

```bash
git clone https://github.com/netcoreltd/codex-vibe-code.git my-project
cd my-project
rm -f README.md
rm -rf .git
```

Mở Codex tại root của dự án:

```bash
codex
```

Khi copy template cho project mới, viết lại `SPEC.md`, `tasks/plan.md`, `tasks/todo.md`, và `tasks/test-plan.md` theo sản phẩm thật. Các file này là scaffold vận hành, không phải spec cố định cho mọi project.

## Token-efficient mode

- `AGENTS.md` chỉ giữ luật luôn cần: mục đích repo, token mode, thứ tự đọc file, skill routing tối thiểu.
- Hướng dẫn dài nằm trong `.agents/references/` và chỉ đọc khi task cần.
- Tách phase rõ: `spec -> plan -> build -> review -> ship/e2e`. Nên mở session mới hoặc compact context giữa các phase lớn.
- Ưu tiên đọc lát cắt file nhỏ, không scan toàn repo nếu chưa cần.
- Giảm MCP/tool overhead: chỉ bật/gọi tool cần thiết; với GitHub data thường dùng `gh` CLI hoặc predownload diff/log vào file thay vì gọi MCP nhiều vòng.

## Workflow chính

### 1. Làm rõ yêu cầu với `$vibe-spec`

Nếu bắt đầu từ ý tưởng mới:

```text
$vibe-spec

Tôi muốn làm app quản lý công việc cá nhân với Next.js và Supabase.
```

Nếu onboard vào codebase có sẵn:

```text
$vibe-spec

Dự án này đã có source code. Hãy đọc codebase, hiểu kiến trúc, tính năng hiện tại, stack công nghệ, rồi tạo SPEC.md phản ánh đúng trạng thái hiện tại. Sau đó hỏi tôi về tính năng mới cần bổ sung.
```

### 2. Lập kế hoạch với `$vibe-plan`

```text
$vibe-plan

Trước khi lập kế hoạch, hãy tra cứu phiên bản mới nhất của runtime, framework, dependency chính và Docker image liên quan. Ghi phiên bản đã chọn vào tasks/plan.md kèm lý do.
```

Codex sẽ tạo hoặc cập nhật:

- `tasks/plan.md`: task chi tiết, acceptance criteria, verification steps, file dự kiến chạm.
- `tasks/todo.md`: checklist tiến độ ngắn gọn.
- `tasks/test-plan.md`: test/E2E cases nếu task có UI hoặc browser flow.

### 3. Implement từng task với `$vibe-build`

```text
$vibe-build T01
```

Codex chỉ làm task được yêu cầu, chạy verification, cập nhật checklist, rồi dừng ở checkpoint. Nếu code chạm input, auth, data storage, permissions, secrets, hoặc external integrations, Codex phải áp dụng thêm `$security-and-hardening`.

### 4. Test với `$vibe-test`

```text
$vibe-test
```

Với bug fix, dùng Prove-It pattern: viết test fail trước, xác nhận fail, implement fix, xác nhận pass, chạy regression suite.

### 5. Review với `$vibe-review`

```text
$vibe-review
```

Review theo 5 chiều: correctness, readability, architecture, security, performance. Findings phải có file/line và fix recommendation.

### 6. Ship với `$vibe-ship`

```text
$vibe-ship
```

Với thay đổi production-bound, yêu cầu Codex spawn `code-reviewer`, `security-auditor`, và `test-engineer` song song, chờ đủ kết quả, rồi synthesize thành GO/NO-GO kèm rollback plan.

## Skills nên gọi thêm theo loại dự án

| Loại dự án | Skill nên kèm theo |
|------------|--------------------|
| Backend API, REST/GraphQL, module boundaries | `$api-and-interface-design` |
| Frontend, UI/UX, component | `$frontend-ui-engineering` |
| Auth, input, storage, external integrations | `$security-and-hardening` |
| CI/CD, pipeline, automation | `$ci-cd-and-automation` |
| Hiệu năng cao | `$performance-optimization` |
| Cần thông tin framework/library mới nhất | `$source-driven-development` |

## Frontend/backend profiles

- Frontend work đọc `.agents/references/frontend-profile.md`: stack chỉ chọn khi `SPEC.md` yêu cầu, checklist responsive/accessibility/states/performance, và slice mẫu `route shell -> static UI -> state/data -> validation -> tests -> e2e`.
- Backend/API work đọc `.agents/references/backend-profile.md`: stack chỉ chọn khi `SPEC.md` ghi lý do, checklist contract/validation/authz/errors/pagination/observability/performance, và slice mẫu `contract -> route/service -> persistence -> hardening -> tests -> docs`.
- Không tạo component library, design tokens, generic service layer, hoặc shared abstraction sớm. Chỉ tách khi có 2-3 use case thật.

## Codex notes

- `AGENTS.md` là file instructions chính. Codex đọc file này khi bắt đầu session.
- Repo skills được Codex scan từ `.agents/skills` từ working directory lên repo root.
- Custom agents được định nghĩa bằng TOML trong `.codex/agents`.
- Codex chỉ spawn subagents khi bạn yêu cầu rõ ràng.
- Custom slash prompts cũ của Codex vẫn tồn tại nhưng đã deprecated; template này dùng skills để chia sẻ workflow trong repo.

## Credits

Template gốc được tạo trong quá trình Vibe Code tại NETCORE LTD bởi Dung Vo. Bộ skills kế thừa ý tưởng từ [agent-skills](https://github.com/addyosmani/agent-skills) của Addy Osmani và [caveman](https://github.com/JuliusBrussee/caveman) của Julius Brussee, sau đó được chỉnh lại cho Codex.

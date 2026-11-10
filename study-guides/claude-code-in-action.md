# Đề cương ôn tập: Claude Code in Action

> Tài liệu tự ôn trước khi kiểm tra kiến thức; không phải đề thi chính thức.  
> Ghi chú đầy đủ: [Claude Code in Action](../claude-code-in-action-notes.md)

## Mục tiêu cần đạt

- Điều khiển phiên dài bằng scope, Plan mode, compaction, rewind, goal, loop và worktrees.
- Đặt đúng quy tắc vào CLAUDE.md, Skill, permission mode hoặc Hook.
- Phân biệt cloud scheduled tasks, non-interactive mode và GitHub automation.
- Kiểm chứng kết quả không giám sát và cài Plugin an toàn.

## Điều khiển phiên dài

| Công cụ | Ghi nhớ |
| --- | --- |
| Plan mode | Khảo sát và lập kế hoạch khi chưa sửa source files |
| /compact | Tóm tắt; thêm chỉ dẫn để giữ chi tiết cần thiết |
| Rewind | Quay về checkpoint; có thể khôi phục code, conversation hoặc cả hai |
| /goal | Theo đuổi điều kiện hoàn thành kiểm tra được từ transcript |
| Loop | Polling theo chu kỳ; fixed loop trong lesson tối đa 7 ngày |
| Git worktrees | Cây làm việc độc lập cho phiên song song |
| .worktreeinclude | Liệt kê file ignored cần copy vào worktree |

## Chọn đúng instruction surface

| Thành phần | Vai trò |
| --- | --- |
| CLAUDE.md | Hướng dẫn nền gọn; không cưỡng chế |
| Skill | Quy trình lặp lại; description giúp kích hoạt theo task |
| Permission mode | Mức tự chủ và hành động cần xác nhận |
| Hook | Logic tại event; có thể gate hoặc chặn hành động |

Imports trong CLAUDE.md giúp tổ chức nội dung, nhưng được mở inline và không tự giảm context. Quy tắc bắt buộc cần enforcement bằng Hook.

## Permission modes

Nhớ sáu mode theo lesson:

- **Manual:** đọc không hỏi; hành động khác hỏi khi cần.
- **Accept edits:** đọc/sửa file và một số lệnh file-system phổ biến không hỏi.
- **Plan:** khảo sát và đề xuất thay đổi, không sửa file.
- **Auto:** classifier rà phần lớn hành động để phát hiện rủi ro; không kiểm tra code đúng hay không.
- **Don't ask:** hành động cần hỏi bị tự từ chối, phù hợp pipeline không có người trả lời.
- **Bypass permissions:** bỏ kiểm tra quyền; chỉ dùng trong container/VM cô lập.

## Hooks: điều dễ sai

- PreToolUse chạy trước tool và có thể allow/deny/ask.
- PostToolUse chạy sau tool; không ngăn hành động đã hoàn thành.
- Stop có thể giữ lượt mở nếu test chưa đạt.
- Exit code 2 chặn; exit code 1 không chặn.
- Muốn bổ sung context sau compaction, lesson dùng SessionStart với matcher compact.
- updatedInput thay toàn bộ input object; giữ trường không đổi nếu chỉnh một phần.

## Tự động hóa

| Cách | Nơi chạy | Phù hợp |
| --- | --- | --- |
| Cloud scheduled task | Hạ tầng Anthropic | Cron, HTTP API hoặc GitHub event |
| Non-interactive mode (-p) | CLI/script/CI của bạn | Pipeline cần input/output và exit code |
| Agent SDK | Ứng dụng TypeScript/Python | Nhúng agent vào sản phẩm |
| GitHub Code Review | Dịch vụ hosted | PR findings; không approve/block hay push fix |
| GitHub Action | GitHub Actions | Workflow tùy biến theo mention/event/lịch |

Theo lesson, cloud scheduled tasks đang ở research preview; lịch lặp tối đa mỗi giờ. Mặc định dùng clone mới của default branch và push nhánh claude/, có ngoại lệ cho PR event. Non-interactive mode hỗ trợ JSON schema, structured_output và --resume.

## Kiểm chứng phiên chạy không giám sát

1. Dùng Auto mode thay Bypass permissions.
2. Chạy /code-review rồi tự đọc git diff.
3. Đối chiếu file với kế hoạch và acceptance criteria.
4. Dùng Stop Hook gate test; PostToolUse có thể chạy lint/type check.
5. Kiểm tra JSON result và exit code của CLI.
6. Xin review độc lập từ session/subagent mới cho thay đổi quan trọng.

Classifier kiểm tra rủi ro hành động, không xác minh correctness. Mức kiểm chứng cần tăng khi mức giám sát giảm.

## Plugins

- Plugin có thể đóng gói Skills, subagents, Hooks, MCP config và thành phần khác.
- Plugin chạy code với quyền người cài; xem nội dung trước khi bật.
- Hooks của Plugin và Hooks cá nhân cùng chạy.
- Thành phần có thể được namespace; Plugin không ghi đè cấu hình cá nhân.
- Manifest .claude-plugin/plugin.json là tùy chọn theo lesson.
- Cài và chia sẻ qua marketplace tin cậy; kiểm tra quyền và thành phần đi kèm.

## Điểm dễ nhầm

- CLAUDE.md hướng dẫn; Hook có thể enforce.
- Auto không phải test runner; classifier không đánh giá code đúng/sai.
- Code Review hosted đăng findings; GitHub Action phục vụ workflow tùy biến.
- Scheduled task khác non-interactive CLI về hạ tầng và điểm tích hợp.
- Summary không thay thế diff, test log hoặc bằng chứng.

## Tự kiểm tra

1. **Khi nào dùng /goal?** Khi điều kiện hoàn thành cụ thể và kiểm tra được từ transcript.
2. **Vì sao hướng dẫn không đủ cho quy tắc cứng?** Hook thực thi tại event; instruction có thể bị bỏ sót.
3. **Auto và Bypass khác gì?** Auto có classifier rà hành động; Bypass bỏ qua kiểm tra quyền.
4. **Code Review hosted có tự merge PR không?** Không; nó đăng findings, quyết định thuộc về người.
5. **Xác minh unattended run thế nào?** Đọc diff, gate test, kiểm tra exit code, xin review độc lập.
6. **Điều gì cần nhớ về Plugin?** Nó chạy code với quyền người cài; đọc Hooks, agents và MCP config trước khi cài.

## Checklist trước khi thi

- [ ] Chọn đúng công cụ cho session, configuration, automation và verification.
- [ ] Nhớ sáu permission modes và exit code Hook.
- [ ] Phân biệt hosted Code Review với GitHub Action.
- [ ] Dùng bằng chứng để kiểm chứng unattended run.

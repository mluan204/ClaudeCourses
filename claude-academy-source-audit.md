# Kiểm chứng nguồn Claude Academy — 8 tài liệu ôn tập

Ngày kiểm chứng: 2026-11-10. Phạm vi: các file trong `study-guides/`; chỉ ghi nhận điểm nội dung có thể sai hoặc lỗi thời. Đã kiểm tra lại sau các cập nhật trong guide.

## Kết quả cần ghi nhận

| Tài liệu | Kết quả sau cập nhật |
|---|---|
| `study-guides/introduction-to-agent-skills.md` | Đã sửa chính xác: `SKILL.md` chứa hướng dẫn; YAML frontmatter là tùy chọn, mọi field đều tùy chọn và `description` được khuyến nghị. Các câu mục tiêu/ôn tập cũng không còn xem frontmatter là bắt buộc. Khớp [Claude Code Skills docs](https://code.claude.com/docs/en/skills). |
| `study-guides/claude-code-in-action.md` | Sáu permission mode và mô tả Auto, `dontAsk`, `bypassPermissions` khớp [Permission docs](https://code.claude.com/docs/en/permissions). Hook §6 nay nói rõ `PostToolUse` chạy sau khi tool hoàn tất thành công, còn lỗi có event `PostToolUseFailure`; phân biệt `PreCompact`, `SessionStart` sau compact và `PostCompact` cũng khớp [Hooks docs](https://code.claude.com/docs/en/hooks). |
| `study-guides/building-with-the-claude-api.md` | Đã nêu rõ Anthropic không cung cấp embedding model riêng, đúng với [Embeddings docs](https://platform.claude.com/docs/en/build-with-claude/embeddings). Structured outputs được mô tả ở mức khái niệm, kèm kiểm tra schema/business rules; không có tham số API lỗi thời. Nếu thêm ví dụ gọi API, dùng `output_config.format` với `type: "json_schema"` theo [Structured outputs docs](https://platform.claude.com/docs/en/build-with-claude/structured-outputs). Thẻ `<output_format>` trong ví dụ hiện tại là một phần của prompt XML, không phải tham số API. |
| `study-guides/ai-capabilities-and-limitations.md` | Không thấy mâu thuẫn nội dung đáng kể với [course hiện hành](https://academy.claude.com/courses/ai-capabilities-and-limitations). |
| `study-guides/ai-fluency-framework-foundations.md` | Không thấy mâu thuẫn nội dung đáng kể với [course hiện hành](https://academy.claude.com/courses/ai-fluency-framework-foundations). |
| `study-guides/claude-101.md` | Không thấy mâu thuẫn đáng kể; tính năng được diễn đạt có điều kiện theo gói/cấu hình. Khớp chủ đề [course hiện hành](https://academy.claude.com/courses/claude-101). |
| `study-guides/claude-code-101.md` | Không thấy claim lỗi thời đáng kể trong nội dung đã kiểm tra; guide tránh đóng đinh các tên mode/command có thể thay đổi. Tham chiếu [course](https://academy.claude.com/courses/claude-code-101) và [current docs](https://code.claude.com/docs/en/overview). |
| `study-guides/introduction-to-subagents.md` | Ví dụ và mô tả subagent không có mismatch nội dung đáng kể với [subagents docs](https://code.claude.com/docs/en/sub-agents); guide không dựa vào wizard `/agents` cũ. |

## Nguồn chính thức

- Claude Academy: [AI Capabilities and Limitations](https://academy.claude.com/courses/ai-capabilities-and-limitations), [AI Fluency Framework Foundations](https://academy.claude.com/courses/ai-fluency-framework-foundations), [Claude 101](https://academy.claude.com/courses/claude-101), [Building with the Claude API](https://academy.claude.com/courses/building-with-the-claude-api), [Claude Code 101](https://academy.claude.com/courses/claude-code-101), [Claude Code in Action](https://academy.claude.com/courses/claude-code-in-action), [Introduction to Agent Skills](https://academy.claude.com/courses/introduction-to-agent-skills), [Introduction to Subagents](https://academy.claude.com/courses/introduction-to-subagents).
- Claude Code docs: [Skills](https://code.claude.com/docs/en/skills), [Permissions](https://code.claude.com/docs/en/permissions), [Hooks](https://code.claude.com/docs/en/hooks), [Subagents](https://code.claude.com/docs/en/sub-agents), [Overview](https://code.claude.com/docs/en/overview).
- Claude Platform docs: [Embeddings](https://platform.claude.com/docs/en/build-with-claude/embeddings), [Structured outputs](https://platform.claude.com/docs/en/build-with-claude/structured-outputs).

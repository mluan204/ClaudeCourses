# Claude Academy Study Notes (Tiếng Việt)

Bộ tài liệu tự học và hệ thống hóa kiến thức toàn diện về **Hệ sinh thái Anthropic & Claude**, được biên soạn và chuẩn hóa bằng tiếng Việt dựa trên các khóa học chính thức của [Claude Academy](https://academy.claude.com/).

Tất cả các tài liệu được thiết kế theo cấu trúc module đồng nhất, bảo toàn 100% nội dung gốc kết hợp bổ sung phân tích thực chiến, câu hỏi ôn tập, checklist đánh giá và liên kết mục lục tự động.

---

## Danh mục tài liệu học tập

| STT | Tài liệu | Khóa học gốc (Claude Academy) | Phạm vi & Trọng tâm |
| :---: | :--- | :--- | :--- |
| **1** | [ai-fluency-framework-foundations-notes.md](./ai-fluency-framework-foundations-notes.md) | AI Fluency: Framework and foundations | Khung năng lực 4D (Delegation, Description, Discernment, Diligence) & 6 kỹ thuật prompt cốt lõi. |
| **2** | [ai-capabilities-and-limitations-notes.md](./ai-capabilities-and-limitations-notes.md) | AI Capabilities and Limitations | Bản chất LLM: Next Token Prediction, Knowledge, Working Memory, Steerability & Ma trận chẩn đoán lỗi. |
| **3** | [claude-101-notes.md](./claude-101-notes.md) | Claude 101 | Làm chủ Claude Desktop/Web: Chat, Cowork, Code, Projects, Artifacts, Skills, Connectors, Enterprise Search. |
| **4** | [claude-code-101-notes.md](./claude-code-101-notes.md) | Claude Code 101 | Tác tử lập trình dòng lệnh (CLI Coding Agent), agentic loop, quy trình Explore → Plan → Code → Commit, CLAUDE.md. |
| **5** | [introduction-to-agent-skills-notes.md](./introduction-to-agent-skills-notes.md) | Introduction to agent skills | Xây dựng Agent Skills, cấu hình `SKILL.md`, progressive disclosure, công cụ tiền phê duyệt `allowed-tools`. |
| **6** | [introduction-to-subagents-notes.md](./introduction-to-subagents-notes.md) | Introduction to subagents | Kiến trúc đa tác tử phân cấp, cô lập ngữ cảnh (context isolation), 4 design patterns và least privilege. |
| **7** | [building-with-the-claude-api-notes.md](./building-with-the-claude-api-notes.md) | Building with the Claude API | Xây dựng ứng dụng Claude API production: Prompt evaluation, Tool use, RAG, MCP, Vision, Agents & Workflows. |
| **8** | [claude-code-in-action-notes.md](./claude-code-in-action-notes.md) | Claude Code in Action | Phiên dài, permission modes, Hooks, automation, verification và Plugins. |

---

## Tài liệu ôn thi

Thư mục `study-guides/` có tài liệu tự ôn cho từng khóa học: giải thích kiến thức trọng tâm, ví dụ và tình huống ứng dụng, câu hỏi kèm đáp án giải thích, điểm dễ nhầm và checklist tự đánh giá. Đây là tài liệu hỗ trợ học tập, không phải đề thi chính thức.

Nội dung đã được đối chiếu với Claude Academy và tài liệu Anthropic ngày 10/11/2026. [Xem báo cáo kiểm chứng nguồn](./claude-academy-source-audit.md). Chi tiết sản phẩm, API và permission có thể thay đổi theo phiên bản.

| Khóa học | Tài liệu ôn thi |
| --- | --- |
| AI Fluency: Framework and Foundations | [Mở tài liệu](./study-guides/ai-fluency-framework-foundations.md) |
| AI Capabilities and Limitations | [Mở tài liệu](./study-guides/ai-capabilities-and-limitations.md) |
| Claude 101 | [Mở tài liệu](./study-guides/claude-101.md) |
| Claude Code 101 | [Mở tài liệu](./study-guides/claude-code-101.md) |
| Introduction to agent skills | [Mở tài liệu](./study-guides/introduction-to-agent-skills.md) |
| Introduction to subagents | [Mở tài liệu](./study-guides/introduction-to-subagents.md) |
| Building with the Claude API | [Mở tài liệu](./study-guides/building-with-the-claude-api.md) |
| Claude Code in Action | [Mở tài liệu](./study-guides/claude-code-in-action.md) |

## Lộ trình học gợi ý

Học theo thứ tự từ nền tảng AI đến sử dụng Claude, rồi đi sâu vào Claude Code:

| Bước | Khóa học | Trọng tâm |
| ---: | --- | --- |
| 1 | [AI Capabilities and Limitations](./study-guides/ai-capabilities-and-limitations.md) | Hiểu năng lực và giới hạn của mô hình ngôn ngữ lớn. |
| 2 | [AI Fluency: Framework and Foundations](./study-guides/ai-fluency-framework-foundations.md) | Học khung 4D để giao việc, mô tả, đánh giá và sử dụng AI có trách nhiệm. |
| 3 | [Claude 101](./study-guides/claude-101.md) | Làm quen với các sản phẩm và cách làm việc cùng Claude. |
| 4 | [Claude Code 101](./study-guides/claude-code-101.md) | Nắm agentic loop và quy trình Explore → Plan → Code → Commit. |
| 5 | [Introduction to agent skills](./study-guides/introduction-to-agent-skills.md) | Tạo workflow tái sử dụng cho Claude Code. |
| 6 | [Introduction to subagents](./study-guides/introduction-to-subagents.md) | Phân rã task và giao việc cho agent chuyên biệt. |
| 7 | [Claude Code in Action](./study-guides/claude-code-in-action.md) | Mở rộng sang phiên dài, tự động hóa và kiểm chứng. |
| 8 | [Building with the Claude API](./study-guides/building-with-the-claude-api.md) | Xây ứng dụng với API, tool use, RAG, MCP và agent workflows. |

Nếu mục tiêu là xây ứng dụng qua API, có thể học **Building with the Claude API** ngay sau Claude 101; khóa này phù hợp hơn khi đã có nền tảng Python, JSON và quyền truy cập API.

## Cấu trúc chuẩn của mỗi tài liệu

Mỗi bài học được tổ chức theo cấu trúc thống nhất:
1. **Thông tin khóa học & Metadata:** Đơn vị, nền tảng, thời lượng, số bài học và mục tiêu chuẩn đầu ra.
2. **Mục lục điều hướng (Table of Contents):** Đánh số thứ tự với liên kết anchor tự động đến từng phần.
3. **Nội dung chi tiết từng Lesson:** Giải thích lý thuyết bản chất, cú pháp cấu hình, prompt mẫu và kịch bản thực hành.
4. **Quy trình & Best Practices:** Workflow thực chiến, checklist nghiệm thu và các anti-patterns cần tránh.
5. **Tổng kết & Câu hỏi ôn tập:** Tóm lược kiến thức cốt lõi, bảng ghi nhớ nhanh (cheat sheet) và câu hỏi tự kiểm tra có kèm gợi ý đáp án.
6. **Nguồn tham khảo:** Liên kết trực tiếp tới tài liệu chính thức của Anthropic và Claude Academy.

---

## Giấy phép & Bản quyền

- Nội dung gốc thuộc bản quyền của [Anthropic](https://www.anthropic.com/) / [Claude Academy](https://academy.claude.com/).
- Bộ tài liệu ghi chú được biên soạn và bản địa hóa phục vụ mục đích học tập và nghiên cứu cá nhân.


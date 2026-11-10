# Đề cương ôn tập: Claude Code 101

> Tài liệu tự ôn trước khi kiểm tra kiến thức; không phải đề thi chính thức.  
> Ghi chú đầy đủ: [Claude Code 101](../claude-code-101-notes.md)

## Mục tiêu cần đạt

- Giải thích Claude Code là coding agent làm việc với repository và công cụ.
- Nắm vòng lặp agentic: thu thập ngữ cảnh → hành động → kiểm chứng.
- Vận dụng Explore → Plan → Code → Commit.
- Biết dùng context, permissions, CLAUDE.md, subagents, Skills, MCP và Hooks.

## Coding agent và agentic loop

Claude Code có thể đọc/tìm file, sửa code, chạy lệnh/test và tiếp tục dựa trên kết quả. Vòng lặp điển hình:

1. **Gather context:** hiểu yêu cầu, cấu trúc repository và file liên quan.
2. **Take action:** chọn tool, sửa hoặc chạy lệnh trong phạm vi được cấp.
3. **Verify results:** đọc output, xem diff/test và sửa tiếp nếu cần.

Claude không nạp toàn bộ codebase cùng lúc; nó tìm phần liên quan. Người dùng cần duyệt hành động và kết quả.

## Workflow Explore → Plan → Code → Commit

- **Explore:** khảo sát kiến trúc, điểm vào, dependency và luồng dữ liệu.
- **Plan:** đề xuất bước làm, file dự kiến đổi, rủi ro và cách test.
- **Code:** triển khai từng phần, giữ diff trong phạm vi.
- **Commit:** rà diff, chạy kiểm tra phù hợp và ghi nhận thay đổi khi được yêu cầu.

Với task lớn, chưa bắt đầu sửa khi yêu cầu hoặc kế hoạch còn mơ hồ.

## Context và permissions

- Context window chứa prompt, lịch sử, file đã đọc, output và hướng dẫn.
- Quản lý context bằng cách chỉ cung cấp nội dung cần thiết, chia task, tóm tắt hoặc làm mới phiên.
- Permission quyết định tool/hành động nào được chạy tự động hoặc cần xác nhận.
- Dùng least privilege; cẩn trọng với lệnh phá hủy, secrets, cài dependency và deploy.
- Output lệnh/test là bằng chứng; không chỉ dựa vào câu tự báo cáo.

## Thành phần tùy biến

| Thành phần | Vai trò |
| --- | --- |
| **CLAUDE.md** | Hướng dẫn nền về dự án, kiến trúc và quy ước |
| **Subagent** | Nhiệm vụ con trong context riêng |
| **Skill** | Hướng dẫn/quy trình tái sử dụng |
| **MCP** | Kết nối với tools/resources bên ngoài |
| **Hook** | Chạy logic tại event trong vòng đời Claude Code |

Phân biệt nhanh: CLAUDE.md cho hướng dẫn thường trực; Skill cho quy trình theo task; Subagent cho công việc độc lập; MCP cung cấp tích hợp; Hook chạy logic tại event.

## Review và hoàn tất

- Prompt nên nêu task, bối cảnh, phạm vi và acceptance criteria.
- Review code tập trung vào bug, edge case, bảo mật, regression và test.
- Reviewer với context mới giúp có góc nhìn độc lập.
- Trước khi kết thúc: kiểm tra diff, file thay đổi và những kiểm tra đã chạy.
- Làm theo hướng dẫn cài đặt/phiên bản chính thức phù hợp môi trường.

## Điểm dễ nhầm

- Claude Code là agent có thể hành động; code completion chỉ gợi ý code.
- Explore là tìm hiểu; Plan là đề xuất; cả hai chưa phải triển khai.
- CLAUDE.md hướng dẫn nhưng không thay thế permission hoặc Hook.
- Subagent không mặc nhiên có toàn bộ hội thoại của main agent.
- MCP là tích hợp; Hook là automation gắn với event.
- Không mặc định commit mọi task nếu không được yêu cầu.

## Tự kiểm tra

1. **Agentic loop có ba giai đoạn nào?** Thu thập context, hành động, kiểm chứng.
2. **Khi nào cần Plan trước Code?** Khi task lớn, nhiều file, rủi ro hoặc chưa rõ.
3. **Vì sao xem diff sau khi Claude báo xong?** Phát hiện thay đổi ngoài phạm vi và rà kết quả thực.
4. **CLAUDE.md, Skill và Hook khác gì?** Hướng dẫn nền, quy trình tái dùng và logic tự động tại event.
5. **Khi nào dùng Subagent?** Task độc lập cần context/góc nhìn riêng.
6. **Permission mode giải quyết gì?** Kiểm soát mức tự chủ và hành động cần xác nhận.
7. **Nêu workflow tiêu chuẩn.** Explore → Plan → Code → Commit.
8. **Vì sao tránh nhiều phiên sửa chung working tree?** Có thể ghi đè hoặc xung đột thay đổi.

## Checklist trước khi thi

- [ ] Giải thích được agentic loop và workflow bốn bước.
- [ ] Chọn đúng thành phần cho tình huống.
- [ ] Nêu rủi ro context/permissions và cách giảm.
- [ ] Mô tả review bằng diff và kết quả test.

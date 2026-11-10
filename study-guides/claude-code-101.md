# Tài liệu ôn thi: Claude Code 101

> Bản ôn tập đầy đủ về quy trình lập trình với Claude Code; không phải đề thi chính thức. Đối chiếu [ghi chú khóa học](../claude-code-101-notes.md), [trang Claude Academy](https://academy.claude.com/courses/claude-code-101) và [tài liệu Claude Code](https://code.claude.com/docs/en/).

## 1. Mục tiêu học tập

Bạn cần giải thích Claude Code khác chatbot/code completion như thế nào; hiểu agentic loop, tools, permissions và context; thực hiện workflow **Explore → Plan → Code → Commit**; quản lý phiên; duy trì hướng dẫn bằng `CLAUDE.md`; và chọn đúng công cụ mở rộng như Skills, subagents, MCP và hooks.

## 2. Coding agent và agentic loop

Claude Code là công cụ lập trình có thể khảo sát repository, chọn công cụ, đọc/sửa file, chạy lệnh và dùng kết quả quan sát được để quyết định bước tiếp theo. Vòng lặp cốt lõi:

1. **Gather context:** hiểu yêu cầu, tìm file/liên hệ liên quan, đọc hướng dẫn dự án.
2. **Take action:** gọi tool phù hợp trong phạm vi quyền cho phép.
3. **Observe and verify:** xem output, kiểm tra thay đổi hoặc test, rồi sửa tiếp nếu bằng chứng cho thấy cần.

Điều phân biệt agent với autocomplete không chỉ là khả năng viết code, mà còn là khả năng tự điều phối nhiều hành động trong giới hạn được cấp. Tuy vậy, agent có thể hiểu sai yêu cầu, chọn cách sửa sai hoặc diễn giải output nhầm; con người vẫn cần đặt phạm vi và xem kết quả.

### Cài đặt và mở phiên làm việc

Claude Code có thể được dùng qua terminal, tích hợp IDE, ứng dụng desktop hoặc môi trường web tùy cách triển khai hiện hành. Cài đặt và đăng nhập theo hướng dẫn chính thức tương ứng với hệ điều hành/tài khoản; một số môi trường dùng gói Claude, số khác dùng API billing. Trước khi giao task, mở đúng thư mục repository, kiểm tra branch và thay đổi Git đang có, đọc hướng dẫn dự án, rồi chọn mức permission phù hợp. Không đặt API key trong prompt hay file được commit.

Prompt đầu tiên nên nêu task, bối cảnh, phạm vi và tiêu chí hoàn thành. Với task nhiều bước, yêu cầu Claude khảo sát/lập kế hoạch trước khi cho phép sửa; đọc kế hoạch và điều chỉnh khi giả định không đúng.

## 3. Tools, permissions và mức tự chủ

- **Tools** là hành động có thể dùng, chẳng hạn đọc file, tìm kiếm, sửa hoặc chạy lệnh.
- **Permissions** quyết định hành động nào được tự động, cần hỏi, hay bị chặn.
- **Context** là hướng dẫn, prompt, lịch sử, file và output đang được phiên dùng.

Ba yếu tố tương tác: tool tạo năng lực hành động; permission đặt ranh giới; context định hướng lựa chọn. Chọn quyền theo rủi ro task. Đọc/test trong sandbox thường ít rủi ro hơn xóa file, thay đổi dữ liệu, cài phần mềm, truy cập secret hoặc deploy. Mode có tên/cách hoạt động cụ thể có thể đổi theo phiên bản; đọc cấu hình/docs của bản đang chạy trước khi dựa vào tên mode.

## 4. Workflow Explore → Plan → Code → Commit

### Explore — khảo sát

Đọc README/hướng dẫn, cấu trúc thư mục, điểm vào, test hiện có, thay đổi chưa commit và các file liên quan. Tìm nơi dữ liệu đi qua thay vì chỉ đọc file trùng từ khóa. Mục tiêu là hiểu hành vi hiện có và giới hạn task.

### Plan — lập kế hoạch

Với task nhiều bước/rủi ro, mô tả các bước, file dự kiến, lựa chọn thiết kế, khả năng ảnh hưởng và cách kiểm chứng. Sửa kế hoạch khi khám phá phát hiện thông tin mới. Lập kế hoạch không phải bảo đảm kế hoạch đúng; nó làm giả định nhìn thấy được trước khi thay đổi lớn.

### Code — triển khai

Sửa từng phần nhỏ, bám tiêu chí nghiệm thu, theo quy ước repo. Không mở rộng phạm vi tự phát. Nếu task hoặc yêu cầu thay đổi, cập nhật kế hoạch/tiêu chí trước.

### Commit — kiểm tra và ghi nhận

Đọc diff; chạy test/lint/typecheck/build phù hợp với dự án; xác minh không có file/secret ngoài phạm vi. Commit là hành động ghi nhận Git và chỉ thực hiện khi người dùng/quy trình yêu cầu hoặc đã được ủy quyền — không phải bước tự động bắt buộc của mọi task.

**Prompt mẫu:** “Trong repository này, sửa lỗi khi form gửi hai lần. Khảo sát luồng hiện tại trước; không đổi API công khai. Tiêu chí: một lần submit tạo tối đa một request, trạng thái loading được hiển thị và test regression mới bao phủ trường hợp gửi lặp. Sau khi sửa, chạy test liên quan và báo file/diff cùng lệnh đã chạy.”

## 5. Quản lý context và review

Context window giới hạn lượng thông tin một phiên xử lý. Khi phiên dài, có thể kiểm tra context, nén nội dung hoặc làm mới phiên theo lệnh/tính năng hiện hành của Claude Code. Trong khóa học, các lệnh cần biết gồm `/context`, `/compact` và `/clear`; xác minh cú pháp theo docs mới nếu phiên bản thay đổi.

- Dùng `/context` để hiểu phần nào đang chiếm context.
- `/compact` tạo bản tóm tắt rút gọn; trước khi nén, nêu điều cần giữ như quyết định, file, constraint và test chưa chạy.
- `/clear` bắt đầu context hội thoại sạch khi task mới không cần lịch sử cũ.

Review tối thiểu:

1. So sánh diff với yêu cầu và phạm vi.
2. Kiểm tra logic, edge cases, bảo mật và regression.
3. Đọc kết quả thực tế từ test/lệnh; phân biệt **pass**, **fail**, **not run**.
4. Với thay đổi quan trọng, yêu cầu góc nhìn độc lập/context mới và xử lý mọi phát hiện.

Một lời tóm tắt “đã xong” không thay thế diff hoặc bằng chứng chạy kiểm tra.

## 6. Tùy biến Claude Code

| Thành phần | Dùng khi | Hạn chế cần nhớ |
| --- | --- | --- |
| **`CLAUDE.md`** | Quy ước/bối cảnh dự án thường xuyên cần đọc | Là hướng dẫn; không cưỡng chế hành động |
| **Skill** | Quy trình nhiều bước dùng lại, được gọi khi phù hợp | Nội dung cần rõ; quyền tool không mặc nhiên tăng |
| **Subagent** | Giao việc hẹp/độc lập trong context riêng | Cần giao đủ input và xác minh phần trả về |
| **MCP** | Kết nối tool, dữ liệu hoặc dịch vụ ngoài | Quản lý quyền và tin cậy server |
| **Hook** | Chạy logic tại event, ví dụ gate kiểm tra | Script/config có thể ảnh hưởng thao tác; cần kiểm thử |

`CLAUDE.md` nên ngắn, cụ thể và có thể kiểm tra: cấu trúc, lệnh build/test, quy ước, ràng buộc và điều không được làm. Đưa chi tiết chỉ dùng đôi khi vào tài liệu/Skill phụ để tránh làm file nền dài, nhưng nhớ hướng dẫn cách tìm tài liệu đó. Không ghi secret vào file hướng dẫn.

**MCP** (Model Context Protocol) chuẩn hóa cách host/client kết nối server; server có thể cung cấp tools (hành động), resources (dữ liệu) và prompts (mẫu chỉ dẫn). Kiểm tra server được cấu hình ở đâu, quyền nào được cấp và dữ liệu có thể gửi ra ngoài ra sao.

## 7. Bài tập tình huống có lời giải mẫu

**Tình huống:** “Hãy thêm chức năng export CSV” trong một dự án không quen thuộc.

- **Explore:** tìm luồng dữ liệu, UI/API, format ngày/encoding, auth và test; kiểm tra thay đổi Git hiện có.
- **Plan:** xác định trường được xuất, quyền, xử lý dấu phẩy/quote/newline, file lớn, tên file và test.
- **Code:** triển khai nhỏ; không tự chọn hoặc xuất dữ liệu nhạy cảm nếu chưa rõ yêu cầu.
- **Verify:** test dữ liệu rỗng, Unicode, dấu quote, quyền và file tải xuống; đọc diff.
- **Commit:** chỉ commit nếu được yêu cầu/ủy quyền; tóm tắt điều đã kiểm tra và phần chưa kiểm tra.

Nếu yêu cầu không nói rõ trường nào được export, nên hỏi hoặc báo giả định thay vì âm thầm xuất toàn bộ dữ liệu.

## 8. Câu hỏi ôn tập

1. **Agentic loop gồm những pha nào?** Thu thập context, hành động bằng tool, quan sát và kiểm chứng.
2. **Tool và permission khác gì?** Tool là khả năng hành động; permission xác định có được hành động và cần phê duyệt không.
3. **Vì sao Explore trước Code?** Để hiểu kiến trúc, hướng dẫn, test và thay đổi đang có trước khi tác động.
4. **Khi nào nên lập Plan?** Task nhiều bước, nhiều file, mơ hồ hoặc có rủi ro đáng kể.
5. **`CLAUDE.md` có đảm bảo Claude không chạy lệnh bị cấm không?** Không; đó là hướng dẫn, cần permission/hook hoặc kiểm soát khác cho quy tắc bắt buộc.
6. **`/compact` và `/clear` khác nhau thế nào?** Compact rút gọn/giữ context liên quan; clear mở context sạch cho công việc mới.
7. **Vì sao phải đọc diff sau khi test pass?** Test chỉ kiểm tra một số hành vi; diff cho thấy phạm vi và nội dung thay đổi.
8. **Khi nào dùng subagent?** Việc phụ rõ ràng, có thể tách, cần góc nhìn riêng hoặc context độc lập.
9. **MCP tools/resources/prompts lần lượt làm gì?** Hành động, cung cấp dữ liệu, cung cấp template/chỉ dẫn.
10. **Cần báo gì nếu test không chạy?** Ghi rõ test nào chưa chạy và lý do; không báo là pass.
11. **Có phải luôn commit sau Code?** Không; commit cần theo yêu cầu hoặc quy trình đã được ủy quyền.
12. **Một lời nhắc trong `CLAUDE.md` về “không xóa production data” đủ chưa?** Không nếu phải cưỡng chế; dùng quyền/hook/quy trình bảo vệ và kiểm tra trước hành động.

## 9. Nhầm lẫn cần tránh

- Claude Code không chỉ gợi ý đoạn code; nó có thể hành động qua tools trong quyền được cấp.
- Tự chủ cao hơn không đồng nghĩa kết quả đúng hơn.
- Plan không sửa file; còn kế hoạch phải được đánh giá như một đề xuất.
- `CLAUDE.md`, Skill, subagent, MCP và Hook giải quyết vấn đề khác nhau.
- Test pass không chứng minh không có lỗi ngoài phạm vi test.

## 10. Tự đánh giá trước kỳ thi

- [ ] Mô tả agentic loop và vai trò của tool/permission/context.
- [ ] Áp dụng Explore → Plan → Code → Commit cho một task mới.
- [ ] Nêu cách quản lý phiên dài và chọn lệnh context phù hợp.
- [ ] Phân biệt năm thành phần tùy biến và chọn đúng trường hợp.
- [ ] Đề xuất review dựa trên diff, test và bằng chứng.

## Nguồn tham khảo

- [Claude Academy — Claude Code 101](https://academy.claude.com/courses/claude-code-101)
- [Claude Code — Overview](https://code.claude.com/docs/en/overview)
- [Claude Code — Common workflows](https://code.claude.com/docs/en/common-workflows)
- [Claude Code — Memory (`CLAUDE.md`)](https://code.claude.com/docs/en/memory)
- [Claude Code — Hooks](https://code.claude.com/docs/en/hooks)

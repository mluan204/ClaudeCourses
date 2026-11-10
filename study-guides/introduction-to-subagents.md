# Tài liệu ôn thi: Introduction to Subagents

> Bản ôn tập đầy đủ về phân rã và ủy nhiệm công việc cho subagents trong Claude Code; không phải đề thi chính thức. Đối chiếu [ghi chú khóa học](../introduction-to-subagents-notes.md), [trang Claude Academy](https://academy.claude.com/courses/introduction-to-subagents) và [tài liệu subagents](https://code.claude.com/docs/en/sub-agents).

## 1. Mục tiêu học tập

Bạn nên có thể giải thích context riêng của subagent; xác định phần việc phù hợp để ủy nhiệm; viết input/output rõ; tạo subagent tùy chỉnh với role và tool access tối thiểu; phân biệt làm song song với làm tuần tự; và tổng hợp kết quả có kiểm chứng.

## 2. Mô hình main agent và subagent

**Main agent** chịu trách nhiệm với yêu cầu tổng thể: lập kế hoạch, quyết định phần việc nào có thể tách, giao nhiệm vụ, nhận kết quả, xử lý mâu thuẫn và trả lời người dùng. **Subagent** là worker cho một nhiệm vụ chuyên biệt, chạy trong context riêng và trả kết quả về main agent.

Luồng cơ bản:

1. Main agent hiểu mục tiêu, ràng buộc và tiêu chí hoàn thành.
2. Tách một nhiệm vụ độc lập, mô tả scope và output.
3. Khởi chạy subagent với prompt, hướng dẫn và tools phù hợp.
4. Subagent đọc/khảo sát/thực hiện trong quyền đã cấp.
5. Subagent báo kết quả, bằng chứng, trở ngại và phần chưa xác minh.
6. Main agent kiểm tra và tích hợp kết quả.

Subagent không mặc nhiên có toàn bộ lịch sử hội thoại hoặc suy nghĩ của main agent. Vì thế, bàn giao cần đủ thông tin: kết quả mong muốn, file/nguồn, constraint, điều không được làm, mức quyền và định dạng báo cáo. Context riêng giữ luồng chính gọn, nhưng cũng có thể làm mất một giả định nếu main agent không truyền nó.

## 3. Khi nào nên dùng?

Phù hợp khi công việc phụ có thể hoàn thành tương đối độc lập và có điểm bàn giao rõ:

- **Explore/research:** khảo sát thư mục hoặc nguồn, báo sơ đồ/phát hiện.
- **Independent review:** đọc diff từ góc nhìn mới, chỉ báo lỗi và bằng chứng.
- **Documentation:** soạn tài liệu cho một module với style/ràng buộc riêng.
- **Parallel analysis:** xem nhiều module/nguồn độc lập để main agent tổng hợp.
- **Reusable specialist:** công việc lặp lại cần role, hướng dẫn hoặc quyền riêng.

Không nên dùng nếu task rất nhỏ; các bước phụ thuộc sát nhau; main agent cần theo dõi từng bước; output phụ thuộc context chưa thể truyền; nhiều agent cùng sửa file; hoặc overhead giao việc lớn hơn lợi ích. “Chạy song song” không tự làm tiến độ nhanh nếu kết quả phải chờ lẫn nhau hoặc không đủ người rà soát.

## 4. Bốn pattern thiết kế

| Pattern | Cách làm | Tác dụng |
| --- | --- | --- |
| **Clear role** | Một chức năng hẹp, mục tiêu cụ thể và phạm vi rõ | Giảm lan man/chồng lấn |
| **Structured output** | Quy định schema/đầu mục bắt buộc | Main agent dễ tổng hợp và phát hiện thiếu |
| **Obstacle reporting** | Yêu cầu liệt kê blocker, giả định, lệnh lỗi và phần chưa kiểm tra | Tránh nhầm “không thấy lỗi” với “đã kiểm tra đủ” |
| **Limit tool access** | Chỉ cấp công cụ cần cho nhiệm vụ | Giảm rủi ro và giữ vai trò nhất quán |

Ví dụ output code review: `Summary`; `Findings` (severity, file/line, bằng chứng, tác động); `Checks run`; `Unverified areas`; `Obstacles`. Yêu cầu không tạo finding nếu không có bằng chứng, và báo rõ nếu không chạy test.

## 5. Tạo custom subagent

Subagent thường được mô tả trong một file Markdown với YAML frontmatter. Ví dụ giản lược:

```markdown
---
name: code-reviewer
description: Reviews proposed changes for bugs and security issues. Use after implementation; report findings only.
tools: Read, Grep, Glob
model: inherit
---

Inspect the requested diff and the relevant surrounding code. Do not edit files.
Report only actionable findings with evidence, severity, and affected location.
List files or checks you could not inspect.
```

- **`name`:** định danh agent.
- **`description`:** mục đích và lúc nên dùng; ảnh hưởng đến việc Claude chọn/delegate.
- **`tools`:** danh sách tool được cấp (cú pháp/tên phụ thuộc phiên bản/cấu hình).
- **`model`:** chọn model hoặc kế thừa model chính, tùy hỗ trợ hiện hành.
- **Body:** system prompt—vai trò, quy trình, phạm vi, tiêu chí và format output.

Project-level agent thường đặt trong `.claude/agents/` và có thể được chia sẻ cùng repo; user-level trong thư mục cấu hình người dùng để dùng nhiều dự án. Xem docs hiện hành về precedence/scope và field được hỗ trợ trước khi triển khai. Reviewer chỉ đọc thường không cần tool chỉnh sửa. Agent có quyền chạy command phải được giao constraint rõ và xem xét kỹ hơn.

## 6. Giao việc và phối hợp

Một task handoff tốt bao gồm:

```text
Mục tiêu: Tìm nguyên nhân test đăng nhập thất bại trên Windows.
Scope: test auth, cấu hình test và code liên quan; không sửa file.
Context: Lỗi xảy ra sau thay đổi parser; chỉ dựa vào working tree hiện tại.
Yêu cầu: Tìm đường đi của test và 2–3 giả thuyết có bằng chứng.
Không làm: Không cài dependency, không thay đổi config.
Output: Findings (file/line, bằng chứng, mức tin cậy), checks đã chạy,
        obstacles và phần chưa xác minh.
```

Main agent cần kiểm tra:

1. Nhiệm vụ có thật sự độc lập không?
2. Hai agent có đụng cùng file/dữ liệu không?
3. Kết quả có cùng định nghĩa hoặc đơn vị để so sánh không?
4. Nguồn/bằng chứng có thể kiểm tra được không?
5. Ai sẽ tổng hợp, quyết định mâu thuẫn và chịu trách nhiệm cuối?

Khi chạy song song, giao input/scope tách biệt và output tương thích. Có thể dùng một agent đọc-only để rà code trong khi main agent triển khai, nhưng main agent phải đối chiếu findings với code và test. Tránh để hai agent cùng ghi một file nếu không có cách phối hợp/xung đột rõ ràng.

## 7. Bài tập ứng dụng có lời giải mẫu

**Tình huống:** Cần chuẩn bị thay đổi API gồm ba phần: khảo sát schema hiện tại, review phương án bảo mật, cập nhật tài liệu. Chưa bắt đầu sửa.

- Tách **khảo sát schema** và **review phương án bảo mật** thành hai task đọc-only có thể song song nếu cùng được cung cấp đủ context và không phụ thuộc lẫn nhau.
- Tài liệu chỉ nên viết sau khi đã thống nhất schema/quyết định bảo mật; đây là bước phụ thuộc nên chạy sau.
- Cả hai agent trả về nguồn/file/line, giả định và phần chưa xác minh.
- Main agent so sánh phát hiện, giải quyết mâu thuẫn và kiểm tra tài liệu với quyết định cuối.

Nếu reviewer không có bằng chứng, hoặc hai báo cáo mâu thuẫn, kết luận đó chưa sẵn sàng để đưa vào thay đổi. Cần kiểm tra trực tiếp hoặc yêu cầu điều tra có scope chính xác hơn.

## 8. Câu hỏi ôn tập

1. **Main agent khác subagent thế nào?** Main agent điều phối và chịu trách nhiệm tích hợp; subagent xử lý task chuyên biệt trong context riêng.
2. **Subagent có tự biết toàn bộ lịch sử chat không?** Không mặc nhiên; cần truyền context cần thiết.
3. **Nêu bốn pattern thiết kế.** Role rõ, output có cấu trúc, báo trở ngại, giới hạn tool.
4. **Khi nào parallelize hợp lý?** Khi các task tương đối độc lập, input đủ, output có thể hợp nhất và có người kiểm tra.
5. **Vì sao output có schema vẫn cần review?** Cấu trúc chỉ giúp thống nhất/đủ trường, không đảm bảo nội dung đúng.
6. **Reviewer chỉ đọc nên có quyền edit không?** Thường không; cấp quyền tối thiểu cho vai trò.
7. **Nên giao “hãy xem toàn bộ app và sửa hết” không?** Không; quá rộng, thiếu tiêu chí và không tách được trách nhiệm.
8. **Điều gì cần đưa vào handoff?** Mục tiêu, bối cảnh, scope, constraint, quyền, output mong muốn và cách báo blocker.
9. **Làm gì với finding quan trọng từ subagent?** Kiểm tra lại bằng chứng và xác minh độc lập trước khi hành động.
10. **Task nào không thích hợp cho subagent?** Bước nhỏ, tuần tự phụ thuộc sát, yêu cầu tương tác liên tục hoặc chia sẻ file dễ xung đột.
11. **Obstacle reporting cần nêu gì?** Phần bị chặn, giả định, lệnh/test lỗi, giới hạn và vùng chưa xác minh.
12. **Ai chịu trách nhiệm cho câu trả lời tổng hợp?** Main agent/người vận hành; không chuyển trách nhiệm cho subagent.

## 9. Nhầm lẫn cần tránh

- Context riêng vừa là lợi thế vừa là lý do phải bàn giao đầy đủ.
- “Không tìm thấy lỗi” có thể có nghĩa là chưa xem đủ; yêu cầu liệt kê vùng chưa kiểm tra.
- Song song chỉ hữu ích với tính độc lập; phụ thuộc tuần tự cần được giữ thứ tự.
- Nhiều agent cùng sửa không tương đương cộng tác an toàn.
- Main agent phải kiểm chứng, không chỉ nối các câu trả lời lại.

## 10. Tự đánh giá trước kỳ thi

- [ ] Tách được task độc lập khỏi task phụ thuộc.
- [ ] Viết handoff gồm bối cảnh, scope, giới hạn và output.
- [ ] Thiết kế subagent với role, tools và prompt tối thiểu.
- [ ] Dự đoán rủi ro khi parallelize.
- [ ] Kiểm tra và tổng hợp findings dựa trên bằng chứng.

## Nguồn tham khảo

- [Claude Academy — Introduction to subagents](https://academy.claude.com/courses/introduction-to-subagents)
- [Claude Code Docs — Subagents](https://code.claude.com/docs/en/sub-agents)

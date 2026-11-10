# Đề cương ôn tập: Introduction to Subagents

> Tài liệu tự ôn trước khi kiểm tra kiến thức; không phải đề thi chính thức.  
> Ghi chú đầy đủ: [Introduction to subagents](../introduction-to-subagents-notes.md)

## Mục tiêu cần đạt

- Giải thích context isolation và vai trò main agent/subagent.
- Phân rã task thành phần độc lập có thể delegate.
- Thiết kế subagent với role, structured output, obstacle reporting và least privilege.
- Biết khi nào chạy song song và khi nào không nên dùng subagent.

## Mô hình cơ bản

**Main agent** điều phối yêu cầu, phân công và tổng hợp. **Subagent** xử lý nhiệm vụ chuyên biệt trong context riêng rồi trả kết quả.

Lợi ích:

- Công việc trung gian không làm đầy context chính.
- Nhiệm vụ độc lập có thể chạy song song.
- Có thể cấp prompt, model và tools phù hợp với vai trò.
- Tạo góc nhìn mới cho research hoặc code review độc lập.

Subagent không mặc nhiên có toàn bộ hội thoại hay Skills đã gọi ở main agent. Main agent cần cung cấp đủ input và xác minh output.

## Bốn pattern thiết kế

| Pattern | Thiết kế | Vì sao |
| --- | --- | --- |
| **Clear role** | Vai trò/nhiệm vụ hẹp, rõ | Giảm lệch phạm vi |
| **Structured output** | Schema/format và mục bắt buộc | Dễ tổng hợp, đối chiếu, tự động hóa |
| **Obstacle reporting** | Trở ngại, thử nghiệm, lỗi, workaround, phần chưa xác minh | Thấy được giới hạn kết quả |
| **Limit tool access** | Chỉ cấp tools cần thiết | Giảm rủi ro và nhiễu |

Subagent review chỉ đọc thường không cần Edit/Write. Dùng least privilege.

## Input và output khi delegate

Input nên nêu mục tiêu, phạm vi, file/nguồn cần xem, ràng buộc, tiêu chí, quyền được dùng và định dạng output.

Output tốt có kết luận, bằng chứng, file liên quan, phần chưa xác minh, trở ngại và bước tiếp theo.

## Khi nào nên dùng?

- Research/khám phá codebase.
- Independent code/security review.
- Tạo tài liệu với system prompt riêng.
- Parallelize các nhiệm vụ độc lập, ví dụ rà nhiều module không phụ thuộc nhau.

## Khi nào không nên dùng?

- Task quá nhỏ so với chi phí điều phối.
- Các bước phụ thuộc sát, cần giữ mạch suy luận.
- Cần theo dõi tiến trình liên tục hoặc nhận toàn bộ output.
- Nhiều agent có thể sửa cùng file hoặc tạo kết quả trùng lặp.

## Quy trình thiết kế

1. Xác định mục tiêu tổng thể.
2. Chia thành nhiệm vụ độc lập; kiểm tra dependency.
3. Chọn scope: project-level cho repo, user-level cho nhiều dự án.
4. Viết role và description để định tuyến đúng task.
5. Chọn tools, model và permission tối thiểu.
6. Định nghĩa output có cấu trúc và obstacle report.
7. Giao nhiệm vụ tường minh.
8. Kiểm tra bằng chứng, giải quyết mâu thuẫn rồi mới tổng hợp.

## Điểm dễ nhầm

- Chạy song song không nhanh hơn nếu task phụ thuộc nhau.
- Tóm tắt khi bàn giao có thể làm mất chi tiết cần cho bước sau.
- Output có cấu trúc không đồng nghĩa output đúng.
- Main agent vẫn chịu trách nhiệm cho kết quả cuối cùng.
- Không delegate task nhỏ chỉ để tăng số lượng agent.

## Tự kiểm tra

1. **Subagent khác main agent thế nào?** Main agent điều phối; subagent xử lý việc hẹp trong context riêng.
2. **Context isolation có lợi gì?** Giữ context chính gọn và tạo không gian riêng cho nhiệm vụ phụ.
3. **Bốn pattern thiết kế là gì?** Clear role, structured output, obstacle reporting, limit tool access.
4. **Khi nào nên chạy song song?** Khi task độc lập, input rõ và kết quả tổng hợp được.
5. **Vì sao cần obstacle reporting?** Để biết phần nào bị chặn, thiếu hoặc chưa xác minh.
6. **Main agent phải làm gì với kết quả?** Kiểm tra bằng chứng, xác minh phát hiện quan trọng, giải quyết mâu thuẫn và tổng hợp.
7. **Nêu hai trường hợp không nên dùng.** Task nhỏ; hoặc các bước phụ thuộc chặt, cần giữ context liên tục.

## Checklist trước khi thi

- [ ] Phân biệt main agent/subagent.
- [ ] Nhớ bốn pattern và lý do dùng.
- [ ] Đánh giá tính độc lập trước khi parallelize.
- [ ] Thiết kế được input/output và báo trở ngại.
- [ ] Nêu trách nhiệm kiểm chứng của main agent.

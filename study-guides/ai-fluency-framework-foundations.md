# Đề cương ôn tập: AI Fluency — Framework and Foundations

> Tài liệu tự ôn trước khi kiểm tra kiến thức; không phải đề thi chính thức.  
> Ghi chú đầy đủ: [AI Fluency: Framework and Foundations](../ai-fluency-framework-foundations-notes.md)

## Mục tiêu cần đạt

- Giải thích AI Fluency là cộng tác với AI hiệu quả, có phán đoán và có trách nhiệm.
- Vận dụng khung 4D vào một nhiệm vụ.
- Viết yêu cầu có bối cảnh, kết quả mong muốn, quy trình và ràng buộc.
- Đánh giá đầu ra thay vì mặc định tin vào câu trả lời.

## Khung xương sống: 4D

| Năng lực | Câu hỏi cần đặt | Thành phần chính |
| --- | --- | --- |
| **Delegation** — giao việc | Việc nào nên giao, công cụ nào phù hợp, phần nào cần người làm? | Problem awareness, platform awareness, task delegation |
| **Description** — mô tả | Cần tạo kết quả gì, theo quy trình nào, và muốn tương tác ra sao? | Product, process, performance description |
| **Discernment** — đánh giá | Kết quả có đúng, phù hợp và đáng tin không? | Product, process, performance discernment |
| **Diligence** — trách nhiệm | Dữ liệu, tác động, minh bạch và triển khai có được quản lý tốt không? | Creation, transparency, deployment diligence |

Cách nhớ: **Delegation** chọn việc; **Description** nói rõ việc; **Discernment** kiểm tra kết quả; **Diligence** chịu trách nhiệm khi sử dụng.

## Mô tả nhiệm vụ tốt

Nêu đủ bốn phần:

- **Bối cảnh:** vai trò, mục tiêu, người đọc và dữ liệu.
- **Nhiệm vụ/đầu ra:** hành động, định dạng và tiêu chí hoàn thành.
- **Quy trình:** bước cần làm, nguồn được phép dùng, lúc cần hỏi lại.
- **Ràng buộc/tương tác:** điều cần tránh, giọng điệu, mức chi tiết, cách nêu điều chưa chắc chắn.

## Sáu kỹ thuật viết prompt

1. Cung cấp bối cảnh liên quan.
2. Đưa ví dụ cho đầu ra mong muốn.
3. Nêu ràng buộc cụ thể.
4. Chia việc phức tạp thành bước nhỏ.
5. Yêu cầu xem xét/lập kế hoạch trước khi trả lời khi cần.
6. Xác định vai trò hoặc giọng điệu nếu giúp task rõ hơn.

Làm việc theo vòng lặp: **Description → đầu ra → Discernment → phản hồi cụ thể → đầu ra mới**. Phản hồi tốt nói rõ điều gì đúng, điều gì chưa đạt và cần sửa thế nào.

## Khái niệm nền

- Mô hình ngôn ngữ tạo văn bản bằng cách dự đoán token tiếp theo; sự trôi chảy không chứng minh sự thật.
- Pre-training tạo năng lực/kiến thức nền; fine-tuning định hình cách làm theo chỉ dẫn và một số xu hướng hành vi.
- AI có thể hỗ trợ viết, tóm tắt, phân tích và tạo ý tưởng, nhưng vẫn cần nguồn, tiêu chí và kiểm chứng.

## Điểm dễ nhầm

- Giao việc cho AI không đồng nghĩa giao luôn trách nhiệm cho AI.
- Product description nói đầu ra; process description nói cách làm; task tốt thường nêu cả hai.
- Discernment kiểm tra cả kết quả lẫn quy trình.
- Minh bạch và phê duyệt là một phần của Diligence, không phải bước trang trí.

## Tự kiểm tra

1. **4D gồm gì?** Delegation, Description, Discernment, Diligence.
2. **Prompt tốt cần gì?** Bối cảnh, nhiệm vụ, đầu ra, quy trình và ràng buộc phù hợp.
3. **Khi nào nên giao task cho AI?** Khi nền tảng phù hợp, mục tiêu rõ, dữ liệu có thể cung cấp an toàn và có cách kiểm tra.
4. **Discernment kiểm tra gì?** Tính đúng, độ phù hợp, chất lượng đầu ra và cách tạo ra nó.
5. **Nêu ba cách giảm rủi ro.** Cung cấp nguồn, yêu cầu nêu phần chưa chắc chắn, xác minh dữ kiện quan trọng.
6. **Vì sao dùng ví dụ trong prompt?** Để minh họa cấu trúc, phong cách hoặc mức chi tiết cụ thể.
7. **Diligence áp dụng lúc nào?** Trước, trong và sau khi tạo/triển khai kết quả.
8. **Câu hỏi kiểm tra trước khi gửi kết quả?** Tôi đã xác minh dữ kiện, nguồn, quyền dùng dữ liệu và phần cần phê duyệt chưa?

## Checklist trước khi thi

- [ ] Giải thích và áp dụng từng chữ D bằng ví dụ.
- [ ] Viết prompt có bối cảnh, nhiệm vụ, đầu ra và ràng buộc.
- [ ] Phân biệt câu trả lời trôi chảy với câu trả lời đã kiểm chứng.
- [ ] Nêu cách minh bạch và chịu trách nhiệm khi triển khai.

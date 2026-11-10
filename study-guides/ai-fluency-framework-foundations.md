# Tài liệu ôn thi: AI Fluency — Framework and Foundations

> Bản ôn tập đầy đủ để tự học và luyện vận dụng; không phải đề thi hay đáp án chính thức của Claude Academy. Đối chiếu kiến thức với [ghi chú khóa học](../ai-fluency-framework-foundations-notes.md) và [trang Claude Academy](https://academy.claude.com/courses/ai-fluency-framework-foundations).

## 1. Mục tiêu và bản đồ khóa học

Sau khi ôn, bạn nên có thể: giải thích AI Fluency; phân biệt ba hình thức Automation, Augmentation và Agency; áp dụng 4D; mô tả yêu cầu theo đầu ra, quy trình và cách tương tác; đánh giá kết quả; chịu trách nhiệm với dữ liệu và sản phẩm AI hỗ trợ.

AI Fluency là năng lực cộng tác với AI **hiệu quả, hiệu suất, có đạo đức và an toàn**. Đây không phải danh sách mẹo prompt. Người dùng cần quyết định việc nào nên giao, mô tả rõ kết quả, đánh giá phần AI làm, rồi chịu trách nhiệm cho cách sử dụng kết quả. Các nguyên tắc này dùng được với nhiều mô hình; ví dụ trong khóa có thể dùng Claude.

## 2. Ba hình thức làm việc với AI

| Hình thức | Vai trò của AI | Ví dụ | Phần người dùng giữ |
| --- | --- | --- | --- |
| **Automation — tự động hóa** | Thực hiện một nhiệm vụ xác định theo chỉ dẫn | Chuyển ghi chú thành danh sách việc | Xác định dữ liệu, quy tắc và kiểm tra lỗi |
| **Augmentation — tăng cường năng lực** | Cùng người dùng phân tích, tạo ý tưởng hoặc giải quyết vấn đề | Đưa ra ba phương án, sau đó cùng phản biện | Chọn hướng đi và đưa ra phán đoán cuối |
| **Agency — trao quyền chủ động** | Thực hiện chuỗi bước trong mục tiêu, giới hạn và quyền đã định | Tìm tài liệu, tổng hợp và soạn báo cáo nháp | Đặt mục tiêu/giới hạn, theo dõi, phê duyệt và chịu trách nhiệm |

Ba hình thức là một phổ về mức chủ động được giao, không phải thứ bậc “càng tự động càng tốt”. Task lặp lại, tiêu chí kiểm tra rõ phù hợp với Automation. Task cần phán đoán thường hợp với Augmentation. Agency chỉ phù hợp khi phạm vi, quyền và điều kiện dừng được mô tả đủ rõ. Cả bốn năng lực 4D đều cần trong cả ba hình thức.

## 3. Khung 4D

### D1 — Delegation: quyết định giao việc

Trước khi gửi prompt, phân tích ba phần:

1. **Problem Awareness:** Mục tiêu thật là gì? Thành công trông như thế nào? Sai sót gây hậu quả gì? Phần nào cần chuyên môn hoặc phán đoán của con người?
2. **Platform Awareness:** Công cụ có khả năng cần thiết không? Có thể đọc file, duyệt web, chạy code hoặc truy cập dữ liệu không? Nguồn và quyền truy cập có phù hợp không?
3. **Task Delegation:** Chia phần việc giữa người và AI. Giao phần có thể diễn đạt và kiểm tra; giữ lại quyết định có rủi ro, trách nhiệm hoặc thiếu tiêu chí rõ.

Giao việc tốt không phải “đưa hết cho AI”. Người giao cần cung cấp đủ bối cảnh, nhưng không nên chia sẻ dữ liệu nhạy cảm không cần thiết.

### D2 — Description: diễn đạt công việc

Mô tả ba lớp:

- **Product description:** Sản phẩm cuối là gì, dành cho ai, ở định dạng/độ dài nào, tiêu chí nào quyết định là đạt?
- **Process description:** AI cần làm theo bước nào, dùng nguồn nào, xử lý bất định ra sao, khi nào cần hỏi lại?
- **Performance description:** Muốn tương tác theo cách nào: đề xuất trước hay làm ngay, nêu giả định, hỏi từng câu hay báo phần chưa chắc?

Prompt hiệu quả thường có mục tiêu, bối cảnh, đầu ra, tiêu chí, ràng buộc và cách xử lý phần còn thiếu. Chỉ đưa những thông tin hỗ trợ nhiệm vụ; nhiều chữ hơn không đồng nghĩa mô tả tốt hơn.

**Ví dụ — yêu cầu mơ hồ:** “Viết báo cáo về kết quả khảo sát.”

**Phiên bản có thể kiểm tra:** “Dựa chỉ trên bảng khảo sát đính kèm, viết bản tóm tắt tiếng Việt tối đa 500 từ cho ban giám hiệu. Nêu ba xu hướng có số liệu, hai giới hạn của mẫu và một đề xuất. Không suy diễn quan hệ nhân quả. Gắn mỗi số liệu với tên cột hoặc bảng; hỏi tôi nếu định nghĩa cột không rõ.”

Sáu kỹ thuật nền tảng: cung cấp bối cảnh; đưa ví dụ; nêu ràng buộc; chia việc phức tạp; yêu cầu lập kế hoạch/xem xét trước khi trả lời khi hữu ích; chỉ định vai trò hoặc giọng điệu khi nó làm yêu cầu cụ thể hơn. Giao vai trò không thay thế dữ liệu và tiêu chí.

### D3 — Discernment: đánh giá

Đánh giá ở ba lớp:

- **Product discernment:** Kết quả có đúng, đủ, hữu ích, đúng định dạng và phù hợp người đọc không?
- **Process discernment:** Nguồn, bước làm và giả định có thích hợp không? Có bỏ qua bước cần thiết không?
- **Performance discernment:** Cách tương tác có hiệu quả không? Prompt có thiếu rõ ràng? Ta có kiểm tra kết quả đủ kỹ không?

Description và Discernment tạo thành vòng lặp: mô tả → nhận đầu ra → so với tiêu chí → nêu phản hồi cụ thể → sửa yêu cầu/đầu ra → kiểm tra lại. Phản hồi “chưa ổn” ít hữu ích; phản hồi tốt chỉ ra đoạn nào sai tiêu chí, bằng chứng gì và yêu cầu sửa ra sao.

### D4 — Diligence: cẩn trọng và trách nhiệm

- **Creation diligence:** Cân nhắc công cụ, nguồn dữ liệu, quyền riêng tư, thiên lệch và rủi ro ngay trong lúc tạo.
- **Transparency diligence:** Minh bạch về sự tham gia của AI khi người nhận cần biết để diễn giải hoặc đánh giá sản phẩm.
- **Deployment diligence:** Trước khi sử dụng/xuất bản, kiểm tra quyền, tác động, phê duyệt và trách nhiệm nếu kết quả sai.

AI có thể hỗ trợ tạo nội dung; việc đưa nội dung đó vào quyết định hoặc phát hành vẫn là lựa chọn của con người/tổ chức. Không chuyển trách nhiệm sang công cụ.

## 4. Bài tập vận dụng có lời giải mẫu

**Tình huống:** Bạn cần viết báo cáo học thuật từ năm bài nghiên cứu PDF. Báo cáo có thể ảnh hưởng đến đề xuất của khoa.

1. **Delegation:** Giao cho AI trích xuất câu hỏi nghiên cứu, phương pháp và phát hiện để lập bảng so sánh; giữ việc xác định chất lượng bằng chứng và khuyến nghị cuối cùng cho người nghiên cứu. Kiểm tra khả năng đọc PDF và chính sách dữ liệu.
2. **Description:** Nêu đối tượng độc giả, cấu trúc bảng, yêu cầu trích trang, quy tắc “không tìm thấy thì ghi chưa xác định”, và yêu cầu tách kết quả tác giả nêu với suy luận của người viết.
3. **Discernment:** Đối chiếu trích dẫn, số liệu và diễn giải với PDF gốc; tìm phản ví dụ; kiểm tra xem năm bài có bị khái quát quá mức không.
4. **Diligence:** Không tải tài liệu hạn chế lên dịch vụ nếu chưa được phép; lưu lại nguồn; công khai cách AI hỗ trợ theo quy định của khoa; người nghiên cứu duyệt bản cuối.

**Tiêu chí chấm:** Câu trả lời tốt nêu cả phần giao và phần giữ lại, tiêu chí đầu ra kiểm tra được, cách kiểm chứng bằng nguồn gốc, và rủi ro/biện pháp trách nhiệm.

## 5. Câu hỏi ôn tập

1. **AI Fluency khác việc biết nhiều prompt ở điểm nào?** Đây là năng lực cộng tác toàn diện: quyết định giao việc, mô tả, đánh giá và chịu trách nhiệm; prompt chỉ là một phần.
2. **Automation và Agency khác nhau thế nào?** Automation thực hiện một nhiệm vụ xác định; Agency có mức chủ động cao hơn trong chuỗi công việc nhưng vẫn trong mục tiêu/quyền do con người đặt.
3. **Nêu ba thành phần của Delegation.** Nhận thức vấn đề, nhận thức nền tảng, phân công nhiệm vụ.
4. **Phân biệt Product, Process và Performance Description.** Sản phẩm cần tạo; cách làm; cách tương tác.
5. **Một prompt dài có luôn tốt hơn prompt ngắn không?** Không. Tính liên quan, thứ tự và tiêu chí rõ quan trọng hơn số chữ.
6. **Discernment chỉ kiểm tra câu trả lời cuối không?** Không; còn xem quy trình và cách cộng tác.
7. **Mô tả vòng Description–Discernment.** Mô tả, đánh giá so với tiêu chí, phản hồi cụ thể, điều chỉnh và lặp đến khi đạt.
8. **Diligence có ba phần nào?** Cẩn trọng khi tạo, minh bạch và có trách nhiệm khi triển khai.
9. **Một dấu hiệu cần giữ quyết định ở con người?** Hậu quả sai cao, yêu cầu mơ hồ, cần thẩm quyền/chuyên môn hoặc không có cách kiểm chứng đủ tin cậy.
10. **Viết lại yêu cầu “tóm tắt báo cáo này” thành yêu cầu kiểm tra được.** Câu trả lời cần nêu người đọc, giới hạn độ dài, nội dung cần có, dữ liệu/nguồn được phép dùng và cách báo phần không rõ.

## 6. Những nhầm lẫn thường gặp

- 4D không phải bốn bước tuyến tính chỉ dùng một lần; chúng là các năng lực liên kết và có thể quay lại nhiều lần.
- Giao task cho AI không giao luôn quyền quyết định hay trách nhiệm.
- “Đóng vai chuyên gia” không bù được thiếu dữ liệu, nguồn hoặc tiêu chí.
- Đầu ra trôi chảy không chứng minh đúng; cần Discernment.
- Minh bạch không chỉ là ghi “có dùng AI”; phải tuân theo bối cảnh, chính sách và nhu cầu của người nhận.

## 7. Tự đánh giá trước kỳ thi

- [ ] Phân biệt Automation, Augmentation và Agency bằng tình huống mới.
- [ ] Áp dụng ba thành phần của Delegation và ba lớp Description.
- [ ] Viết một prompt có tiêu chí hoàn thành, nguồn và cách hỏi lại.
- [ ] Đánh giá đầu ra, quy trình và cách tương tác.
- [ ] Giải thích các quyết định về quyền riêng tư, minh bạch và triển khai.

## Nguồn tham khảo

- [Claude Academy — AI Fluency: Framework and Foundations](https://academy.claude.com/courses/ai-fluency-framework-foundations)
- [Claude Academy — AI Capabilities and Limitations (khóa bổ trợ)](https://academy.claude.com/courses/ai-capabilities-and-limitations)

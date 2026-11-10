# Đề cương ôn tập: Claude 101

> Tài liệu tự ôn trước khi kiểm tra kiến thức; không phải đề thi chính thức.  
> Ghi chú đầy đủ: [Claude 101](../claude-101-notes.md)

## Mục tiêu cần đạt

- Viết yêu cầu rõ và cải thiện kết quả qua nhiều lượt.
- Chọn Chat, Cowork hoặc Code theo công việc.
- Phân biệt Projects, Artifacts, Skills, Connectors, Enterprise Search và Research.
- Sử dụng Claude có phán đoán, bảo mật và trách nhiệm.

## Khung yêu cầu cơ bản

Một prompt vững thường nêu:

1. **Bối cảnh:** vai trò, mục tiêu, người đọc và thông tin nền.
2. **Nhiệm vụ:** hành động cụ thể cần Claude thực hiện.
3. **Đầu ra:** định dạng, độ dài, giọng điệu và tiêu chí hoàn thành.
4. **Ràng buộc:** nguồn được dùng, điều cần tránh, cách xử lý phần chưa rõ.

Làm việc theo vòng lặp: xem kết quả → chỉ ra phần cần sửa → bổ sung bối cảnh/tiêu chí → kiểm tra bản mới.

## Chọn hình thức làm việc

| Hình thức | Dùng khi | Tương tác |
| --- | --- | --- |
| **Chat** | Hỏi đáp, viết, phân tích hoặc brainstorm theo lượt | Trao đổi trực tiếp |
| **Cowork** | Giao một quy trình nhiều bước để Claude thực hiện | Claude lập kế hoạch/thực hiện trong workspace, rồi trả kết quả để duyệt |
| **Code** | Làm việc trực tiếp với repository | Khám phá, sửa file, chạy lệnh/test và kiểm tra diff |

## Phân biệt tính năng

| Tính năng | Vai trò |
| --- | --- |
| **Projects** | Giữ chỉ dẫn và tài liệu dùng lại cho một mảng việc |
| **Artifacts** | Sản phẩm đầu ra riêng để xem, sửa, chia sẻ hoặc phát triển tiếp |
| **Skills** | Quy trình/kiến thức tái sử dụng được gọi khi phù hợp |
| **Connectors** | Kết nối Claude với nguồn dữ liệu/công cụ có quyền truy cập |
| **Enterprise Search** | Tìm kiếm tri thức nội bộ có phân quyền |
| **Research** | Nghiên cứu nhiều bước, tổng hợp nguồn và cung cấp link dẫn chứng |

Ghi nhớ: **Projects lưu bối cảnh; Artifacts là sản phẩm; Skills mô tả quy trình.** Connectors kết nối nguồn; Enterprise Search tìm tri thức nội bộ; Research phù hợp khảo sát nhiều nguồn.

## Làm việc có trách nhiệm

- Cấp đúng quyền và chỉ kết nối nguồn cần thiết.
- Không đưa mật khẩu, token, API key hoặc dữ liệu nhạy cảm không cần thiết vào prompt.
- Kiểm tra nguồn và ngày cập nhật; AI có thể sai hoặc diễn giải quá mức.
- Rà soát kết quả trước khi gửi, chia sẻ hoặc dùng để ra quyết định.
- Minh bạch về vai trò AI khi phù hợp; con người vẫn chịu trách nhiệm triển khai.

## Điểm dễ nhầm

- Project Instructions là hướng dẫn theo workspace; Skill là quy trình dùng lại.
- Artifact là sản phẩm đứng riêng, không chỉ là tên khác của câu trả lời Chat.
- Connector chỉ cho Claude truy cập dữ liệu theo quyền được cấp.
- Giao task cho Cowork không có nghĩa là bỏ qua việc duyệt kết quả.

## Tự kiểm tra

1. **Prompt tốt cần gì?** Bối cảnh, nhiệm vụ, đầu ra và ràng buộc.
2. **Khi nào chọn Cowork thay vì Chat?** Khi muốn giao quy trình nhiều bước thay vì trao đổi từng bước.
3. **Projects, Artifacts, Skills khác nhau thế nào?** Bối cảnh, sản phẩm và quy trình tái sử dụng.
4. **Khi nào dùng Enterprise Search?** Khi câu trả lời phụ thuộc tri thức nội bộ đã kết nối và có quyền truy cập.
5. **Research khác câu hỏi thường ở đâu?** Research khảo sát nhiều nguồn và trả kết quả có dẫn chứng.
6. **Vì sao cần kiểm tra lại?** Claude có thể sai, dùng nguồn cũ hoặc bỏ sót dữ liệu.
7. **Nêu hai việc trước khi kết nối dữ liệu nội bộ.** Kiểm tra quyền/phạm vi và tuân thủ chính sách tổ chức.
8. **Khi nào lập trình viên nên dùng Code?** Khi task cần thao tác với codebase, chạy lệnh hoặc kiểm tra test/diff.

## Checklist trước khi thi

- [ ] Viết prompt gồm bối cảnh, nhiệm vụ, đầu ra và ràng buộc.
- [ ] Chọn Chat/Cowork/Code đúng tình huống.
- [ ] Phân biệt sáu tính năng trong bảng.
- [ ] Nhớ quyền, nguồn và kiểm chứng là trách nhiệm người dùng.

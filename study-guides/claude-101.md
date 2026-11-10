# Tài liệu ôn thi: Claude 101

> Bản ôn tập đầy đủ về sử dụng Claude trong công việc hằng ngày; không phải đề thi chính thức. Đối chiếu với [ghi chú khóa học](../claude-101-notes.md) và [trang Claude Academy](https://academy.claude.com/courses/claude-101). Giao diện, quyền truy cập và tên tính năng có thể thay đổi theo thời điểm, gói tài khoản và cấu hình tổ chức.

## 1. Mục tiêu học tập

Bạn cần biết cách bắt đầu cuộc trò chuyện, cải thiện kết quả qua nhiều lượt, chọn đúng hình thức làm việc, sử dụng ngữ cảnh/tài liệu dùng lại và phân biệt các tính năng chính. Xuyên suốt khóa học là nguyên tắc: Claude hỗ trợ công việc; người dùng xác định mục tiêu, cung cấp thông tin phù hợp và kiểm tra kết quả.

## 2. Viết yêu cầu và cộng tác qua nhiều lượt

### Khung prompt

Một yêu cầu thường hữu ích khi có các phần sau:

1. **Bối cảnh:** mục tiêu, người đọc, tình huống và dữ liệu liên quan.
2. **Nhiệm vụ:** hành động cụ thể cần thực hiện.
3. **Đầu ra:** cấu trúc, định dạng, độ dài và tiêu chí chấp nhận.
4. **Ràng buộc:** nguồn được dùng, điều cần tránh, thời hạn/độ chi tiết và cách xử lý điều chưa rõ.

Không phải nhiệm vụ nào cũng cần đủ bốn phần ở mức chi tiết. Với việc đơn giản, prompt ngắn có thể đủ. Với đầu ra quan trọng, hãy làm cho chất lượng có thể kiểm tra.

**Yêu cầu ban đầu:** “Soạn email cập nhật dự án.”

**Yêu cầu cải thiện:** “Soạn email tiếng Việt dưới 180 từ cho nhóm dự án. Dùng các ghi chú đính kèm, nêu tiến độ đã hoàn thành, một rủi ro còn mở, người phụ trách và việc cần làm tuần tới. Không tự gán ngày/người nếu ghi chú không nói; đánh dấu thông tin còn thiếu bằng câu hỏi.”

### Vòng lặp cải thiện

1. Xem đầu ra theo từng tiêu chí.
2. Chỉ rõ phần đạt/chưa đạt và bằng chứng.
3. Bổ sung dữ liệu, ví dụ hoặc ưu tiên còn thiếu.
4. Yêu cầu sửa đúng phần cần sửa.
5. Đọc lại bản cuối và kiểm tra dữ kiện.

Phản hồi “viết hay hơn” không nói rõ cách sửa; tốt hơn là “giữ nguyên số liệu, giảm phần mở đầu còn hai câu, thêm mục rủi ro và không suy luận nguyên nhân”.

## 3. Chọn cách làm việc: Chat, Cowork, Code

| Hình thức | Khi phù hợp | Cách người dùng tham gia |
| --- | --- | --- |
| **Chat** | Hỏi đáp, phân tích, soạn thảo, brainstorm qua trao đổi từng lượt | Cung cấp bối cảnh, xem và điều chỉnh đầu ra |
| **Cowork** | Giao quy trình nhiều bước cần làm trên workspace/tài liệu | Định nghĩa mục tiêu và quyền, xem kế hoạch/kết quả, duyệt sản phẩm |
| **Code** | Công việc cần đọc hoặc thay đổi phần mềm/repository | Nêu tiêu chí, cho phép đúng phạm vi, xem diff và kiểm tra chạy |

Chọn theo **công việc** chứ không theo sở thích tên sản phẩm. Nếu chỉ cần thảo luận cách giải quyết, Chat đủ dùng. Nếu cần giao chuỗi việc trên tệp, Cowork có thể phù hợp. Nếu cần sửa code và chạy kiểm tra, Code/Claude Code phù hợp hơn. Tính năng cụ thể có thể yêu cầu gói hoặc quyền do tổ chức bật.

## 4. Các tính năng và ranh giới giữa chúng

| Tính năng | Vai trò | Ví dụ sử dụng | Điều cần kiểm tra |
| --- | --- | --- | --- |
| **Projects** | Gom chỉ dẫn và tài liệu cho một không gian/công việc lâu dài | Dự án nghiên cứu có thuật ngữ, rubric và nguồn nền | Tài liệu còn mới không; chỉ đưa phần phù hợp |
| **Artifacts** | Tạo đầu ra riêng có thể xem, chỉnh sửa, chia sẻ hoặc tiếp tục phát triển | Bản trình bày, biểu đồ, prototype hoặc nội dung có cấu trúc | Dữ liệu, liên kết, khả năng dùng lại và quyền chia sẻ |
| **Skills** | Hướng dẫn/quy trình tái sử dụng được dùng khi phù hợp với tác vụ | Quy trình chuẩn để biên tập báo cáo | Phạm vi, hướng dẫn và quyền cần thiết |
| **Connectors** | Kết nối nguồn dữ liệu/công cụ ngoài theo quyền được cấp | Tìm hoặc đọc dữ liệu từ dịch vụ được kết nối | Nguồn nào được kết nối, quyền truy cập và dữ liệu nào được gửi |
| **Enterprise Search** | Tìm trong tri thức nội bộ được kết nối, theo quyền người dùng/tổ chức | Tìm chính sách hoặc tài liệu nội bộ | Quyền, nguồn và trích dẫn phải đúng tài liệu |
| **Research** | Thực hiện nghiên cứu nhiều bước trên nguồn, tổng hợp và cung cấp dẫn chứng | So sánh quy định công khai từ nhiều nguồn | Chất lượng/độ mới của nguồn và việc citation có hỗ trợ khẳng định |

Ghi nhớ: **Project giữ bối cảnh; Artifact là sản phẩm; Skill là quy trình; Connector là kết nối; Search/Research là các cách tìm và tổng hợp thông tin.** Cách gọi hoặc vị trí trong giao diện có thể thay đổi. Không đồng nhất Skill trong Claude app với tệp cấu hình Claude Code nếu đề bài đang hỏi về một sản phẩm cụ thể.

## 5. Bài tập tổng hợp có lời giải mẫu

**Tình huống:** Bạn cần chuẩn bị báo cáo quý từ tài liệu dự án, dữ liệu bảng tính và thông tin mới trên web.

1. Chọn **Project** nếu có bối cảnh, rubric và nguồn dự án dùng lặp lại.
2. Dùng **Connector/Enterprise Search** nếu dữ liệu nội bộ có thể truy cập an toàn và quyền đã được cấp; dùng **Research** cho câu hỏi cần khảo sát nguồn web nhiều bước.
3. Viết yêu cầu nêu đối tượng, khoảng thời gian, ba chỉ số, cấu trúc bảng, nguồn được phép và quy tắc ghi “không tìm thấy”.
4. Có thể yêu cầu **Artifact** nếu cần đầu ra độc lập để trình bày/chỉnh sửa.
5. Đối chiếu số liệu với bảng tính; mở nguồn web để xác thực ngày/khẳng định; kiểm tra quyền chia sẻ trước khi gửi.

**Đáp án tốt** giải thích được vì sao chọn từng tính năng và không khẳng định rằng connector/search tự bảo đảm kết quả đúng.

## 6. Làm việc có trách nhiệm

- Không nhập mật khẩu, API key, dữ liệu cá nhân hoặc tài liệu hạn chế nếu không cần/không được phép.
- Chỉ kết nối nguồn cần cho task và hiểu phạm vi truy cập.
- Kiểm tra thông tin mới bằng nguồn có thẩm quyền và ngày cập nhật.
- Xem lại số liệu, kết luận, liên kết, định dạng và đối tượng nhận trước khi chia sẻ.
- Tuân theo quy tắc về minh bạch/AI trong học tập, công việc và tổ chức.
- Giữ trách nhiệm cuối cùng về quyết định và sản phẩm được phát hành.

## 7. Câu hỏi ôn tập

1. **Một prompt có bốn phần nền tảng nào?** Bối cảnh, nhiệm vụ, đầu ra và ràng buộc.
2. **Vì sao nên làm việc qua nhiều lượt?** Lượt sau có thể dùng đánh giá cụ thể để cải thiện kết quả thay vì chấp nhận bản đầu.
3. **Chat, Cowork và Code khác nhau theo tiêu chí nào?** Hình thức công việc/tương tác: trao đổi, giao quy trình trên workspace, hoặc làm trực tiếp với codebase.
4. **Project có phải sản phẩm cuối không?** Không; Project lưu bối cảnh/hướng dẫn, còn Artifact là một đầu ra riêng.
5. **Skill khác Connector thế nào?** Skill hướng dẫn quy trình; Connector mở kết nối đến nguồn/công cụ.
6. **Khi nào Enterprise Search hữu ích?** Khi câu trả lời phụ thuộc tri thức nội bộ đã kết nối, trong phạm vi quyền được cấp.
7. **Research khác hỏi một câu thông thường thế nào?** Research phù hợp khảo sát nhiều nguồn, tổng hợp và cung cấp dẫn chứng; vẫn phải kiểm tra nguồn.
8. **Claude có thể luôn truy cập mọi tài liệu nội bộ không?** Không; quyền phụ thuộc kết nối, tài khoản và cấu hình.
9. **Nêu ba bước kiểm tra trước khi chia sẻ Artifact/báo cáo.** Xác minh dữ kiện, rà người nhận/quyền, kiểm tra bản hiển thị/liên kết.
10. **Nếu Claude tạo trích dẫn nhưng link không hỗ trợ nội dung thì làm gì?** Đối chiếu nguồn gốc, sửa hoặc bỏ khẳng định; không coi citation là bằng chứng tự thân.

## 8. Nhầm lẫn cần tránh

- Mọi câu trả lời Claude không tự động là Artifact.
- Project không làm mọi file trở thành nguồn sự thật; file cũ/sai vẫn có thể ảnh hưởng kết quả.
- Connector tạo đường truy cập, không thay người dùng quyết định quyền và phạm vi.
- Có citation không có nghĩa kết luận chắc chắn đúng.
- Cowork/Research và các tính năng khác có thể khác theo gói, khu vực hoặc chính sách quản trị.

## 9. Tự đánh giá trước kỳ thi

- [ ] Viết prompt đáp ứng một tác vụ thực tế với đầu ra và tiêu chí rõ.
- [ ] Chọn Chat/Cowork/Code và giải thích lựa chọn.
- [ ] Phân biệt Projects, Artifacts, Skills, Connectors, Search và Research.
- [ ] Nhận diện dữ liệu/quyền cần quản lý.
- [ ] Kiểm tra kết quả trước khi dùng hoặc chia sẻ.

## Nguồn tham khảo

- [Claude Academy — Claude 101](https://academy.claude.com/courses/claude-101)
- [Claude Academy — AI Fluency: Framework and Foundations](https://academy.claude.com/courses/ai-fluency-framework-foundations)
- [Claude — trang sản phẩm chính thức](https://www.anthropic.com/claude)

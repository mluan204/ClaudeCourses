# Tài liệu ôn thi: AI Capabilities and Limitations

> Bản ôn tập đầy đủ để hiểu, chẩn đoán và xử lý giới hạn của mô hình ngôn ngữ lớn (LLM); không phải đề thi chính thức. Xem thêm [ghi chú khóa học](../ai-capabilities-and-limitations-notes.md) và [trang Claude Academy](https://academy.claude.com/courses/ai-capabilities-and-limitations).

## 1. Kết quả cần đạt

Bạn cần phân biệt AI tạo sinh với các loại AI khác; giải thích hai giai đoạn pre-training/fine-tuning; mô tả bốn thuộc tính **Next Token Prediction, Knowledge, Working Memory, Steerability**; nhận diện khi thuộc tính nào đang giới hạn kết quả; và chọn biện pháp khắc phục phù hợp thay vì chỉ gửi lại prompt chung chung.

Bốn thuộc tính là các **phổ năng lực–giới hạn**, không phải công tắc bật/tắt. Một tác vụ có thể đòi hỏi dự đoán ngôn ngữ tốt, thông tin mô hình không biết, tài liệu dài quá sức chứa hiệu dụng và hướng dẫn nhiều ràng buộc cùng lúc. Vì thế, một lỗi thường do nhiều thuộc tính tương tác.

## 2. Nền tảng: mô hình được huấn luyện và tạo câu trả lời ra sao?

“AI” bao gồm nhiều họ hệ thống, chẳng hạn phân loại, dự đoán và tạo sinh. LLM tạo văn bản dựa trên ngữ cảnh hiện có; nó không mặc nhiên là cơ sở dữ liệu được tra cứu mỗi khi trả lời.

| Loại tác vụ/hệ thống | Kết quả điển hình | Ví dụ |
| --- | --- | --- |
| **Classification** | Gán lớp/nhãn cho đầu vào | Phân loại email là spam hay không |
| **Prediction** | Ước tính giá trị/sự kiện mục tiêu | Dự báo nhu cầu tuần tới |
| **Generative AI** | Tạo nội dung mới dựa trên mẫu/điều kiện | Viết bản tóm tắt hoặc tạo hình ảnh |

Ranh giới thực tế có thể chồng lấp; điểm cần nhớ là khả năng tạo văn bản không đồng nghĩa hệ thống được tối ưu hoặc có dữ liệu để xác minh mọi dự đoán.

Hai giai đoạn huấn luyện cần nhớ:

1. **Pre-training:** mô hình học các mẫu từ dữ liệu huấn luyện, gồm ngôn ngữ, quan hệ và kiến thức có trong dữ liệu đó.
2. **Fine-tuning:** điều chỉnh mô hình để phản hồi theo chỉ dẫn và tạo cách ứng xử giống trợ lý hơn.

Quá trình này giải thích một số “dấu vân tay” hành vi: mô hình có thể chiều theo người hỏi (**sycophancy**), dài dòng (**verbosity**), quá thận trọng (**over-caution**) hoặc thể hiện mức tự tin không khớp độ chắc chắn. Đây là xu hướng có thể xuất hiện, không phải hành vi bắt buộc trong mọi câu trả lời.

### Next Token Prediction

Mô hình chia đầu vào/đầu ra thành **token** rồi ước tính token nào có khả năng tiếp theo dựa trên chuỗi đã có. Nó lặp lại thao tác đó để tạo câu hoàn chỉnh. Cơ chế này làm tốt các mẫu quen thuộc như viết lại, tóm tắt, hoàn thiện định dạng; nó không tự xác minh rằng mọi khẳng định là đúng.

**Hallucination** là nội dung sai, bịa hoặc không có căn cứ nhưng được trình bày như thể thật. Nó dễ xảy ra khi yêu cầu đòi chi tiết hiếm, nguồn không có sẵn, người dùng ép trả lời dù thiếu dữ liệu, hoặc mô hình nối một mẫu quen thuộc với một phần không biết. Yêu cầu “hãy chắc chắn” không thay thế nguồn hay kiểm chứng.

### Knowledge

Kiến thức phụ thuộc vào nội dung, độ phổ biến, thời điểm, chất lượng và sự nhất quán của dữ liệu huấn luyện. Chủ đề phổ biến, ổn định thường có độ bao phủ tốt hơn lĩnh vực ngách, mới, tranh luận hoặc ít tài liệu. “Knowledge cutoff” nhắc ta rằng thông tin sau một mốc có thể vắng mặt; kể cả thông tin trước mốc đó cũng có thể không được biết hoặc nhớ chính xác.

Khi cần dữ kiện mới hoặc dẫn chứng, đưa tài liệu gốc vào context, dùng tìm kiếm/retrieval/tool phù hợp, rồi kiểm tra nội dung được truy xuất. Có nguồn không có nghĩa là nguồn đã được đọc đúng; citation cũng cần đối chiếu với trang hoặc đoạn gốc.

### Working Memory: context window

**Context window** là lượng token có thể được xử lý trong một lượt/phiên theo khả năng của hệ thống. Đây là giới hạn kỹ thuật và không đồng nghĩa với trí nhớ dài hạn giữa các phiên. Dù nội dung nằm trong cửa sổ, mô hình vẫn có thể không chú ý đồng đều đến mọi chi tiết — đặc biệt trong tài liệu dài, nhiều chủ đề hoặc có tiêu chí nằm sâu giữa phần văn bản.

Chiến lược quản lý:

- Đưa mục tiêu, ràng buộc và định nghĩa quan trọng vào phần dễ thấy; nhắc lại tiêu chí quyết định gần câu hỏi cuối.
- Chia tài liệu lớn thành phần; yêu cầu trích bằng chứng theo từng phần trước khi tổng hợp.
- Dùng cấu trúc/nhãn cho nhiều tài liệu và chỉ cung cấp nguồn liên quan.
- Tóm tắt trung gian, nhưng kiểm tra bản tóm tắt để tránh làm rơi chi tiết quan trọng.
- Với phiên mới, cung cấp lại thông tin bắt buộc; đừng mặc định hệ thống nhớ phiên trước.

### Steerability

**Steerability** là mức độ mô hình làm theo chỉ dẫn. Yêu cầu cụ thể, ngắn, có thứ tự ưu tiên và tiêu chí kiểm tra thường dễ tuân thủ hơn yêu cầu trừu tượng hoặc mâu thuẫn. Dù prompt tốt cải thiện khả năng điều khiển, nó không bảo đảm hoàn hảo.

- **Instruction drift:** mô hình dần bỏ hoặc làm yếu một ràng buộc qua lượt dài hay chuỗi bước.
- **Letter vs. spirit:** làm đúng câu chữ nhưng bỏ lỡ mục đích thật.
- **Sai định dạng:** yêu cầu định dạng không được nêu hoặc kiểm tra đủ cụ thể.
- **Xung đột chỉ dẫn:** hai yêu cầu cùng cấp đòi kết quả trái nhau.

Khắc phục bằng cách ưu tiên hóa yêu cầu, tách điều kiện bắt buộc khỏi tùy chọn, đưa ví dụ hợp lệ/không hợp lệ, dùng schema hoặc validator và yêu cầu mô hình nêu giả định/điểm chưa rõ.

## 3. Chẩn đoán lỗi theo triệu chứng

1. **Ghi nhận điều quan sát được:** thông tin sai, thiếu, cũ, bỏ sót ràng buộc, sai định dạng hay trả quá tự tin?
2. **Đặt giả thuyết về thuộc tính:** prediction, knowledge, working memory, steerability — có thể nhiều hơn một.
3. **Chọn biện pháp đúng loại:** thêm nguồn cho thiếu knowledge; chia nhỏ cho vấn đề context; viết tiêu chí rõ cho steerability; yêu cầu xác minh cho chi tiết có nguy cơ bịa.
4. **Tạo điều kiện kiểm tra:** trích trang, nêu giả định, xuất theo schema, đối chiếu nguồn hoặc chạy kiểm thử.
5. **Xác minh độc lập:** kiểm tra sự kiện quan trọng với tài liệu gốc hoặc phép thử khác.

| Triệu chứng | Thuộc tính có thể liên quan | Hỗ trợ phù hợp |
| --- | --- | --- |
| Bịa nguồn/tên bài báo | Prediction, Knowledge | Đưa nguồn; xác minh từng trích dẫn; không tìm thấy thì ghi rõ |
| Dùng giá/chính sách cũ | Knowledge/cutoff | Tìm nguồn chính thức mới, kiểm tra ngày hiệu lực |
| Bỏ yêu cầu nằm trong tài liệu dài | Working Memory | Chunk, checklist, trích bằng chứng theo mục |
| Trả sai format | Steerability | Schema/mẫu đầu ra, tiêu chí kiểm tra tự động |
| Đồng ý với tiền đề sai | Sycophancy/Steerability | Yêu cầu kiểm tra tiền đề, đưa lập luận phản biện và bằng chứng |

## 4. Bài tập tình huống có đáp án mẫu

**Tình huống:** Bạn hỏi mô hình tóm tắt quy định mới ban hành dựa trên tập PDF 300 trang. Nó nêu một mốc thời gian không tìm thấy trong PDF và bỏ sót ngoại lệ ở cuối tài liệu.

- Mốc thời gian có thể là **Knowledge/Next Token Prediction**: mô hình lấp chỗ trống bằng thông tin có vẻ hợp lý. Yêu cầu nguồn/trang, kiểm tra văn bản chính thức cập nhật, và không chấp nhận ngày không có căn cứ.
- Ngoại lệ bị bỏ sót có thể là **Working Memory**: đầu vào dài và phần quan trọng nằm xa câu hỏi. Chia theo chương, trích ngoại lệ theo checklist rồi tổng hợp có dẫn vị trí.
- Nếu prompt chỉ nói “tóm tắt”, **Steerability** cũng liên quan: nêu rõ phải tìm ngoại lệ, ngày hiệu lực, đối tượng áp dụng và mục “không xác định”.
- Đầu ra trôi chảy không chứng minh chính xác; kiểm tra lại toàn bộ phát hiện quan trọng với PDF/văn bản gốc.

**Bài tập tự làm:** Tạo một prompt mới yêu cầu tóm tắt quy định, gồm phạm vi tài liệu, các trường bắt buộc, cách dẫn nguồn, cách báo thiếu dữ liệu và giới hạn suy luận. Đáp án tốt có thể kiểm tra từng trường, không buộc mô hình đoán.

## 5. Câu hỏi ôn tập

1. **Vì sao LLM có thể viết trôi chảy nhưng sai?** Nó tạo chuỗi có khả năng tiếp theo; độ trôi chảy không tự kiểm tra độ đúng.
2. **Pre-training và fine-tuning đóng vai trò gì?** Học mẫu/kiến thức nền; sau đó điều chỉnh hành vi làm theo chỉ dẫn.
3. **Hallucination nghĩa là gì?** Nội dung sai hoặc thiếu căn cứ được trình bày như thật.
4. **Knowledge khác Working Memory thế nào?** Knowledge là những gì mô hình có thể biết từ huấn luyện/nguồn; Working Memory là những gì đang có trong context xử lý hiện tại.
5. **Context window có đồng nghĩa với trí nhớ hoàn hảo không?** Không; nó giới hạn lượng token, còn việc chú ý/khôi phục chi tiết vẫn có thể sai.
6. **Khi nào nên dùng web/retrieval?** Khi thông tin cần cập nhật, hẹp, cần chứng cứ hoặc nằm trong kho tài liệu bên ngoài.
7. **Thế nào là instruction drift?** Ràng buộc bị bỏ hoặc suy yếu qua lượt dài hay nhiều bước.
8. **Prompt có thể loại bỏ hallucination hoàn toàn không?** Không; prompt giúp định hướng, nguồn và kiểm chứng giảm rủi ro nhưng không tạo bảo đảm tuyệt đối.
9. **Phân biệt sycophancy và over-caution.** Một bên đồng thuận/chiều theo quá mức; bên kia dè dặt hoặc từ chối quá mức.
10. **Mô hình bỏ điều kiện giữa báo cáo dài — bước đầu nên làm gì?** Xác định bị mất ở đâu, sau đó tách nội dung và dùng checklist/trích bằng chứng; không chỉ lặp lại câu hỏi y nguyên.

## 6. Nhầm lẫn cần tránh

- Context window lớn hơn không biến mô hình thành kho dữ liệu luôn nhớ mọi thứ.
- Retrieval có thể đưa bằng chứng vào ngữ cảnh nhưng không đảm bảo truy xuất đúng hay diễn giải đúng.
- Knowledge cutoff không phải ranh giới duy nhất của kiến thức; dữ liệu cũ vẫn có thể vắng mặt hoặc sai.
- Temperature/độ ngẫu nhiên (nếu cấu hình có) không phải thước đo sự thật.
- Một lỗi có thể có nhiều nguyên nhân; chẩn đoán nên dựa trên bằng chứng và thử cách sửa tương ứng.

## 7. Tự đánh giá trước kỳ thi

- [ ] Giải thích bốn thuộc tính và cho một ví dụ giới hạn của mỗi thuộc tính.
- [ ] Phân biệt generative AI với classification/prediction AI ở mức khái niệm.
- [ ] Nêu được cách pre-training và fine-tuning định hình năng lực/hành vi.
- [ ] Chẩn đoán được một tình huống có nhiều thuộc tính va chạm.
- [ ] Chọn biện pháp dựa trên loại lỗi và nêu cách kiểm chứng hiệu quả.

## Nguồn tham khảo

- [Claude Academy — AI Capabilities and Limitations](https://academy.claude.com/courses/ai-capabilities-and-limitations)
- [Claude Academy — AI Fluency: Framework and Foundations](https://academy.claude.com/courses/ai-fluency-framework-foundations)

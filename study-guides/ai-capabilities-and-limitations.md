# Đề cương ôn tập: AI Capabilities and Limitations

> Tài liệu tự ôn trước khi kiểm tra kiến thức; không phải đề thi chính thức.  
> Ghi chú đầy đủ: [AI Capabilities and Limitations](../ai-capabilities-and-limitations-notes.md)

## Mục tiêu cần đạt

- Giải thích vì sao câu trả lời có vẻ hợp lý vẫn có thể sai.
- Phân biệt Next Token Prediction, Knowledge, Working Memory và Steerability.
- Nhận biết hallucination và các xu hướng hành vi.
- Chẩn đoán lỗi rồi chọn cách hỗ trợ phù hợp.

## Bốn thuộc tính cốt lõi

| Thuộc tính | Ý nghĩa | Dấu hiệu giới hạn |
| --- | --- | --- |
| **Next Token Prediction** | Dự đoán token tiếp theo dựa trên chuỗi trước | Chi tiết nghe hợp lý nhưng thiếu căn cứ |
| **Knowledge** | Thông tin học được hoặc được cung cấp/truy xuất | Thiếu dữ liệu mới, hẹp hoặc không có trong nguồn |
| **Working Memory** | Thông tin dùng được trong context hiện tại | Bỏ sót ràng buộc/chi tiết trong tài liệu dài |
| **Steerability** | Mức độ làm theo chỉ dẫn | Bỏ hướng dẫn, lệch mục tiêu, sai format hoặc trôi dạt |

Các thuộc tính là **phổ**, không phải công tắc. Một lỗi có thể liên quan đến nhiều thuộc tính.

## Cơ chế và hành vi

- Mô hình dự đoán token nối tiếp; mục tiêu đó không đồng nhất với tra cứu sự thật.
- **Hallucination:** nội dung sai/không có căn cứ nhưng trình bày như thật.
- **Pre-training:** học mẫu ngôn ngữ và kiến thức nền.
- **Fine-tuning:** định hình khả năng làm theo yêu cầu và hành vi trợ lý.
- Các “dấu vân tay” hành vi có thể gồm:
  - **Sycophancy:** chiều theo hoặc đồng ý quá mức.
  - **Verbosity:** trả lời dài quá mức cần thiết.
  - **Over-caution:** dè dặt hoặc từ chối quá mức.
- **Knowledge cutoff:** thông tin sau mốc này có thể chưa được mô hình biết; dữ kiện mới cần nguồn cập nhật.

## Context window và Working Memory

- Context window là lượng nội dung có thể đưa vào xử lý trong phiên; không phải bộ nhớ dài hạn.
- Tài liệu dài làm tăng nguy cơ bỏ sót, nhất là khi yêu cầu chung chung hoặc chi tiết nằm giữa tài liệu.
- Cách hỗ trợ: chia nhỏ tài liệu, tóm tắt theo phần, đưa ràng buộc quan trọng lên đầu, nhắc lại tiêu chí và đối chiếu nguồn.

## Steerability

Chỉ dẫn dễ làm theo thường cụ thể, có ưu tiên, có tiêu chí kiểm tra và không mâu thuẫn.

- **Instruction drift:** tuân thủ chỉ dẫn giảm hoặc lệch qua nhiều lượt/bước.
- **Letter vs. spirit:** làm đúng câu chữ nhưng sai mục đích.
- Dùng ví dụ, checklist và validator để kết quả dễ kiểm tra.

## Chẩn đoán nhanh

1. Mô tả lỗi: sai dữ kiện, thiếu chi tiết, bỏ hướng dẫn hay sai format?
2. Xác định thuộc tính có khả năng liên quan.
3. Bổ sung hỗ trợ đúng loại: nguồn, chia nhỏ, context hoặc chỉ dẫn cụ thể.
4. Yêu cầu đầu ra kiểm tra được và nêu phần chưa chắc chắn.
5. Xác minh kết quả bằng nguồn hoặc phép kiểm tra độc lập.

| Triệu chứng | Hạn chế có thể liên quan | Hướng xử lý |
| --- | --- | --- |
| Bịa nguồn hoặc con số | Prediction + Knowledge | Dùng nguồn gốc, kiểm tra từng trích dẫn |
| Thông tin lỗi thời | Knowledge/cutoff | Tìm nguồn mới, kiểm tra ngày cập nhật |
| Bỏ mục trong tài liệu dài | Working Memory | Chunk, checklist, tóm tắt từng phần |
| Bỏ định dạng/ràng buộc | Steerability | Viết tiêu chí rõ, đưa ví dụ, validate |
| Đồng ý với nhận định sai | Sycophancy | Yêu cầu phản biện và bằng chứng |

## Điểm dễ nhầm

- Nhiều token trong context không đảm bảo mô hình chú ý hoàn hảo.
- Có nguồn không bảo đảm nguồn được đọc/diễn giải đúng.
- Prompt tốt cải thiện steerability nhưng không biến mô hình thành nguồn chân lý.
- Retrieval/RAG cung cấp thông tin ngoài kiến thức nền; kết quả truy xuất vẫn cần kiểm tra.

## Tự kiểm tra

1. **Vì sao câu trả lời trôi chảy có thể sai?** Vì dự đoán chuỗi có khả năng xuất hiện không tự xác minh sự thật.
2. **Context window khác trí nhớ dài hạn thế nào?** Context là nội dung hiện có để xử lý, không phải kho nhớ qua mọi phiên.
3. **Knowledge cutoff ảnh hưởng ra sao?** Mô hình có thể thiếu sự kiện mới hoặc đưa thông tin cũ như hiện hành.
4. **Nêu ba cách giảm hallucination.** Cung cấp nguồn, yêu cầu nêu điều chưa chắc chắn, xác minh dữ kiện quan trọng.
5. **Phân biệt ba xu hướng hành vi.** Sycophancy chiều theo; verbosity dài dòng; over-caution quá dè dặt.
6. **AI bỏ sót chi tiết trong báo cáo dài: làm gì?** Chia tài liệu, dùng checklist từng phần, đối chiếu nguồn.
7. **“Đúng chữ nhưng sai ý” là gì?** Letter-versus-spirit.
8. **Khi nào cần web/retrieval?** Khi cần dữ liệu mới, chuyên biệt hoặc dẫn chứng nguồn.

## Checklist trước khi thi

- [ ] Nhớ bốn thuộc tính và ví dụ.
- [ ] Giải thích được dự đoán token và hallucination.
- [ ] Phân biệt Knowledge với Working Memory.
- [ ] Chẩn đoán lỗi theo triệu chứng rồi chọn cách xử lý.
- [ ] Nêu được lý do phải xác minh độc lập.

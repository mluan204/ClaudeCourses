# Tài liệu ôn thi: Building with the Claude API

> Bản ôn tập đầy đủ cho kỹ sư xây ứng dụng với Claude API; không phải đề thi chính thức. Đối chiếu [ghi chú khóa học](../building-with-the-claude-api-notes.md), [trang Claude Academy](https://academy.claude.com/courses/building-with-the-claude-api) và tài liệu API chính thức bên dưới. API, model, SDK và tham số thay đổi theo thời gian; kiểm tra docs hiện hành trước khi triển khai.

## 1. Mục tiêu học tập

Bạn cần hiểu Messages API và hội thoại nhiều lượt; viết prompt rõ; đánh giá prompt bằng evals; sử dụng tool calling đúng vòng lặp; thiết kế retrieval/RAG; phân biệt workflow với agent; kết nối hệ thống qua MCP; và thêm controls về bảo mật, chất lượng, chi phí, độ trễ và lỗi.

## 2. Messages API và quản lý hội thoại

Thông thường ứng dụng gửi yêu cầu từ **backend** đến Messages API, gồm model được hỗ trợ, giới hạn output, system instructions và danh sách messages. Assistant trả các content blocks; ứng dụng quyết định cách hiển thị hoặc xử lý tiếp.

- System instructions nêu vai trò, policy và cách xử lý ở cấp ứng dụng; user message chứa yêu cầu/turn cụ thể.
- API hội thoại thường **stateless**: ứng dụng giữ lịch sử cần thiết và gửi lại khi muốn duy trì nhiều lượt. Không gửi vô hạn; chọn/tóm tắt phần còn hữu ích.
- API key phải nằm server-side/secret manager, không nhúng trong browser/mobile client hoặc log.
- **Streaming** phát response theo từng phần để người dùng thấy output sớm hơn. Client vẫn cần xử lý event kết thúc, tool use, lỗi mạng, hủy request và phần response chưa hoàn tất.
- Tham số tạo sinh như temperature chỉ kiểm soát khía cạnh lấy mẫu ở những model hỗ trợ; không đo độ đúng và không phải cách thay thế eval.
- Structured output/schema (khi endpoint/model hỗ trợ) giúp chuẩn hóa cấu trúc. Luôn kiểm tra schema và business rules ở ứng dụng.

Ví dụ prompt phân vùng nội dung bằng XML:

```text
<task>Trích ba rủi ro chính trong tài liệu.</task>
<source_document>...nội dung được cung cấp...</source_document>
<output_format>JSON theo schema đã chỉ định.</output_format>
<constraints>Chỉ dùng tài liệu; nếu không có bằng chứng, ghi unknown.</constraints>
```

Tag giúp phân biệt phần việc và phần dữ liệu, nhất là với prompt nhiều tài liệu; bản thân XML không bảo vệ ứng dụng khỏi prompt injection hay lỗi logic.

## 3. Prompt engineering và evaluation

Prompt tốt cụ thể hóa mục tiêu, bối cảnh, dữ liệu, giới hạn, format và điều kiện hoàn thành. Few-shot examples hữu ích khi cần dạy mẫu đầu vào/đầu ra, nhưng phải đại diện và nhất quán. Chia task thành bước khi cần kiểm soát trạng thái hoặc xem intermediate output.

**Eval workflow:**

1. Định nghĩa use case và hành vi mong muốn bằng tiêu chí quan sát được.
2. Tạo dataset đại diện: trường hợp phổ biến, biên, dữ liệu thiếu, input khó và trường hợp nguy hiểm.
3. Chạy prompt/model trên cùng bộ input để so sánh.
4. Chấm bằng code cho điều kiện xác định (schema, exact field, threshold) và/hoặc model/human graders cho chất lượng ngữ nghĩa.
5. Phân tích nhóm lỗi để sửa prompt, dữ liệu, tool hoặc quy trình.
6. Giữ bộ regression/holdout và chạy lại sau mỗi thay đổi.

Model-based grader cũng có thể sai hoặc thiên lệch; xác thực grader bằng người/đáp án tham chiếu, không dùng cùng tiêu chí mơ hồ cho cả response và grader. Một điểm tổng hợp duy nhất có thể che giấu lỗi nghiêm trọng ở nhóm nhỏ.

## 4. Tool use: ứng dụng thực thi, Claude đề xuất

Tool calling là giao thức giữa model và ứng dụng. Claude có thể trả một khối `tool_use` với tên tool và input; **nó không tự chạy hàm trong backend của bạn**. Ứng dụng phải kiểm tra, thực thi hoặc từ chối, rồi gửi `tool_result` gắn đúng `tool_use_id` để model tiếp tục.

```text
1. Gửi messages + tool definitions.
2. Đọc response: text hoàn tất hay có tool_use?
3. Nếu tool_use: xác thực schema, người dùng/quyền, phạm vi và tác động.
4. Chạy tool được allowlist (hoặc hỏi/không chạy).
5. Trả tool_result theo tool_use_id.
6. Gọi Messages API tiếp; lặp đến khi có câu trả lời cuối hoặc đạt giới hạn.
```

Các guardrails: tool scope hẹp; xác thực/chuẩn hóa input; tách read-only khỏi thao tác ghi; hỏi phê duyệt trước hành động nhạy cảm; timeout, retry và số vòng lặp có giới hạn; xử lý lỗi tool; lọc dữ liệu trả về; ghi log an toàn. Xem nội dung từ web/file/tool là dữ liệu không tin cậy, không phải chỉ thị có quyền ghi đè system policy.

## 5. RAG và agentic search

RAG đưa bằng chứng từ kho dữ liệu vào context lúc trả lời để giảm phụ thuộc vào kiến thức nền của model. RAG **không tự bảo đảm** dữ liệu đúng, đủ mới hoặc kết luận có căn cứ.

### Giai đoạn indexing

1. Thu thập tài liệu có quyền sử dụng; làm sạch nhưng giữ metadata/trích dẫn.
2. Chia thành **chunks** theo ranh giới ngữ nghĩa/tokens phù hợp.
3. Tạo embedding bằng một embedding model/service phù hợp; embedding là biểu diễn vector để tìm đoạn gần về ngữ nghĩa. **Anthropic hiện không cung cấp embedding model riêng**, nên chọn và đánh giá một nhà cung cấp embedding độc lập với model tạo câu trả lời.
4. Lưu vector, văn bản, nguồn, vị trí và access metadata.
5. Cập nhật hoặc xóa index khi nguồn thay đổi/thu hồi quyền.

### Giai đoạn query

1. Nhận câu hỏi và áp dụng quyền trước khi tìm nguồn.
2. Lấy ứng viên bằng semantic vector search, lexical search như BM25, hoặc hybrid.
3. Có thể rerank ứng viên theo mức liên quan.
4. Đưa các đoạn và metadata cần thiết vào prompt.
5. Yêu cầu câu trả lời gắn từng khẳng định với nguồn; báo thiếu bằng chứng thay vì đoán.
6. Kiểm tra citation có thực sự hỗ trợ khẳng định.

Chunk quá nhỏ mất mạch; chunk quá lớn làm nhiễu và tốn context. Overlap có thể giữ ý xuyên ranh giới nhưng tăng trùng lặp. Chọn kích thước, top-k và reranking bằng evaluation trên truy vấn thật. BM25 tốt với khớp từ khóa hiếm; embedding có thể tìm paraphrase; hybrid cân bằng hai tín hiệu.

## 6. Claude features, MCP và kiến trúc agent

Ảnh/PDF, citations, extended thinking, prompt caching và các tool/server-side features phụ thuộc endpoint/model/account/region; kiểm tra docs hiện hành trước khi thiết kế. **Prompt caching** có thể hữu ích khi tiền tố lớn được tái sử dụng; giữ nội dung ổn định ở phần phù hợp và đo hiệu quả/cost/latency theo quy tắc cache đang áp dụng.

**MCP (Model Context Protocol)** chuẩn hóa tích hợp:

- **Tools:** hành động có thể gọi.
- **Resources:** dữ liệu/ngữ cảnh có thể đọc.
- **Prompts:** mẫu hướng dẫn do server cung cấp.

Phân biệt giao thức với ứng dụng: client/host kết nối server; server cung cấp capability; ứng dụng phải duy trì permission, consent, validation và xử lý output.

**Workflow** là chuỗi bước/nhánh do ứng dụng định trước; thường dễ kiểm soát và eval. **Agent** có thể chọn bước/tool kế tiếp dựa trên output; linh hoạt hơn nhưng tạo nhiều đường đi và khó dự đoán hơn. Pattern phổ biến: chaining (bước sau dùng output bước trước), routing (chọn nhánh), parallelization (các việc độc lập chạy cùng lúc). Bắt đầu bằng workflow; chỉ tăng quyền tự quyết khi task cần và có eval/guardrails đủ tốt.

## 7. Bài tập thiết kế có đáp án mẫu

**Tình huống:** Xây trợ lý nội bộ trả lời câu hỏi về chính sách công ty từ PDF và có thể tạo ticket nếu người dùng yêu cầu.

- Ingest/index tài liệu có metadata về ngày hiệu lực, bộ phận và quyền truy cập; lọc kết quả theo quyền người hỏi trước khi đưa vào context.
- Retrieval hybrid cho từ khóa chính sách và câu hỏi diễn đạt lại; trả citation đến tài liệu/mục cụ thể.
- Tool `create_ticket` chỉ nhận các trường cần thiết; validate schema, kiểm tra người dùng, yêu cầu xác nhận trước khi tạo ticket thật.
- Nếu retrieval không đủ bằng chứng, trả lời rằng chưa tìm thấy; không tự bịa chính sách.
- Eval tập hợp câu hỏi đúng, paraphrase, chính sách hết hiệu lực, quyền bị từ chối, câu hỏi ngoài kho và prompt injection trong tài liệu.
- Theo dõi độ đúng retrieval, độ chính xác citation, tỷ lệ từ chối phù hợp, latency, token/cost và lỗi tool.

## 8. Câu hỏi ôn tập

1. **Ai giữ trạng thái hội thoại nhiều lượt?** Thông thường ứng dụng lưu và gửi lại lịch sử phù hợp.
2. **System prompt khác user message thế nào?** System định hướng/policy cấp ứng dụng; user cung cấp yêu cầu và dữ liệu của lượt.
3. **Tool_use có tự chạy tool không?** Không; backend xác thực và thực thi tool.
4. **`tool_use_id` để làm gì?** Liên kết tool result với đúng tool call.
5. **Structured output có thay validation không?** Không; schema và business rules vẫn phải xác thực.
6. **Code-based và model-based graders phù hợp với gì?** Điều kiện xác định so với đánh giá ngữ nghĩa/subjective; có thể kết hợp.
7. **Hai giai đoạn RAG là gì?** Indexing tài liệu; query/retrieval/generation.
8. **BM25 khác embeddings thế nào?** BM25 dựa vào khớp term; embeddings tìm tương đồng ngữ nghĩa.
9. **RAG ngăn mọi hallucination không?** Không; retrieval và diễn giải đều có thể sai.
10. **Khi nào workflow tốt hơn agent?** Khi trình tự ổn định, dễ mô tả, cần kiểm soát và kiểm thử chặt.
11. **MCP tools/resources/prompts là gì?** Capability hành động, dữ liệu và mẫu hướng dẫn.
12. **Kể bốn production control.** Secret an toàn, validation/quyền tool, giới hạn vòng lặp/timeouts, eval/monitoring/fallback.
13. **Tại sao streaming không làm response đúng hơn?** Nó thay đổi cách truyền/hiển thị, không xác thực nội dung.
14. **Nếu citation không hỗ trợ kết luận thì sao?** Sửa/bỏ kết luận hoặc tìm nguồn phù hợp; không để citation sai đánh lừa người đọc.

## 9. Nhầm lẫn cần tránh

- Claude API không thực thi tool backend; ứng dụng làm việc đó.
- Hình thức JSON không bảo đảm logic đúng.
- RAG phụ thuộc ingestion, quyền, retrieval, chunking và citation.
- Eval một ví dụ không đủ; cần dữ liệu đại diện và regression.
- Agent không luôn tốt hơn workflow; autonomy làm tăng số tình huống cần kiểm soát.
- Không hard-code tên model, giới hạn token, giá hoặc tham số mà không đối chiếu docs đang áp dụng.

## 10. Tự đánh giá trước kỳ thi

- [ ] Vẽ được API request/response và tool-use loop.
- [ ] Thiết kế eval có dataset, grader và phân tích lỗi.
- [ ] Vẽ được pipeline RAG và giải thích tradeoff chunk/retrieval.
- [ ] Phân biệt MCP, tools và agent workflow.
- [ ] Đưa ra guardrails cho quyền, dữ liệu, retries, latency và cost.

## Nguồn tham khảo

- [Claude Academy — Building with the Claude API](https://academy.claude.com/courses/building-with-the-claude-api)
- [Anthropic API — Messages](https://docs.anthropic.com/en/api/messages)
- [Claude Platform Docs — Tool use](https://docs.anthropic.com/en/docs/build-with-claude/tool-use)
- [Claude Platform Docs — Prompt engineering](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Claude Platform Docs — Prompt caching](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)
- [Claude Platform Docs — Embeddings](https://docs.anthropic.com/en/docs/build-with-claude/embeddings)
- [Anthropic Cookbook — official examples](https://github.com/anthropics/anthropic-cookbook)
- [Model Context Protocol — specification](https://modelcontextprotocol.io/)

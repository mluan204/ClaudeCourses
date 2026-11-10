# Đề cương ôn tập: Building with the Claude API

> Tài liệu tự ôn trước khi kiểm tra kiến thức; không phải đề thi chính thức.  
> Ghi chú đầy đủ: [Building with the Claude API](../building-with-the-claude-api-notes.md)

## Mục tiêu cần đạt

- Hiểu request/response và vai trò backend trong ứng dụng dùng Claude API.
- Thiết kế prompt, multi-turn chat, structured output và evaluation.
- Xử lý tool use đúng vòng lặp; hiểu RAG, embeddings, BM25 và MCP.
- Chọn workflow hay agent và thêm guardrails trước production.

## API và hội thoại

- Messages API nhận model, giới hạn output và danh sách messages; xác thực API ở backend, không nhúng secret vào client.
- Ứng dụng quản lý lịch sử multi-turn và kiểm soát context gửi lại.
- System prompt chứa hướng dẫn cấp ứng dụng; user message chứa yêu cầu của lượt.
- Streaming trả nội dung dần để giảm thời gian chờ cảm nhận; vẫn phải xử lý lỗi/kết thúc stream.
- Temperature điều chỉnh độ biến thiên, không bảo đảm tính đúng.

## Prompt, output và evaluation

- Viết prompt có mục tiêu, bối cảnh, constraints và format rõ.
- XML tags phân vùng instruction, context và dữ liệu.
- Few-shot examples minh họa pattern input/output; cần đại diện, không mâu thuẫn.
- Structured output/schema chuẩn hóa dữ liệu; backend vẫn validate schema và business rules.
- Evaluation cần dataset đại diện, runner, grader và phân tích lỗi.
- Dùng code-based grading cho điều kiện xác định, model-based grading cho chất lượng ngữ nghĩa; giữ holdout và edge cases.

## Tool use

Vòng lặp chuẩn:

1. Gửi prompt cùng định nghĩa tools.
2. Claude trả tool use và input.
3. Backend xác thực input/quyền rồi thực thi tool.
4. Backend gửi tool_result gắn đúng tool_use_id.
5. Gọi Claude tiếp tục; giới hạn lượt, timeout và quyền.

**Claude không tự thực thi hàm backend.** Ứng dụng quyết định có chạy tool hay không. Dùng tools hẹp, ưu tiên read-only, yêu cầu phê duyệt hành động nhạy cảm và bảo vệ khỏi prompt injection.

## RAG và retrieval

| Thành phần | Vai trò |
| --- | --- |
| Chunking | Chia tài liệu thành phần có kích thước/ngữ cảnh phù hợp |
| Embeddings | Mã hóa ngữ nghĩa để tìm đoạn tương tự |
| Vector search | Lấy chunk gần truy vấn theo embedding |
| BM25 | Tìm theo từ khóa/term |
| Hybrid search | Kết hợp tín hiệu semantic và lexical |
| Reranking | Sắp xếp lại ứng viên trước khi đưa context vào prompt |
| Citation | Nối kết luận về nguồn, tài liệu và vị trí |

Pipeline thường gồm ingestion/indexing rồi query/retrieval/generation. Chunk quá nhỏ mất ngữ cảnh; quá lớn giảm độ chính xác và tốn context. Citation cần truy ngược được nguồn.

## MCP, agents và workflows

- MCP chuẩn hóa kết nối giữa host/client và server.
- **Tools** cho hành động; **resources** cung cấp dữ liệu; **prompts** cung cấp template.
- Workflow là chuỗi bước định trước; agent chọn hành động linh hoạt theo kết quả.
- **Chaining:** bước sau dùng output bước trước.
- **Routing:** chọn nhánh theo điều kiện.
- **Parallelization:** chạy tác vụ độc lập cùng lúc.
- Inspect môi trường trước hành động; giới hạn tool và có fallback/human handoff.

## Checklist production

- Quản lý API key server-side; rate limit, timeout và retry có kiểm soát.
- Validate tool input và structured output ở backend.
- Bảo vệ dữ liệu nhạy cảm, prompt injection và quyền tool.
- Có evaluation/regression dataset.
- Theo dõi latency, token, cost, lỗi và chất lượng mà không log lộ dữ liệu.
- Giới hạn agent loop; có fallback và phê duyệt người dùng.

## Điểm dễ nhầm

- Tool call là yêu cầu từ model; backend mới thực thi.
- Structured output không thay thế validation.
- Embedding và BM25 dùng tín hiệu khác nhau; hybrid có thể kết hợp cả hai.
- RAG giảm phụ thuộc kiến thức nền nhưng không bảo đảm nguồn truy xuất đúng.
- Agent linh hoạt hơn nhưng khó dự đoán; workflow cố định thường dễ kiểm thử.
- Streaming cải thiện hiển thị, không làm nội dung tự đúng hơn.

## Tự kiểm tra

1. **Ai thực thi tool call?** Backend/application sau khi kiểm tra input, quyền và chính sách.
2. **tool_use_id để làm gì?** Ghép tool result với đúng yêu cầu tool use.
3. **Structured output có cần validation?** Có; kiểm schema và logic nghiệp vụ.
4. **BM25 và embeddings khác nhau thế nào?** BM25 nhấn mạnh term; embeddings tìm tương đồng ngữ nghĩa.
5. **RAG có hai giai đoạn lớn nào?** Ingestion/indexing và query/retrieval/generation.
6. **Khi nào workflow phù hợp hơn agent?** Khi bước ổn định, cần dự đoán và dễ kiểm thử.
7. **Nêu ba thành phần evaluation.** Dataset, grader phù hợp, phân tích lỗi/regression.
8. **Nêu ba guardrails tool use.** Least privilege, validation, approval/giới hạn lượt.

## Checklist trước khi thi

- [ ] Vẽ được tool-use loop từ request đến tool_result.
- [ ] Phân biệt system prompt, user message và lịch sử multi-turn.
- [ ] Nêu retrieval pipeline và tradeoff chunking.
- [ ] Phân biệt MCP tools/resources/prompts.
- [ ] Giải thích workflow vs agent, chaining/routing/parallelization.
- [ ] Nhớ production controls: security, eval, limits, monitoring, fallback.

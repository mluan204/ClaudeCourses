# Building with the Claude API

## Tài liệu học tập -- Claude Academy

> **Khóa học:** Building with the Claude API\
> **Nền tảng:** Anthropic API / Python SDK\
> **Đơn vị:** Anthropic / Claude Academy\
> **Thời lượng khóa học:** khoảng 9 giờ\
> **Số bài học:** 67 bài học + 8 bài kiểm tra\
> **Mục tiêu:** Làm chủ quy trình phát triển ứng dụng tích hợp Claude API: tương tác cơ bản, đánh giá prompt (evals), kỹ thuật prompt engineering nâng cao, tool use (function calling), RAG & Agentic Search, vision, prompt caching, embeddings và thiết kế kiến trúc tác tử (Agents & Workflows) trong production.

------------------------------------------------------------------------

## Mục lục

1.  [Bản đồ tổng quan khóa học](#1-bản-đồ-tổng-quan-khóa-học)
2.  [Phần I -- Accessing Claude with the API](#2-phần-i----accessing-claude-with-the-api)
3.  [Phần II -- Prompt Evaluation](#3-phần-ii----prompt-evaluation)
4.  [Phần III -- Prompt Engineering Techniques](#4-phần-iii----prompt-engineering-techniques)
5.  [Phần IV -- Tool Use with Claude](#5-phần-iv----tool-use-with-claude)
6.  [Phần V -- RAG and Agentic Search](#6-phần-v----rag-and-agentic-search)
7.  [Phần VI -- Features of Claude](#7-phần-vi----features-of-claude)
8.  [Phần VII -- Model Context Protocol (MCP)](#8-phần-vii----model-context-protocol-mcp)
9.  [Phần VIII -- Anthropic Apps: Claude Code and Computer Use](#9-phần-viii----anthropic-apps-claude-code-and-computer-use)
10. [Phần IX -- Agents and Workflows](#10-phần-ix----agents-and-workflows)
11. [Kiến trúc tham khảo cho một ứng dụng Claude API production](#11-kiến-trúc-tham-khảo-cho-một-ứng-dụng-claude-api-production)
12. [Bảng chọn kỹ thuật theo bài toán](#12-bảng-chọn-kỹ-thuật-theo-bài-toán)
13. [Checklist trước khi đưa ứng dụng lên production](#13-checklist-trước-khi-đưa-ứng-dụng-lên-production)
14. [Tổng kết và câu hỏi ôn tập](#14-tổng-kết-và-câu-hỏi-ôn-tập)

------------------------------------------------------------------------
# 1. Bản đồ tổng quan khóa học

Khóa học gồm 9 nhóm nội dung:

  ------------------------------------------------------------------------
  Phần                                        Số bài Nội dung chính
  --------------------- ---------------------------- ---------------------
  Accessing Claude with                            9 API key, request, hội
  the API                                            thoại nhiều lượt,
                                                     system prompt,
                                                     temperature,
                                                     streaming, structured
                                                     output

  Prompt evaluation                                6 Thiết kế eval, tạo dữ
                                                     liệu test, chạy đánh
                                                     giá, model-based và
                                                     code-based grading

  Prompt engineering                               6 Prompt rõ ràng, cụ
  techniques                                         thể, XML tags,
                                                     examples

  Tool use with Claude                            13 Function calling,
                                                     tool schema, tool
                                                     result, multi-turn,
                                                     batch, fine-grained
                                                     tools, web search và
                                                     text editor

  RAG and Agentic                                  7 Chunking, embeddings,
  Search                                             retrieval, BM25,
                                                     multi-index RAG

  Features of Claude                               8 Extended thinking,
                                                     image/PDF, citations,
                                                     prompt caching, code
                                                     execution và Files
                                                     API

  Model Context                                   11 MCP client/server,
  Protocol                                           tools, resources,
                                                     prompts

  Anthropic apps                                   4 Claude Code, Computer
                                                     Use và MCP
                                                     integration

  Agents and workflows                             8 Parallelization,
                                                     chaining, routing,
                                                     agent/tool,
                                                     environment
                                                     inspection
  ------------------------------------------------------------------------

**Tư duy xuyên suốt:** Một ứng dụng AI đáng tin cậy không chỉ cần prompt
tốt. Nó cần API integration đúng, dữ liệu đầu vào phù hợp, công cụ có
kiểm soát, đánh giá định lượng, kiến trúc phù hợp và cơ chế giám sát.

------------------------------------------------------------------------

# 2. Phần I -- Accessing Claude with the API

## Bài 1. Accessing the API

Claude API cho phép ứng dụng gửi nội dung đến mô hình Anthropic và nhận
phản hồi có cấu trúc. Ứng dụng thường gửi: - Model cần sử dụng. - Giới
hạn token đầu ra. - Danh sách messages. - System prompt nếu cần. - Các
tham số điều khiển khác.

Một request không chỉ là "gửi câu hỏi"; đó là một cấu hình xác định mô
hình, ngữ cảnh, giới hạn và cách tạo phản hồi.

### Luồng tổng quát

1.  Ứng dụng nhận input từ người dùng.
2.  Backend xác thực và kiểm tra input.
3.  Backend tạo request gửi đến Anthropic API.
4.  API trả về message gồm các content blocks và metadata.
5.  Backend xử lý kết quả, lưu trạng thái nếu cần và trả về client.

### Ví dụ Python

``` python
import anthropic

client = anthropic.Anthropic()  # đọc ANTHROPIC_API_KEY từ environment

response = client.messages.create(
    model="MODEL_ID",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "Giải thích REST API trong 3 ý."}
    ],
)

print(response.content)
```

`MODEL_ID` là placeholder: hãy chọn model ID đang được hỗ trợ trong tài
liệu hiện hành. Không hardcode API key trong source code.

## Bài 2. Getting an API key

API key xác thực ứng dụng với Anthropic. Cần: - Tạo và quản lý key qua
tài khoản/console chính thức. - Đưa key vào environment variable hoặc
secret manager. - Không commit key vào Git, không gửi key cho
frontend/browser. - Luân chuyển hoặc thu hồi key khi nghi ngờ bị lộ. -
Dùng key riêng theo môi trường hoặc ứng dụng khi phù hợp.

Ví dụ thiết lập biến môi trường:

``` bash
# macOS / Linux
export ANTHROPIC_API_KEY="your-secret-key"

# PowerShell
$env:ANTHROPIC_API_KEY="your-secret-key"
```

Backend nên giữ key và gọi API thay cho frontend. Nếu client cần gọi
trực tiếp trong một kiến trúc đặc biệt, phải dùng cơ chế credential ngắn
hạn/được thiết kế an toàn, không nhúng secret dài hạn.

## Bài 3. Making a request

Request Messages API cơ bản gồm `model`, `max_tokens` và `messages`.

### Message format

Hội thoại thường dùng hai role chính: - `user`: nội dung do người dùng
cung cấp. - `assistant`: phản hồi của Claude.

Một message có thể chứa nhiều content blocks, không nhất thiết chỉ là
một chuỗi văn bản. Với các tính năng như tool use, hình ảnh hoặc dữ liệu
đa phương thức, cần đọc đúng loại block.

### Response format

Response thường có: - `content`: các content blocks. - `role`: vai trò
assistant. - `model`: model thực tế xử lý. - `stop_reason`: lý do kết
thúc. - `usage`: thông tin token đầu vào/đầu ra.

Không nên giả định mọi response chỉ có một text block. Hãy kiểm tra loại
block và `stop_reason`, nhất là khi có tool use hoặc giới hạn token.

## Bài 4. Multi-turn conversations

Claude API không tự động nhớ các request trước như một phiên chat được
lưu phía server theo cách ứng dụng thường mong đợi. Ứng dụng cần quản lý
và gửi lại lịch sử hội thoại cần thiết.

Ví dụ:

``` python
messages = [
    {"role": "user", "content": "Tôi đang học Python."},
    {"role": "assistant", "content": "Bạn muốn bắt đầu từ phần nào?"},
    {"role": "user", "content": "Giải thích list và tuple."},
]

response = client.messages.create(
    model="MODEL_ID",
    max_tokens=1200,
    messages=messages,
)
```

### Quản lý lịch sử hội thoại

-   Lưu messages theo conversation/session ID.
-   Chỉ gửi phần lịch sử cần thiết.
-   Giữ đúng cấu trúc role và content.
-   Có chính sách cắt bớt hoặc tóm tắt lịch sử khi dài.
-   Không tin tưởng nội dung do người dùng cung cấp như system
    instruction.
-   Có thể tách dữ liệu nghiệp vụ khỏi nội dung hội thoại.

### Context window và token

Context window chứa input, lịch sử, system prompt, tool definitions và
các nội dung liên quan. Khi vượt giới hạn, request có thể lỗi hoặc cần
rút gọn. Theo dõi usage để kiểm soát chi phí và độ trễ.

## Bài 5. System prompts

System prompt cung cấp chỉ dẫn cấp ứng dụng để định hướng hành vi của
Claude, ví dụ vai trò, mục tiêu, phong cách, quy tắc và giới hạn.

Ví dụ:

``` python
response = client.messages.create(
    model="MODEL_ID",
    max_tokens=800,
    system=(
        "Bạn là trợ lý hỗ trợ khách hàng của cửa hàng. "
        "Trả lời bằng tiếng Việt, ngắn gọn và lịch sự. "
        "Chỉ sử dụng chính sách được cung cấp; nếu thiếu dữ liệu, "
        "hãy nói rõ và không tự bịa chính sách."
    ),
    messages=[
        {"role": "user", "content": "Tôi có thể đổi hàng sau 30 ngày không?"}
    ],
)
```

### System prompt tốt

-   Xác định vai trò và nhiệm vụ rõ ràng.
-   Nêu nguồn thông tin được phép sử dụng.
-   Đưa ra quy tắc khi thiếu dữ liệu.
-   Nêu định dạng đầu ra nếu cần.
-   Tách instruction đáng tin khỏi dữ liệu người dùng/tài liệu được truy
    xuất.
-   Không đưa secret hoặc dữ liệu nhạy cảm không cần thiết.

System prompt không thay thế kiểm tra quyền ở backend. Không để mô hình
tự quyết định các hành động nhạy cảm mà không có kiểm soát ở tầng ứng
dụng.

## Bài 6. Temperature

Temperature điều chỉnh mức độ ngẫu nhiên trong quá trình lấy mẫu
token: - Thấp hơn: kết quả thường ổn định, ít biến thiên hơn. - Cao hơn:
kết quả có thể đa dạng, sáng tạo hơn nhưng khó dự đoán hơn.

Gợi ý: - Trích xuất, phân loại, chuẩn hóa: bắt đầu với mức thấp. - Viết
ý tưởng, brainstorm: có thể thử mức cao hơn. - Không dùng temperature
như công cụ đảm bảo tính đúng. - Với tác vụ cần kết quả nhất quán, nên
kết hợp prompt rõ, structured output và eval.

Tham số và khoảng giá trị hỗ trợ phụ thuộc model/API hiện hành.

## Bài 7. Response streaming

Streaming gửi phản hồi theo từng event/chunk thay vì chờ toàn bộ kết quả
hoàn tất.

### Lợi ích

-   Người dùng nhìn thấy phản hồi sớm hơn.
-   Tạo trải nghiệm chat tự nhiên.
-   Có thể hiển thị tiến độ khi output dài.

### Điều cần xử lý

-   Nhiều event có thể tạo thành một message.
-   Không phải mọi event đều là text.
-   Cần xử lý lỗi và kết thúc stream.
-   Không xem text chưa hoàn tất là kết quả cuối.
-   Với tool use, cần xử lý content blocks và chuyển tiếp vòng gọi tool
    đúng cách.

Streaming cải thiện perceived latency, không nhất thiết làm model tạo
nội dung nhanh hơn.

## Bài 8. Structured data

Structured output giúp yêu cầu Claude trả về dữ liệu theo schema, thường
là JSON có cấu trúc xác định.

Ví dụ mục tiêu: trích xuất đơn hàng thành các trường `order_id`,
`status`, `amount`.

Schema có thể yêu cầu: - Kiểu dữ liệu. - Trường bắt buộc. - Enum cho tập
giá trị hợp lệ. - Cấu trúc object/array lồng nhau.

Ví dụ JSON mong muốn:

``` json
{
  "order_id": "ORD-1024",
  "status": "shipped",
  "amount": 125000
}
```

### Vì sao cần schema?

-   Giảm lỗi parse.
-   Dễ validate và lưu database.
-   Tạo contract giữa model và ứng dụng.
-   Giảm sự mơ hồ về định dạng.

Tuy nhiên, JSON hợp lệ không đồng nghĩa dữ liệu đúng. Backend vẫn phải
validate business rules, kiểu dữ liệu, quyền và tính hợp lệ của giá trị.
Dùng tính năng structured outputs theo đúng cú pháp API/model hiện hành;
không tự giả định prompt "hãy trả JSON" là đủ.

## Bài 9. Quiz on accessing Claude with the API --- kiến thức cần nhớ

-   API key phải được bảo vệ ở backend.
-   Request cần model, token limit và messages.
-   Ứng dụng tự quản lý lịch sử multi-turn.
-   System prompt định hướng hành vi nhưng không thay thế authorization.
-   Temperature tác động tính biến thiên, không bảo đảm độ chính xác.
-   Streaming trả dữ liệu theo event.
-   Structured output cần schema và validation phía ứng dụng.

------------------------------------------------------------------------

# 3. Phần II -- Prompt Evaluation

## Bài 10. Prompt evaluation

Prompt evaluation (eval) là quy trình đo lường chất lượng prompt một
cách có hệ thống. Không nên đánh giá prompt chỉ bằng một vài lần thử
hoặc cảm giác "câu trả lời có vẻ ổn".

Một eval thường có: 1. Bộ test cases đại diện cho tình huống thật. 2.
Prompt/model/configuration cần đánh giá. 3. Cách chạy model trên từng
test case. 4. Grader hoặc tiêu chí chấm. 5. Báo cáo kết quả và phân tích
lỗi.

### Vì sao cần eval?

-   So sánh hai phiên bản prompt.
-   Phát hiện regression khi thay model hoặc thay dữ liệu.
-   Đo chất lượng theo từng loại input.
-   Tìm edge cases.
-   Có cơ sở quyết định trước khi deploy.

## Bài 11. A typical eval workflow

Quy trình: 1. Xác định nhiệm vụ và định nghĩa "đúng". 2. Thu thập hoặc
tạo test dataset. 3. Chạy baseline với prompt hiện tại. 4. Chấm kết quả
bằng grader phù hợp. 5. Phân tích lỗi theo nhóm. 6. Sửa prompt hoặc kiến
trúc. 7. Chạy lại cùng dataset để so sánh. 8. Lưu kết quả và đưa eval
vào quy trình phát triển.

Không nên chỉ tối ưu điểm trung bình. Hãy xem từng nhóm lỗi, độ ổn định
và các tình huống rủi ro cao.

## Bài 12. Generating test datasets

Dataset nên đại diện cho phân phối input thực tế: - Trường hợp phổ
biến. - Trường hợp hiếm nhưng quan trọng. - Input thiếu hoặc mơ hồ. -
Input sai định dạng. - Nội dung độc hại hoặc prompt injection. - Các
trường hợp biên về độ dài/ngôn ngữ.

Có thể dùng mô hình để tạo dữ liệu tổng hợp, nhưng cần con người rà
soát: - Có thực tế không? - Có trùng lặp không? - Có đáp án/tiêu chí
đánh giá hợp lý không? - Có vô tình làm lệch dataset về ví dụ dễ không?

Tách dataset phát triển và dataset holdout để tránh điều chỉnh prompt
quá mức theo bộ test đã nhìn thấy.

## Bài 13. Running the eval

Mỗi test case cần được chạy với cấu hình có thể tái lập: - Prompt
version. - Model ID. - Tham số generation. - Input. - Thời điểm và phiên
bản ứng dụng. - Output và usage.

Lưu output thô để có thể kiểm tra lại. Nếu dùng model có tính ngẫu
nhiên, chạy nhiều lần cho các trường hợp quan trọng để đo độ ổn định.

## Bài 14. Model-based grading

Model-based grader dùng một mô hình để đánh giá output theo rubric.

Ví dụ rubric cho trợ lý hỗ trợ: - Đúng thông tin: 0--4 điểm. - Đủ ý:
0--2 điểm. - Tuân thủ chính sách: 0--2 điểm. - Dễ hiểu: 0--2 điểm.

### Cách viết grader

-   Cung cấp nhiệm vụ và tiêu chí rõ.
-   Định nghĩa thang điểm và ví dụ.
-   Yêu cầu nêu bằng chứng từ output.
-   Không để grader đánh giá theo cảm tính chung chung.
-   Nếu có thể, ẩn danh phiên bản prompt để giảm thiên kiến.

Model grader có thể thiên lệch, không ổn định hoặc bỏ sót lỗi. Với quyết
định quan trọng, cần đối chiếu human review và grader độc lập.

## Bài 15. Code-based grading

Code-based grader dùng quy tắc máy kiểm tra được: - JSON parse thành
công. - Có đủ key bắt buộc. - Giá trị nằm trong enum. - Mã sản phẩm có
tồn tại. - Kết quả toán học chính xác. - Test/unit test pass.

Ưu điểm: nhất quán, nhanh, rẻ và dễ tái lập. Nhược điểm: khó đánh giá
chất lượng ngôn ngữ, tính hữu ích hoặc sắc thái.

### Kết hợp hai loại grader

  Loại           Hợp với
  -------------- -----------------------------------------------
  Code-based     Format, schema, con số, quy tắc xác định
  Model-based    Mức độ liên quan, đầy đủ, chất lượng diễn đạt
  Human review   Tình huống phức tạp, chính sách và rủi ro cao

## Bài 16. Quiz on prompt evaluation --- kiến thức cần nhớ

-   Eval là quy trình lặp, không phải một lần thử prompt.
-   Dataset phải đại diện và có edge cases.
-   Giữ baseline để so sánh.
-   Model grader cần rubric và kiểm chứng.
-   Code grader phù hợp tiêu chí xác định.
-   Theo dõi regression, không chỉ điểm trung bình.

------------------------------------------------------------------------

# 4. Phần III -- Prompt Engineering Techniques

## Bài 17. Prompt engineering

Prompt engineering là thiết kế input để mô hình hiểu nhiệm vụ, sử dụng
đúng thông tin và tạo output phù hợp. Đây là một quá trình thử nghiệm có
đo lường.

Một prompt hiệu quả thường có: - Nhiệm vụ rõ. - Bối cảnh đủ dùng. - Dữ
liệu đầu vào được phân định. - Ràng buộc và điều không được làm. - Định
dạng output. - Ví dụ nếu cần. - Tiêu chí xử lý khi thiếu thông tin.

## Bài 18. Being clear and direct

Dùng chỉ dẫn trực tiếp, tránh câu dài và mơ hồ.

Chưa tốt: \> Hãy xem qua nội dung này và giúp tôi xử lý sao cho hợp lý.

Tốt hơn: \> Tóm tắt tài liệu thành 5 gạch đầu dòng. Nêu riêng các quyết
định, deadline và vấn đề chưa được giải quyết. Không suy đoán thông tin
không có trong tài liệu.

Nêu rõ động từ hành động: phân loại, trích xuất, so sánh, viết lại, kiểm
tra, đề xuất. Nếu cần một kết quả cụ thể, nói rõ kết quả đó.

## Bài 19. Being specific

Mô tả chính xác: - Đối tượng và phạm vi. - Độ dài hoặc số lượng. - Độc
giả mục tiêu. - Giọng điệu. - Định dạng. - Tiêu chí thành công. - Giới
hạn nguồn dữ liệu.

Ví dụ:

``` text
Đọc 3 phản hồi khách hàng bên dưới.
Phân loại mỗi phản hồi thành: billing, delivery, product hoặc other.
Trả về JSON array gồm id, category và one_sentence_summary.
Không tự tạo category mới. Nếu không đủ dữ kiện, dùng other.
```

Càng cụ thể, output càng dễ đánh giá và tích hợp.

## Bài 20. Structure with XML tags

XML-like tags giúp phân tách instruction, dữ liệu và output format.
Chúng đặc biệt hữu ích với prompt dài hoặc có nhiều thành phần.

Ví dụ:

``` text
Bạn là trợ lý phân tích phản hồi khách hàng.

<task>
Phân loại phản hồi và nêu lý do ngắn.
</task>

<categories>
- billing
- delivery
- product
- other
</categories>

<customer_feedback>
{{feedback}}
</customer_feedback>

<output_format>
Trả về JSON với các trường category và reason.
</output_format>
```

Tên tag có thể tự chọn, miễn nhất quán và dễ hiểu. XML tags không phải
cơ chế bảo mật; nội dung trong tag dữ liệu vẫn có thể chứa prompt
injection. Hãy nói rõ đâu là dữ liệu không đáng tin và không được xem nó
là chỉ thị.

## Bài 21. Providing examples

Few-shot prompting là cung cấp một số ví dụ input-output để mô hình hiểu
pattern mong muốn.

Ví dụ:

``` text
Phân loại cảm xúc thành positive, neutral hoặc negative.

<examples>
  <example>
    <input>Sản phẩm rất tốt, giao hàng nhanh.</input>
    <output>positive</output>
  </example>
  <example>
    <input>Đơn hàng đã được ghi nhận.</input>
    <output>neutral</output>
  </example>
  <example>
    <input>Sản phẩm bị hỏng khi nhận.</input>
    <output>negative</output>
  </example>
</examples>

<task>
Phân loại: "Tôi đã chờ một tuần nhưng vẫn chưa nhận được hàng."
</task>
```

Ví dụ tốt: - Đại diện cho các trường hợp cần xử lý. - Nhất quán về
format. - Có cả trường hợp dễ và khó. - Không chứa mâu thuẫn. - Không
quá dài nếu không cần thiết.

Few-shot giúp mô hình bắt chước pattern nhưng không bảo đảm suy luận
đúng. Nên dùng eval để kiểm tra tác động của ví dụ.

## Bài 22. Quiz on prompt engineering --- kiến thức cần nhớ

-   Rõ ràng và trực tiếp.
-   Cụ thể về phạm vi, đầu ra và tiêu chí.
-   Dùng XML tags để phân vùng nội dung.
-   Dùng ví dụ để minh họa pattern.
-   Không nhầm prompt structure với bảo mật.
-   Đo lường prompt bằng eval.

------------------------------------------------------------------------

# 5. Phần IV -- Tool Use with Claude

## Bài 23. Introducing tool use

Tool use cho phép Claude yêu cầu ứng dụng thực hiện một hành động bên
ngoài mô hình, ví dụ tra thời tiết, truy vấn database hoặc tạo reminder.

**Điểm quan trọng:** Claude không tự thực thi function Python của bạn.
Claude phát ra một tool call có tên và arguments; ứng dụng của bạn phải
kiểm tra, thực thi và gửi kết quả trở lại.

### Vòng tool-use cơ bản

1.  Gửi prompt và danh sách tool definitions cho Claude.
2.  Claude trả về `tool_use` block nếu cần gọi công cụ.
3.  Backend xác thực tên tool và arguments.
4.  Backend thực thi function tương ứng.
5.  Backend gửi `tool_result` trong message tiếp theo.
6.  Claude dùng kết quả để trả lời hoặc gọi tool tiếp.

## Bài 24. Project overview

Một ứng dụng tool use thường có các lớp: - **Model/API layer:** gửi
messages, nhận response. - **Tool registry:** ánh xạ tên tool sang hàm
thực thi. - **Validation:** kiểm tra arguments và quyền. -
**Execution:** gọi API/database/function. - **Result handling:** chuẩn
hóa kết quả, xử lý lỗi. - **Conversation manager:** lưu lịch sử và tiếp
tục vòng lặp.

Không để model quyết định quyền truy cập. Backend phải kiểm tra người
dùng hiện tại có được phép thực hiện hành động đó không.

## Bài 25. Tool functions

Tool function là hàm ứng dụng thực sự thực hiện công việc.

Ví dụ:

``` python
def get_weather(location: str) -> dict:
    if not location.strip():
        raise ValueError("location is required")
    # Gọi weather provider thật ở đây
    return {"location": location, "temperature_c": 28}
```

Hàm tool nên: - Có trách nhiệm rõ ràng. - Kiểm tra input. - Xử lý lỗi và
timeout. - Trả dữ liệu có cấu trúc. - Không thực hiện hành động vượt
quyền. - Có log phù hợp nhưng không ghi secret.

## Bài 26. Tool schemas

Tool schema mô tả với Claude: - Tên tool. - Mô tả công dụng và thời điểm
nên dùng. - JSON schema cho input. - Required fields, types, enum và
giới hạn.

Ví dụ schema minh họa:

``` python
tools = [{
    "name": "get_weather",
    "description": "Lấy thời tiết hiện tại tại một địa điểm.",
    "input_schema": {
        "type": "object",
        "properties": {
            "location": {
                "type": "string",
                "description": "Tên thành phố hoặc địa điểm."
            }
        },
        "required": ["location"]
    }
}]
```

Mô tả tool cần rõ và không chồng chéo. Schema tốt giúp giảm arguments
sai nhưng backend vẫn phải validate độc lập.

## Bài 27. Handling message blocks

Response có thể chứa nhiều block: - `text`: văn bản thông thường. -
`tool_use`: yêu cầu gọi tool. - Các block khác tùy tính năng/model.

Không nên chỉ lấy `response.content[0].text` rồi bỏ qua phần còn lại.
Hãy duyệt các block và xử lý theo `type`.

Ngoài ra, `stop_reason` cho biết model kết thúc vì lý do gì. Khi có tool
call, ứng dụng phải tiếp tục chu trình thay vì coi response đó là câu
trả lời cuối.

## Bài 28. Sending tool results

Sau khi thực thi tool, gửi kết quả bằng message `user` chứa
`tool_result` tương ứng với `tool_use_id`.

Ví dụ cấu trúc khái niệm:

``` python
tool_result_block = {
    "type": "tool_result",
    "tool_use_id": tool_use_id,
    "content": "Nhiệt độ tại Biên Hòa hiện là 28°C."
}
```

Tool result phải khớp đúng ID của tool call. Có thể trả nội dung thành
công hoặc thông báo lỗi an toàn. Không gửi stack trace, secret hoặc dữ
liệu nội bộ không cần thiết cho model.

## Bài 29. Multi-turn conversations with tools

Một tool call có thể là bước giữa của hội thoại: - Người dùng hỏi. -
Claude gọi tool A. - Backend trả kết quả A. - Claude gọi tool B dựa trên
kết quả A. - Backend trả kết quả B. - Claude tổng hợp câu trả lời cuối.

Ứng dụng cần giữ đầy đủ chuỗi message theo đúng định dạng API. Cần có
giới hạn số vòng lặp để tránh tool loop vô hạn.

## Bài 30. Implementing multiple turns

Pseudo-code:

``` python
messages = [{"role": "user", "content": user_input}]

for _ in range(MAX_TOOL_ROUNDS):
    response = client.messages.create(
        model="MODEL_ID",
        max_tokens=1500,
        tools=tools,
        messages=messages,
    )

    messages.append({"role": "assistant", "content": response.content})

    tool_blocks = [
        block for block in response.content
        if block.type == "tool_use"
    ]

    if not tool_blocks:
        break

    results = []
    for tool_call in tool_blocks:
        result = execute_tool_safely(
            name=tool_call.name,
            arguments=tool_call.input,
        )
        results.append({
            "type": "tool_result",
            "tool_use_id": tool_call.id,
            "content": result,
        })

    messages.append({"role": "user", "content": results})
```

Đây là pseudo-code minh họa, cần bổ sung kiểm tra quyền, schema
validation, lỗi API, timeout, logging và định dạng tool result đúng
SDK/API version.

## Bài 31. Using multiple tools

Có thể khai báo nhiều tools cho một request. Claude chọn tool phù hợp
dựa trên mô tả và context.

Thiết kế tốt: - Mỗi tool có mục đích riêng. - Tên và mô tả không mơ
hồ. - Tránh quá nhiều tool gần như trùng nhau. - Chỉ cung cấp tools cần
thiết cho task. - Kiểm soát quyền từng tool.

Nếu một task cần nhiều công cụ, backend cần xác định công cụ nào có thể
chạy song song và công cụ nào phụ thuộc kết quả trước.

## Bài 32. Fine-grained tool calling

Fine-grained tool calling hỗ trợ truyền arguments theo dạng
streaming/chi tiết hơn trong những trường hợp phù hợp. Điều này hữu ích
với input tool lớn hoặc cần bắt đầu xử lý sớm.

Cần lưu ý: - Arguments có thể chưa hoàn chỉnh trong các event đầu. -
Không thực thi tool khi JSON/input chưa hoàn chỉnh và chưa validate. -
Ghép đúng các event theo block/index. - Chỉ thực hiện hành động sau khi
nhận đủ dữ liệu cần thiết và kiểm tra hợp lệ.

Không nhầm streaming tool arguments với việc cho phép thực thi từng phần
thiếu kiểm soát.

## Bài 33. The text edit tool

Text editor tool là ví dụ về tool cho phép Claude đọc, tạo và chỉnh sửa
file thông qua giao diện tool được định nghĩa. Mô hình đề xuất thao tác;
ứng dụng thực thi.

Khi xây dựng: - Giới hạn root directory được phép truy cập. - Chặn path
traversal (`../`) và đường dẫn tuyệt đối ngoài vùng. - Kiểm tra quyền
đọc/ghi. - Có cơ chế backup hoặc undo. - Trả lỗi rõ ràng. - Ghi nhận
file nào đã thay đổi.

Không cho phép tool tùy ý đọc toàn bộ filesystem hoặc sửa file nhạy cảm.

## Bài 34. The web search tool

Web search tool cho phép Claude yêu cầu backend tìm kiếm web và sử dụng
kết quả trong câu trả lời.

Một implementation tốt cần: - Xác định phạm vi truy vấn. - Gọi search
provider. - Chuẩn hóa tiêu đề, snippet, URL và ngày. - Giới hạn số kết
quả và timeout. - Giữ nguồn để có thể dẫn chứng. - Phân biệt nội dung
nguồn với instruction của ứng dụng.

Kết quả web là dữ liệu không đáng tin hoàn toàn. Không để nội dung trang
web ghi đè system prompt hoặc kích hoạt hành động nhạy cảm.

## Bài 35. Quiz on tool use --- kiến thức cần nhớ

-   Tool call là yêu cầu từ Claude, backend mới là nơi thực thi.
-   Tool schema giúp mô hình tạo arguments phù hợp.
-   Phải xử lý tất cả content blocks và `stop_reason`.
-   `tool_result` phải khớp `tool_use_id`.
-   Multi-turn tool use cần giữ lịch sử đúng định dạng.
-   Validate input, quyền, timeout và lỗi.
-   Đặt giới hạn vòng lặp.
-   Tool có quyền ghi/xóa cần guardrails chặt.

------------------------------------------------------------------------

# 6. Phần V -- RAG and Agentic Search

## Bài 36. Introducing Retrieval-Augmented Generation (RAG)

RAG kết hợp truy xuất thông tin bên ngoài với khả năng tạo sinh của mô
hình.

### Vì sao cần RAG?

-   Kiến thức riêng của doanh nghiệp không nằm sẵn trong model.
-   Dữ liệu thay đổi thường xuyên.
-   Cần trả lời dựa trên tài liệu có nguồn.
-   Muốn giảm việc mô hình tự suy đoán.

### RAG pipeline cơ bản

**Ingestion/indexing:** 1. Thu thập tài liệu. 2. Làm sạch và chuẩn hóa.
3. Chia tài liệu thành chunks. 4. Tạo embeddings và/hoặc chỉ mục từ
khóa. 5. Lưu nội dung, metadata và vector.

**Query/retrieval:** 1. Nhận câu hỏi. 2. Chuyển câu hỏi thành embedding
hoặc truy vấn từ khóa. 3. Tìm chunks liên quan. 4. Có thể rerank. 5. Đưa
context vào prompt. 6. Yêu cầu Claude trả lời dựa trên nguồn, nêu rõ khi
không đủ dữ liệu.

## Bài 37. Text chunking strategies

Chunking là chia tài liệu dài thành các đoạn nhỏ để tìm kiếm và đưa vào
context.

Các chiến lược: - **Fixed-size:** chia theo số token/độ dài cố định. -
**Overlap:** các chunk chồng lấn một phần để giữ ngữ cảnh ở ranh giới. -
**Sentence/paragraph-based:** chia theo cấu trúc ngôn ngữ. -
**Semantic:** chia theo chủ đề/ngữ nghĩa. - **Structure-aware:** tôn
trọng heading, section, bảng, code block hoặc cấu trúc tài liệu.

### Trade-off

-   Chunk quá nhỏ: mất ngữ cảnh.
-   Chunk quá lớn: kết quả tìm kiếm kém chính xác và tốn token.
-   Overlap quá nhiều: trùng lặp, tăng storage và noise.
-   Cắt giữa bảng/code: làm hỏng ý nghĩa.

Lưu metadata như document ID, heading, trang, URL, quyền truy cập và
thời gian cập nhật. Khi truy xuất, phải áp dụng quyền tài liệu của người
dùng.

## Bài 38. Text embeddings

Embedding là biểu diễn văn bản thành vector số trong không gian nhiều
chiều. Những đoạn có ý nghĩa gần nhau thường có vector gần nhau theo
metric phù hợp.

Quy trình: 1. Chọn embedding model. 2. Chuyển chunks thành vectors. 3.
Lưu vectors trong vector database/index. 4. Embed query bằng cùng hệ
embedding tương thích. 5. Tìm nearest neighbors.

### Similarity

Cosine similarity thường được dùng để so độ gần hướng giữa hai vector.
Tuy nhiên, metric và cách chuẩn hóa phải phù hợp với embedding model và
vector store.

Embedding semantic search có thể tìm nội dung diễn đạt khác từ nhưng gần
nghĩa; nó không thay thế tìm kiếm từ khóa khi cần exact match, mã định
danh hoặc tên riêng.

## Bài 39. The full RAG flow

Một flow RAG đầy đủ:

1.  Người dùng gửi câu hỏi.
2.  Backend xác thực người dùng.
3.  Query được chuẩn hóa/rewritten nếu cần.
4.  Retrieval tìm các chunks có liên quan.
5.  Áp dụng permission filter.
6.  Reranking sắp xếp lại kết quả nếu cần.
7.  Chọn context trong ngân sách token.
8.  Gửi câu hỏi + context cho Claude.
9.  Claude tạo câu trả lời dựa trên tài liệu.
10. Backend trả câu trả lời và citations/nguồn.
11. Logging và eval chất lượng retrieval/generation.

### Prompt RAG gợi ý

``` text
Trả lời câu hỏi chỉ dựa trên tài liệu trong <sources>.
Nếu tài liệu không đủ thông tin, hãy nói rõ là chưa đủ dữ liệu.
Không tự tạo số liệu, chính sách hoặc trích dẫn.
Khi đưa ra nhận định, dẫn nguồn bằng document_id và vị trí nếu có.

<question>{{question}}</question>
<sources>
{{retrieved_chunks}}
</sources>
```

Retrieved content vẫn là dữ liệu không đáng tin; cần chống prompt
injection trong tài liệu.

## Bài 40. Implementing the RAG flow

Các thành phần triển khai: - Document loader. - Parser/normalizer. -
Chunker. - Embedding service. - Vector store. - Retriever. - Reranker
(tùy chọn). - Prompt builder. - Claude API client. - Citation mapper. -
Evaluation/monitoring.

Nên đo riêng: - Retrieval recall: có lấy được tài liệu cần thiết
không? - Retrieval precision: kết quả có liên quan không? - Answer
faithfulness: câu trả lời có được hỗ trợ bởi context không? - Answer
relevance: có trả lời đúng câu hỏi không? - Citation correctness: nguồn
dẫn có thật sự hỗ trợ nhận định không?

Lỗi RAG có thể đến từ retrieval kém hoặc generation kém. Cần xác định
tầng gây lỗi thay vì chỉ sửa prompt.

## Bài 41. BM25 lexical search

BM25 là thuật toán xếp hạng tìm kiếm từ khóa dựa trên tần suất term
trong tài liệu, độ dài tài liệu và độ phổ biến của term trong toàn bộ
corpus.

Ưu điểm: - Tốt với exact terms, tên sản phẩm, mã lỗi, mã đơn. - Không
cần embedding model. - Dễ hiểu và hiệu quả cho nhiều corpus.

Hạn chế: - Khó nhận ra từ đồng nghĩa hoặc diễn đạt khác. - Có thể bỏ lỡ
tài liệu liên quan về mặt ngữ nghĩa.

## Bài 42. A Multi-Index RAG pipeline

Hybrid/multi-index retrieval kết hợp nhiều phương pháp: - Vector
semantic search. - BM25 lexical search. - Metadata filters. - Các index
riêng theo loại tài liệu hoặc domain.

Kết quả có thể được hợp nhất bằng rank fusion hoặc một cơ chế reranking.
Ví dụ: - Người dùng hỏi "lỗi E-1042": BM25 có thể tìm chính xác mã
lỗi. - Người dùng hỏi "tại sao ứng dụng phản hồi chậm": vector search có
thể tìm tài liệu diễn đạt tương đương.

### Best practices

-   Đừng mặc định vector search luôn tốt hơn keyword search.
-   Lưu metadata để lọc đúng tenant/quyền truy cập.
-   Rerank khi có nhiều kết quả.
-   Giới hạn context theo token budget.
-   Đo chất lượng từng retrieval strategy bằng dataset thật.

------------------------------------------------------------------------

# 7. Phần VI -- Features of Claude

## Bài 43. Extended thinking

Extended thinking cho phép mô hình dành thêm khả năng xử lý cho các tác
vụ cần suy luận phức tạp, tùy model và cấu hình hỗ trợ.

Phù hợp: - Phân tích nhiều ràng buộc. - Lập kế hoạch nhiều bước. - Debug
phức tạp. - So sánh phương án có trade-off.

Không cần bật cho mọi request. Tác vụ đơn giản có thể không hưởng lợi
nhưng vẫn tăng latency/cost. Thiết kế ứng dụng nên chọn tính năng theo
độ khó và ngân sách; không dựa vào việc hiển thị suy nghĩ nội bộ như
bằng chứng rằng câu trả lời đúng.

## Bài 44. Image support

Claude có thể nhận hình ảnh để phân tích khi model/API hỗ trợ. Ứng dụng
cần: - Gửi ảnh theo định dạng được API chấp nhận. - Chọn media type
đúng. - Kiểm soát kích thước, độ phân giải và dung lượng. - Kiểm tra
quyền sử dụng và dữ liệu cá nhân. - Cung cấp câu hỏi cụ thể về ảnh.

Use cases: - Đọc biểu đồ/sơ đồ. - Phân tích screenshot giao diện. -
Trích xuất thông tin từ ảnh. - So sánh các vùng/đối tượng trong ảnh.

Giới hạn: ảnh mờ, chữ nhỏ, biểu đồ phức tạp hoặc ngữ cảnh thiếu có thể
gây sai. Cần xác minh số liệu quan trọng.

## Bài 45. PDF support

PDF có thể chứa text, bảng, biểu đồ và hình ảnh. Khi xử lý PDF: - Kiểm
tra kích thước và số trang. - Bảo đảm file hợp lệ và quyền truy cập. -
Dùng phương thức PDF input được API hỗ trợ. - Chia nhỏ tài liệu quá lớn
khi cần. - Giữ page number/metadata để dẫn nguồn. - Kiểm tra bảng,
footnote và bố cục phức tạp.

Không giả định trích xuất PDF luôn hoàn hảo. Scan chất lượng thấp có thể
cần OCR hoặc preprocessing.

## Bài 46. Citations

Citations giúp người dùng kiểm tra nguồn hỗ trợ câu trả lời. Một ứng
dụng có citation tốt cần: - Lưu nguồn gốc từng chunk. - Gắn document ID,
URL, page/section hoặc offsets. - Liên kết nhận định với đúng đoạn
nguồn. - Không tạo citation không tồn tại. - Kiểm tra nguồn có thật sự
hỗ trợ claim không.

Citation không chỉ là một URL đặt cuối câu. Nó phải cho phép người dùng
truy ngược đến bằng chứng cụ thể.

## Bài 47. Prompt caching

Prompt caching giúp tái sử dụng phần input lặp lại giữa nhiều request,
có thể giảm latency và chi phí cho workload phù hợp.

Ví dụ nội dung lặp: - System prompt dài. - Tool definitions. - Tài liệu
hướng dẫn cố định. - Context dùng chung của ứng dụng.

Nguyên tắc: - Đặt phần ổn định/lặp lại ở vị trí phù hợp theo quy tắc
caching hiện hành. - Để nội dung thay đổi theo request ở phía sau. -
Đừng cache dữ liệu nhạy cảm nếu chính sách không cho phép. - Theo dõi
cache read/write tokens và hiệu quả thực tế. - Cache có thời hạn và điều
kiện cụ thể; không giả định tồn tại vĩnh viễn.

## Bài 48. Rules of prompt caching

Cần tuân theo quy tắc về: - Vị trí cache breakpoint. - Phần prefix phải
giống nhau để cache hit. - Các loại content được hỗ trợ. - Thời gian
sống/TTL. - Cách tính usage và chi phí. - Những thay đổi nhỏ làm cache
miss.

Hãy đọc tài liệu caching hiện hành vì chi tiết API, TTL và cách tính phí
có thể thay đổi.

## Bài 49. Prompt caching in action

Ví dụ ứng dụng có system prompt và tài liệu chính sách dài giống nhau
cho hàng nghìn câu hỏi: 1. Tách phần instruction và policy ổn định. 2.
Đặt cache breakpoint theo API. 3. Mỗi request chỉ thay phần câu hỏi và
thông tin riêng. 4. Theo dõi cache hit/read/write. 5. So sánh latency và
chi phí trước/sau.

Không cache một prompt khổng lồ nếu nội dung thay đổi liên tục; cache
hiệu quả nhất khi có prefix lặp ổn định.

## Bài 50. Code execution and the Files API

Một số workflow có thể cho Claude sử dụng môi trường thực thi code hoặc
thao tác với file thông qua khả năng được API cung cấp.

Use cases: - Phân tích dữ liệu. - Tạo/chuyển đổi file. - Chạy tính
toán. - Xử lý tài liệu đầu vào và tạo artifact đầu ra.

Cần quan tâm: - Sandbox và giới hạn tài nguyên. - Quyền đọc/ghi file. -
File size/type và thời hạn lưu trữ. - Dữ liệu nhạy cảm. - Kiểm tra
output trước khi gửi cho người dùng. - Không thực thi code/file không
đáng tin trên hệ thống production.

Tính năng, tên API và quyền truy cập phụ thuộc sản phẩm/model/account;
cần đối chiếu tài liệu hiện hành.

## Bài 51. Quiz on features of Claude --- kiến thức cần nhớ

-   Extended thinking phù hợp tác vụ suy luận khó, không phải mọi
    request.
-   Image/PDF input cần xử lý định dạng và giới hạn.
-   Citation phải truy ngược được đến nguồn.
-   Prompt caching hữu ích với prefix lặp lại.
-   Theo dõi cache hit và token usage.
-   Code execution/Files cần sandbox, quyền và kiểm tra output.

------------------------------------------------------------------------

# 8. Phần VII -- Model Context Protocol (MCP)

## Bài 52. Introducing MCP

MCP (Model Context Protocol) là giao thức mở chuẩn hóa cách ứng dụng AI
kết nối với tools, resources và prompts từ các hệ thống bên ngoài.

### Vì sao cần MCP?

Nếu mỗi ứng dụng AI phải tích hợp từng dịch vụ bằng giao thức riêng,
việc xây dựng và bảo trì rất tốn công. MCP tạo một lớp giao tiếp chung
để client có thể làm việc với nhiều server tương thích.

### Kiến trúc cơ bản

-   **Host:** ứng dụng AI chứa MCP client.
-   **Client:** quản lý kết nối và giao tiếp với một server.
-   **Server:** cung cấp capabilities.
-   **Tools:** hành động có thể gọi.
-   **Resources:** dữ liệu có thể đọc.
-   **Prompts:** template/quy trình prompt được server công bố.

MCP không tự động làm dịch vụ an toàn; host và server vẫn cần xác thực,
authorization và kiểm soát dữ liệu.

## Bài 53. MCP clients

MCP client là thành phần trong host kết nối với MCP server. Nó thực
hiện: - Khởi tạo kết nối. - Trao đổi thông tin capabilities. - Khám phá
tools/resources/prompts. - Gửi request và nhận response. - Xử lý lỗi,
timeout và lifecycle.

Một host có thể có nhiều client kết nối nhiều MCP servers. Mỗi server có
thể cung cấp nhóm chức năng riêng.

## Bài 54. Project setup

Khi bắt đầu một MCP project: 1. Xác định use case và capabilities cần
thiết. 2. Chọn SDK/ngôn ngữ. 3. Tạo project và dependency. 4. Xác định
transport phù hợp. 5. Xây server/client tối thiểu. 6. Kiểm tra bằng
inspector/debug tool. 7. Thêm authentication, authorization và logging.
8. Viết integration tests.

Tách business logic khỏi lớp giao thức để có thể kiểm thử và tái sử dụng
độc lập.

## Bài 55. Defining tools with MCP

MCP server có thể khai báo tools với: - Tên và mô tả. - Input schema. -
Handler thực thi. - Kết quả có cấu trúc hoặc nội dung phù hợp.

Tool nên có mô tả dễ hiểu, schema chặt, lỗi có thể xử lý và giới hạn
quyền. Phân biệt rõ tool chỉ đọc với tool có tác dụng phụ như tạo, sửa
hoặc xóa.

## Bài 56. The server inspector

Inspector là công cụ hỗ trợ kiểm tra MCP server: - Server khởi động được
không? - Capabilities nào được công bố? - Tools/resources/prompts có
schema đúng không? - Gọi tool có trả kết quả đúng không? - Lỗi có dễ
hiểu không?

Dùng inspector trong phát triển để debug trước khi kết nối vào ứng dụng
lớn.

## Bài 57. Implementing a client

Một MCP client cần: 1. Kết nối tới server qua transport phù hợp. 2. Khởi
tạo session/handshake. 3. Khám phá capabilities. 4. Lấy danh sách
tools/resources/prompts. 5. Gọi capability cần thiết. 6. Xử lý kết quả
và lỗi. 7. Đóng kết nối đúng lifecycle.

Không hardcode giả định rằng mọi server đều cung cấp cùng một nhóm
tools.

## Bài 58. Defining resources

Resources là dữ liệu mà MCP server cung cấp để client đọc, ví dụ: - Tài
liệu. - Nội dung database được phép truy cập. - Metadata dự án. - File
hoặc trạng thái hệ thống.

Resource thường được định danh bằng URI và có metadata/content tương
ứng. Cần xác định quyền đọc, loại nội dung và cách cập nhật.

## Bài 59. Accessing resources

Client có thể khám phá và đọc resources từ server. Khi triển khai: -
Kiểm tra URI và quyền. - Xử lý resource không tồn tại. - Quản lý nội
dung lớn. - Giữ metadata nguồn. - Không mặc định nội dung resource là
instruction đáng tin.

Resources cung cấp context; tools thực hiện hành động. Phân biệt hai
loại này giúp thiết kế hệ thống rõ ràng hơn.

## Bài 60. Defining prompts

MCP server có thể cung cấp prompt templates để client sử dụng lại các
workflow chuẩn.

Một prompt template nên: - Có tên và mô tả. - Khai báo arguments cần
thiết. - Tách input động khỏi nội dung cố định. - Có hướng dẫn rõ và
output mong đợi.

Ví dụ: prompt `summarize_issue` nhận issue ID hoặc nội dung issue, sau
đó tạo tóm tắt theo format thống nhất.

## Bài 61. Prompts in the client

Client có thể khám phá prompt templates và yêu cầu server tạo prompt với
arguments. Sau đó host đưa prompt content vào cuộc hội thoại/model
request theo workflow của mình.

Lợi ích: - Tái sử dụng prompt giữa ứng dụng. - Tập trung hóa quy
trình. - Giảm trùng lặp. - Cho phép server cập nhật template.

Không nên xem prompt template từ server bên ngoài là mặc nhiên đáng tin.
Cần kiểm tra nguồn và quyền trước khi dùng.

## Bài 62. Quiz on MCP --- kiến thức cần nhớ

-   MCP chuẩn hóa giao tiếp giữa host/client/server.
-   Tools thực hiện hành động; resources cung cấp dữ liệu; prompts cung
    cấp template.
-   Client cần discovery và lifecycle management.
-   Inspector hỗ trợ kiểm thử server.
-   Cần xác thực, authorization, validation và logging.
-   Nội dung từ server vẫn cần được xem xét theo mức độ tin cậy.

------------------------------------------------------------------------

# 9. Phần VIII -- Anthropic Apps: Claude Code and Computer Use

## Bài 63. Anthropic apps: Claude Code and Computer Use

Khóa học giới thiệu cách các ứng dụng Anthropic có thể tham gia vào
workflow phát triển và tự động hóa.

**Claude Code** là coding agent có thể khám phá repository, chỉnh sửa
file và chạy lệnh trong phạm vi quyền được cấp.

**Computer Use** cho phép mô hình tương tác với giao diện máy tính thông
qua các thao tác UI được hệ thống hỗ trợ. Nó hữu ích khi công việc chỉ
có thể thực hiện qua giao diện, nhưng cần guardrails mạnh vì thao tác UI
có thể tạo tác động thật.

### Phân biệt

-   API: tích hợp năng lực model vào ứng dụng của bạn.
-   Claude Code: agent chuyên cho công việc phát triển phần mềm.
-   Computer Use: tương tác với UI/desktop.
-   MCP: giao thức kết nối công cụ và dữ liệu.

## Bài 64. Claude Code setup

Khi thiết lập Claude Code: - Chọn môi trường phù hợp
(terminal/IDE/desktop/web được hỗ trợ). - Cài đặt theo hướng dẫn chính
thức. - Xác thực tài khoản hoặc cấu hình credential đúng cách. - Mở đúng
repository. - Kiểm tra Git state và quyền. - Thử task chỉ đọc trước khi
cho phép sửa.

Không dùng một lệnh cài đặt cũ từ tài liệu không cập nhật; quy trình cài
đặt thay đổi theo nền tảng và phiên bản.

## Bài 65. Claude Code in action

Một workflow phát triển điển hình: 1. Explore codebase. 2. Plan thay
đổi. 3. Implement từng phần. 4. Run tests/build. 5. Review diff. 6.
Commit sau khi được xác nhận.

Có thể dùng Claude Code để khảo sát code, thêm tính năng, debug, viết
test, review và cập nhật tài liệu. Luôn quản lý context, permissions và
trạng thái Git.

## Bài 66. Enhancements with MCP servers

MCP có thể mở rộng Claude Code bằng cách kết nối dịch vụ ngoài như issue
tracker, Git provider, database hoặc tài liệu nội bộ.

Quy trình: 1. Chọn server phù hợp. 2. Xác minh nguồn và quyền. 3. Cấu
hình kết nối. 4. Kiểm tra capabilities. 5. Thử thao tác chỉ đọc. 6. Chỉ
cấp quyền ghi khi cần. 7. Theo dõi hành động và dữ liệu được truy cập.

MCP không thay thế permission model của ứng dụng. Mỗi tool vẫn cần
authorization và kiểm tra input.

------------------------------------------------------------------------

# 10. Phần IX -- Agents and Workflows

## Bài 67. Agents and workflows

Agent là hệ thống sử dụng model để quyết định bước tiếp theo, có thể gọi
tools, quan sát kết quả và tiếp tục. Workflow thường là chuỗi bước được
thiết kế trước; agent có quyền linh hoạt chọn hành động trong phạm vi
cho phép.

Một hệ thống agent cần: - Mục tiêu rõ. - Context và nguồn dữ liệu. -
Tools được giới hạn. - Quy tắc dừng. - Cơ chế kiểm chứng. - Logging,
tracing và eval. - Human approval ở các bước rủi ro.

Không nên tạo agent tự chủ chỉ vì có thể. Với tác vụ ổn định, workflow
xác định thường dễ kiểm thử, dự đoán và vận hành hơn.

## Bài 68. Parallelization workflows

Parallelization chia task thành các phần độc lập có thể xử lý đồng thời.

Ví dụ: - Tóm tắt nhiều tài liệu riêng biệt. - Phân tích nhiều nhóm phản
hồi. - Review nhiều module độc lập. - Tìm kiếm trên nhiều nguồn.

Kiến trúc: 1. Decompose task thành sub-tasks. 2. Chạy các sub-tasks song
song. 3. Thu kết quả. 4. Tổng hợp/đối chiếu. 5. Kiểm tra tính nhất quán.

### Khi nào phù hợp?

-   Các sub-task ít phụ thuộc nhau.
-   Có thể chạy độc lập.
-   Có cách hợp nhất kết quả rõ ràng.

### Rủi ro

-   Chi phí tăng theo số worker.
-   Kết quả mâu thuẫn.
-   Giới hạn rate/concurrency.
-   Một subtask lỗi làm thiếu kết quả.
-   Race condition nếu nhiều worker cùng ghi một tài nguyên.

Dùng concurrency limit, timeout, retry có kiểm soát và aggregator có
tiêu chí.

## Bài 69. Chaining workflows

Chaining là nối nhiều bước theo thứ tự, output bước trước làm input bước
sau.

Ví dụ: 1. Trích xuất dữ liệu từ tài liệu. 2. Chuẩn hóa dữ liệu. 3. Phân
loại. 4. Tạo báo cáo. 5. Kiểm tra kết quả.

Phù hợp khi nhiệm vụ có thứ tự logic và mỗi bước có output contract rõ.

Best practices: - Định nghĩa schema giữa các bước. - Validate output
trước khi chuyển bước. - Lưu intermediate results. - Xử lý lỗi/retry ở
từng stage. - Tránh truyền toàn bộ output thừa sang bước sau.

Chaining thường dễ debug hơn một agent lớn nếu quy trình đã biết trước.

## Bài 70. Routing workflows

Routing chọn đường xử lý dựa trên input hoặc kết quả phân loại.

Ví dụ: - Câu hỏi billing → billing workflow. - Câu hỏi kỹ thuật →
technical support. - Yêu cầu hoàn tiền → quy trình cần xác minh và phê
duyệt. - Input không rõ → hỏi thêm hoặc chuyển người thật.

Router có thể dùng rule-based logic hoặc model classifier. Với nhánh rủi
ro cao, nên dùng rule/validation rõ ràng thay vì để model tự quyết hoàn
toàn.

## Bài 71. Agents and tools

Tools mở rộng khả năng của agent. Agent chọn tool dựa trên mục tiêu,
context và kết quả trước đó.

Thiết kế tốt: - Tool nhỏ, rõ trách nhiệm. - Schema chặt. - Mô tả chính
xác. - Permission theo least privilege. - Tool result có cấu trúc. - Có
timeout, retry và idempotency khi phù hợp.

Agent cần giới hạn số bước/vòng lặp, ngân sách token và chi phí. Với
hành động có tác dụng phụ, dùng confirmation hoặc approval.

## Bài 72. Environment inspection

Environment inspection là việc agent kiểm tra môi trường trước khi hành
động: - File và thư mục hiện có. - Phiên bản runtime/dependency. - Trạng
thái Git. - Cấu hình và khả năng của công cụ. - Dữ liệu đầu vào và điều
kiện thực thi.

Đây là bước quan trọng vì agent không nên giả định môi trường giống nhau
giữa local, staging và production.

Chỉ cho phép agent xem các thông tin cần thiết. Không để inspection vô
tình làm lộ environment variables, credentials hoặc dữ liệu riêng tư.

## Bài 73. Workflows vs agents

  -----------------------------------------------------------------------
  Tiêu chí                Workflow                Agent
  ----------------------- ----------------------- -----------------------
  Luồng xử lý             Định nghĩa trước        Model chọn bước tiếp
                                                  theo trong phạm vi

  Tính dự đoán            Thường cao hơn          Biến thiên hơn

  Kiểm thử                Dễ kiểm thử từng bước   Cần eval theo hành vi
                                                  và tool decisions

  Phù hợp                 Quy trình ổn định, lặp  Nhiệm vụ mở, cần thích
                          lại                     ứng

  Kiểm soát               Dễ giới hạn             Cần guardrails, budget
                                                  và stop conditions
  -----------------------------------------------------------------------

### Chọn kiến trúc

-   Dùng workflow khi các bước đã biết và cần độ tin cậy cao.
-   Dùng agent khi cần khám phá, quyết định hoặc thích ứng.
-   Dùng hybrid khi phần lớn quy trình cố định nhưng có một số bước cần
    model lựa chọn.

## Bài 74. Quiz on Agents and Workflows --- kiến thức cần nhớ

-   Parallelization cho các task độc lập.
-   Chaining cho các bước phụ thuộc tuần tự.
-   Routing chọn nhánh xử lý.
-   Agent dùng model để quyết định bước tiếp theo và gọi tools.
-   Workflow thường dễ dự đoán và kiểm thử hơn.
-   Hybrid thường là lựa chọn thực tế.
-   Luôn có giới hạn, kiểm chứng và human approval phù hợp.

------------------------------------------------------------------------

# 11. Kiến trúc tham khảo cho một ứng dụng Claude API production

Một kiến trúc tổng quát:

``` text
User / Frontend
      |
      v
Backend API
  - Authentication / Authorization
  - Input validation
  - Rate limiting
      |
      v
Application Orchestrator
  - Prompt builder
  - Conversation state
  - Model selection
  - Tool registry
  - RAG retrieval
      |
      +------> Anthropic Messages API
      |
      +------> Tool services / MCP servers
      |
      +------> Vector DB / BM25 index
      |
      v
Post-processing
  - Schema validation
  - Citation mapping
  - Policy checks
  - Logging / tracing
      |
      v
Response to user
```

### Các nguyên tắc production

**Security** - API key chỉ ở backend/secret manager. - Authorization
thực hiện ở backend, không giao cho model. - Giới hạn tool và data
access. - Chống prompt injection từ user, web và retrieved documents. -
Không log secret hoặc dữ liệu nhạy cảm không cần thiết.

**Reliability** - Timeout và retry có giới hạn. - Xử lý rate limit và
lỗi API. - Idempotency cho thao tác có side effect. - Fallback hoặc
human handoff khi cần. - Giới hạn tool loop và agent budget.

**Quality** - Eval dataset có đại diện. - Code-based và model-based
graders. - Human review cho tình huống rủi ro. - Theo dõi hallucination,
tool errors và citation quality.

**Cost and performance** - Theo dõi input/output tokens. - Dùng
streaming khi UX cần. - Cân nhắc prompt caching với prefix lặp lại. -
Chọn model theo độ khó. - Giới hạn context và retrieval results. - Chạy
song song khi task độc lập và chi phí hợp lý.

**Observability** - Ghi request ID, model, prompt version, latency,
usage và tool calls. - Không ghi dữ liệu nhạy cảm không cần thiết. -
Theo dõi lỗi theo từng stage. - Có dashboard cho cost, latency, error
rate và quality.

------------------------------------------------------------------------

# 12. Bảng chọn kỹ thuật theo bài toán

  Nhu cầu                                       Kỹ thuật nên xem xét
  --------------------------------------------- ----------------------------------------
  Chat nhiều lượt                               Message history + context management
  Đầu ra phải theo schema                       Structured output + backend validation
  Muốn hành động qua API/database               Tool use / function calling
  Trả lời theo tài liệu nội bộ                  RAG
  Tìm mã lỗi/tên riêng chính xác                BM25/lexical search
  Tìm theo ý nghĩa                              Embeddings/vector search
  Kết hợp exact và semantic search              Hybrid/multi-index retrieval
  Tích hợp tool chuẩn hóa giữa nhiều ứng dụng   MCP
  Giảm latency/chi phí với prefix lặp           Prompt caching
  Phản hồi dần cho UI                           Streaming
  Công việc cố định nhiều bước                  Workflow/chaining
  Nhiệm vụ cần tự chọn hành động                Agent
  Nhiều nhánh độc lập                           Parallelization
  Phân loại để chuyển quy trình                 Routing
  Đánh giá prompt/model                         Evaluation pipeline

------------------------------------------------------------------------

# 13. Checklist trước khi đưa ứng dụng lên production

## API integration

-   [ ] API key nằm ở backend/secret manager.
-   [ ] Model ID và tham số được cấu hình tập trung.
-   [ ] Có timeout, retry và xử lý rate limit.
-   [ ] Kiểm tra `stop_reason`, content blocks và usage.
-   [ ] Lịch sử hội thoại có giới hạn.

## Prompt và output

-   [ ] System prompt có vai trò, phạm vi và quy tắc rõ.
-   [ ] User input và retrieved data được phân định.
-   [ ] Có schema cho output máy đọc.
-   [ ] Backend validate output và business rules.
-   [ ] Có cách xử lý khi model thiếu dữ liệu.

## Tools và MCP

-   [ ] Tool schema rõ và chặt.
-   [ ] Backend kiểm tra quyền cho từng hành động.
-   [ ] Có timeout, logging và error handling.
-   [ ] Giới hạn số vòng tool/agent.
-   [ ] MCP server đáng tin và chỉ có quyền cần thiết.
-   [ ] Hành động ghi/xóa cần approval phù hợp.

## RAG và dữ liệu

-   [ ] Chunking phù hợp cấu trúc tài liệu.
-   [ ] Embedding/index có versioning.
-   [ ] Có metadata và permission filtering.
-   [ ] Có kiểm tra retrieval quality.
-   [ ] Citations truy ngược được về nguồn.
-   [ ] Có xử lý prompt injection trong tài liệu.

## Evaluation và vận hành

-   [ ] Có test dataset đại diện.
-   [ ] Có baseline và regression eval.
-   [ ] Kết hợp code/model/human grading phù hợp.
-   [ ] Theo dõi token, cost, latency, error rate.
-   [ ] Có logging/tracing nhưng không lộ dữ liệu nhạy cảm.
-   [ ] Có fallback và human handoff.

------------------------------------------------------------------------

# 14. Tổng kết và câu hỏi ôn tập

## 14.1. Lộ trình tự học và thực hành

1.  **API basics:** tạo Python project, gọi Messages API, quản lý API
    key an toàn.
2.  **Multi-turn chat:** xây chat endpoint có conversation ID và giới
    hạn lịch sử.
3.  **Prompt engineering:** thử prompt rõ ràng, XML tags và few-shot
    examples.
4.  **Structured output:** trích xuất dữ liệu thành JSON schema,
    validate ở backend.
5.  **Evaluation:** tạo 30--50 test cases, chạy baseline và so sánh
    prompt.
6.  **Tool use:** tạo tool tra cứu dữ liệu chỉ đọc, sau đó thêm tool có
    approval.
7.  **RAG:** nạp tài liệu, chunk, embed, vector search và citation.
8.  **Hybrid search:** thêm BM25 và so sánh với semantic search.
9.  **MCP:** xây server nhỏ cung cấp một tool và một resource; kết nối
    bằng client.
10. **Agent/workflow:** xây workflow chaining trước, sau đó thử
    routing/parallelization.
11. **Production:** thêm rate limit, logging, retry, eval, cost
    monitoring và human handoff.

------------------------------------------------------------------------

## 14.2. Câu hỏi ôn tập

1.  Messages API cần những thành phần cốt lõi nào?
2.  Vì sao backend cần tự quản lý multi-turn history?
3.  System prompt khác user message thế nào?
4.  Temperature ảnh hưởng điều gì và không bảo đảm điều gì?
5.  Streaming thay đổi trải nghiệm người dùng ra sao?
6.  Structured output giải quyết vấn đề gì? Vì sao vẫn cần validation?
7.  Prompt eval cần những thành phần nào?
8.  Model-based grading khác code-based grading thế nào?
9.  Vì sao dataset cần edge cases và holdout set?
10. XML tags giúp prompt dài dễ hiểu hơn thế nào?
11. Few-shot examples có tác dụng gì?
12. Ai thực thi tool call: Claude hay backend?
13. `tool_use_id` dùng để làm gì?
14. Vì sao phải giới hạn tool loop?
15. RAG gồm hai giai đoạn lớn nào?
16. Chunking quá nhỏ hoặc quá lớn gây vấn đề gì?
17. Embeddings và BM25 khác nhau ra sao?
18. Vì sao cần hybrid retrieval?
19. Citation tốt cần truy ngược được những gì?
20. Prompt caching có lợi nhất khi nào?
21. MCP phân biệt tools, resources và prompts thế nào?
22. MCP host, client và server có vai trò gì?
23. Khi nào dùng workflow thay vì agent?
24. Parallelization, chaining và routing khác nhau thế nào?
25. Những guardrails nào cần có trước khi đưa agent vào production?

## Đáp án gợi ý

1.  Model, max token, messages và các tham số cần thiết.
2.  API request không mặc nhiên giữ toàn bộ state ứng dụng; backend phải
    quản lý context.
3.  System prompt là chỉ dẫn cấp ứng dụng; user message là input của
    người dùng.
4.  Temperature điều chỉnh mức độ biến thiên, không bảo đảm sự thật/độ
    chính xác.
5.  Hiển thị output dần, giảm thời gian chờ cảm nhận.
6.  Chuẩn hóa format; vẫn phải kiểm tra schema và business rules ở
    backend.
7.  Dataset, prompt/config, runner, grader và phân tích kết quả.
8.  Model grader đánh giá chất lượng ngữ nghĩa; code grader kiểm tra
    điều kiện xác định.
9.  Để đánh giá khả năng tổng quát và tránh chỉ tối ưu trên ví dụ dễ.
10. Phân vùng task, context, dữ liệu và format.
11. Minh họa pattern input-output.
12. Backend/application thực thi sau khi xác thực tool call.
13. Ghép tool result với đúng tool call.
14. Tránh vòng lặp vô hạn, tăng chi phí hoặc hành động ngoài ý muốn.
15. Ingestion/indexing và query/retrieval-generation.
16. Chunk nhỏ mất ngữ cảnh; chunk lớn giảm độ chính xác và tốn token.
17. Embedding tìm tương đồng ngữ nghĩa; BM25 tìm theo term.
18. Để kết hợp exact match với semantic match.
19. Document ID, URL, trang/section hoặc vị trí nguồn.
20. Khi prefix/input ổn định được lặp lại qua nhiều request.
21. Tools thực hiện hành động; resources cung cấp dữ liệu; prompts cung
    cấp template.
22. Host chứa client; client giao tiếp; server cung cấp capabilities.
23. Khi quy trình cố định, cần dễ kiểm thử và dự đoán.
24. Parallel chạy độc lập đồng thời; chaining nối tuần tự; routing chọn
    nhánh.
25. Least privilege, validation, approval, limits, monitoring, eval và
    fallback.

------------------------------------------------------------------------

## Nguồn tham khảo

### Tài liệu khóa học

- [Claude Academy --- Building with the Claude API](https://academy.claude.com/courses/building-with-the-claude-api)
- [Anthropic API Documentation](https://docs.anthropic.com/en/api)

### Tài liệu bổ sung

- [Anthropic Python SDK](https://github.com/anthropics/anthropic-sdk-python)
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)

> **Ghi chú về phạm vi:** Tài liệu này tổng hợp cấu trúc và nội dung cốt lõi của khóa học gồm 67 bài học và 8 quizzes, kết hợp giải thích lý thuyết, code mẫu Python và lưu ý thực chiến production. Đây là tài liệu học tập được biên soạn lại, không phải bản transcript nguyên văn. Các tham số API và cú pháp SDK có thể cập nhật theo phiên bản mới; hãy đối chiếu tài liệu chính thức khi triển khai.


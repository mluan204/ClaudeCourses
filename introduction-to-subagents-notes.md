# Introduction to Subagents

## Tài liệu học tập -- Claude Academy

> **Khóa học:** Introduction to subagents\
> **Nền tảng:** Claude Code\
> **Đơn vị:** Anthropic / Claude Academy\
> **Thời lượng khóa học:** khoảng 45 phút\
> **Số bài học:** 4\
> **Mục tiêu:** Hiểu cách subagent hoạt động, tự tạo subagent chuyên
> biệt, thiết kế chúng đáng tin cậy và biết khi nào nên hoặc không nên
> giao việc cho chúng.

------------------------------------------------------------------------

## Mục lục

1.  [Tổng quan về Subagents](#1-tổng-quan-về-subagents)
2.  [Lesson 1 -- What are subagents?](#2-lesson-1--what-are-subagents)
3.  [Lesson 2 -- Creating a subagent](#3-lesson-2--creating-a-subagent)
4.  [Lesson 3 -- Designing effective
    subagents](#4-lesson-3--designing-effective-subagents)
5.  [Lesson 4 -- Using subagents
    effectively](#5-lesson-4--using-subagents-effectively)
6.  [Quy trình thiết kế một hệ thống
    Subagents](#6-quy-trình-thiết-kế-một-hệ-thống-subagents)
7.  [Ví dụ thực tế](#7-ví-dụ-thực-tế)
8.  [Những lỗi và anti-pattern cần
    tránh](#8-những-lỗi-và-anti-pattern-cần-tránh)
9.  [Checklist đánh giá Subagent](#9-checklist-đánh-giá-subagent)
10. [Tổng kết và câu hỏi ôn tập](#10-tổng-kết-và-câu-hỏi-ôn-tập)

------------------------------------------------------------------------

# 1. Tổng quan về Subagents

## 1.1. Subagent là gì?

**Subagent** là một agent phụ được Claude Code khởi chạy để thực hiện
một nhiệm vụ chuyên biệt trong một **context window riêng biệt**. Agent
chính (main agent) giao nhiệm vụ cho subagent, subagent tự thực hiện
công việc trong ngữ cảnh riêng, sau đó trả kết quả hoặc bản tóm tắt về
cho agent chính.

Có thể hình dung:

-   **Main agent:** Người điều phối, hiểu mục tiêu tổng thể, chia nhỏ
    công việc và tổng hợp kết quả.
-   **Subagent:** Người thực thi một nhiệm vụ cụ thể, có chỉ dẫn và
    quyền sử dụng công cụ phù hợp với nhiệm vụ đó.

Ví dụ: Khi yêu cầu Claude Code xây dựng một tính năng lớn, agent chính
có thể giao: - Subagent nghiên cứu cấu trúc codebase và tìm nơi cần thay
đổi. - Subagent review bảo mật cho phần authentication. - Subagent kiểm
tra tài liệu API hiện có.

Mỗi subagent tập trung vào phạm vi riêng và gửi lại phát hiện quan
trọng. Agent chính dùng kết quả đó để đưa ra quyết định hoặc tiếp tục
triển khai.

## 1.2. Vì sao cần Subagents?

Trong các phiên làm việc dài, context window của agent chính có thể bị
lấp đầy bởi: - Nội dung đọc từ nhiều file. - Log, kết quả tìm kiếm và
output từ terminal. - Các bước điều tra trung gian. - Những chi tiết
không cần thiết cho quyết định cuối cùng.

Subagent giải quyết vấn đề bằng cách đưa công việc phụ sang một context
độc lập. Agent chính không cần chứa toàn bộ quá trình suy nghĩ và dữ
liệu trung gian của subagent; nó chỉ nhận phần kết quả cần thiết.

### Ba lợi ích cốt lõi

**1. Focused execution -- Tập trung nhiệm vụ**

Subagent được giao một mục tiêu cụ thể nên có thể tập trung vào một lĩnh
vực như code review, nghiên cứu codebase hoặc viết tài liệu.

**2. Context isolation -- Cô lập ngữ cảnh**

Subagent có context window riêng. Nội dung điều tra dài không trực tiếp
làm nhiễu context của agent chính.

**3. Concise handoff -- Bàn giao kết quả có chọn lọc**

Subagent trả lại kết luận, bằng chứng và các thông tin cần thiết thay vì
đưa toàn bộ quá trình làm việc vào hội thoại chính.

> Lưu ý: Subagent không làm biến mất chi phí context. Nó chuyển phần lớn
> công việc sang context riêng, nhưng agent chính vẫn phải tiếp nhận và
> xử lý kết quả trả về. Vì vậy, báo cáo quá dài vẫn có thể làm tăng
> context của agent chính.

## 1.3. Subagent hoạt động như thế nào?

Luồng cơ bản:

1.  Agent chính nhận yêu cầu tổng thể.
2.  Agent chính xác định một phần việc có thể giao độc lập.
3.  Agent chính khởi chạy subagent và gửi prompt/nhiệm vụ cùng thông tin
    cần thiết.
4.  Subagent bắt đầu trong context window riêng, sử dụng system prompt
    và bộ công cụ được cấp.
5.  Subagent thực hiện nhiệm vụ, thu thập bằng chứng và tạo kết quả.
6.  Subagent gửi thông điệp cuối cùng hoặc bản tóm tắt về agent chính.
7.  Agent chính sử dụng kết quả để tổng hợp, quyết định hoặc tiếp tục
    công việc.

``` text
User request
     |
     v
 Main Agent
     |
     +-------- Delegate task A --------> Subagent A
     |                                      |
     |                                  Own context
     |                                      |
     |                                  Result / summary
     |                                      |
     +<-------------------------------------+
     |
     +-------- Delegate task B --------> Subagent B
     |                                      |
     |                                  Own context
     |                                      |
     |                                  Result / summary
     |                                      |
     +<-------------------------------------+
     |
     v
 Synthesize results
     |
     v
 Final response / next action
```

## 1.4. Context Window của Subagent

Mỗi subagent có context window độc lập với agent chính. Điều này có một
số ý nghĩa:

-   Subagent không mặc nhiên nhìn thấy toàn bộ lịch sử hội thoại của
    agent chính.
-   Agent chính cần truyền đủ thông tin, mục tiêu, phạm vi và các ràng
    buộc cần thiết.
-   Subagent sử dụng system prompt riêng được định nghĩa trong cấu hình
    của nó.
-   Khi hoàn thành, kết quả được trả về cho agent chính; phần lớn nội
    dung trung gian không được đưa nguyên vẹn vào hội thoại chính.

**Nguyên tắc quan trọng:** Đừng cho rằng subagent tự biết mọi thứ agent
chính đã biết. Hãy xem việc giao nhiệm vụ như một lần bàn giao công
việc: cung cấp đủ context để người nhận có thể hoàn thành độc lập.

## 1.5. Built-in Subagents và Custom Subagents

Claude Code có các subagent tích hợp sẵn và cho phép tạo subagent tùy
chỉnh.

Một số built-in subagent thường gặp:

  -----------------------------------------------------------------------
  Subagent                            Vai trò
  ----------------------------------- -----------------------------------
  `Explore`                           Khám phá, tìm kiếm và điều hướng
                                      codebase; phù hợp với công việc
                                      đọc/khảo sát.

  `Plan`                              Nghiên cứu và phân tích codebase
                                      trong quá trình lập kế hoạch trước
                                      khi đề xuất hướng triển khai.

  `general-purpose`                   Thực hiện các nhiệm vụ tổng quát
                                      khi cần một agent phụ có khả năng
                                      sử dụng nhiều công cụ.
  -----------------------------------------------------------------------

Tên, khả năng và cách cấu hình built-in agent có thể thay đổi theo phiên
bản Claude Code.

**Custom subagent** là agent do người dùng định nghĩa. Ví dụ: -
`code-reviewer`: Review chất lượng và lỗi tiềm ẩn trong code. -
`security-auditor`: Kiểm tra các vấn đề bảo mật theo checklist. -
`documentation-writer`: Tạo hoặc cập nhật tài liệu kỹ thuật. -
`test-writer`: Phân tích yêu cầu và đề xuất hoặc viết test.

### Khi nào cần Custom Subagent?

Tạo custom subagent khi một công việc: - Lặp đi lặp lại trong nhiều
phiên làm việc. - Có phạm vi, tiêu chí hoàn thành và quy trình tương đối
ổn định. - Cần system prompt chuyên biệt. - Cần giới hạn tool access
khác với agent chính. - Cần một định dạng kết quả thống nhất để agent
chính dễ tổng hợp.

------------------------------------------------------------------------

# 2. Lesson 1 -- What are subagents?

## 2.1. Mục tiêu bài học

Sau bài này, cần hiểu: - Subagent là gì và giải quyết vấn đề nào. - Cách
Claude Code tạo context riêng cho subagent. - Dữ liệu được truyền vào và
kết quả được trả về như thế nào. - Vai trò của built-in subagents và
custom subagents.

## 2.2. Cơ chế giao việc (Delegation)

Delegation là việc agent chính giao một nhiệm vụ cụ thể cho subagent
thay vì tự thực hiện toàn bộ.

Một nhiệm vụ tốt để giao thường có: - Mục tiêu rõ ràng. - Phạm vi xác
định. - Kết quả mong đợi có thể mô tả. - Có thể thực hiện tương đối độc
lập. - Không yêu cầu agent chính phải liên tục can thiệp vào từng bước.

Ví dụ chưa tốt:

> Hãy kiểm tra toàn bộ dự án và cải thiện mọi thứ.

Vấn đề: phạm vi quá rộng, không có tiêu chí thành công, không xác định
output.

Ví dụ tốt hơn:

> Khảo sát luồng authentication trong thư mục `src/auth`. Không chỉnh
> sửa file. Xác định các điểm vào chính, cách xác thực token, nơi kiểm
> tra quyền truy cập và các rủi ro đáng chú ý. Trả về danh sách file
> liên quan, mô tả luồng xử lý và các phát hiện kèm bằng chứng.

Nhiệm vụ thứ hai có phạm vi, giới hạn và output rõ ràng hơn.

## 2.3. Input và Output của Subagent

### Input thường cần có

1.  **Task:** Công việc cụ thể cần thực hiện.
2.  **Context:** Thông tin nền cần thiết.
3.  **Scope:** Những thư mục, file hoặc chủ đề cần tập trung.
4.  **Constraints:** Những việc được phép và không được phép làm.
5.  **Expected output:** Cấu trúc kết quả cần trả về.

### Output nên có

-   Kết luận chính.
-   Bằng chứng hoặc đường dẫn file liên quan.
-   Các vấn đề phát hiện được.
-   Những giả định đã sử dụng.
-   Trở ngại hoặc phần chưa thể xác minh.
-   Đề xuất bước tiếp theo nếu phù hợp.

Output càng có cấu trúc thì agent chính càng dễ sử dụng.

## 2.4. Tư duy quan trọng: Subagent không phải bản sao của Main Agent

Không nên xem subagent là một bản sao có đầy đủ lịch sử và mục tiêu của
agent chính. Nó là một worker chuyên biệt được giao một nhiệm vụ.

Agent chính cần làm tốt ba việc: 1. Chọn đúng phần việc để giao. 2.
Truyền đúng thông tin. 3. Kiểm tra và tích hợp kết quả.

Nếu đầu vào mơ hồ, subagent có thể hoàn thành một công việc khác với
điều agent chính thực sự cần.

------------------------------------------------------------------------

# 3. Lesson 2 -- Creating a subagent

## 3.1. Mục tiêu bài học

Bài học hướng dẫn tạo custom subagent cho các workflow như: - Code
review. - Viết test. - Tạo tài liệu. - Kiểm tra bảo mật. - Nghiên cứu
codebase.

Subagent tùy chỉnh được định nghĩa bằng một file Markdown có phần **YAML
frontmatter** ở đầu file và phần nội dung hướng dẫn bên dưới.

## 3.2. Tạo Subagent bằng Claude Code

Có thể yêu cầu Claude Code tạo file subagent thay vì tự viết toàn bộ cấu
hình.

Ví dụ prompt:

> Tạo một subagent tên `code-reviewer` trong phạm vi project. Agent này
> chuyên review code, không được chỉnh sửa file. Chỉ cấp quyền đọc và
> tìm kiếm cần thiết. Agent phải kiểm tra bug, security, performance và
> maintainability. Kết quả trả về theo cấu trúc gồm severity, file/line,
> vấn đề, tác động và đề xuất sửa.

Claude có thể tạo: - Tên agent. - Description. - System prompt. - Tool
access. - Model phù hợp. - File cấu hình tại đúng vị trí.

Sau khi tạo, cần kiểm tra lại nội dung file, đặc biệt là description,
quyền công cụ và format đầu ra.

**Lưu ý phiên bản:** Bài giảng có thể minh họa wizard tương tác qua lệnh
`/agents`. Theo nội dung khóa học được cập nhật, wizard tạo agent trong
`/agents` đã bị loại bỏ từ Claude Code v2.1.198. File định nghĩa
subagent và các trường cấu hình vẫn được sử dụng; cách phù hợp là yêu
cầu Claude tạo file hoặc chỉnh sửa trực tiếp trong thư mục agents. Không
nên phụ thuộc vào các ảnh chụp giao diện wizard cũ.

## 3.3. Chọn Scope -- Phạm vi sử dụng

Có hai vị trí phổ biến để lưu custom subagent:

### Project-level

``` text
your-project/
└── .claude/
    └── agents/
        ├── code-reviewer.md
        ├── security-auditor.md
        └── documentation-writer.md
```

Đường dẫn:

``` text
.claude/agents/
```

Đặc điểm: - Chỉ dùng trong project tương ứng. - Có thể commit vào Git để
chia sẻ với nhóm. - Phù hợp với agent có quy tắc gắn với codebase hoặc
quy trình của dự án.

### User-level

``` text
~/.claude/
└── agents/
    ├── code-reviewer.md
    └── researcher.md
```

Đường dẫn:

``` text
~/.claude/agents/
```

Đặc điểm: - Có thể sử dụng trên nhiều project của cùng người dùng. - Phù
hợp với agent mang tính cá nhân hoặc quy trình chung. - Không cần sao
chép cấu hình vào từng repository.

### Chọn scope nào?

  -----------------------------------------------------------------------
  Nhu cầu                             Scope nên chọn
  ----------------------------------- -----------------------------------
  Agent tuân theo coding convention   Project-level
  của một dự án                       

  Agent review bảo mật dùng chung     User-level hoặc project-level tùy
  nhiều dự án                         yêu cầu

  Agent cần được cả team sử dụng      Project-level và commit vào Git

  Agent phục vụ workflow cá nhân      User-level
  -----------------------------------------------------------------------

## 3.4. Cấu trúc file Subagent

Ví dụ file `.claude/agents/code-reviewer.md`:

``` markdown
---
name: code-reviewer
description: Reviews code for bugs, security issues, performance
  problems, and maintainability. Use after code changes or when
  the user requests an independent review.
tools: Read, Glob, Grep
model: sonnet
---

You are a senior code reviewer.

Your task is to inspect code and identify actionable problems.

Rules:
- Do not modify files.
- Focus on correctness, security, performance, and maintainability.
- Do not report speculative issues as confirmed bugs.
- Include file paths and line references where possible.

Return findings using this structure:
1. Severity
2. File and location
3. Problem
4. Impact
5. Suggested fix

If no issues are found, state that clearly and mention the
scope reviewed.
```

Cấu trúc gồm hai phần:

**1. YAML frontmatter:** Nằm giữa hai dòng `---`, chứa metadata và cấu
hình agent.

**2. Markdown body:** Nội dung system prompt mô tả vai trò, nhiệm vụ,
quy tắc và cách trả kết quả.

## 3.5. Các trường cấu hình quan trọng

  -----------------------------------------------------------------------
  Field                               Ý nghĩa
  ----------------------------------- -----------------------------------
  `name`                              Tên định danh của subagent.

  `description`                       Mô tả nhiệm vụ và tình huống nên sử
                                      dụng agent. Đây là thông tin quan
                                      trọng giúp Claude quyết định khi
                                      nào cần delegate.

  `tools`                             Danh sách công cụ được phép sử
                                      dụng.

  `disallowedTools`                   Danh sách công cụ bị loại khỏi
                                      quyền truy cập kế thừa.

  `model`                             Model sử dụng, ví dụ `sonnet`,
                                      `opus`, `haiku` hoặc `inherit`.

  `permissionMode`                    Chế độ quyền và phê duyệt thao tác.

  `maxTurns`                          Giới hạn số lượt agentic turn.

  `skills`                            Các skill được nạp vào subagent khi
                                      khởi chạy.

  `mcpServers`                        Khai báo hoặc tham chiếu MCP server
                                      phù hợp.

  `hooks`                             Hook gắn với vòng đời của subagent,
                                      tùy phạm vi hỗ trợ.

  `memory`                            Cấu hình bộ nhớ của subagent nếu
                                      được hỗ trợ.

  `background`                        Cấu hình cách chạy nền.

  `effort`                            Mức effort của model nếu được hỗ
                                      trợ.

  `isolation`                         Cấu hình môi trường cô lập, ví dụ
                                      worktree trong workflow hỗ trợ.

  `color`                             Màu hiển thị trong giao diện quản
                                      lý task.
  -----------------------------------------------------------------------

Không phải mọi trường đều cần khai báo. Với một subagent cơ bản, `name`
và `description` là hai trường bắt buộc quan trọng nhất; các trường còn
lại được thêm tùy use case và phiên bản.

### `name`

Tên nên: - Ngắn gọn, dễ hiểu. - Dùng chữ thường và dấu gạch ngang. - Thể
hiện đúng chuyên môn hoặc nhiệm vụ.

Ví dụ: - `code-reviewer` - `security-auditor` - `api-researcher` -
`documentation-writer`

### `description`

Description không chỉ để con người đọc. Nó còn giúp agent chính xác định
khi nào nên gọi subagent.

Description yếu:

``` yaml
description: Helps with code.
```

Description tốt hơn:

``` yaml
description: Reviews changed code for correctness, security,
  performance, and maintainability. Use after implementation
  when an independent review is needed.
```

Một description tốt cần làm rõ: - Agent chuyên xử lý việc gì? - Khi nào
nên sử dụng? - Loại đầu ra hoặc giá trị mà agent mang lại?

### `tools`

Giới hạn công cụ giúp subagent chỉ có những năng lực cần thiết.

Ví dụ agent nghiên cứu chỉ cần:

``` yaml
tools: Read, Glob, Grep
```

Agent cần chạy lệnh kiểm tra có thể cần thêm công cụ thực thi phù hợp,
tùy môi trường.

Nguyên tắc: **Cấp quyền tối thiểu cần thiết (Least Privilege).**

Không nên cấp toàn bộ công cụ nếu nhiệm vụ chỉ cần đọc file.

### `model`

Chọn model dựa trên độ phức tạp: - Model nhẹ hơn: tác vụ tìm kiếm, phân
loại hoặc tổng hợp đơn giản. - Model mạnh hơn: phân tích sâu, review
phức tạp hoặc nhiệm vụ cần suy luận nhiều. - `inherit`: kế thừa lựa chọn
model theo cơ chế của phiên làm việc.

Không nên mặc định mọi subagent đều cần model mạnh nhất. Hãy cân bằng
chất lượng, tốc độ và chi phí.

### `permissionMode`

Quyền của subagent cần được thiết kế cẩn thận. Chế độ cụ thể và cách áp
dụng phụ thuộc phiên bản Claude Code cũng như quyền của phiên cha. Không
dùng chế độ quyền rộng chỉ để tiết kiệm thao tác xác nhận.

## 3.6. Kiểm tra sau khi tạo

Sau khi tạo file: 1. Kiểm tra file nằm đúng thư mục. 2. Kiểm tra YAML
frontmatter hợp lệ. 3. Kiểm tra `name` và `description`. 4. Kiểm tra
quyền tool có đúng nhiệm vụ không. 5. Gọi thử subagent bằng một task
thực tế. 6. Kiểm tra output có đúng format và có bằng chứng không. 7.
Điều chỉnh system prompt nếu agent làm sai phạm vi hoặc trả kết quả
thiếu.

------------------------------------------------------------------------

# 4. Lesson 3 -- Designing effective subagents

## 4.1. Mục tiêu bài học

Một subagent có thể được tạo và chạy thành công nhưng chưa chắc tạo ra
kết quả đáng tin cậy.

Bài học tập trung vào bốn pattern thiết kế: 1. Vai trò và phạm vi rõ
ràng. 2. Structured output -- đầu ra có cấu trúc. 3. Obstacle reporting
-- báo cáo trở ngại. 4. Limited tool access -- giới hạn quyền công cụ.

Mục tiêu là biến subagent từ một agent chung chung thành một worker
chuyên biệt, dễ dự đoán, có thể hoàn thành nhiệm vụ và bàn giao kết quả
rõ ràng.

## 4.2. Pattern 1 -- Define a clear role

Mỗi subagent nên có một vai trò và phạm vi chuyên biệt.

Ví dụ: - Code reviewer: tìm vấn đề trong code. - Researcher: tìm kiếm
thông tin và cung cấp bằng chứng. - Documentation writer: viết tài liệu
theo cấu trúc xác định. - Security auditor: đánh giá rủi ro bảo mật.

Không nên mô tả agent bằng những yêu cầu quá chung chung như:

> Bạn là một chuyên gia lập trình. Hãy giúp tôi mọi thứ cần thiết.

Thay vào đó:

> Bạn là security auditor chuyên kiểm tra authentication và
> authorization trong ứng dụng web. Chỉ phân tích code, không chỉnh sửa
> file. Ưu tiên các lỗi có thể khai thác và phải cung cấp vị trí cùng
> bằng chứng cho từng phát hiện.

Vai trò rõ ràng giúp: - Giảm phạm vi không cần thiết. - Giảm việc agent
tự mở rộng nhiệm vụ. - Tăng tính nhất quán giữa các lần chạy. - Giúp
agent chính chọn đúng subagent.

## 4.3. Pattern 2 -- Structured Output

**Structured output** là việc quy định trước định dạng kết quả mà
subagent phải trả về.

Nếu không có format, mỗi lần subagent có thể trả kết quả theo một kiểu
khác nhau: - Lúc thì đoạn văn dài. - Lúc thì bullet points. - Lúc thiếu
đường dẫn file. - Lúc không phân biệt lỗi nghiêm trọng và vấn đề nhỏ.

Điều này làm agent chính mất thêm công sức đọc, kiểm tra và chuyển đổi
kết quả.

### Ví dụ format cho Code Review

``` markdown
## Review Summary
- Scope:
- Overall status:
- Files reviewed:

## Findings

### [Critical / High / Medium / Low]
- File:
- Line:
- Issue:
- Evidence:
- Impact:
- Suggested fix:

## Unverified Areas
- ...

## Final Assessment
- ...
```

### Vì sao Structured Output quan trọng?

**1. Dễ tổng hợp**

Agent chính biết chính xác thông tin nằm ở đâu.

**2. Không bỏ sót thông tin**

Các trường bắt buộc nhắc subagent cung cấp đủ dữ liệu.

**3. Dễ so sánh**

Nhiều subagent có thể trả kết quả theo cùng một cấu trúc.

**4. Xác định điểm kết thúc**

Khi biết cần cung cấp những mục nào, subagent có tiêu chí rõ hơn để hoàn
tất nhiệm vụ.

**5. Dễ tự động hóa**

Kết quả có cấu trúc dễ được xử lý bởi workflow hoặc agent khác hơn so
với văn bản tự do.

## 4.4. Pattern 3 -- Obstacle Reporting

Subagent không phải lúc nào cũng hoàn thành được toàn bộ nhiệm vụ. Có
thể gặp: - File không tồn tại. - Không có quyền truy cập. - Thiếu
dependency. - Lệnh thực thi thất bại. - Dữ liệu không đủ để kết luận. -
Một phần code không thể kiểm tra. - Phát hiện mâu thuẫn giữa tài liệu và
code.

Nếu không yêu cầu báo cáo trở ngại, subagent có thể chỉ trả về kết quả
chung chung hoặc bỏ qua phần chưa hoàn thành. Agent chính sẽ không biết
điều gì đã thực sự được kiểm tra.

### Cách thiết kế

Thêm một mục riêng trong output:

``` markdown
## Obstacles and Limitations

- Blocker:
- What was attempted:
- What failed:
- Workaround:
- Remaining uncertainty:
- What the main agent should do next:
```

### Ví dụ

``` markdown
## Obstacles and Limitations

- Blocker: Could not run integration tests.
- What was attempted: Executed the test command.
- What failed: Database service was unavailable.
- Workaround: Reviewed test setup and relevant test code.
- Remaining uncertainty: Runtime integration behavior is unverified.
- Next step: Start the database service and rerun integration tests.
```

Điều này giúp agent chính: - Biết chính xác phần nào đã hoàn thành. -
Không nhầm kết quả chưa kiểm chứng thành kết luận chắc chắn. - Tiếp tục
xử lý đúng điểm bị chặn. - Tránh lặp lại những bước đã thất bại.

**Nguyên tắc:** Báo cáo trung thực những gì đã làm, chưa làm và không
thể xác minh.

## 4.5. Pattern 4 -- Limit Tool Access

Không phải subagent nào cũng cần toàn bộ quyền truy cập của agent chính.

Ví dụ:

  -----------------------------------------------------------------------
  Loại Subagent                       Tool access phù hợp
  ----------------------------------- -----------------------------------
  Researcher                          Read, Glob, Grep

  Code reviewer                       Read, Glob, Grep; có thể cần công
                                      cụ thực thi phù hợp để kiểm tra

  Documentation writer                Read và công cụ ghi file nếu được
                                      giao tạo tài liệu

  Code modifier                       Read, Edit, Write và các công cụ
                                      cần thiết

  Security auditor                    Ưu tiên quyền đọc và tìm kiếm; chỉ
                                      cấp quyền bổ sung khi thực sự cần
  -----------------------------------------------------------------------

### Vì sao cần giới hạn tool?

**1. Giảm rủi ro**

Agent chỉ có thể thực hiện những thao tác nằm trong quyền được cấp.

**2. Giữ đúng vai trò**

Một reviewer chỉ đọc code sẽ ít có khả năng vô tình sửa code.

**3. Tăng khả năng kiểm soát**

Agent chính dễ dự đoán hành vi của subagent hơn.

**4. Hạn chế tác động ngoài ý muốn**

Không cấp quyền ghi hoặc thực thi nếu nhiệm vụ không cần.

### Least Privilege

Đây là nguyên tắc bảo mật: chỉ cấp quyền tối thiểu để hoàn thành nhiệm
vụ.

Ví dụ: - Nhiệm vụ chỉ đọc → không cấp quyền chỉnh sửa. - Nhiệm vụ chỉ
tìm kiếm → không cần công cụ thực thi lệnh. - Nhiệm vụ viết tài liệu →
cấp quyền ghi đúng phạm vi cần thiết. - Nhiệm vụ triển khai code → chỉ
cấp các quyền cần thiết và giữ cơ chế kiểm soát phù hợp.

## 4.6. Kết hợp bốn pattern

Một subagent hiệu quả cần kết hợp cả bốn yếu tố:

  Pattern               Câu hỏi cần trả lời
  --------------------- ----------------------------------------------
  Clear role            Agent chịu trách nhiệm về việc gì?
  Structured output     Kết quả phải được trả về theo định dạng nào?
  Obstacle reporting    Nếu gặp vấn đề, agent phải báo cáo ra sao?
  Limited tool access   Agent thực sự cần những công cụ nào?

Có thể ghi nhớ bằng công thức:

> **Effective Subagent = Clear Role + Structured Output + Obstacle
> Reporting + Least Privilege**

------------------------------------------------------------------------

# 5. Lesson 4 -- Using subagents effectively

## 5.1. Mục tiêu bài học

Biết cách quyết định: - Khi nào nên delegate cho subagent. - Khi nào nên
giữ nhiệm vụ trong main agent. - Khi nào có thể chạy nhiều subagent song
song. - Những anti-pattern nào thường khiến việc sử dụng subagent kém
hiệu quả.

## 5.2. Quy tắc quyết định quan trọng

Một câu hỏi hữu ích:

> **Công việc trung gian trong quá trình thực hiện có quan trọng đối với
> quyết định tiếp theo không?**

Nếu câu trả lời là **không**, và chỉ cần kết quả cuối cùng, subagent
thường là lựa chọn phù hợp.

Nếu câu trả lời là **có**, agent chính cần quan sát, phản ứng và điều
chỉnh liên tục thì nên giữ nhiệm vụ trong luồng chính.

Ví dụ:

**Nên dùng subagent:** - Tìm hiểu cấu trúc authentication trong
codebase. - Review độc lập một phần code. - Khảo sát các dependency đang
sử dụng. - Tìm các vị trí có nguy cơ lộ secret. - Tạo bản tổng hợp tài
liệu từ một tập file.

**Nên giữ trong main agent:** - Debug một lỗi mà mỗi phát hiện sẽ quyết
định bước tiếp theo. - Thực hiện chuỗi thay đổi phụ thuộc chặt chẽ vào
kết quả từng bước. - Làm việc cần người điều phối liên tục đánh giá và
thay đổi hướng đi. - Nhiệm vụ nhỏ đến mức việc khởi chạy subagent không
đáng công.

## 5.3. Use Case 1 -- Research and Exploration

Đây là một trong những trường hợp phù hợp nhất.

Ví dụ: Muốn hiểu luồng đăng nhập của một dự án.

Main agent giao cho researcher:

> Khảo sát toàn bộ luồng đăng nhập. Tìm entry point, nơi validate
> credentials, cách tạo và xác minh token, middleware kiểm tra quyền và
> các file liên quan. Không chỉnh sửa code. Trả về sơ đồ luồng, danh
> sách file và những điểm chưa rõ.

Subagent có thể đọc nhiều file, tìm kiếm các symbol và tổng hợp phát
hiện trong context riêng.

Main agent chỉ nhận: - Các file quan trọng. - Luồng xử lý. - Các phát
hiện. - Những điểm cần điều tra thêm.

Lợi ích: Không cần đưa toàn bộ output tìm kiếm vào context chính.

## 5.4. Use Case 2 -- Independent Code Review

Subagent rất hữu ích khi cần review code độc lập.

Khi main agent vừa viết code, nó đã có nhiều context về: - Ý định ban
đầu. - Những giả định đã sử dụng. - Các quyết định thiết kế. - Những
thay đổi đã thực hiện.

Một reviewer độc lập có thể tiếp cận code mà không bị ảnh hưởng bởi toàn
bộ quá trình tạo ra nó. Điều này giúp tìm ra các vấn đề mà người viết có
thể bỏ qua.

Có thể tạo reviewer chuyên kiểm tra: - Logic và correctness. -
Security. - Performance. - Error handling. - Maintainability. -
Regression risk.

**Lưu ý:** Review độc lập không đồng nghĩa với review luôn đúng. Agent
chính vẫn cần xác minh phát hiện dựa trên code và bằng chứng.

## 5.5. Use Case 3 -- Custom System Prompt

Một số công việc cần cách hành xử khác với agent lập trình mặc định.

Ví dụ:

**Documentation agent** - Tập trung viết tài liệu. - Tuân thủ cấu trúc
README hoặc API reference. - Không tự ý thay đổi code. - Kiểm tra sự
nhất quán giữa tài liệu và source.

**Security auditor** - Sử dụng checklist bảo mật. - Phân loại mức độ
nghiêm trọng. - Yêu cầu bằng chứng cho mỗi phát hiện. - Không đưa ra kết
luận thiếu căn cứ.

**Test planner** - Phân tích yêu cầu và các trường hợp biên. - Đề xuất
test cases. - Phân loại unit, integration và end-to-end tests.

Custom system prompt giúp chuẩn hóa quy trình chuyên biệt thay vì phải
nhắc lại trong mỗi prompt.

## 5.6. Khi nào KHÔNG nên sử dụng Subagents?

### Trường hợp 1: Task quá nhỏ

Ví dụ: - Đổi tên một biến. - Giải thích một đoạn code ngắn. - Sửa một
lỗi cú pháp đơn giản.

Khởi chạy subagent có thể tạo thêm chi phí điều phối mà không đem lại
lợi ích tương xứng.

### Trường hợp 2: Các bước phụ thuộc chặt chẽ vào nhau

Ví dụ pipeline:

``` text
Agent A: Reproduce bug
       |
       v
Agent B: Debug bug
       |
       v
Agent C: Fix bug
```

Nghe có vẻ hợp lý, nhưng việc sửa bug thường phụ thuộc trực tiếp vào
những phát hiện trong quá trình tái hiện và debug.

Nếu mỗi agent chỉ nhận một bản tóm tắt, thông tin quan trọng có thể bị
mất khi bàn giao. Agent sau có thể hiểu sai nguyên nhân hoặc phải điều
tra lại.

Trong trường hợp này, một agent duy trì liên tục context và điều chỉnh
từng bước có thể hiệu quả hơn.

### Trường hợp 3: Cần theo dõi toàn bộ quá trình

Nếu agent chính cần xem log, đánh giá kết quả và thay đổi hướng xử lý
ngay khi có thông tin mới, việc đưa toàn bộ quá trình sang subagent có
thể làm giảm khả năng kiểm soát.

### Trường hợp 4: Chạy test nhưng cần toàn bộ output

Một subagent chỉ trả về:

> Tests failed.

là chưa đủ nếu agent chính cần biết: - Test nào thất bại? - Stack trace
ra sao? - Expected và actual khác nhau ở đâu? - Có lỗi môi trường hay
lỗi code? - Có bao nhiêu test pass/fail?

Nếu toàn bộ output test là dữ liệu quan trọng để debug, nên giữ quá
trình chạy và phân tích trong luồng chính hoặc yêu cầu subagent trả lại
đầy đủ phần output cần thiết.

## 5.7. Parallel Execution -- Chạy song song

Subagents có thể giúp phân chia các nhiệm vụ độc lập để thực hiện song
song.

Ví dụ khi audit một ứng dụng:

``` text
                 Main Agent
                      |
       +--------------+--------------+
       |              |              |
       v              v              v
  Security Agent  API Agent    Performance Agent
       |              |              |
       v              v              v
  Security report API report   Performance report
       |              |              |
       +--------------+--------------+
                      |
                      v
                Main Agent
                      |
                      v
                Final Report
```

Các nhiệm vụ phù hợp để chạy song song: - Kiểm tra bảo mật. - Khảo sát
API. - Đánh giá hiệu năng. - Kiểm tra tài liệu.

Điều kiện quan trọng: các nhiệm vụ phải tương đối độc lập và có thể thực
hiện mà không cần chờ kết quả của nhau.

### Lợi ích

-   Rút ngắn thời gian thực hiện các phần việc độc lập.
-   Mỗi agent tập trung vào một phạm vi.
-   Giảm lượng thông tin trung gian trong context chính.
-   Có thể thu thập nhiều góc nhìn khác nhau.

### Rủi ro

-   Nhiều agent có thể kiểm tra trùng lặp.
-   Các agent có thể đưa ra kết luận mâu thuẫn.
-   Output tổng hợp có thể quá dài.
-   Nếu cùng sửa một file, có thể xảy ra xung đột.
-   Chi phí sử dụng model và công cụ có thể tăng.

Vì vậy, cần xác định rõ phạm vi và output của từng agent trước khi chạy
song song.

------------------------------------------------------------------------

# 6. Quy trình thiết kế một hệ thống Subagents

Dưới đây là quy trình thực tế có thể áp dụng khi xây dựng workflow với
Claude Code.

## Bước 1 -- Xác định mục tiêu tổng thể

Viết rõ kết quả cuối cùng cần đạt được.

Ví dụ:

> Audit toàn bộ ứng dụng web để phát hiện lỗi bảo mật, vấn đề hiệu năng
> và các rủi ro về maintainability.

## Bước 2 -- Phân rã nhiệm vụ

Tách thành các phần việc có phạm vi riêng:

  -----------------------------------------------------------------------
  Subagent                Nhiệm vụ                Output
  ----------------------- ----------------------- -----------------------
  Security auditor        Kiểm tra                Security findings
                          authentication,         
                          authorization, input    
                          validation              

  Performance analyst     Tìm bottleneck và truy  Performance report
                          vấn kém hiệu quả        

  Code reviewer           Kiểm tra logic và       Code review report
                          maintainability         

  Documentation analyst   Kiểm tra tài liệu và    Documentation report
                          mức độ đồng bộ          
  -----------------------------------------------------------------------

## Bước 3 -- Kiểm tra tính độc lập

Tự hỏi: - Các nhiệm vụ có thể chạy đồng thời không? - Có nhiệm vụ nào
phải chờ kết quả của nhiệm vụ khác không? - Có hai agent cùng chỉnh sửa
một file không? - Main agent có cần theo dõi quá trình thực hiện không?

Nếu các nhiệm vụ phụ thuộc nhiều vào nhau, hãy giữ chúng trong một
workflow liên tục hoặc chỉ delegate phần khảo sát độc lập.

## Bước 4 -- Thiết kế từng Subagent

Với mỗi agent, xác định:

1.  Name.
2.  Description.
3.  System prompt.
4.  Tool access.
5.  Model phù hợp.
6.  Expected output.
7.  Cách báo cáo trở ngại.
8.  Điều kiện hoàn thành.

## Bước 5 -- Giao nhiệm vụ rõ ràng

Mỗi prompt giao việc nên có: - Mục tiêu. - Phạm vi. - Context cần
thiết. - Giới hạn. - Output format. - Điều kiện dừng hoặc báo cáo trở
ngại.

## Bước 6 -- Tổng hợp và xác minh

Main agent cần: - Đọc kết quả từ từng subagent. - So sánh các phát
hiện. - Loại bỏ trùng lặp. - Xác minh các kết luận quan trọng. - Làm rõ
những điểm mâu thuẫn. - Đưa ra kết luận tổng thể.

Không nên coi mọi kết quả từ subagent là sự thật đã được xác minh.

------------------------------------------------------------------------

# 7. Ví dụ thực tế

## 7.1. Tạo Code Reviewer

File: `.claude/agents/code-reviewer.md`

``` markdown
---
name: code-reviewer
description: Reviews code changes for bugs, security,
  performance, and maintainability. Use after implementation
  when an independent review is needed.
tools: Read, Glob, Grep
model: sonnet
---

You are a senior software engineer specializing in code review.

Your responsibility is to inspect code and report actionable issues.

Rules:
- Do not modify any files.
- Focus on correctness, security, performance, and maintainability.
- Avoid speculative findings.
- Include file paths and line references.
- Explain the impact of each issue.

Return:

## Summary
- Scope reviewed:
- Overall assessment:

## Findings
For each finding:
- Severity:
- File and line:
- Issue:
- Evidence:
- Impact:
- Suggested fix:

## Limitations
- Files not reviewed:
- Checks not performed:
- Obstacles:

If no issues are found, explicitly state that no actionable
issues were identified within the reviewed scope.
```

## 7.2. Tạo Researcher

File: `.claude/agents/codebase-researcher.md`

``` markdown
---
name: codebase-researcher
description: Explores a codebase to explain architecture,
  data flow, dependencies, and implementation details.
  Use when the main agent needs focused investigation.
tools: Read, Glob, Grep
model: sonnet
---

You are a codebase research specialist.

Investigate the requested topic without modifying files.

Your tasks:
- Locate relevant entry points.
- Trace important function calls and data flow.
- Identify related modules and dependencies.
- Support conclusions with file paths and symbols.
- Separate confirmed facts from assumptions.

Return:

## Research Question
## Key Findings
## Relevant Files
## Data Flow / Architecture
## Evidence
## Unknowns and Obstacles
## Recommended Next Steps
```

## 7.3. Tạo Documentation Writer

File: `.claude/agents/documentation-writer.md`

``` markdown
---
name: documentation-writer
description: Creates or updates technical documentation
  based on existing source code. Use when documentation
  must be generated or brought up to date.
tools: Read, Glob, Grep, Write, Edit
model: sonnet
---

You are a technical documentation specialist.

Write documentation based on verified implementation details.

Rules:
- Read the relevant source code before writing.
- Do not invent APIs, parameters, or behavior.
- Use clear headings and practical examples.
- Keep terminology consistent.
- Identify information that cannot be verified.

Before finishing, report:
- Files created or updated.
- Main sections written.
- Source files used.
- Unverified details and obstacles.
```

## 7.4. Prompt điều phối nhiều Subagents

``` text
Audit this project using specialized subagents.

Delegate independent tasks:

1. Security:
   Inspect authentication, authorization, input validation,
   secrets handling, and common security risks.

2. Performance:
   Inspect expensive operations, inefficient data access,
   unnecessary rendering, and resource usage.

3. Code quality:
   Review architecture, error handling, maintainability,
   and potential logic bugs.

Rules:
- Keep the investigations independent.
- Do not allow agents to modify source files.
- Require evidence and file references for every finding.
- Require each agent to report obstacles and unverified areas.
- Avoid duplicate findings.

After all agents finish:
- Consolidate the reports.
- Resolve duplicate or conflicting findings.
- Prioritize issues by severity.
- Produce one final audit report.
- Clearly distinguish verified findings from hypotheses.
```

------------------------------------------------------------------------

# 8. Những lỗi và anti-pattern cần tránh

## 8.1. Delegation quá mơ hồ

**Vấn đề:** Giao một nhiệm vụ rộng mà không xác định phạm vi và kết quả.

**Hậu quả:** Subagent tự suy diễn, làm quá nhiều hoặc trả về kết quả
không dùng được.

**Cách khắc phục:** Nêu rõ mục tiêu, phạm vi, giới hạn và output.

## 8.2. Tạo quá nhiều Subagents

**Vấn đề:** Chia một nhiệm vụ nhỏ thành nhiều agent không cần thiết.

**Hậu quả:** - Tăng chi phí điều phối. - Tăng lượng output phải tổng
hợp. - Dễ trùng lặp công việc. - Khó kiểm soát kết quả.

**Cách khắc phục:** Chỉ tạo agent khi có chuyên môn hoặc phạm vi độc lập
thực sự cần thiết.

## 8.3. Không định nghĩa Output Format

**Vấn đề:** Mỗi subagent trả kết quả theo một cách khác nhau.

**Hậu quả:** Main agent phải tự chuẩn hóa và có thể bỏ sót thông tin.

**Cách khắc phục:** Quy định cấu trúc đầu ra và các trường bắt buộc.

## 8.4. Không yêu cầu báo cáo trở ngại

**Vấn đề:** Subagent không nói rõ phần nào chưa hoàn thành.

**Hậu quả:** Main agent có thể hiểu nhầm rằng toàn bộ nhiệm vụ đã được
kiểm tra.

**Cách khắc phục:** Luôn có mục Obstacles, Limitations hoặc Unverified
Areas.

## 8.5. Cấp quá nhiều quyền

**Vấn đề:** Agent chỉ cần đọc nhưng được cấp quyền chỉnh sửa hoặc thực
thi.

**Hậu quả:** Tăng rủi ro thay đổi ngoài ý muốn.

**Cách khắc phục:** Áp dụng Least Privilege.

## 8.6. Chia pipeline phụ thuộc thành nhiều agent

**Vấn đề:** Agent sau cần chi tiết từ quá trình thực hiện của agent
trước nhưng chỉ nhận bản tóm tắt.

**Hậu quả:** Mất thông tin, phải điều tra lại hoặc đưa ra quyết định
sai.

**Cách khắc phục:** Giữ các bước phụ thuộc trong cùng một luồng thực
hiện; chỉ delegate phần việc độc lập.

## 8.7. Tin hoàn toàn vào kết quả Subagent

**Vấn đề:** Coi báo cáo của agent phụ là kết luận cuối cùng.

**Hậu quả:** Có thể chấp nhận phát hiện sai, thiếu bằng chứng hoặc giả
định chưa được kiểm tra.

**Cách khắc phục:** Main agent xác minh các phát hiện quan trọng, đặc
biệt là lỗi bảo mật, lỗi nghiêm trọng và thay đổi có tác động lớn.

------------------------------------------------------------------------

# 9. Checklist đánh giá Subagent

Sử dụng checklist này trước khi đưa một subagent vào workflow thực tế.

## Định nghĩa nhiệm vụ

-   [ ] Tên agent thể hiện đúng vai trò.
-   [ ] Description nêu rõ khi nào cần sử dụng.
-   [ ] Phạm vi nhiệm vụ đủ cụ thể.
-   [ ] System prompt mô tả rõ trách nhiệm và giới hạn.

## Quyền truy cập

-   [ ] Tool access chỉ gồm công cụ cần thiết.
-   [ ] Agent không có quyền ghi nếu chỉ cần đọc.
-   [ ] Model được chọn phù hợp với độ phức tạp.
-   [ ] Permission được thiết lập an toàn.

## Chất lượng kết quả

-   [ ] Có output format rõ ràng.
-   [ ] Có bằng chứng hoặc đường dẫn file.
-   [ ] Có phân biệt kết luận và giả định.
-   [ ] Có mục báo cáo trở ngại.
-   [ ] Có mô tả phần chưa được xác minh.

## Kiểm thử

-   [ ] Đã chạy thử với một nhiệm vụ thực tế.
-   [ ] Agent không vượt khỏi phạm vi.
-   [ ] Output có thể sử dụng trực tiếp.
-   [ ] Không tạo ra thay đổi ngoài ý muốn.
-   [ ] Agent hoàn thành trong giới hạn hợp lý.

------------------------------------------------------------------------

# 10. Tổng kết và câu hỏi ôn tập

## 10.1. Những kiến thức quan trọng nhất

1.  **Subagent là agent phụ có context window riêng**, được main agent
    giao một nhiệm vụ cụ thể và nhận lại kết quả hoặc bản tóm tắt.
2.  **Mục tiêu lớn nhất là cô lập công việc trung gian**, giữ context
    chính tập trung vào mục tiêu tổng thể.
3.  **Custom subagent được định nghĩa bằng Markdown và YAML
    frontmatter**, thường lưu trong `.claude/agents/` hoặc
    `~/.claude/agents/`.
4.  **Description rất quan trọng** vì giúp main agent xác định khi nào
    nên sử dụng subagent.
5.  **Structured output** giúp kết quả nhất quán, dễ tổng hợp và dễ kiểm
    tra.
6.  **Obstacle reporting** giúp main agent biết rõ những gì chưa hoàn
    thành hoặc chưa thể xác minh.
7.  **Limited tool access** giúp giảm rủi ro và giữ agent đúng vai trò.
8.  **Subagent phù hợp với công việc độc lập**, đặc biệt là research,
    exploration, code review và workflow có system prompt chuyên biệt.
9.  **Không nên delegate mọi thứ**: task nhỏ, pipeline phụ thuộc chặt
    chẽ hoặc công việc cần theo dõi liên tục thường nên giữ trong main
    agent.
10. **Main agent vẫn chịu trách nhiệm tổng hợp và xác minh** kết quả từ
    các subagent.

## 10.2. Bảng ghi nhớ nhanh

  Khái niệm            Ghi nhớ
  -------------------- --------------------------------------------
  Subagent             Agent phụ chạy trong context riêng
  Main agent           Điều phối và tổng hợp kết quả
  Delegation           Giao nhiệm vụ cho agent phụ
  Context isolation    Tách ngữ cảnh công việc
  Custom subagent      Agent chuyên biệt do người dùng định nghĩa
  YAML frontmatter     Metadata và cấu hình agent
  Description          Giúp định tuyến nhiệm vụ
  Structured output    Chuẩn hóa kết quả
  Obstacle reporting   Báo cáo trở ngại và giới hạn
  Least privilege      Chỉ cấp quyền cần thiết
  Parallel execution   Chạy đồng thời các nhiệm vụ độc lập

## 10.3. Câu hỏi tự kiểm tra

**Câu 1.** Subagent khác main agent ở điểm nào?

**Câu 2.** Vì sao subagent có context window riêng lại giúp các phiên
Claude Code dài hiệu quả hơn?

**Câu 3.** Khi nào nên lưu subagent trong `.claude/agents/` và khi nào
nên lưu trong `~/.claude/agents/`?

**Câu 4.** Vì sao `description` không chỉ là phần mô tả cho con người?

**Câu 5.** Structured output giải quyết những vấn đề gì?

**Câu 6.** Obstacle reporting cần có những thông tin nào?

**Câu 7.** Vì sao một code reviewer chỉ đọc code thường không cần quyền
Edit hoặc Write?

**Câu 8.** Nêu ba tình huống phù hợp để sử dụng subagent.

**Câu 9.** Vì sao pipeline tái hiện bug → debug → sửa bug có thể không
phù hợp khi chia thành ba subagent riêng biệt?

**Câu 10.** Main agent cần làm gì trước khi sử dụng kết quả do subagent
trả về?

### Gợi ý đáp án

1.  Main agent điều phối công việc tổng thể; subagent thực hiện nhiệm vụ
    chuyên biệt trong context riêng.
2.  Công việc trung gian không chiếm trực tiếp context chính; main agent
    chỉ nhận phần kết quả cần thiết.
3.  Project-level dành cho một repository; user-level dùng chung giữa
    nhiều project.
4.  Description giúp Claude nhận biết tình huống cần delegate.
5.  Chuẩn hóa output, giảm thiếu sót, dễ tổng hợp và dễ tự động hóa.
6.  Trở ngại, thao tác đã thử, lỗi gặp phải, workaround, phần chưa xác
    minh và bước tiếp theo.
7.  Vì reviewer chỉ cần đọc và phân tích; cấp quyền ghi không cần thiết
    làm tăng rủi ro.
8.  Research codebase, independent code review, security audit hoặc tạo
    tài liệu.
9.  Các bước phụ thuộc vào phát hiện trước đó; việc bàn giao qua tóm tắt
    có thể làm mất chi tiết quan trọng.
10. Kiểm tra bằng chứng, xác minh phát hiện quan trọng, xử lý mâu thuẫn
    và tổng hợp kết quả.

------------------------------------------------------------------------

## Nguồn tham khảo

### Tài liệu khóa học

-   [Introduction to subagents -- Claude
    Academy](https://academy.claude.com/courses/introduction-to-subagents)
-   [What are
    subagents?](https://academy.claude.com/courses/introduction-to-subagents/what-are-subagents)
-   [Creating a
    subagent](https://academy.claude.com/courses/introduction-to-subagents/creating-a-subagent)
-   [Designing effective
    subagents](https://academy.claude.com/courses/introduction-to-subagents/designing-effective-subagents)
-   [Using subagents
    effectively](https://academy.claude.com/courses/introduction-to-subagents/using-subagents-effectively)

### Tài liệu bổ sung

-   [Claude Code -- Subagents
    documentation](https://code.claude.com/docs/en/sub-agents)
-   [Steering Claude Code: Skills, hooks, rules, subagents, and
    more](https://claude.com/blog/steering-claude-code-skills-hooks-rules-subagents-and-more)

> **Ghi chú về phạm vi:** Tài liệu này tổng hợp các mục tiêu và nội dung
> cốt lõi của khóa học, đồng thời bổ sung ví dụ cấu hình và hướng dẫn
> thực hành để phục vụ việc học. Đây là tài liệu học tập được biên soạn
> lại, không phải bản transcript nguyên văn của video. Các trường cấu
> hình và giao diện Claude Code có thể thay đổi theo phiên bản; hãy đối
> chiếu tài liệu chính thức khi triển khai.

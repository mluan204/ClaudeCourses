# Claude Code 101

## Tài liệu học tập -- Claude Academy

> **Khóa học:** Claude Code 101\
> **Nền tảng:** Claude Code (CLI / Terminal)\
> **Đơn vị:** Anthropic / Claude Academy\
> **Thời lượng khóa học:** khoảng 1,5 giờ\
> **Số bài học:** 12 bài học + 1 bài kiểm tra\
> **Mục tiêu:** Hiểu bản chất của AI coding agent, cơ chế agentic loop, kiểm soát quyền hạn và ngữ cảnh; làm chủ quy trình Explore -> Plan -> Code -> Commit; tùy biến agent với CLAUDE.md, subagents, skills, MCP và hooks.

------------------------------------------------------------------------

## Mục lục

1. [Tổng quan về Claude Code](#1-tổng-quan-về-claude-code)
2. [Lesson 1 -- What is Claude Code?](#2-lesson-1----what-is-claude-code)
3. [Lesson 2 -- How Claude Code works](#3-lesson-2----how-claude-code-works)
4. [Lesson 3 -- Installing Claude Code](#4-lesson-3----installing-claude-code)
5. [Lesson 4 -- Your first prompt](#5-lesson-4----your-first-prompt)
6. [Lesson 5 -- The Explore → Plan → Code → Commit workflow](#6-lesson-5----the-explore--plan--code--commit-workflow)
7. [Lesson 6 -- Context management](#7-lesson-6----context-management)
8. [Lesson 7 -- Code review](#8-lesson-7----code-review)
9. [Lesson 8 -- The CLAUDE.md file](#9-lesson-8----the-claudemd-file)
10. [Lesson 9 -- Subagents](#10-lesson-9----subagents)
11. [Lesson 10 -- Skills](#11-lesson-10----skills)
12. [Lesson 11 -- MCP (Model Context Protocol)](#12-lesson-11----mcp-model-context-protocol)
13. [Lesson 12 -- Hooks](#13-lesson-12----hooks)
14. [Bảng tổng hợp các khái niệm](#14-bảng-tổng-hợp-các-khái-niệm)
15. [Thực hành mẫu: Thêm bộ lọc trạng thái đơn hàng](#15-thực-hành-mẫu-thêm-bộ-lọc-trạng-thái-đơn-hàng)
16. [Checklist sử dụng Claude Code](#16-checklist-sử-dụng-claude-code)
17. [Tổng kết và câu hỏi ôn tập](#17-tổng-kết-và-câu-hỏi-ôn-tập)

------------------------------------------------------------------------
# 1. Tổng quan về Claude Code

Claude Code là một **AI coding agent** hoạt động trực tiếp trong môi
trường phát triển. Thay vì chỉ trả lời bằng văn bản, nó có thể đọc
codebase, tìm file, chỉnh sửa code, chạy lệnh terminal, thực thi test,
phân tích lỗi và lặp lại các bước để hoàn thành nhiệm vụ trong phạm vi
quyền được cấp.

Khóa học gồm bốn phần:

1. **What is Claude Code?** — AI coding agent, agentic loop, context, tools và permissions.
2. **Your first prompt** — cài đặt và cách điều khiển mức độ tự chủ.
3. **Daily workflows** — Explore → Plan → Code → Commit, quản lý context và review.
4. **Customizing Claude Code** — CLAUDE.md, subagents, skills, MCP và hooks.

Sau khóa học, người học cần hiểu cách agent hoạt động, cài đặt Claude
Code, viết prompt có tiêu chí rõ ràng, tổ chức quy trình phát triển,
quản lý context, tùy biến agent và kết nối công cụ bên ngoài.

------------------------------------------------------------------------

# 2. Lesson 1 -- What is Claude Code?

## 2.1. Claude Code là gì?

Claude Code là công cụ lập trình theo hướng agentic của Anthropic. Nó làm việc trực tiếp với repository và có thể:

- Khám phá thư mục, đọc file và tìm kiếm code.
- Truy vết hàm, class, symbol và luồng xử lý.
- Đề xuất kế hoạch thay đổi.
- Tạo, chỉnh sửa hoặc xóa file.
- Chạy lệnh terminal, build, lint và test.
- Đọc output, phân tích lỗi và thử cách khắc phục.
- Tóm tắt thay đổi và hỗ trợ review.

## 2.2. Phân biệt các loại công cụ AI lập trình

| Loại | Cách hoạt động | Ví dụ |
| --- | --- | --- |
| Chatbot AI | Trả lời dựa trên nội dung được cung cấp | Giải thích một hàm |
| Code completion | Gợi ý phần code tiếp theo khi gõ | Hoàn thành câu lệnh |
| Coding agent | Tự khám phá, thao tác nhiều file, chạy lệnh và kiểm chứng | Thêm API endpoint kèm test |
  Khác biệt cốt lõi là **khả năng hành động nhiều bước**. Agent không chỉ
nói "hãy sửa file A"; nó có thể đọc file A, tìm nơi liên quan, chỉnh
sửa, chạy test và tiếp tục dựa trên kết quả.

## 2.3. Agent không phải lúc nào cũng đúng

Agent có thể hiểu sai yêu cầu, giả định sai kiến trúc, bỏ sót edge case,
tạo test chưa đủ hoặc sửa ngoài phạm vi. Người dùng cần xác định mục
tiêu, giới hạn phạm vi, cấp quyền phù hợp và kiểm tra kết quả.

## 2.4. Khi nào nên dùng?

- Truy vết bug qua nhiều file.
- Thêm tính năng theo yêu cầu.
- Refactor có phạm vi rõ.
- Viết/cập nhật test.
- Giải thích codebase chưa quen.
- Review pull request.
- Tự động hóa thao tác phát triển lặp lại.

**Ghi nhớ:** Hãy xem Claude Code như một cộng sự kỹ thuật có thể thao
tác, không phải nguồn chân lý tuyệt đối.

## 2.5. Bổ sung từ bài học gốc

- Claude Code chạy được ở **terminal, VS Code, JetBrains, Claude Desktop và web**. Khác với Claude.ai, nó truy cập trực tiếp file, terminal và codebase, nên tự làm việc thay vì để bạn copy–paste.
- **AI agent** là phần mềm hành động trong môi trường để đạt mục tiêu, thường là một LLM chạy trong vòng lặp thời gian thực. Agent có thể dùng tool, dịch vụ bên ngoài hoặc agent khác.
- Ví dụ việc làm được: truy vết bug, giải thích tính năng xuyên codebase; refactor một hàm và **cập nhật mọi file tham chiếu đến nó**; chạy build, test, cài package rồi dựa vào output để quyết định bước tiếp; **tìm tài liệu và API reference mới trên web**.
- Claude Code không nạp toàn bộ codebase vào context mà **tìm kiếm có chiến lược** những phần cần thiết.
- Claude Code có thể sai: hiểu nhầm ý, gây bug mới, làm quá mức cần thiết (over-engineering). Vì vậy người dùng phải luôn theo sát.

------------------------------------------------------------------------

# 3. Lesson 2 -- How Claude Code works

## 3.1. Agentic loop — vòng lặp tác tử

Một vòng lặp điển hình có ba giai đoạn:

1. **Gather context — Thu thập ngữ cảnh**
    - Đọc yêu cầu, cấu trúc thư mục, file và tài liệu.
    - Tìm kiếm symbol, hàm, class hoặc nơi gọi liên quan.
    - Xác định thông tin còn thiếu.
2. **Take action — Thực hiện hành động**
    - Chọn tool phù hợp.
    - Đọc/sửa file, chạy lệnh, gọi test.
    - Thực hiện các bước để tiến gần mục tiêu.
3. **Verify results — Kiểm chứng**
    - Đọc output lệnh.
    - Kiểm tra diff và lỗi.
    - Chạy lại test hoặc điều chỉnh nếu chưa đạt.

Agent lặp lại chu trình này nhiều lần. Một tác vụ chỉ nên được xem là
hoàn thành khi kết quả được kiểm chứng, không phải chỉ vì agent nói đã
xong.

## 3.2. Tools — công cụ hành động

Tools cho phép agent tương tác với môi trường: đọc/tìm kiếm file, chỉnh
sửa nội dung, chạy lệnh terminal, kiểm tra build/test và tương tác với
dịch vụ được kết nối. Đọc file thường ít rủi ro hơn xóa file, chạy
script thay đổi dữ liệu hoặc deploy.

## 3.3. Permissions — quyền hạn

Permissions quyết định agent được phép làm gì và khi nào cần người dùng xác nhận. Nguyên tắc:

- Chỉ cấp quyền cần thiết (least privilege).
- Đọc kỹ lệnh có tác động lớn.
- Cẩn thận với xóa dữ liệu, ghi đè, cài package, truy cập mạng, secrets và deploy.
- Không đưa API key, mật khẩu hoặc token vào prompt hay file được commit.
- Không bật tự động chấp nhận cho tác vụ rủi ro cao nếu chưa hiểu phạm vi.

## 3.4. Context window — cửa sổ ngữ cảnh

Context window là lượng thông tin mô hình có thể sử dụng trong một
phiên/lượt làm việc. Nó có thể chứa yêu cầu, lịch sử hội thoại, nội dung
file đã đọc, output lệnh/test, hướng dẫn dự án và dữ liệu từ công cụ.

Khi context quá dài, thông tin quan trọng có thể bị loãng hoặc khó được
ưu tiên; hiệu suất và độ chính xác có thể giảm. Quản lý context là kỹ
năng cốt lõi.

## 3.5. Mối quan hệ giữa các thành phần

- **Model** suy luận và chọn bước tiếp theo.
- **Context** cung cấp thông tin để suy luận.
- **Tools** cho phép thực hiện hành động.
- **Permissions** giới hạn hành động được phép.

Tóm lại: Context cho agent biết điều gì đang diễn ra; tools cho agent
khả năng hành động; permissions xác định ranh giới; agentic loop giúp
thực hiện và kiểm chứng.

## 3.6. Bổ sung từ bài học gốc

- Vòng lặp: **Prompt → thu thập context → hành động → kiểm chứng → hoàn tất hoặc quay lại thử tiếp**. Bạn có thể thêm thông tin, ngắt hoặc đổi hướng Claude ở bất kỳ điểm nào trong vòng lặp.
- Khi context window đầy, Claude Code **tự động compact**: tóm tắt hoặc bỏ bớt nội dung để giải phóng chỗ.
- Bốn chế độ quyền:

| Chế độ | Hành vi |
| --- | --- |
| **Manual** | Hỏi trước mỗi lần sửa file hoặc chạy lệnh shell |
| **Auto-accept** | Tự chấp nhận sửa file và các lệnh filesystem thông dụng (`mkdir`, `mv`, `rm`); các lệnh khác vẫn phải duyệt |
| **Plan mode** | Chỉ đọc file và chạy lệnh để khảo sát, không sửa source code, rồi lập kế hoạch |
| **Auto mode** | Không hỏi các thao tác thường ngày; một bộ phân loại (classifier) chạy nền chặn các hành động không thể hoàn tác, mang tính phá hủy hoặc vượt ra ngoài môi trường |

- Chế độ khởi động mặc định tùy phiên bản và cấu hình. Mọi tùy chọn quyền nằm trong **file settings**. Bỏ qua cơ chế quyền sẽ khiến lỗi khó bị phát hiện hơn.

------------------------------------------------------------------------

# 4. Lesson 3 -- Installing Claude Code

Claude Code có thể được dùng trong terminal, VS Code, JetBrains, Claude
Desktop hoặc môi trường web được hỗ trợ.

| Môi trường | Phù hợp khi |
| --- | --- |
| Terminal | Muốn kiểm soát trực tiếp, dùng lệnh và làm việc với repository |
| VS Code | Muốn kết hợp agent với editor quen thuộc |
| JetBrains | Đang dùng IDE thuộc hệ sinh thái JetBrains |
| Claude Desktop | Muốn giao diện desktop tích hợp |
| Web | Muốn làm việc trong môi trường trình duyệt được hỗ trợ |

### Quy trình cài đặt tổng quát

1. Kiểm tra yêu cầu hệ thống và hướng dẫn cài đặt hiện hành.
2. Cài theo phương thức dành cho hệ điều hành/môi trường.
3. Mở terminal tại thư mục repository hoặc mở dự án trong IDE.
4. Đăng nhập bằng tài khoản Claude được hỗ trợ hoặc cấu hình API key
    theo hướng dẫn.
5. Kiểm tra phiên bản và xác nhận công cụ chạy được.
6. Thử một tác vụ chỉ đọc, ví dụ giải thích cấu trúc dự án.
7. Sau đó mới giao tác vụ sửa file.

Lệnh cài đặt thay đổi theo hệ điều hành và phiên bản. Ưu tiên tài liệu
chính thức: https://docs.claude.com/en/docs/claude-code/overview

## 4.1. Kiểm tra trước khi làm việc

- Đang ở đúng thư mục dự án và branch chưa?
- Git working tree có thay đổi chưa commit không?
- Có `.env`, khóa bí mật hoặc dữ liệu nhạy cảm không?
- Dự án có hướng dẫn chạy test/build không?
- Agent đang chạy với quyền nào?

**Mẹo:** Trước khi sửa code, yêu cầu agent đọc cấu trúc dự án và giải
thích cách chạy test. Điều này giúp phát hiện mở nhầm repository hoặc
thiếu dependency.

## 4.2. Bổ sung từ bài học gốc — lệnh cài đặt

| Môi trường | Lệnh |
| --- | --- |
| macOS / Linux / WSL | `curl -fsSL https://claude.ai/install.sh \| bash` |
| Windows PowerShell | `irm https://claude.ai/install.ps1 \| iex` |
| Windows CMD | `curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd` |
| Homebrew | `brew install --cask claude-code` (mặc định **không tự cập nhật**) |
| WinGet | `winget install Anthropic.ClaudeCode` (mặc định **không tự cập nhật**) |

Lệnh cài có thể thay đổi, nên đối chiếu với trang cài đặt chính thức trước khi chạy.

- Chạy `claude` trong thư mục dự án; nếu báo không tìm thấy lệnh thì mở lại terminal. Lần đầu sẽ chọn theme màu, rồi đăng nhập bằng tài khoản Claude (Pro, Max, Team, Enterprise…) hoặc API key.
- Claude Code truy cập được **thư mục nơi bạn khởi chạy và mọi thư mục con** của nó.
- **VS Code**: cài extension "Claude Code" của Anthropic (có dấu tích xanh), rồi mở bằng `Ctrl/Cmd + Shift + P` → "Claude Code: Open in New Tab" hoặc bấm logo Claude.
- **JetBrains**: cài plugin từ JetBrains Marketplace và khởi động lại IDE.
- **Desktop**: tab **Code** trong ứng dụng Claude Desktop cho phép chọn thư mục, đổi chế độ quyền và dùng môi trường cloud.
- **Web**: vào `claude.ai/code`; **chỉ làm việc với repository GitHub**.
- Cách chọn: terminal nhận tính năng mới sớm nhất; IDE gần như tương đương; Desktop hợp với việc chạy nền; web hợp với việc từ xa trên GitHub.

------------------------------------------------------------------------

# 5. Lesson 4 -- Your first prompt

Prompt tốt không chỉ nói "làm gì", mà còn mô tả kết quả mong muốn, bối
cảnh, giới hạn và cách xác minh.

## 5.1. Cấu trúc prompt hiệu quả

1. **Goal:** Cần đạt điều gì?
2. **Context:** Tính năng nằm ở đâu, công nghệ nào, vấn đề hiện tại?
3. **Scope:** File/module nào được phép hoặc nên thay đổi?
4. **Constraints:** Điều gì không được làm? Có yêu cầu tương thích nào?
5. **Acceptance criteria:** Hành vi nào phải đúng?
6. **Verification:** Cần chạy test, lint hay build nào?

Ví dụ:

``` text
Hãy khảo sát cách ứng dụng xử lý đăng nhập trước khi sửa code.

Mục tiêu: thêm chức năng đăng xuất ở trang tài khoản.
- Tìm hiểu flow authentication và convention hiện tại.
- Không thay đổi API đăng nhập.
- Khi đăng xuất, xóa session theo cơ chế hiện tại và chuyển về trang đăng nhập.
- Xử lý loading và lỗi nếu có.

Trước tiên nêu các file dự kiến thay đổi và kế hoạch.
Chưa sửa code cho đến khi tôi duyệt.
Sau khi triển khai, chạy test phù hợp và tóm tắt diff.
```

## 5.2. Ba cách kiểm soát mức độ tự chủ

**Manual approval / phê duyệt thủ công**

- Agent đề xuất hành động và chờ xác nhận ở các điểm cần quyền.
- Phù hợp với người mới, task chưa rõ hoặc thay đổi có rủi ro.
- Kiểm soát tốt nhưng cần tương tác nhiều.

**Auto-accept**

- Tự động chấp nhận một số hành động theo cấu hình.
- Tăng tốc tác vụ quen thuộc, phạm vi nhỏ.
- Rủi ro là agent có thể thực hiện thay đổi không mong muốn nhanh hơn.
- Chỉ dùng khi hiểu quyền được cấp và có Git/test để kiểm tra, khôi phục.

**Plan Mode**

- Tập trung khảo sát, phân tích và lập kế hoạch trước khi thay đổi.
- Phù hợp feature nhiều bước, refactor hoặc codebase chưa quen.
- Giúp phát hiện giả định sai trước khi triển khai.

| Tình huống | Cách phù hợp |
| --- | --- |
| Khám phá codebase | Chỉ đọc/khảo sát trước |
| Bug nhỏ, đã hiểu rõ | Triển khai với phê duyệt phù hợp |
| Feature nhiều module | Plan Mode trước |
| Xóa dữ liệu, đổi schema, deploy | Phê duyệt chặt và kiểm tra thủ công |
| Tác vụ lặp lại đã kiểm chứng | Cân nhắc auto-accept có giới hạn |

### Chia nhỏ yêu cầu

Thay vì "xây dựng lại toàn bộ website", hãy chia thành:

1. Khảo sát kiến trúc và module.
2. Đề xuất thiết kế và kế hoạch.
3. Triển khai một phần nhỏ.
4. Chạy test và review.
5. Tiếp tục phần tiếp theo.

Task càng lớn càng cần chia giai đoạn, tiêu chí nghiệm thu và điểm kiểm
tra.

## 5.3. Bổ sung từ bài học gốc

- Phím **Shift + Tab** chuyển vòng giữa các chế độ quyền (manual → auto-accept → plan mode…). Thanh trạng thái hiển thị chế độ hiện tại, ví dụ "plan mode on".
- Khi bị chặn ở **auto mode**, Claude thường chuyển sang cách an toàn hơn hoặc hỏi bạn. Không có chế độ nào là "đúng": hãy chọn mức giám sát bạn thấy thoải mái.
- **Plan Mode** sẽ hỏi lại để làm rõ, rồi trả về một kế hoạch chi tiết để bạn duyệt trước khi Claude thực thi. Chế độ này hợp với thay đổi nhiều bước và review code an toàn.
- Ví dụ của bài: bật plan mode rồi yêu cầu thêm nút chuyển sáng/tối trên header, với màu tương phản suy ra từ theme sáng hiện có.

------------------------------------------------------------------------

# 6. Lesson 5 -- The Explore → Plan → Code → Commit workflow

Đây là quy trình trung tâm của khóa học, giúp biến yêu cầu thành thay
đổi code có thể kiểm tra.

## 6.1. Explore — Khảo sát

Mục tiêu là hiểu hệ thống trước khi chỉnh sửa:

- Đọc cấu trúc thư mục.
- Tìm entry point và module liên quan.
- Đọc code hiện tại và test.
- Tìm convention, dependency và luồng gọi.
- Xác định tác động phụ và edge case.

Prompt:

``` text
Hãy chỉ khảo sát, chưa chỉnh sửa file.
Tìm hiểu luồng xử lý upload ảnh:
- Entry point và các module liên quan.
- Cách validate file.
- Nơi lưu trữ và xử lý lỗi.
- Các test hiện có.
Cuối cùng tóm tắt kiến trúc và điểm cần lưu ý.
```

Đầu ra mong muốn: bản đồ file liên quan, hiểu biết về luồng hiện tại,
rủi ro và câu hỏi còn thiếu.

## 6.2. Plan — Lập kế hoạch

Kế hoạch tốt cần nêu:

- Mục tiêu và phạm vi.
- File/module dự kiến thay đổi.
- Các bước triển khai theo thứ tự.
- Quyết định kỹ thuật và lý do.
- Rủi ro, edge case.
- Test cần thêm/cập nhật.
- Điều kiện hoàn thành.

Prompt:

``` text
Dựa trên kết quả khảo sát, đề xuất kế hoạch triển khai.
Chia thành bước nhỏ, nêu file dự kiến thay đổi, rủi ro và test cần chạy.
Nếu có nhiều phương án, so sánh ngắn và đề xuất một phương án.
Chưa viết code.
```

Đừng duyệt kế hoạch chỉ vì nghe hợp lý. Kiểm tra nó có đúng yêu cầu, giữ
tương thích và không mở rộng phạm vi không cần thiết không.

## 6.3. Code — Triển khai

- Triển khai theo từng bước.
- Giữ convention của codebase.
- Tránh sửa file không liên quan.
- Viết/cập nhật test cùng code.
- Chạy kiểm tra sau mỗi phần quan trọng.
- Khi phát sinh vấn đề, yêu cầu agent giải thích nguyên nhân trước khi
    sửa tiếp.

Prompt:

``` text
Triển khai bước 1 và 2 trong kế hoạch đã duyệt.
Giữ nguyên API hiện tại ngoài phạm vi yêu cầu.
Viết test cho hành vi mới và chạy test liên quan.
Nếu gặp vấn đề ngoài dự kiến, dừng lại và báo cáo.
```

## 6.4. Commit — Kiểm tra và ghi nhận

Trước khi commit:

- Xem `git diff`.
- Kiểm tra file mới, bị xóa và bị sửa.
- Chạy test, lint, typecheck hoặc build phù hợp.
- Kiểm tra secrets, log nhạy cảm và cấu hình môi trường.
- Đảm bảo không ghi đè thay đổi của người khác.
- Viết commit message mô tả đúng thay đổi.

Prompt review:

``` text
Review toàn bộ diff như một reviewer độc lập.
Tập trung vào bug, regression, bảo mật, xử lý lỗi và thiếu test.
Không tự sửa trong lượt review này.
Liệt kê phát hiện theo mức độ nghiêm trọng, kèm file/vị trí.
```

"Commit" trong workflow là bước chuẩn bị và ghi nhận thay đổi bằng Git;
không có nghĩa phải để agent tự commit/push mà không xem xét.

## 6.5. Vì sao workflow hiệu quả?

- Explore giảm hiểu nhầm.
- Plan giảm sửa sai hướng.
- Code biến kế hoạch thành thay đổi có kiểm soát.
- Commit/review phát hiện lỗi trước khi đưa vào nhánh chính.

Nếu test thất bại, quay lại Explore hoặc Plan để tìm nguyên nhân, không
yêu cầu agent sửa ngẫu nhiên liên tục.

## 6.6. Bổ sung từ bài học gốc

- Ví dụ prompt ở plan mode: thêm chuyển đổi ảnh sang **WebP** vào pipeline upload ảnh, hỏi rõ chỗ đặt, có cần dependency mới không và hướng tiếp cận.
- Ngoài plan mode, có thể dùng **subagent Explore** để tóm tắt codebase mà không sửa gì.
- Mẹo trong bước Code:
  - Ghi **tiêu chí thành công** vào kế hoạch để Claude tự kiểm chứng.
  - Cài extension **Claude in Chrome** để Claude Code điều khiển tab trình duyệt và test giao diện web.
  - Cho Claude một **bộ test đáng tin cậy** để liên tục đối chiếu.
  - Nếu Claude lặp lại cùng một lỗi, bảo nó **ghi cách giải vào CLAUDE.md**.
- Bước Commit: tự kiểm tra thay đổi; chạy **subagent code-reviewer** để có góc nhìn mới không bị thiên lệch bởi phiên chính; nhờ Claude viết commit message theo phong cách của bạn.

------------------------------------------------------------------------

# 7. Lesson 6 -- Context management

Context là tài nguyên hữu hạn. Phiên dài có thể tích lũy nhiều file,
output, giả định và yêu cầu cũ.

## 7.1. Vì sao cần quản lý?

Context quá lớn hoặc chứa nhiều thông tin không liên quan có thể làm
loãng yêu cầu quan trọng, khiến agent dựa vào giả định cũ, tăng chi
phí/độ trễ và giảm khả năng tập trung.

## 7.2. Các lệnh quan trọng

| Lệnh | Mục đích | Khi dùng |
| --- | --- | --- |
| `/context` | Xem tình trạng/thông tin sử dụng context | Khi phiên dài hoặc muốn kiểm tra context |
| `/compact` | Nén/tóm lược hội thoại để giải phóng không gian | Khi tiếp tục cùng nhiệm vụ nhưng context đã dài |
| `/clear` | Bắt đầu phiên hội thoại sạch | Khi chuyển nhiệm vụ hoặc muốn loại ngữ cảnh cũ |
  Hành vi chi tiết có thể thay đổi theo phiên bản; dùng `/help` để xác
nhận lệnh hiện hành.

## 7.3. Chọn đúng thao tác

- Tiếp tục cùng task: cân nhắc `/compact`, sau đó kiểm tra agent còn
    nắm mục tiêu, quyết định và trạng thái không.
- Chuyển task: dùng `/clear` để tránh thông tin cũ ảnh hưởng nhiệm vụ
    mới.
- Không rõ context đang chứa gì: dùng `/context`.
- Cần giữ trạng thái: tạo bản handoff trước khi nén hoặc mở phiên mới.

Mẫu handoff:

``` text
Tạo bản handoff để tiếp tục ở phiên sau, gồm:
1. Mục tiêu hiện tại.
2. Việc đã khảo sát và triển khai.
3. File đã thay đổi.
4. Quyết định kỹ thuật và lý do.
5. Test đã chạy và kết quả.
6. Vấn đề còn tồn tại.
7. Bước tiếp theo cụ thể.
Không khẳng định task hoàn tất nếu chưa kiểm chứng.
```

Nguyên tắc: chỉ đưa vào context file cần thiết; tránh output khổng lồ;
lưu quy tắc bền vững trong `CLAUDE.md`; dùng subagents cho việc độc lập.

## 7.4. Bổ sung từ bài học gốc

- Auto-compact xảy ra khi gần đầy context: chi tiết được tóm tắt và các kết quả tool không cần thiết bị bỏ. **Có thể mất chi tiết.**
- `/compact` giữ lại "trí nhớ" về việc đã làm; `/clear` xóa sạch, không còn nhớ gì về phiên trước; `/context` hiển thị dung lượng đã dùng, các nhóm chiếm nhiều chỗ nhất và biểu đồ trực quan.
- Prompt mơ hồ buộc Claude khám phá và suy luận nhiều hơn, nên **tốn context hơn**.
- MCP server mặc định nạp tên tool và chỉ dẫn vào context; chi tiết đầy đủ chỉ nạp khi cần. Hãy **tắt các server không liên quan** đến dự án.
- Dùng subagent cho việc tìm kiếm (ví dụ tìm các endpoint xác thực): subagent có context riêng và chỉ trả về bản tóm tắt.

------------------------------------------------------------------------

# 8. Lesson 7 -- Code review

AI có thể hỗ trợ review nhưng không thay thế kiểm thử và trách nhiệm của
lập trình viên.

## 8.1. Các khía cạnh cần review

- **Correctness:** logic có đúng yêu cầu, xử lý null/undefined, race
    condition và edge case không?
- **Regression:** có làm hỏng hành vi cũ, API hoặc module khác không?
- **Security:** có lộ secret, thiếu authorization/validation,
    injection hoặc path traversal không?
- **Maintainability:** code có theo kiến trúc, convention; có
    duplication hoặc abstraction thừa không?
- **Testing:** test có bao phủ hành vi mới, lỗi và edge case không?

## 8.2. Reviewer có context mới

Có thể dùng một reviewer/subagent độc lập, không dựa hoàn toàn vào ngữ
cảnh triển khai. Cung cấp mục tiêu ban đầu, diff và tiêu chí review; yêu
cầu chỉ báo cáo, không tự sửa. Cách này giảm nguy cơ agent tự hợp lý hóa
quyết định nó vừa đưa ra.

Prompt:

``` text
Review diff như một senior engineer độc lập. Không chỉnh sửa file.

Ưu tiên:
1. Bug có thể xảy ra trong production.
2. Regression hoặc thay đổi ngoài yêu cầu.
3. Lỗ hổng bảo mật và xử lý dữ liệu.
4. Thiếu test hoặc test sai mục tiêu.

Mỗi phát hiện cần có mức độ Critical/High/Medium/Low,
file/vị trí, điều kiện xảy ra, hậu quả và đề xuất sửa.
Nếu không tìm thấy lỗi đáng kể, nêu rõ phạm vi đã kiểm tra
và giới hạn của review.
```

Không sửa mọi nhận xét máy móc. Hãy xác minh phát hiện có đúng codebase,
tái hiện được, thuộc phạm vi và có đáng sửa hay không. Kết hợp AI review
với test tự động và review của con người.

## 8.3. Bổ sung từ bài học gốc

- **Đọc diff thật** thay vì tin bản tóm tắt của Claude, vì chính phiên đã viết code là người đánh giá kém nhất.
- `/diff` mở trình xem các thay đổi chưa commit và cho biết **từng lượt của Claude đã sửa gì**. `/diff` và `/code-review` cần git repo; nếu không có, hãy nhờ Claude review các file đã sửa bằng lời.
- Luôn kiểm tra: thay đổi không được yêu cầu, test bị nới lỏng hoặc bị xóa, package mới, giá trị bị hard-code.
- `/rewind` (hoặc nhấn **Esc hai lần** khi ô nhập trống) khôi phục code và hội thoại về điểm trước. Lệnh này **không** hoàn tác được file do lệnh shell thay đổi (ví dụ cài package).
- `/code-review` là người review thứ hai trong **context sạch**: chỉ báo cáo, không sửa gì nếu không được yêu cầu, chạy nền và có tính vào usage. Mức effort: `/code-review low` chỉ hiện phát hiện có độ tin cậy cao, `high` quét rộng hơn; mức đã chọn được giữ cho các lần sau.
- Phân loại từng phát hiện thành ba nhóm: **sửa ngay / hỏi lại lý do / để sau**. Ví dụ hỏi lại: reviewer nói thiếu `trim()` trong `isValidEmail` nhưng dòng 4 đã có. Khi yêu cầu sửa, đòi kèm bằng chứng (giữ test email rỗng, đưa output test).
- **Claude Code Review** là sản phẩm riêng cho team, đăng nhận xét lên pull request GitHub; Owner của tổ chức bật tính năng này.
- Nếu một lỗi review lặp đi lặp lại, hãy biến nó thành quy tắc trong CLAUDE.md.

------------------------------------------------------------------------

# 9. Lesson 8 -- The CLAUDE.md file

`CLAUDE.md` là file hướng dẫn Markdown giúp Claude Code hiểu cách làm
việc trong dự án. Có thể xem đây là tài liệu chỉ dẫn dự án được đưa vào
context.

## 9.1. Nên lưu nội dung gì?

- Mô tả ngắn về dự án.
- Kiến trúc và cấu trúc thư mục.
- Ngôn ngữ, framework và phiên bản.
- Quy ước đặt tên, format, coding style.
- Cách cài dependency, chạy app, test, lint, build.
- Quy tắc API, database, authentication.
- Những điều không được làm.
- Quy trình review và tiêu chí hoàn thành.

Ví dụ:

``` markdown
# Project Guide

## Overview
Ứng dụng quản lý công việc, dùng React và TypeScript.

## Architecture
- src/features/: feature theo domain.
- src/components/: UI component dùng chung.
- src/lib/: tiện ích và tích hợp bên ngoài.
- tests/: test tự động.

## Development
- Install: npm install
- Dev: npm run dev
- Test: npm test
- Lint: npm run lint
- Build: npm run build

## Conventions
- Ưu tiên TypeScript strict.
- Component dùng PascalCase.
- Hàm và biến dùng camelCase.
- Tái sử dụng component hiện có trước khi tạo mới.
- Không thêm dependency nếu chưa giải thích lý do.

## Safety
- Không commit .env, token hoặc secret.
- Không chạy lệnh xóa dữ liệu production.
- Không đổi schema nếu chưa có migration và kế hoạch rollback.

## Before Finishing
- Chạy test liên quan.
- Kiểm tra lint và typecheck.
- Tóm tắt file thay đổi và kết quả kiểm chứng.
```

Đây là ví dụ minh họa, cần điều chỉnh theo repository thật.

## 9.2. Nguyên tắc viết tốt

- Ngắn, cụ thể, có thể hành động.
- Ưu tiên quy tắc ổn định, dùng thường xuyên.
- Không nhồi toàn bộ tài liệu vào một file.
- Không ghi thông tin bí mật.
- Loại bỏ hướng dẫn lỗi thời/mâu thuẫn.
- Phân biệt quy tắc bắt buộc và gợi ý.

`CLAUDE.md` không phải cơ chế bảo mật tuyệt đối. Quyền thực tế vẫn cần
cấu hình qua permissions và môi trường thực thi.

## 9.3. Bổ sung từ bài học gốc

- CLAUDE.md được **đọc tự động ở đầu mỗi phiên** và nội dung được nối vào prompt, giống "kịch bản onboarding" cho codebase.
- Ví dụ của bài: Next.js 15 (App Router), Tailwind, Drizzle ORM; lệnh `pnpm dev`, `pnpm test`, `pnpm lint`; thụt lề 2 dấu cách, ưu tiên named export, API route đặt trong `app/api/`, ưu tiên server action hơn API route.
- Hai cấp:
  - **Project**: file ở thư mục gốc dự án, commit vào Git để cả team dùng.
  - **User**: file trong thư mục cấu hình cá nhân (`~/.claude/CLAUDE.md`), áp dụng cho mọi dự án.
- Mẹo:
  - Khi phải sửa Claude lặp lại một điều, bảo nó lưu quy tắc vào bộ nhớ.
  - Tham chiếu tài liệu bằng `@` + đường dẫn, ví dụ `@README.md`.
  - Có thể bắt đầu không cần CLAUDE.md, rồi chạy `/init` để Claude tự tạo một file.

------------------------------------------------------------------------

# 10. Lesson 9 -- Subagents

## 10.1. Subagent là gì?

Subagent là agent phụ được giao nhiệm vụ hoặc vai trò chuyên biệt. Agent
chính ủy quyền công việc, nhận kết quả rồi tổng hợp.

Ví dụ:

- **Codebase explorer:** khảo sát kiến trúc, tìm file liên quan.
- **Test specialist:** đề xuất hoặc chạy test.
- **Code reviewer:** tìm bug/rủi ro trong diff.
- **Documentation assistant:** cập nhật tài liệu.
- **Security reviewer:** rà soát vấn đề bảo mật trong phạm vi được giao.

## 10.2. Khi nào nên dùng?

- Có nhiều nhiệm vụ độc lập.
- Cần góc nhìn chuyên môn khác nhau.
- Muốn tách khảo sát/review khỏi context triển khai.
- Muốn tái sử dụng vai trò cho nhiều task.

Không cần tạo subagent cho mọi thao tác nhỏ; việc ủy quyền có chi phí
điều phối và kết quả vẫn phải được kiểm tra.

## 10.3. Giao việc hiệu quả

Nêu rõ vai trò, mục tiêu, phạm vi file/module, quyền được phép, định
dạng đầu ra và điều không được làm.

Ví dụ:

``` text
Giao một subagent đóng vai trò code reviewer.
Kiểm tra diff của tính năng upload ảnh.
Không sửa file và không chạy lệnh ghi dữ liệu.
Báo cáo theo mức độ nghiêm trọng, kèm vị trí,
điều kiện tái hiện và đề xuất khắc phục.
```

Agent chính giữ mục tiêu tổng thể, quyết định kiến trúc, kế hoạch và
trạng thái. Subagent xử lý phần việc tách biệt; kết quả trả về nên có
cấu trúc, tập trung.

## 10.4. Bổ sung từ bài học gốc

- Subagent có thể **chạy song song**, mỗi subagent có context riêng và chỉ trả về bản tóm tắt.
- Subagent là file Markdown có YAML frontmatter. Cách dễ nhất là mô tả cho Claude và để nó tự tạo, ví dụ: "tạo subagent code-reviewer chỉ đọc trong `.claude/agents/`, kiểm tra bug và khả năng đọc hiểu".
- `.claude/agents/` chỉ dùng cho dự án hiện tại; `~/.claude/agents/` dùng cho mọi dự án. Nếu thư mục chưa tồn tại lúc mở phiên, cần khởi động lại Claude Code.
- Trường `description` cho Claude biết khi nào nên gọi subagent.
- Subagent có thể có **bộ nhớ bền vững** giữa các cuộc hội thoại. Khóa `skills` nạp sẵn **toàn bộ nội dung** skill vào context của subagent, khác với cơ chế nạp theo nhu cầu ở hội thoại chính.

------------------------------------------------------------------------

# 11. Lesson 10 -- Skills

## 11.1. Skill là gì?

Skills là hướng dẫn/quy trình tái sử dụng để Claude Code thực hiện một
nhóm công việc nhất quán. Hữu ích khi thường xuyên yêu cầu agent làm
cùng một quy trình.

Ví dụ:

- Tạo API endpoint theo convention.
- Viết unit test.
- Review pull request.
- Tạo UI component theo design system.
- Chuẩn hóa tài liệu kỹ thuật.
- Kiểm tra accessibility.

## 11.2. Phân biệt CLAUDE.md, Skill và Subagent

| Thành phần | Vai trò |
| --- | --- |
| `CLAUDE.md` | Quy tắc nền tảng, hướng dẫn chung của dự án |
| Skill | Quy trình chuyên biệt có thể tái sử dụng |
| Subagent | Tác nhân/vai trò riêng để làm một phần việc |
Ví dụ: `CLAUDE.md` quy định dự án dùng TypeScript và yêu cầu test; Skill
`create-api` hướng dẫn tạo endpoint, validate request và viết test;
Subagent `reviewer` độc lập rà soát diff.

## 11.3. Một Skill tốt nên có

1. Tên và mục đích rõ.
2. Trường hợp nên dùng.
3. Đầu vào cần thiết.
4. Các bước thực hiện.
5. Quy tắc và giới hạn.
6. Tiêu chí đầu ra.
7. Ví dụ nếu quy trình dễ hiểu nhầm.

Ví dụ nội dung:

``` markdown
# API Endpoint Workflow

## When to use
Khi cần thêm hoặc cập nhật API endpoint.

## Steps
1. Khảo sát route và pattern hiện có.
2. Xác định request/response contract.
3. Kiểm tra authentication và authorization.
4. Triển khai theo convention.
5. Viết test cho success và error cases.
6. Chạy test, lint và typecheck.

## Constraints
- Không tự ý đổi API contract cũ.
- Không thêm dependency nếu chưa nêu lý do.
- Không ghi secret vào log.

## Output
Tóm tắt endpoint, file thay đổi, test đã chạy và kết quả.
```

Skill chuẩn hóa quy trình nhưng không bảo đảm kết quả đúng; vẫn cần kiểm
chứng.

## 11.4. Bổ sung từ bài học gốc

- Skill cá nhân đặt ở `~/.claude/skills/` (ví dụ: phong cách commit message, định dạng tài liệu). Skill dự án đặt ở `.claude/skills/` trong repo, ai clone repo đều dùng được (ví dụ: brand guideline, font, màu thiết kế web).
- Ban đầu chỉ **tên và description** của skill được nạp; nội dung đầy đủ chỉ nạp khi khớp với yêu cầu, nhờ vậy tiết kiệm context.
- Slash command phải gõ thủ công; skill tự áp dụng khi Claude nhận ra tình huống phù hợp.

------------------------------------------------------------------------

# 12. Lesson 11 -- MCP (Model Context Protocol)

## 12.1. MCP là gì?

MCP (Model Context Protocol) là giao thức mở giúp ứng dụng AI kết nối
với công cụ và nguồn dữ liệu bên ngoài theo cách có cấu trúc.

Thông qua MCP server, Claude Code có thể được cung cấp khả năng tương tác với:

- Công cụ quản lý issue/project.
- Git hosting.
- Cơ sở dữ liệu.
- Tài liệu nội bộ.
- Dịch vụ thiết kế hoặc hệ thống nghiệp vụ.
- API và công cụ phát triển khác.

## 12.2. Các thành phần cơ bản

- **MCP Host:** ứng dụng AI sử dụng MCP, ví dụ Claude Code.
- **MCP Client:** thành phần trong host duy trì kết nối và trao đổi
    giao thức.
- **MCP Server:** cung cấp tools, resources hoặc prompts.
- **Tools:** hành động có thể gọi, ví dụ tìm issue hoặc tạo ticket.
- **Resources:** dữ liệu/ngữ cảnh có thể đọc.
- **Prompts:** mẫu prompt/quy trình do server cung cấp nếu có.

## 12.3. MCP và API thông thường

API là giao diện của một dịch vụ. MCP chuẩn hóa cách ứng dụng AI khám
phá và sử dụng công cụ/ngữ cảnh từ các server. Một MCP server có thể bọc
API hiện có để agent sử dụng thuận tiện hơn.

## 12.4. Quy trình tích hợp tổng quát

1. Chọn MCP server đáng tin cậy.
2. Đọc tài liệu, quyền truy cập và xác thực.
3. Cấu hình server theo tài liệu Claude Code hiện hành.
4. Kiểm tra kết nối.
5. Xem tools server cung cấp.
6. Thử tác vụ chỉ đọc trước.
7. Chỉ cấp quyền ghi khi cần.

## 12.5. Bảo mật

- Chỉ dùng server từ nguồn đáng tin cậy.
- Đọc kỹ tool có quyền đọc, ghi hay xóa.
- Giới hạn quyền theo least privilege.
- Không chia sẻ token trong repository.
- Cân nhắc dữ liệu nhạy cảm gửi sang dịch vụ ngoài.
- Xem nội dung trả về từ công cụ là dữ liệu đầu vào, không phải chỉ
    thị đáng tin tuyệt đối.

MCP mở rộng năng lực agent nhưng cũng mở rộng bề mặt rủi ro. Hãy đánh
giá MCP server như một dependency có quyền truy cập hệ thống.

## 12.6. Bổ sung từ bài học gốc

- Ví dụ server: **Linear** (lấy chi tiết issue), **Context7** (tài liệu mới nhất của các dependency).
- Thêm server bằng `claude mcp add`. Có hai loại kết nối:
  - **HTTP**: server từ xa, do nhà cung cấp host.
  - **stdio**: process chạy local trên máy bạn.
- `/mcp` trong phiên hiển thị các server đã kết nối, trạng thái của chúng và cho phép tắt server không cần.
- Ba scope:
  - **Local**: dự án hiện tại, chỉ riêng bạn.
  - **User**: mọi dự án của bạn.
  - **Project**: file `.mcp.json`, commit vào Git để chia sẻ.
- Mỗi server đã kết nối đều thêm tên tool và chỉ dẫn vào context, kể cả khi không dùng. Một CLI tương đương (như `gh`, `aws`) **tiết kiệm context hơn**. **Tool search** bật mặc định, giữ định nghĩa đầy đủ của tool MCP ngoài context cho tới khi cần.
- Kết hợp MCP server với một Skill hướng dẫn cách dùng tool sẽ cho kết quả tốt hơn.

------------------------------------------------------------------------

# 13. Lesson 12 -- Hooks

## 13.1. Hooks là gì?

Hooks là cơ chế chạy hành động/kiểm tra tại những thời điểm nhất định
trong vòng đời làm việc của Claude Code. Chúng giúp áp dụng quy tắc
**deterministic** — dựa trên cấu hình và sự kiện, thay vì chỉ hy vọng
mô hình luôn nhớ.

Ví dụ:

- Tự động format sau khi file được sửa.
- Chặn một số lệnh không được phép.
- Chạy kiểm tra trước/sau một hành động.
- Gửi thông báo khi task hoàn thành hoặc cần chú ý.
- Ghi log sự kiện.

## 13.2. Hooks khác prompt như thế nào?

| Prompt / hướng dẫn | Hook |
| --- | --- |
| Nói điều agent nên làm | Chạy logic theo sự kiện đã cấu hình |
| Phụ thuộc mô hình diễn giải và tuân thủ | Có tính xác định hơn nếu cấu hình đúng |
| Hợp với mục tiêu, phong cách, quy trình | Hợp với guardrail, validation, notification |
  Ví dụ: "Nhớ chạy formatter" là hướng dẫn. Hook chạy formatter sau sự
kiện chỉnh sửa file là cơ chế tự động.

## 13.3. Nguyên tắc thiết kế

- Làm một việc rõ ràng.
- Chạy nhanh, tránh làm chậm agent.
- Có log và thông báo lỗi dễ hiểu.
- Không chứa secret.
- Kiểm thử trong repository thử nghiệm.
- Không xem hook là biện pháp bảo vệ duy nhất cho thao tác cực kỳ nhạy
    cảm.

Tên sự kiện, cấu trúc cấu hình và khả năng hooks có thể thay đổi theo
phiên bản; cần tra tài liệu chính thức trước khi cấu hình.

## 13.4. Bổ sung từ bài học gốc

- Hook có tính **xác định (deterministic)**: luôn chạy, còn chỉ dẫn trong CLAUDE.md thì Claude có thể bỏ qua. Nguyên tắc: điều gì *bắt buộc lần nào cũng xảy ra* thì đặt vào hook.
- Cấu hình trong `settings.json` gồm sự kiện, matcher tool (tùy chọn) và lệnh. Đặt trong `.claude/settings.json` thì là cấp dự án và có thể commit để chia sẻ với team. Lệnh `/hooks` hiển thị danh sách hook đã cấu hình.
- Các sự kiện chính:

| Sự kiện | Thời điểm |
| --- | --- |
| `PreToolUse` | Trước khi gọi tool (nhận tên tool và input dạng JSON qua stdin) |
| `PostToolUse` | Sau khi tool chạy xong |
| `UserPromptSubmit` | Khi gửi prompt, trước khi Claude xử lý |
| `Stop` | Khi Claude trả lời xong |
| `Notification` | Khi Claude gửi thông báo |

- Mã thoát:
  - **0**: cho phép tiếp tục.
  - **2**: **chặn hành động** và gửi stderr về cho Claude làm phản hồi.
  - Mã khác: lỗi không chặn, chỉ hiển thị cho bạn.
- Ví dụ:
  - Hook `PostToolUse` với matcher `"Edit|Write"` chạy formatter theo đuôi file (Prettier cho TypeScript, gofmt cho Go).
  - Hook `PreToolUse` chặn việc ghi vào thư mục config production, lệnh chứa `rm -rf`, hoặc commit thẳng vào `main`.
- Dùng biến môi trường `CLAUDE_PROJECT_DIR` để trỏ tới script của dự án, bất kể thư mục làm việc hiện tại là gì.

------------------------------------------------------------------------

# 14. Bảng tổng hợp các khái niệm

| Thành phần | Trả lời câu hỏi | Công dụng |
| --- | --- | --- |
| Agentic loop | Agent làm việc theo nhịp nào? | Thu thập context → hành động → kiểm chứng |
| Context window | Agent có thể dựa vào thông tin nào? | Quản lý thông tin trong phiên |
| Tools | Agent có thể làm gì? | Đọc/sửa file, chạy lệnh, kết nối dịch vụ |
| Permissions | Agent được phép làm gì? | Kiểm soát quyền và phê duyệt |
| `CLAUDE.md` | Dự án có quy tắc nền nào? | Ghi nhớ convention và hướng dẫn |
| Subagents | Có thể ủy quyền vai trò nào? | Tách nhiệm vụ, có góc nhìn độc lập |
| Skills | Quy trình nào cần tái sử dụng? | Chuẩn hóa công việc lặp lại |
| MCP | Kết nối công cụ/dữ liệu ngoài ra sao? | Mở rộng khả năng tương tác |
| Hooks | Quy tắc nào cần tự động thực thi? | Guardrails, format, validation, notification |

# 15. Thực hành mẫu: Thêm bộ lọc trạng thái đơn hàng

### Bước 1 --- Explore

``` text
Khảo sát luồng hiển thị danh sách đơn hàng.
Tìm component, API, model dữ liệu và test liên quan.
Chưa thay đổi file. Tóm tắt kiến trúc và nơi phù hợp để thêm bộ lọc.
```

### Bước 2 --- Plan

``` text
Đề xuất kế hoạch thêm bộ lọc theo trạng thái đơn hàng.
Nêu file dự kiến thay đổi, cách quản lý state, cách cập nhật query/API,
các trạng thái cần hỗ trợ và test cần có. Chưa triển khai.
```

### Bước 3 --- Code

``` text
Triển khai kế hoạch đã duyệt.
Giữ nguyên hành vi mặc định khi chưa chọn bộ lọc.
Xử lý loading, empty state và lỗi.
Viết test cho từng trạng thái lọc và chạy test liên quan.
```

### Bước 4 --- Review

``` text
Review diff như reviewer độc lập.
Tập trung vào logic lọc, đồng bộ URL/query, empty state,
regression và thiếu test. Chỉ báo cáo, không sửa.
```

### Bước 5 --- Hoàn tất

- Xem diff.
- Chạy test/lint/build.
- Kiểm tra thủ công trên giao diện.
- Xác nhận không có file ngoài phạm vi bị thay đổi.
- Commit khi đã hài lòng với kết quả.

------------------------------------------------------------------------

# 16. Checklist sử dụng Claude Code

## Trước khi giao việc

- [ ] Đang ở đúng repository và branch.
- [ ] Đã kiểm tra thay đổi Git chưa commit.
- [ ] Prompt nêu rõ mục tiêu và tiêu chí hoàn thành.
- [ ] Phạm vi và giới hạn được mô tả.
- [ ] Chọn mức quyền phù hợp rủi ro.

## Trong khi làm việc

- [ ] Khảo sát trước khi sửa.
- [ ] Duyệt kế hoạch với task nhiều bước.
- [ ] Chia task lớn thành phần nhỏ.
- [ ] Theo dõi file thay đổi và lệnh được chạy.
- [ ] Quản lý context khi phiên dài.

## Trước khi hoàn tất

- [ ] Đã xem `git diff`.
- [ ] Đã chạy test phù hợp.
- [ ] Đã chạy lint/typecheck/build nếu cần.
- [ ] Đã xem xét bảo mật và regression.
- [ ] Đã kiểm tra file ngoài phạm vi và secret.
- [ ] Tóm tắt điều đã làm, chưa làm và giới hạn kiểm chứng.

------------------------------------------------------------------------

# 17. Tổng kết và câu hỏi ôn tập

## 17.1. Lộ trình thực hành sau khóa học

1. **Ngày 1:** Cài Claude Code, mở repository nhỏ và yêu cầu giải thích
    cấu trúc.
2. **Ngày 2:** Dùng Explore → Plan → Code để sửa một bug nhỏ.
3. **Ngày 3:** Yêu cầu agent viết test và review diff.
4. **Ngày 4:** Tạo `CLAUDE.md` chứa hướng dẫn thực tế của dự án.
5. **Ngày 5:** Thử subagent chuyên review hoặc khảo sát.
6. **Ngày 6:** Tạo Skill cho quy trình lặp lại thường xuyên.
7. **Ngày 7:** Tìm hiểu MCP server đáng tin cậy; thử tác vụ chỉ đọc
    trước.
8. **Sau đó:** Thử hooks trong môi trường an toàn để tự động format
    hoặc validation.

## 17.2. Câu hỏi tự kiểm tra

1. Claude Code khác chatbot AI thông thường ở điểm nào?
2. Ba giai đoạn của agentic loop là gì?
3. Context window ảnh hưởng thế nào đến phiên làm việc dài?
4. Tools và permissions khác nhau ra sao?
5. Khi nào nên dùng Plan Mode?
6. Vì sao không nên bật auto-accept cho mọi tác vụ?
7. Bốn bước của workflow cốt lõi là gì?
8. Explore cần tìm hiểu gì trước khi sửa code?
9. Kế hoạch triển khai tốt cần nêu những gì?
10. Vì sao phải xem diff và chạy test trước khi commit?
11. `/compact`, `/clear` và `/context` có tác dụng gì?
12. Nên đưa thông tin nào vào `CLAUDE.md`?
13. Subagent hữu ích trong tình huống nào?
14. Skill khác `CLAUDE.md` ở điểm nào?
15. MCP giúp Claude Code mở rộng khả năng ra sao?
16. Vì sao cần đánh giá độ tin cậy và quyền của MCP server?
17. Hooks khác hướng dẫn bằng prompt thế nào?
18. Vì sao agent báo "đã xong" chưa đủ để kết luận task hoàn tất?

## Đáp án gợi ý

1. Coding agent có thể khám phá codebase, gọi công cụ, sửa file và kiểm
    chứng nhiều bước.
2. Gather context → Take action → Verify results.
3. Context lớn có thể làm loãng thông tin quan trọng và giảm tập trung.
4. Tools là khả năng hành động; permissions là ranh giới cho phép hành
    động.
5. Khi task nhiều bước, chưa rõ kiến trúc hoặc cần duyệt phương án
    trước khi sửa.
6. Vì agent có thể thực hiện hành động không mong muốn nhanh hơn khả
    năng kiểm tra của người dùng.
7. Explore → Plan → Code → Commit.
8. Cấu trúc, luồng xử lý, module, convention, test và rủi ro.
9. Phạm vi, file, bước làm, quyết định, rủi ro và cách kiểm chứng.
10. Để phát hiện lỗi, thay đổi ngoài ý muốn và bảo đảm chất lượng.
11. Nén context, làm sạch phiên và xem tình trạng context.
12. Quy tắc dự án ổn định, cấu trúc, lệnh chạy/test và convention.
13. Khi có nhiệm vụ độc lập hoặc cần vai trò chuyên biệt như
    review/test/khảo sát.
14. CLAUDE.md là hướng dẫn nền; Skill là quy trình chuyên biệt tái sử
    dụng.
15. Kết nối công cụ và nguồn dữ liệu bên ngoài qua giao thức chuẩn.
16. Vì MCP server có thể truy cập dữ liệu hoặc thực hiện hành động với
    quyền được cấp.
17. Prompt là hướng dẫn cho mô hình; hook là hành động tự động gắn với
    sự kiện.
18. Cần kiểm tra diff, test và kết quả thực tế.

------------------------------------------------------------------------

## Nguồn tham khảo

### Tài liệu khóa học

- [Claude Code 101 --- Claude Academy](https://academy.claude.com/courses/claude-code-101)
- [Claude Code overview](https://docs.claude.com/en/docs/claude-code/overview)
- [Claude Code documentation](https://docs.claude.com/en/docs/claude-code)

### Tài liệu bổ sung

- [Model Context Protocol](https://modelcontextprotocol.io/)

> **Ghi chú về phạm vi:** Tài liệu này tổng hợp 12 bài học của khóa học Claude Code 101 chính thức, bổ sung cấu hình và quy trình thực hành. Tên lệnh, cách cài đặt, cấu hình MCP, skills, subagents và hooks có thể thay đổi theo phiên bản; hãy đối chiếu tài liệu chính thức khi thao tác thực tế.


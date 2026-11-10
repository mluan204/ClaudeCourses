# Claude Code in Action

## Tài liệu học tập -- Claude Academy

> **Khóa học:** Claude Code in Action\
> **Nền tảng:** Claude Code (CLI và môi trường Claude Code)\
> **Đơn vị:** Anthropic / Claude Academy\
> **Thời lượng:** khoảng 1 giờ\
> **Số bài học:** 9 bài học + 1 quiz\
> **Đối tượng:** Người đã dùng Claude Code cho prompt đơn lẻ, có kiến thức cơ bản về Git và command line\
> **Mục tiêu:** Điều khiển phiên dài, cấu hình hướng dẫn và quyền phù hợp, tự động hóa công việc lặp lại, kiểm chứng kết quả chạy không giám sát và chia sẻ cấu hình trong nhóm.

> **Phạm vi ghi chú:** Bài viết diễn giải nội dung của 9 lesson trên Claude Academy, không phải transcript. Các mục “gợi ý thực hành” và câu hỏi ôn tập do người biên soạn tổng hợp. Nội dung được đối chiếu với trang khóa học chính thức ngày 10/11/2026.

---

## Mục lục

1. [Tổng quan khóa học](#1-tổng-quan-khóa-học)
2. [Lesson 1 -- Steering long sessions](#2-lesson-1----steering-long-sessions)
3. [Lesson 2 -- A `CLAUDE.md` that follows](#3-lesson-2----a-claudemd-that-follows)
4. [Lesson 3 -- Verification skills](#4-lesson-3----verification-skills)
5. [Lesson 4 -- Permission modes](#5-lesson-4----permission-modes)
6. [Lesson 5 -- Hooks](#6-lesson-5----hooks)
7. [Lesson 6 -- Scheduled tasks and headless](#7-lesson-6----scheduled-tasks-and-headless)
8. [Lesson 7 -- GitHub Actions and Code Review](#8-lesson-7----github-actions-and-code-review)
9. [Lesson 8 -- Trust it: Verifying unsupervised runs](#9-lesson-8----trust-it-verifying-unsupervised-runs)
10. [Lesson 9 -- Plugins](#10-lesson-9----plugins)
11. [Quiz và câu hỏi ôn tập](#11-quiz-và-câu-hỏi-ôn-tập)
12. [Workflow tổng hợp đề xuất](#12-workflow-tổng-hợp-đề-xuất)
13. [Checklist triển khai vào dự án](#13-checklist-triển-khai-vào-dự-án)
14. [Thuật ngữ](#14-thuật-ngữ)
15. [Nguồn tham khảo](#15-nguồn-tham-khảo)
16. [Kết luận](#16-kết-luận)

---

# 1. Tổng quan khóa học

## 1.1. Khóa học hướng đến điều gì?

Khóa học dành cho người đã quen giao prompt đơn lẻ cho Claude Code và muốn chuyển sang các phiên dài, ít giám sát hơn, có thể tự động hóa và chia sẻ trong nhóm.

Các kỹ năng chính:

- Xác định phạm vi trước khi chạy và điều hướng Claude trong phiên dài.
- Viết `CLAUDE.md` gọn, tạo Skill cho quy trình lặp lại, chọn permission mode phù hợp và dùng Hooks cho quy tắc bắt buộc.
- Tự động hóa qua cloud scheduled tasks, non-interactive mode và GitHub.
- Kiểm chứng kết quả theo mức độ giám sát và đóng gói cấu hình thành Plugin.

## 1.2. Bốn nhóm nội dung

| Nhóm | Lesson | Năng lực |
| --- | --- | --- |
| Điều khiển công việc | 1 | Plan mode, compaction, rewind, goal, loop và worktrees |
| Cấu hình Claude | 2–5 | `CLAUDE.md`, Skills, permission modes và Hooks |
| Tự động hóa | 6–7 | Cloud scheduled tasks, non-interactive mode, GitHub Actions và Code Review |
| Kiểm chứng và chia sẻ | 8–9 | Kiểm chứng phiên chạy không giám sát và đóng gói Plugins |

## 1.3. Nguyên tắc xuyên suốt

Mức độ tự chủ càng cao thì càng cần tiêu chí hoàn thành rõ, quyền phù hợp và bằng chứng kiểm chứng. Tự động hóa không thay thế việc xem xét kết quả.

**Nguồn:** [Claude Code in Action — trang khóa học](https://academy.claude.com/courses/claude-code-in-action).

---

# 2. Lesson 1 -- Steering long sessions

**Chủ đề:** Điều khiển phiên Claude Code kéo dài.  
**Thời lượng trên trang khóa học:** khoảng 6 phút.

## 2.1. Vì sao phiên dài khó kiểm soát?

Task nhiều bước như refactor nhiều file hoặc xây một tính năng có thể kéo dài hàng giờ. Hai thói quen nền tảng là **scope trước khi chạy** và **steer trong khi Claude làm việc**.

## 2.2. Plan mode — lập kế hoạch trước khi sửa code

Trong Plan mode, Claude khảo sát code mà không sửa source files, sau đó đưa kế hoạch để người dùng xem xét.

1. Nêu mục tiêu, phạm vi và giới hạn.
2. Yêu cầu Claude khảo sát repository và lập kế hoạch.
3. Đọc kỹ kế hoạch, bổ sung phần thiếu hoặc sửa giả định sai.
4. Chuyển sang triển khai sau khi kế hoạch đủ rõ.

Điều chỉnh kế hoạch trước khi chạy thường ít tốn công hơn sửa một loạt thay đổi đã triển khai.

## 2.3. Compaction — nén context có định hướng

Lệnh `/compact` tóm tắt hội thoại, dùng summary làm context mới và bỏ các tin nhắn cũ. Summary có thể làm mất chi tiết quan trọng, nên có thể thêm chỉ dẫn để định hướng phần cần giữ:

```text
/compact Focus on the --version flag implementation
```

Có thể chỉ rõ cần giữ mục tiêu, tiêu chí hoàn thành, quyết định đã thống nhất, file đã sửa, trạng thái test và việc còn dang dở.

## 2.4. Rewind — quay lại checkpoint

Mỗi user prompt tạo một checkpoint. Khi prompt đang trống, nhấn **Esc** hai lần để mở menu rewind.

- **Restore code and conversation:** khôi phục cả code lẫn hội thoại.
- **Restore conversation:** chỉ khôi phục hội thoại.
- **Restore code:** chỉ khôi phục file.
- **Summarize from here:** tóm tắt phần sau checkpoint.
- **Summarize up to here:** tóm tắt phần trước checkpoint.

Dùng Restore để quay lại trạng thái trước một hướng triển khai sai. Các lựa chọn Summarize giúp nén context mà không nhất thiết bỏ code.

## 2.5. Goal — chạy đến điều kiện hoàn thành

Lệnh `/goal` đặt điều kiện “done” để Claude tiếp tục qua nhiều lượt cho đến khi evaluator xác nhận điều kiện được đáp ứng.

Ví dụ:

```text
/goal all tests in src/billing pass, and the type checker reports zero errors
```

Hủy goal bằng `/goal clear`. Evaluator chỉ đọc transcript, vì vậy điều kiện cần kiểm tra được từ kết quả Claude ghi ra, chẳng hạn output chạy test.

## 2.6. Loop — lặp prompt theo chu kỳ

Loop chạy prompt theo khoảng thời gian giữa các lượt, cố định hoặc tự điều chỉnh. Nó phù hợp để polling tiến trình bên ngoài, như trạng thái CI hoặc deploy, rồi hành động khi trạng thái thay đổi.

- Nhấn **Esc** để dừng self-paced loop khi nó đang chờ lượt tiếp theo.
- Loop theo chu kỳ cố định tiếp tục đến khi được yêu cầu hủy hoặc hết giới hạn 7 ngày.

## 2.7. Worktrees — chạy nhiều phiên song song

Nhiều phiên cùng sửa một working tree có thể gây xung đột. Git worktrees cấp cho mỗi phiên một cây file riêng; worktree sạch được dọn khi phiên kết thúc.

File `.worktreeinclude` ở gốc repository có thể liệt kê file bị Git ignore cần sao chép vào mỗi worktree, chẳng hạn file môi trường hoặc cấu hình local không được commit.

## 2.8. Bài học rút ra

- Scope trước, triển khai sau.
- Chỉ đạo nội dung cần giữ khi compact.
- Dùng rewind để quay lại khi Claude đi sai hướng.
- Dùng goal khi “done” có thể kiểm tra rõ ràng.
- Dùng loop để polling tác vụ bên ngoài.
- Dùng worktrees để cô lập các phiên làm việc song song.

**Nguồn:** [Lesson 1 — Steering long sessions](https://academy.claude.com/courses/claude-code-in-action/steering-long-sessions).

---

# 3. Lesson 2 -- A `CLAUDE.md` that follows

**Chủ đề:** Viết `CLAUDE.md` gọn, rõ và có khả năng được Claude tuân theo.  
**Thời lượng trên trang khóa học:** khoảng 7 phút.

## 3.1. `CLAUDE.md` là hướng dẫn, không phải cơ chế cưỡng chế

`CLAUDE.md` cung cấp bối cảnh và hướng dẫn cho Claude Code. Nó không tự chặn hành động. Các dòng trong file cùng cạnh tranh sự chú ý; file càng dài thì một quy tắc riêng lẻ càng dễ bị bỏ qua.

Giữ lại hướng dẫn quan trọng, dùng thường xuyên và áp dụng rộng. Với quy tắc bắt buộc như “không push lên main”, dùng Hook có thể chặn hành động thay vì chỉ ghi lời nhắc.

## 3.2. Bốn vị trí của `CLAUDE.md`

| Vị trí | Dùng cho |
| --- | --- |
| Managed policy | Quy tắc do tổ chức quản lý; người dùng không thể loại trừ |
| User | Sở thích cá nhân áp dụng qua các dự án trên máy |
| Project | Quy tắc chung được chia sẻ cùng repository |
| Local | Ghi chú riêng cho một repository, không chia sẻ với nhóm |

Các vị trí này được kết hợp khi Claude Code khởi chạy. Hãy đặt nội dung vào đúng phạm vi để ghi chú cá nhân không trở thành quy tắc áp dụng cho cả nhóm.

## 3.3. Chia file dài bằng imports

Có thể import file khác bằng cú pháp đường dẫn:

```text
@.claude/conventions/code-style.md
@.claude/conventions/testing.md
@.claude/conventions/workflow.md
```

Imports giúp tổ chức file, nhưng Claude mở rộng nội dung inline khi khởi chạy. Chúng **không** giảm lượng context được nạp; dùng để sắp xếp tài liệu, không phải để làm nhẹ context.

## 3.4. Viết quy tắc cụ thể và kiểm tra được

Quy tắc tốt nói rõ vị trí, hành động hoặc kết quả cần đạt:

- Mơ hồ: “Tuân theo best practices cho API.”
- Cụ thể: “Đặt mỗi API route mới trong `src/api/handlers`, một route mỗi file.”

Khi cấm một cách làm, nêu luôn cách thay thế. Ví dụ: “Dùng named exports thay cho default exports.”

Dùng từ nhấn mạnh có chừng mực. Nếu mọi quy tắc đều là “IMPORTANT” hoặc “MUST”, các quy tắc quan trọng nhất không còn nổi bật.

## 3.5. Coi `CLAUDE.md` như tài liệu sống

Khi Claude lặp lại một lỗi, xem đó như tín hiệu để rà lại hướng dẫn. Thêm hoặc sửa quy tắc nếu nó áp dụng rộng; chuyển thủ tục riêng của một loại task sang Skill và chuyển điều kiện không được bỏ qua sang Hook.

**Ghi nhớ:** Giữ `CLAUDE.md` gọn, bỏ dòng không còn cần thiết, và đặt từng loại hướng dẫn vào đúng nơi.

**Nguồn:** [Lesson 2 — A `CLAUDE.md` that follows](https://academy.claude.com/courses/claude-code-in-action/a-claude-md-that-follows).

---

# 4. Lesson 3 -- Verification skills

**Chủ đề:** Đóng gói quy trình kiểm chứng thành Skill có thể tái sử dụng.  
**Thời lượng trên trang khóa học:** khoảng 5 phút.

## 4.1. Vì sao nên bắt đầu bằng verification skill?

Một Skill kiểm chứng có thể tự được kích hoạt khi mô tả task phù hợp. Quy trình mẫu trong lesson:

1. Chạy test suite.
2. Đọc diff.
3. Kiểm tra xem test có bị làm yếu chỉ để pass hay không.
4. Báo pass/fail kèm bằng chứng.

Chạy test xanh chưa đủ nếu thay đổi đã làm test mất ý nghĩa. Cần kiểm tra cả kết quả test lẫn diff.

## 4.2. Skill có thể chứa tài liệu và script

Thư mục Skill có thể chứa thêm tài liệu tham khảo và script:

- `reference.md` chứa giải thích dài hơn; `SKILL.md` liên kết đến đó để chỉ nạp khi cần.
- Script như `check.sh` chạy các bước kiểm tra mà không phải nạp toàn bộ nội dung script vào context.

Giữ `SKILL.md` ngắn, mô tả khi nào Skill được dùng và các bước cần thực hiện.

## 4.3. Chọn đúng nơi đặt quy tắc

| Thành phần | Phù hợp với |
| --- | --- |
| `CLAUDE.md` | Quy ước và bối cảnh áp dụng thường xuyên |
| Skill | Thủ tục hoặc tài liệu tham khảo cho một loại task |
| Hook | Điều kiện bắt buộc không được để Claude bỏ qua |

**Quy tắc kinh nghiệm của lesson:** nếu đã phải gõ cùng một hướng dẫn nhiều bước lần thứ hai, hãy cân nhắc tạo Skill.

**Nguồn:** [Lesson 3 — Verification skills](https://academy.claude.com/courses/claude-code-in-action/verification-skills).

---

# 5. Lesson 4 -- Permission modes

**Chủ đề:** Chọn permission mode theo công việc và mức tự chủ.  
**Thời lượng trên trang khóa học:** khoảng 5 phút.

## 5.1. Sáu permission modes

Lesson mô tả sáu mode:

| Mode | Hành vi |
| --- | --- |
| **Manual** | Cho phép đọc mà không hỏi; các hành động khác có thể yêu cầu xác nhận |
| **Accept edits** | Cho phép đọc, sửa file và một số lệnh Bash thao tác file phổ biến mà không hỏi |
| **Plan** | Khảo sát, nghiên cứu và đề xuất thay đổi mà không sửa file |
| **Auto** | Cho Claude làm việc không cần hỏi từng bước; classifier riêng xem xét phần lớn hành động trước khi chạy |
| **Don't ask** | Cho phép đọc trong working directories và công cụ đã phê duyệt; hành động cần hỏi sẽ tự bị từ chối |
| **Bypass permissions** | Bỏ qua các bước kiểm tra quyền; chỉ dùng trong container hoặc VM cô lập |

Trong phiên CLI, **Shift+Tab** chuyển qua các mode thường dùng: Manual, Accept edits, Plan và Auto. Status bar hiển thị mode hiện tại. Tên mode và khả năng có thể phụ thuộc phiên bản, sản phẩm và cấu hình.

## 5.2. Auto mode không kiểm tra tính đúng của code

Classifier của Auto mode xem xét **ý định và rủi ro của hành động**, không xác minh code có hoạt động hay không. Nó có thể chặn thao tác nguy hiểm nhưng không phát hiện một refactor tạo ra logic lỗi.

Có thể ghép Auto mode với Stop Hook chạy test sau khi Claude hoàn tất: classifier hỗ trợ kiểm soát hành động, còn test kiểm tra kết quả.

## 5.3. Chọn mode theo tình huống

- Đọc và phân tích: Manual.
- Lập kế hoạch trước khi sửa: Plan.
- Chỉnh sửa trong phạm vi rõ, có người xem lại: Accept edits.
- Chạy tự chủ có classifier: Auto.
- CI hoặc scheduled run không có người duyệt prompt: Don't ask, với allowlist hẹp.
- Bypass: chỉ trong môi trường cách ly, không dùng chỉ để tiết kiệm thao tác xác nhận.

**Nguồn:** [Lesson 4 — Permission modes](https://academy.claude.com/courses/claude-code-in-action/permission-modes).

---

# 6. Lesson 5 -- Hooks

**Chủ đề:** Dùng Hooks để thực thi quy tắc tại các điểm cố định trong vòng đời Claude Code.  
**Thời lượng trên trang khóa học:** khoảng 7 phút.

## 6.1. Hook hoạt động thế nào?

Hook là code chạy tại một sự kiện cụ thể. Khác với hướng dẫn trong `CLAUDE.md`, Hook có thể chặn tool call trước khi xảy ra hoặc giữ Claude tiếp tục làm việc khi điều kiện chưa đạt.

| Event | Vai trò |
| --- | --- |
| PreToolUse | Chạy trước tool call; có thể cho phép, hỏi hoặc chặn |
| PostToolUse | Chạy sau tool call thành công; thích hợp cho format hoặc lint |
| Stop / SubagentStop | Chạy khi Claude hoặc subagent muốn kết thúc |
| PreCompact / PostCompact | Chạy quanh thao tác compaction |
| InstructionsLoaded | Chạy khi `CLAUDE.md` hoặc rule file được nạp |
| SessionStart | Chuẩn bị môi trường lúc bắt đầu phiên |

## 6.2. PreToolUse: quyết định hoặc chỉnh input

Hook PreToolUse có thể trả JSON với trường `permissionDecision`:

- allow: cho phép tool call.
- deny: chặn tool call.
- ask: yêu cầu người dùng quyết định.
- defer: chỉ dùng cho một số lượt chạy non-interactive -p, khi tiến trình gọi sẽ tạm dừng và tiếp tục tool call sau.

Hook cũng có thể trả `updatedInput` để sửa input trước khi tool chạy, chẳng hạn thay secret bằng placeholder. `updatedInput` thay toàn bộ object input; cần giữ lại các trường không thay đổi.

## 6.3. Exit code và lỗi chặn

| Exit code | Ý nghĩa |
| --- | --- |
| 0 | Thành công; một số event có thể đưa stdout dạng text vào context |
| 2 | Lỗi chặn; stderr được gửi lại cho Claude |
| Khác 0 và 2 | Không chặn; lỗi được ghi nhận và Claude tiếp tục |

Điểm dễ nhầm: **exit 1 không chặn**. Dùng exit 2 khi cần giữ Claude lại, ví dụ Stop Hook phát hiện test thất bại. PostToolUse chạy sau hành động nên không thể ngăn tool call vừa hoàn thành.

## 6.4. Giữ context sau compaction

Để bổ sung context sau khi compact, lesson hướng dẫn dùng SessionStart với matcher compact. Không dùng PostCompact cho mục đích này vì output của event đó không được đưa trở lại conversation theo cách cần thiết.

## 6.5. Nguyên tắc viết Hook

- Dùng PreToolUse để kiểm soát trước hành động; dùng PostToolUse cho việc chỉ có thể làm sau hành động.
- Dùng Stop Hook để giữ lượt làm việc mở nếu test hoặc điều kiện bắt buộc chưa đạt.
- Giữ Hook nhanh, thông báo lỗi rõ, tránh tạo vòng lặp.
- Không ghi secret ra log; kiểm tra xử lý đường dẫn có khoảng trắng và ký tự đặc biệt.

**Nguồn:** [Lesson 5 — Hooks](https://academy.claude.com/courses/claude-code-in-action/hooks).

---

# 7. Lesson 6 -- Scheduled tasks and headless

**Chủ đề:** Tự động hóa prompt định kỳ và tích hợp Claude Code vào pipeline riêng.  
**Thời lượng trên trang khóa học:** khoảng 7 phút.

## 7.1. Cloud scheduled tasks

Cloud scheduled tasks (trước đây gọi là routines) gom prompt, repository, connectors và trigger thành một tác vụ chạy trên hạ tầng Anthropic. Không cần tự dựng máy chủ hay scheduler.

Tác vụ không nhất thiết phải chạy theo lịch. Các trigger gồm:

- Cron schedule, chẳng hạn mỗi sáng.
- HTTP POST đến API endpoint.
- GitHub event, chẳng hạn một Pull Request mới.

Có thể tạo từ web tại `claude.ai/code/routines` hoặc trong Claude Code bằng `/schedule`.

### Giới hạn cần biết

- Tính năng đang ở trạng thái **research preview**; hành vi và giới hạn có thể thay đổi.
- Lịch lặp chạy tối đa mỗi giờ.
- Mặc định, tác vụ dùng clone mới của default branch và push lên nhánh claude/, trừ khi prompt chỉ định nhánh khác.
- Với trigger GitHub Pull Request, repository đầu tiên của tác vụ bắt đầu từ head commit của Pull Request.

Nên dùng branch protection hoặc rulesets để kiểm soát nhánh có thể nhận push.

## 7.2. Non-interactive mode (headless)

Tài liệu hiện gọi headless là **non-interactive mode**. Cờ `-p` (tương đương `--print`) chạy Claude Code một lần, không mở giao diện tương tác:

```text
claude -p "summarize the changes in this diff"
```

Theo mặc định, -p nạp cùng loại context như phiên tương tác, gồm hooks, Skills, Plugins, MCP servers và `CLAUDE.md`.

## 7.3. Lấy output có cấu trúc và nối nhiều lượt

Có thể yêu cầu JSON theo schema rồi đọc trường `structured_output` từ JSON response:

```text
claude -p "Extract the exported function names from src/core/style.js" --output-format json --json-schema '{"type":"object","properties":{"functions":{"type":"array","items":{"type":"string"}}},"required":["functions"]}'
```

Đây là ví dụ từ lesson; có thể xử lý output bằng jq hoặc công cụ tương tự.

Để nối nhiều bước, lưu `session_id` từ JSON output và dùng `--resume` để tiếp tục session đó. Cờ `--bare` bỏ qua auto-discovery của hooks, Skills, Plugins, MCP servers và `CLAUDE.md`; lesson nêu cách này cho CI cần môi trường khởi động nhất quán. `--bare` không dùng subscription login, nên cần cấu hình `ANTHROPIC_API_KEY`.

## 7.4. Agent SDK

Khi cần nhúng Claude Code vào ứng dụng TypeScript hoặc Python, Agent SDK cung cấp cách gọi query cùng các tùy chọn như allowedTools, system prompt và permission mode. Đây là hướng tích hợp vào sản phẩm, khác với scheduled task chạy trên hạ tầng Anthropic hoặc CLI chạy từ script.

**Nguồn:** [Lesson 6 — Scheduled tasks and headless](https://academy.claude.com/courses/claude-code-in-action/routines-and-headless).

---

# 8. Lesson 7 -- GitHub Actions and Code Review

**Chủ đề:** Chọn giữa dịch vụ Code Review được quản lý và GitHub Action tùy biến.  
**Thời lượng trên trang khóa học:** khoảng 6 phút.

## 8.1. Hai cách đưa Claude vào Pull Request

| Tiêu chí | Code Review được quản lý | GitHub Action |
| --- | --- | --- |
| Hạ tầng | Anthropic host qua Claude GitHub app | Nhóm cấu hình trong GitHub Actions |
| Mục đích | Review Pull Request và đăng findings | Tác vụ CI tùy chỉnh, chẳng hạn triển khai yêu cầu từ comment |
| Kết quả | Inline comments có mức độ nghiêm trọng và summary | Hành động do workflow/prompt cấu hình |
| Giới hạn | Không approve/block PR và không push fix | Có thể được cấu hình để thay đổi repository |
| Thiết lập | Owner của Claude organization bật trong Claude Code admin settings và chọn repository | Dùng `/install-github-app`, workflow và `anthropics/claude-code-action@v1` |

Code Review có thể chạy khi PR mở, mỗi lần có push, hoặc khi có comment @claude review. Lesson ghi tính năng này dành cho Team và Enterprise.

GitHub Action phù hợp khi công việc vượt ra ngoài review, như thực hiện task từ comment, tạo báo cáo theo lịch hoặc chạy khi có GitHub event.

## 8.2. Tinh chỉnh GitHub Action

Lesson nêu các tùy chọn tiêu biểu:

- `trigger_phrase`: cụm từ kích hoạt, mặc định là `@claude`.
- prompt: prompt chạy tự động theo event mà không cần mention.
- `claude_args`: tham số truyền xuống Claude Code, gồm giới hạn lượt chạy, permission mode và allowed tools.
- Có thể dùng API key, Claude subscription OAuth, workload identity federation hoặc định tuyến qua Bedrock, Vertex và Foundry tùy cấu hình.

Ví dụ workflow trong lesson cấp `id-token: write` để action xác thực bằng Claude GitHub app. Với báo cáo chỉ đọc, chỉ cấp các tool cần thiết.

## 8.3. Bảo mật và giới hạn

- Lưu token trong GitHub Secrets; không đưa token vào prompt hoặc log.
- Cấp workflow permissions tối thiểu.
- Xem xét kỹ workflow nhận nội dung từ Pull Request hoặc fork.
- Tách quyền đọc, ghi comment và thay đổi code khi có thể.
- Code Review, GitHub Action và CI là các lớp hỗ trợ khác nhau; không dùng kết quả review AI làm điều kiện duy nhất để merge.

**Nguồn:** [Lesson 7 — GitHub Actions and Code Review](https://academy.claude.com/courses/claude-code-in-action/github-actions-and-code-review).

---

# 9. Lesson 8 -- Trust it: Verifying unsupervised runs

**Chủ đề:** Kiểm chứng kết quả theo mức độ phiên chạy không được giám sát.  
**Thời lượng trên trang khóa học:** khoảng 5 phút.

## 9.1. Giữ Auto mode khi chạy không giám sát

Lesson khuyên dùng Auto mode thay cho Bypass permissions trong các phiên chạy không có người theo dõi. Classifier vẫn rà phần lớn hành động để phát hiện hành động nguy hiểm, nhưng không đánh giá code có đúng hay không. Vì vậy mức kiểm chứng kết quả vẫn phải dựa vào mức độ phiên chạy được giám sát.

## 9.2. Bắt đầu từ diff, không phải summary

Mức kiểm chứng nên tăng theo mức tự chủ. Với phiên chạy không giám sát, đừng bắt đầu chỉ bằng summary của Claude:

1. Chạy `/code-review` để rà thay đổi và tìm vấn đề.
2. Tự đọc git diff.
3. Đối chiếu file đã thay đổi với kế hoạch; xem cả file nằm ngoài phạm vi dự kiến.

Summary gọn gàng không chứng minh rằng diff cũng gọn gàng.

## 9.3. Biến test thành cổng kiểm soát

Lesson đề xuất dùng Hooks để Claude không thể chỉ nói đã chạy test:

- Stop Hook chạy test và từ chối kết thúc lượt khi test thất bại.
- PostToolUse Hook có thể chạy lint và type check sau mỗi lần chỉnh sửa.
- Khi Hook trả exit 2, lỗi được gửi lại để Claude tiếp tục xử lý.

Với non-interactive run, kiểm tra cả JSON result và exit code.

## 9.4. Xin ý kiến độc lập

Có thể mở session mới hoặc subagent không biết quá trình triển khai, rồi yêu cầu review code đã đổi. Reviewer với context mới có thể phát hiện giả định hoặc lỗi mà phiên thực hiện không nhận ra.

## 9.5. Gợi ý phân tầng kiểm chứng

Bảng sau là gợi ý áp dụng, không phải thang đánh giá chính thức của khóa học:

| Rủi ro | Ví dụ | Kiểm chứng gợi ý |
| --- | --- | --- |
| Thấp | Sửa typo, cập nhật tài liệu | Đọc diff và kiểm tra nội dung |
| Trung bình | Sửa UI hoặc business logic | Build, test liên quan và đọc diff |
| Cao | Auth, payment, migration | Test nhiều lớp, review bảo mật và người duyệt |
| Rất cao | Deploy production, xóa dữ liệu | Phê duyệt, kế hoạch rollback và giám sát |

**Nguồn:** [Lesson 8 — Trust it: Verifying unsupervised runs](https://academy.claude.com/courses/claude-code-in-action/trust-it-verifying-unsupervised-runs).

---

# 10. Lesson 9 -- Plugins

**Chủ đề:** Cài đặt và chia sẻ cấu hình Claude Code bằng Plugin.  
**Thời lượng trên trang khóa học:** khoảng 7 phút.

## 10.1. Plugin đóng gói những gì?

Plugin gom cấu hình thành một đơn vị cài đặt. Lesson nêu các thành phần như Skills, subagents, Hooks, MCP server config, language server protocol servers, background monitors, themes và một phần `settings.json`.

Có thể cài plugin theo tên, ví dụ:

```text
/plugin install github@claude-plugins-official
```

Để chia sẻ trong nhóm, có thể thêm marketplace riêng một lần rồi cài plugin từ nguồn đó.

## 10.2. Đọc plugin trước khi cài

Plugin chạy code trên máy với quyền của người cài. Hooks có thể chạy mỗi khi tool phù hợp được gọi; vì vậy cần xem Hooks, agents và MCP servers của plugin trước khi bật.

- Hooks của plugin và Hooks cá nhân cùng được gọi.
- Skills, agents và commands được namespace theo tên Plugin để tránh trùng.
- Plugin không ghi đè cấu hình cá nhân.
- `settings.json` trong Plugin chỉ hỗ trợ một tập khóa giới hạn; lesson nêu agent và subagent status line keys.

## 10.3. Đóng gói Plugin của nhóm

Có thể giữ cấu trúc gần với .claude hiện tại:

- Một thư mục cho mỗi Skill.
- Một file Markdown cho mỗi subagent trong agents.
- `hooks/hooks.json` và `.mcp.json` ở gốc Plugin.

Manifest `.claude-plugin/plugin.json` là tùy chọn, có thể khai báo name, version, description và author. name là trường bắt buộc theo lesson và dùng namespace cho Skill. Version giúp theo dõi cập nhật giữa các thành viên.

## 10.4. Nguyên tắc thực hành

- Chỉ cài từ marketplace và người phát hành mà nhóm tin cậy.
- Kiểm tra quyền và code được plugin mang theo trước khi bật.
- Đóng gói workflow sau khi nó đã ổn định và có ích cho nhiều repository.
- Viết rõ cách cài đặt, cập nhật và gỡ bỏ.

**Nguồn:** [Lesson 9 — Plugins](https://academy.claude.com/courses/claude-code-in-action/plugins).

---

# 11. Quiz và câu hỏi ôn tập

Trang khóa học liệt kê **1 quiz tổng kết**. Các câu hỏi dưới đây do người biên soạn tổng hợp để tự ôn, không phải trích nguyên văn quiz của Claude Academy.

## 11.1. Câu hỏi tự kiểm tra

1. Vì sao nên dùng Plan mode trước khi giao task refactor lớn?
2. Compaction có rủi ro gì và làm sao định hướng nội dung summary?
3. Khác nhau giữa Restore code, Restore conversation và Restore cả hai?
4. Điều kiện `/goal` cần có đặc điểm gì?
5. Khi nào loop phù hợp hơn một lần chạy prompt?
6. Vì sao nhiều phiên không nên sửa chung một working tree?
7. Những loại quy tắc nào thuộc `CLAUDE.md`, Skill và Hook?
8. Vì sao skill kiểm chứng cần đọc diff chứ không chỉ chạy test?
9. Khác nhau giữa Auto và Don't ask?
10. Vì sao exit 1 không phù hợp để chặn Hook?
11. Scheduled task, non-interactive mode và Agent SDK khác nhau thế nào?
12. Code Review hosted và GitHub Action khác nhau ở đâu?
13. Phiên chạy không giám sát cần bằng chứng gì trước khi báo hoàn thành?
14. Vì sao cần xem nội dung Plugin trước khi cài?

## 11.2. Đáp án gợi ý

1. Để phát hiện giả định sai và thiếu phạm vi trước khi sửa code.
2. Summary có thể bỏ chi tiết; thêm chỉ dẫn sau `/compact` để giữ nội dung cần thiết.
3. Chúng khôi phục các phần khác nhau: code, hội thoại hoặc cả hai.
4. Có tiêu chí hoàn thành rõ và kiểm tra được từ transcript, chẳng hạn kết quả test.
5. Khi cần polling trạng thái bên ngoài như CI hoặc deploy.
6. Vì hai phiên có thể đè lên thay đổi của nhau; worktrees tạo cây file riêng.
7. `CLAUDE.md` cho hướng dẫn nền, Skill cho thủ tục lặp lại, Hook cho điều kiện thực thi bắt buộc.
8. Vì test có thể bị làm yếu để pass; diff giúp kiểm tra test và code thực sự đã đổi gì.
9. Auto dùng classifier để xem xét phần lớn hành động; Don't ask tự từ chối những hành động vốn cần hỏi.
10. Theo lesson, exit code 1 không chặn; dùng 2 để trả lỗi chặn cho Claude.
11. Scheduled task chạy trên hạ tầng Anthropic theo trigger; non-interactive mode gọi CLI từ script/pipeline; Agent SDK nhúng Claude Code vào ứng dụng.
12. Code Review chỉ đăng findings và không approve/block PR hoặc push fix; Action dành cho workflow tùy biến.
13. Diff, kết quả test có log/exit code, đối chiếu yêu cầu và phần chưa kiểm chứng.
14. Plugin chạy code bằng quyền của người cài, gồm cả Hooks và tích hợp đi kèm.

---

# 12. Workflow tổng hợp đề xuất

Đây là workflow gợi ý rút ra từ các chủ đề khóa học, không phải một quy trình bắt buộc của Claude Academy.

## 12.1. Vòng đời task

```text
1. Ghi rõ mục tiêu, phạm vi và acceptance criteria.
       |
2. Dùng Plan mode để khảo sát và lập kế hoạch.
       |
3. Đọc lại kế hoạch, điều chỉnh giả định và rủi ro.
       |
4. Chọn permission mode phù hợp với mức rủi ro.
       |
5. Triển khai theo hướng dẫn CLAUDE.md và Skill liên quan.
       |
6. Dùng Hooks cho các kiểm tra không được bỏ qua.
       |
7. Chạy build, test, lint hoặc type check cần thiết.
       |
8. Đọc diff và đối chiếu acceptance criteria.
       |
9. Báo kết quả, bằng chứng, lỗi và phần chưa kiểm chứng.
```

## 12.2. Vai trò các thành phần

| Thành phần | Vai trò |
| --- | --- |
| `CLAUDE.md` | Bối cảnh và quy ước áp dụng thường xuyên |
| Plan mode | Khảo sát và lập kế hoạch trước khi sửa |
| Skill | Quy trình chuyên biệt có thể tái sử dụng |
| Permission mode | Chọn mức tự chủ và xác nhận hành động |
| Hook | Chạy kiểm tra hoặc cưỡng chế điều kiện tại event |
| Worktree | Cô lập cây làm việc của các phiên song song |
| Goal | Tiếp tục làm đến điều kiện hoàn thành |
| Loop | Lặp prompt/polling theo chu kỳ |
| Scheduled task | Chạy prompt trên hạ tầng cloud theo trigger |
| Non-interactive mode | Tích hợp Claude Code CLI vào script hoặc CI |
| GitHub Action | Tự động hóa workflow theo GitHub events |
| Verification | Xác nhận kết quả bằng bằng chứng |
| Plugin | Phân phối cấu hình trong nhóm |

## 12.3. Mẫu prompt giao task

```text
Task:
[Mô tả tính năng hoặc lỗi]

Context:
[Module liên quan, kiến trúc và giới hạn]

Acceptance criteria:
- [Tiêu chí 1]
- [Tiêu chí 2]
- [Tiêu chí 3]

Workflow:
1. Khảo sát repository, chưa sửa file.
2. Lập kế hoạch và liệt kê file dự kiến thay đổi.
3. Nêu rủi ro, dependency và cách kiểm chứng.
4. Triển khai trong phạm vi đã thống nhất.
5. Chạy build/test liên quan.
6. Đọc diff và xác minh từng acceptance criterion.

Final report:
- Tóm tắt thay đổi
- File đã sửa
- Lệnh đã chạy và kết quả thực tế
- Tiêu chí: pass / fail / chưa kiểm chứng
- Rủi ro hoặc việc còn lại

Không báo một kiểm tra đã pass nếu output không chứng minh điều đó.
```

---

# 13. Checklist triển khai vào dự án

Checklist này là gợi ý áp dụng, không phải checklist chính thức của khóa học.

## Giai đoạn A — Nền tảng

- [ ] Rà soát `CLAUDE.md`; giữ quy tắc cụ thể và thường dùng.
- [ ] Ghi đúng lệnh build, test và lint của repository.
- [ ] Thử Plan mode với task có nhiều file.
- [ ] Thực hành `/compact` có chỉ dẫn.
- [ ] Thử rewind trên thay đổi có thể bỏ.

## Giai đoạn B — Chuẩn hóa

- [ ] Tạo Skill cho quy trình verification lặp lại.
- [ ] Đảm bảo Skill kiểm tra cả test lẫn diff.
- [ ] Chọn permission mode theo mức rủi ro và bối cảnh chạy.
- [ ] Thêm Hook cho điều kiện bắt buộc.
- [ ] Kiểm tra Hook không tạo vòng lặp hoặc lộ secret.

## Giai đoạn C — Tự động hóa

- [ ] Thử cloud scheduled task với đầu ra ít rủi ro.
- [ ] Thử non-interactive mode trong script hoặc CI.
- [ ] Giới hạn allowed tools và workflow permissions.
- [ ] Lưu token trong secret store; không đưa vào prompt hoặc log.
- [ ] Phân biệt Code Review hosted với GitHub Action tùy chỉnh.

## Giai đoạn D — Kiểm chứng và chia sẻ

- [ ] Đọc diff, kể cả khi summary có vẻ đầy đủ.
- [ ] Dùng Hook để gate test quan trọng.
- [ ] Báo rõ pass, fail, not run và chưa kiểm chứng.
- [ ] Xin review độc lập cho thay đổi quan trọng.
- [ ] Chỉ chia sẻ Plugin sau khi kiểm tra thành phần và quyền.
- [ ] Viết hướng dẫn cài đặt, cập nhật và gỡ Plugin.

---

# 14. Thuật ngữ

| Thuật ngữ | Giải thích |
| --- | --- |
| Agent | Hệ thống AI có thể thực hiện chuỗi hành động để đạt mục tiêu |
| Context window | Dung lượng context mô hình có thể dùng trong phiên |
| Compaction | Tóm tắt hội thoại để tiếp tục với context mới |
| Rewind | Quay lại checkpoint trước đó |
| Plan mode | Khảo sát và lập kế hoạch trước khi chỉnh sửa |
| Goal | Điều kiện hoàn thành để Claude tiếp tục theo đuổi |
| Loop | Prompt được chạy lặp theo khoảng thời gian |
| Worktree | Cây làm việc Git độc lập |
| `CLAUDE.md` | File hướng dẫn và bối cảnh cho Claude Code |
| Skill | Thư mục hướng dẫn một quy trình có thể tái sử dụng |
| Permission mode | Chế độ quyết định hành động nào cần xác nhận |
| Hook | Code chạy tại một event của Claude Code |
| Non-interactive mode | Chạy Claude Code CLI không có giao diện hội thoại |
| Scheduled task | Tác vụ Claude Code chạy trên hạ tầng Anthropic theo trigger |
| Code Review | Dịch vụ hosted đăng nhận xét lên Pull Request |
| GitHub Action | Tích hợp Claude Code vào workflow GitHub tùy chỉnh |
| Plugin | Gói cài đặt gồm cấu hình và thành phần mở rộng |
| Verification | Kiểm chứng kết quả bằng output và bằng chứng |

---

# 15. Nguồn tham khảo

Nguồn nội dung chính là các trang chính thức của Anthropic / Claude Academy:

- [Claude Code in Action — trang khóa học](https://academy.claude.com/courses/claude-code-in-action)
- [Lesson 1 — Steering long sessions](https://academy.claude.com/courses/claude-code-in-action/steering-long-sessions)
- [Lesson 2 — A `CLAUDE.md` that follows](https://academy.claude.com/courses/claude-code-in-action/a-claude-md-that-follows)
- [Lesson 3 — Verification skills](https://academy.claude.com/courses/claude-code-in-action/verification-skills)
- [Lesson 4 — Permission modes](https://academy.claude.com/courses/claude-code-in-action/permission-modes)
- [Lesson 5 — Hooks](https://academy.claude.com/courses/claude-code-in-action/hooks)
- [Lesson 6 — Scheduled tasks and headless](https://academy.claude.com/courses/claude-code-in-action/routines-and-headless)
- [Lesson 7 — GitHub Actions and Code Review](https://academy.claude.com/courses/claude-code-in-action/github-actions-and-code-review)
- [Lesson 8 — Trust it: Verifying unsupervised runs](https://academy.claude.com/courses/claude-code-in-action/trust-it-verifying-unsupervised-runs)
- [Lesson 9 — Plugins](https://academy.claude.com/courses/claude-code-in-action/plugins)
- [Claude Code Docs](https://code.claude.com/docs/en/)

---

# 16. Kết luận

Thông điệp cốt lõi của *Claude Code in Action* là xây dựng workflow có thể điều khiển và kiểm chứng, thay vì chỉ dựa vào một prompt tốt.

Một workflow đáng tin cậy cần phạm vi và tiêu chí hoàn thành rõ, hướng dẫn gọn, quyền phù hợp, Hooks cho điều kiện bắt buộc, kiểm tra độc lập và báo cáo trung thực về bằng chứng cũng như phần chưa kiểm chứng.

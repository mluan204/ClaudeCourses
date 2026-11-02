# Introduction to Agent Skills

## Tài liệu học tập -- Claude Academy

> **Khóa học:** Introduction to agent skills\
> **Nền tảng:** Claude Code\
> **Đơn vị:** Anthropic / Claude Academy\
> **Thời lượng khóa học:** khoảng 1 giờ\
> **Số bài học:** 6\
> **Mục tiêu:** Hiểu cách Claude Code phát hiện, kích hoạt, cấu hình, tổ chức, chia sẻ và khắc phục sự cố Agent Skills; có thể tự xây dựng skill dùng được trong dự án thực tế.
------------------------------------------------------------------------

## Mục lục

1.  [Tổng quan: Agent Skills là gì?](#1-tổng-quan-agent-skills-là-gì)
2.  [Bài 1 --- What are skills?](#2-bài-1--what-are-skills)
3.  [Bài 2 --- Creating your first
    skill](#3-bài-2--creating-your-first-skill)
4.  [Bài 3 --- Configuration and multi-file
    skills](#4-bài-3--configuration-and-multi-file-skills)
5.  [Bài 4 --- Skills vs. other Claude Code
    features](#5-bài-4--skills-vs-other-claude-code-features)
6.  [Bài 5 --- Sharing skills](#6-bài-5--sharing-skills)
7.  [Bài 6 --- Troubleshooting skills](#7-bài-6--troubleshooting-skills)
8.  [Quy trình xây dựng skill từ đầu đến
    cuối](#8-quy-trình-xây-dựng-skill-từ-đầu-đến-cuối)
9.  [Checklist và bài tập thực hành](#9-checklist-và-bài-tập-thực-hành)
10. [Bảng thuật ngữ](#10-bảng-thuật-ngữ)

------------------------------------------------------------------------

# 1. Tổng quan: Agent Skills là gì?

## 1.1. Khái niệm

**Agent Skill** là một gói hướng dẫn và tài nguyên có cấu trúc, giúp AI
agent thực hiện một nhóm công việc chuyên biệt theo cách nhất quán.
Trong Claude Code, mỗi skill thường là một thư mục chứa file `SKILL.md`
cùng các tài liệu tham khảo, script hoặc asset bổ trợ.

Thay vì phải lặp lại cùng một chỉ dẫn trong mỗi prompt --- ví dụ quy
chuẩn review Pull Request (PR), cách viết commit message, quy trình kiểm
thử hay format tài liệu --- ta đóng gói chỉ dẫn đó thành skill. Claude
Code có thể nhận diện yêu cầu phù hợp và tự động nạp skill khi cần.

Ví dụ các skill có thể xây dựng:

-   `pr-review`: review code theo checklist chất lượng và bảo mật.
-   `commit-message`: tạo commit message theo Conventional Commits.
-   `documentation`: viết tài liệu theo cấu trúc và quy chuẩn của dự án.
-   `debugging`: hướng dẫn quy trình điều tra lỗi theo framework.
-   `frontend-design`: áp dụng design system, typography và nguyên tắc
    responsive.

## 1.2. Skill hoạt động như thế nào?

Có thể hình dung cơ chế theo ba giai đoạn:

1.  **Khám phá (discovery):** Claude Code quét các vị trí skill được hỗ
    trợ.
2.  **Nhận diện (matching):** Ban đầu Claude chỉ cần thông tin `name` và
    `description`; khi người dùng gửi yêu cầu, Claude so khớp ý nghĩa
    yêu cầu với mô tả của các skill.
3.  **Kích hoạt và nạp nội dung (activation/loading):** Khi có skill phù
    hợp, Claude đọc toàn bộ `SKILL.md` và làm theo hướng dẫn. Những tài
    liệu phụ chỉ được đọc khi skill chỉ dẫn hoặc tác vụ cần đến.

Điểm quan trọng: skill không nhất thiết phải được gọi bằng lệnh slash.
Cơ chế chính là kích hoạt theo ngữ nghĩa yêu cầu (semantic matching), vì
vậy chất lượng `description` ảnh hưởng trực tiếp đến khả năng được sử
dụng.

## 1.3. Skill nằm ở đâu?

Hai vị trí phổ biến:

  -----------------------------------------------------------------------
  Phạm vi                 Đường dẫn               Mục đích
  ----------------------- ----------------------- -----------------------
  Cá nhân (Personal)      `~/.claude/skills/`     Dùng lại trong nhiều dự
                                                  án của một người

  Dự án (Project)         `.claude/skills/` trong Chia sẻ quy trình và
                          repository              tiêu chuẩn với cả nhóm
                                                  qua Git
  -----------------------------------------------------------------------

Trên Windows, thư mục skill cá nhân thường ở:

``` text
C:\Users\<your-user>\.claude\skills\
```

Ví dụ cấu trúc:

``` text
my-project/
├── .claude/
│   └── skills/
│       ├── pr-review/
│       │   └── SKILL.md
│       └── documentation/
│           └── SKILL.md
└── src/
```

Skill cá nhân phù hợp với phong cách làm việc riêng; skill dự án phù hợp
với coding convention, kiến trúc, quy trình và tiêu chuẩn chung của
repository.

## 1.4. Khi nào nên tạo skill?

Nên cân nhắc tạo skill khi:

-   Thường xuyên phải giải thích lại cùng một quy trình.
-   Có checklist hoặc tiêu chuẩn chuyên môn cần áp dụng lặp lại.
-   Muốn nhiều dự án hoặc thành viên thực hiện một tác vụ theo cùng
    format.
-   Có kiến thức chỉ cần dùng trong một số loại tác vụ, không cần nạp
    vào mọi cuộc hội thoại.

Nguyên tắc ghi nhớ: **nếu liên tục phải nhắc Claude cùng một điều, đó có
thể là một skill nên được tạo.**

------------------------------------------------------------------------

# 2. Bài 1 --- What are skills?

**Mục tiêu bài học**

-   Định nghĩa được Claude Code skills và cách hoạt động.
-   Biết vị trí lưu skill cá nhân và skill dự án.
-   Phân biệt skill với `CLAUDE.md` và slash commands.
-   Xác định tình huống phù hợp để dùng skill.

## 2.1. Cấu trúc cơ bản của một skill

Một skill tối thiểu có thư mục riêng và file `SKILL.md`. File này gồm
hai phần:

1.  **YAML frontmatter:** metadata để Claude nhận diện skill, quan trọng
    nhất là `description`.
2.  **Phần nội dung Markdown:** các bước thực hiện, quy tắc, checklist,
    ví dụ và tiêu chí đầu ra.

Ví dụ:

``` markdown
---
name: pr-review
description: Reviews pull requests for code quality and security. Use when reviewing PRs or checking code changes.
---

When reviewing a pull request:

1. Inspect the complete diff.
2. Check correctness, edge cases, and security.
3. Prioritize actionable findings by severity.
4. Include file paths and line references.
5. Do not report speculative issues as confirmed bugs.
```

`name` là định danh; `description` giải thích skill làm gì và dùng trong
trường hợp nào; phần Markdown là chỉ dẫn thực thi.

## 2.2. Skill khác gì với chỉ dẫn thông thường?

Nếu đưa toàn bộ hướng dẫn vào prompt, người dùng phải lặp lại chúng mỗi
lần. Nếu đặt mọi thứ trong `CLAUDE.md`, nội dung sẽ được nạp thường
xuyên dù tác vụ hiện tại có liên quan hay không.

Skill giải quyết vấn đề này bằng cách chỉ nạp kiến thức chuyên biệt khi
cần. Nhờ đó, context window ít bị chiếm bởi những hướng dẫn không liên
quan.

## 2.3. Skill và slash command

-   **Slash command:** người dùng chủ động gọi bằng cú pháp lệnh.
-   **Skill:** Claude có thể tự nhận diện và kích hoạt khi yêu cầu phù
    hợp.

Skill có thể làm quy trình chuyên biệt trở nên tự động hơn, trong khi
slash command hữu ích khi người dùng muốn kiểm soát chính xác thời điểm
chạy một thao tác.

## 2.4. Ghi nhớ

-   Skill là thư mục chứa hướng dẫn và tài nguyên, không chỉ là một đoạn
    prompt rời.
-   `SKILL.md` là file trung tâm.
-   `description` giúp Claude quyết định khi nào dùng skill.
-   Personal skill dùng xuyên dự án; project skill được chia sẻ cùng
    repository.
-   Skill được nạp theo nhu cầu, giúp tiết kiệm context.

**Câu hỏi tự ôn tập**

1.  Nếu muốn Claude luôn tuân thủ TypeScript strict mode trong một
    repository, nên đặt ở đâu?
2.  Nếu chỉ muốn checklist review PR được nạp khi review PR, nên dùng
    gì?
3.  Vì sao mô tả skill cần nói rõ tình huống kích hoạt?

------------------------------------------------------------------------

# 3. Bài 2 --- Creating your first skill

**Mục tiêu bài học**

-   Tạo skill từ đầu với frontmatter hợp lệ.
-   Kiểm tra skill có được Claude Code nhận diện hay không.
-   Hiểu cơ chế matching và thứ tự ưu tiên khi trùng tên.

## 3.1. Tạo skill đầu tiên

Ví dụ tạo personal skill chuyên viết PR description:

``` bash
mkdir -p ~/.claude/skills/pr-description
```

Tạo file:

``` text
~/.claude/skills/pr-description/SKILL.md
```

Nội dung:

``` markdown
---
name: pr-description
description: Writes pull request descriptions. Use when creating a PR, writing a PR, or summarizing changes for a pull request.
---

When writing a PR description:

1. Run `git diff main...HEAD` to inspect all changes on this branch.
2. Summarize the purpose of the change.
3. Explain why the change is needed.
4. Group related implementation changes.
5. Mention deleted or renamed files when relevant.

Use this format:

## What
One sentence explaining what this PR does.

## Why
Brief context explaining why the change is needed.

## Changes
- Specific implementation changes
- Related changes grouped together
- Important files deleted or renamed
```

Giải thích:

-   Tên thư mục nên khớp với tên skill để dễ quản lý.
-   `name` xác định skill.
-   `description` là tiêu chí để Claude tìm và kích hoạt skill.
-   Nội dung phía dưới frontmatter quy định cách thực hiện công việc.

## 3.2. Kiểm tra skill

Claude Code theo dõi các thư mục skill. Nếu thư mục `~/.claude/skills`
đã tồn tại khi phiên làm việc bắt đầu, skill mới thường được phát hiện
mà không cần khởi động lại.

Nếu vừa tạo thư mục skill cá nhân trong phiên hiện tại mà Claude chưa
nhận diện, chạy:

``` text
/reload-skills
```

Sau đó có thể yêu cầu Claude liệt kê các skill hiện có hoặc thử một tác
vụ đúng với mô tả, ví dụ:

``` text
Write a PR description for my changes.
```

Khi skill được kích hoạt, Claude sẽ thể hiện việc nạp skill trong
terminal và áp dụng format đã định nghĩa.

## 3.3. Cơ chế matching

Claude Code không cần nạp toàn bộ nội dung mọi skill ngay từ đầu. Nó chỉ
đọc metadata cần thiết để nhận diện. Khi có yêu cầu:

-   Claude phân tích ý định của người dùng.
-   So sánh ý nghĩa yêu cầu với các `description`.
-   Nếu phù hợp, đọc toàn bộ `SKILL.md`.
-   Áp dụng các chỉ dẫn của skill trong tác vụ hiện tại.

Đây là semantic matching, không đơn thuần là so khớp chính xác từng từ.
Ví dụ mô tả "explain code with visual diagrams" có thể phù hợp với yêu
cầu "explain what this function does".

Một skill đơn giản chỉ gồm tên, mô tả và hướng dẫn có thể được nạp tự
động. Skill có cấu hình cho phép trước công cụ hoặc hook có thể phát
sinh yêu cầu phê duyệt tùy cấu hình quyền và phiên bản.

## 3.4. Thứ tự ưu tiên khi trùng tên

Theo bài học, thứ tự ưu tiên giữa các skill cùng tên là:

1.  **Enterprise:** skill được quản lý ở cấp tổ chức.
2.  **Personal:** skill trong thư mục cá nhân.
3.  **Project:** skill trong repository.

Plugin skills nằm ngoài thứ tự này và được namespace, ví dụ:

``` text
/plugin-name:skill-name
```

Do đó, plugin skill có thể cùng tồn tại với skill khác tên giống nhau.

**Khuyến nghị:** đặt tên đủ cụ thể như `frontend-review`,
`backend-review`, `api-documentation` thay vì tên quá chung như `review`
hoặc `docs`.

## 3.5. Cập nhật và xóa skill

-   Cập nhật: sửa file `SKILL.md`.
-   Xóa: xóa thư mục skill tương ứng.
-   Claude Code thường nhận thay đổi trong phiên hiện tại.
-   Nếu trước đó phải chạy `/reload-skills` vì thư mục cá nhân mới được
    tạo trong phiên, tiếp tục chạy lại lệnh sau mỗi lần thay đổi.

------------------------------------------------------------------------

# 4. Bài 3 --- Configuration and multi-file skills

**Mục tiêu bài học**

-   Hiểu các metadata nâng cao.
-   Viết `description` có khả năng kích hoạt chính xác.
-   Dùng `allowed-tools` để cấp quyền trước cho công cụ cần thiết.
-   Tổ chức skill lớn theo progressive disclosure và nhiều file.

## 4.1. Các trường frontmatter quan trọng

Trong Claude Code, các trường frontmatter đều có thể tùy chọn;
`description` là trường được khuyến nghị và có vai trò quan trọng nhất.

  -----------------------------------------------------------------------
  Trường                              Vai trò
  ----------------------------------- -----------------------------------
  `name`                              Tên định danh của skill; nếu bỏ
                                      trống có thể lấy từ tên thư mục

  `description`                       Mô tả chức năng và tình huống nên
                                      kích hoạt

  `allowed-tools`                     Danh sách công cụ được phê duyệt
                                      trước khi skill hoạt động

  `model`                             Chỉ định model dùng cho skill trong
                                      Claude Code
  -----------------------------------------------------------------------

Một số lưu ý về giới hạn được bài học nêu:

-   Khi đưa skill lên Claude.ai hoặc Claude API, `name` là bắt buộc, tối
    đa 64 ký tự, dùng chữ thường, số và dấu gạch nối.
-   `description` khi dùng trên Claude.ai hoặc Claude API là bắt buộc,
    tối đa 1.024 ký tự.
-   Trong Claude Code, `name` có thể mặc định theo tên thư mục;
    `description` vẫn là metadata quan trọng nhất.

Ví dụ:

``` yaml
---
name: codebase-onboarding
description: Helps developers understand repository architecture, key modules, and development workflows. Use when exploring an unfamiliar codebase or onboarding a new developer.
allowed-tools: Read, Grep, Glob, Bash
model: sonnet
---
```

## 4.2. Viết description hiệu quả

Một description tốt phải trả lời hai câu hỏi:

1.  Skill làm nhiệm vụ gì?
2.  Claude nên dùng skill trong tình huống nào?

Mô tả quá chung chung như:

``` yaml
description: Helps with code.
```

không cung cấp đủ ngữ cảnh để phân biệt với hàng loạt skill khác.

Mô tả tốt hơn:

``` yaml
description: Analyzes frontend performance, identifies rendering bottlenecks, and recommends optimizations. Use when the user asks why a page is slow, requests performance profiling, or wants to improve loading and scrolling performance.
```

Các nguyên tắc:

-   Nêu rõ lĩnh vực và kết quả đầu ra.
-   Ghi những tình huống thực tế người dùng thường yêu cầu.
-   Sử dụng từ ngữ gần với cách người dùng diễn đạt.
-   Tránh mô tả mơ hồ hoặc trùng lặp với skill khác.
-   Nếu skill không kích hoạt, bổ sung các cụm từ kích hoạt thực tế thay
    vì chỉ viết lại phần hướng dẫn dài bên dưới.

## 4.3. `allowed-tools` --- Phê duyệt trước công cụ

`allowed-tools` cho phép skill sử dụng các công cụ được liệt kê mà không
cần hỏi lại trong phạm vi quyền của lượt tương ứng.

Ví dụ:

``` yaml
---
name: codebase-onboarding
description: Explains repository architecture and helps developers explore unfamiliar codebases.
allowed-tools: Read, Grep, Glob, Bash
---
```

Ý nghĩa:

-   `Read`: đọc file.
-   `Grep`: tìm nội dung bằng pattern.
-   `Glob`: tìm file theo pattern.
-   `Bash`: chạy lệnh shell.

**Điểm cần phân biệt:** `allowed-tools` là cơ chế phê duyệt trước, không
phải danh sách giới hạn duy nhất các công cụ skill được phép dùng.
Claude không tự động mất quyền truy cập những công cụ khác chỉ vì chúng
không nằm trong danh sách này.

Nếu không khai báo `allowed-tools`, Claude dùng cơ chế cấp quyền thông
thường.

Có thể dùng `disallowed-tools` để loại bỏ những công cụ cụ thể khỏi tập
công cụ Claude có thể sử dụng khi skill hoạt động.

### Lưu ý bảo mật

Cẩn trọng khi thêm `Bash` không giới hạn, vì điều này có thể phê duyệt
các lệnh shell nói chung. Nếu chỉ cần một nhóm lệnh, nên giới hạn theo
pattern, ví dụ:

``` yaml
allowed-tools: Read, Grep, Glob, Bash(git status *)
```

Chỉ cấp quyền tối thiểu cần thiết và tránh tự động phê duyệt thao tác có
thể xóa dữ liệu, thay đổi hệ thống hoặc truy cập thông tin nhạy cảm nếu
không thực sự cần.

## 4.4. Progressive Disclosure --- Nạp kiến thức theo nhu cầu

Khi skill được kích hoạt, nội dung `SKILL.md` được đưa vào context
window. Nếu nhồi toàn bộ tài liệu, ví dụ, quy trình và script vào một
file rất dài thì:

-   Tốn context window.
-   Khó bảo trì và cập nhật.
-   Claude phải tiếp nhận nhiều thông tin không liên quan đến tác vụ
    hiện tại.

**Progressive disclosure** là cách chia nội dung thành nhiều cấp: giữ
hướng dẫn cốt lõi trong `SKILL.md`, còn chi tiết chuyên sâu được đặt ở
file riêng và chỉ đọc khi cần.

Cấu trúc gợi ý:

``` text
frontend-performance/
├── SKILL.md
├── references/
│   ├── performance-checklist.md
│   ├── react-optimization.md
│   └── web-vitals.md
├── scripts/
│   ├── analyze-bundle.sh
│   └── check-assets.py
└── assets/
    └── report-template.md
```

Vai trò từng thư mục:

-   `references/`: tài liệu, checklist, quy chuẩn, hướng dẫn chuyên sâu.
-   `scripts/`: mã thực thi cho tác vụ có thể tự động hóa.
-   `assets/`: hình ảnh, template hoặc dữ liệu hỗ trợ.

Trong `SKILL.md`, cần ghi rõ khi nào Claude nên đọc từng file:

``` markdown
## Performance investigation

1. Start with the general checklist in `references/performance-checklist.md`.
2. If the project uses React, read `references/react-optimization.md`.
3. If the task involves Core Web Vitals, consult `references/web-vitals.md`.
4. Run `scripts/analyze-bundle.sh` when bundle analysis is required.
```

Quy tắc thực hành của bài học: cố gắng giữ `SKILL.md` dưới **500 dòng**.
Nếu dài hơn, hãy đánh giá xem có thể tách nội dung thành file tham khảo
hay không.

## 4.5. Dùng script hiệu quả

Claude có thể thực thi script trong thư mục skill mà không cần nạp toàn
bộ source code của script vào context; phần output mới là nội dung tiêu
thụ token.

Điều này đặc biệt hữu ích cho:

-   Kiểm tra môi trường phát triển.
-   Biến đổi dữ liệu theo quy tắc cố định.
-   Chạy kiểm tra tự động.
-   Những tác vụ đáng tin cậy hơn khi dùng code đã kiểm thử thay vì yêu
    cầu model tự sinh lệnh mỗi lần.

Ví dụ chỉ dẫn trong `SKILL.md`:

``` markdown
Before generating the report:

1. Run `scripts/check-environment.sh`.
2. Review the command output.
3. If the script reports missing dependencies, explain what is missing.
4. Do not modify the environment without user approval.
```

Nên thiết kế script có output rõ ràng, xử lý lỗi tốt và tránh side
effect ngoài dự kiến.

------------------------------------------------------------------------

# 5. Bài 4 --- Skills vs. other Claude Code features

**Mục tiêu bài học**

Phân biệt Skills với `CLAUDE.md`, subagents, hooks và MCP servers; biết
cách kết hợp chúng thay vì dùng skill cho mọi vấn đề.

## 5.1. Bảng so sánh

  -----------------------------------------------------------------------
  Thành phần              Cách hoạt động          Dùng khi
  ----------------------- ----------------------- -----------------------
  `CLAUDE.md`             Nạp chỉ dẫn vào các     Quy tắc luôn áp dụng
                          cuộc hội thoại          cho dự án

  Skills                  Nạp theo yêu cầu phù    Kiến thức/quy trình
                          hợp                     chuyên biệt theo tác vụ

  Subagents               Chạy trong context      Muốn phân công công
                          riêng, nhận nhiệm vụ    việc độc lập
                          được giao               

  Hooks                   Kích hoạt theo sự kiện  Muốn tự động chạy khi
                                                  có event cụ thể

  MCP servers             Cung cấp công cụ và     Cần kết nối hệ thống,
                          tích hợp bên ngoài      API hoặc nguồn dữ liệu
  -----------------------------------------------------------------------

## 5.2. `CLAUDE.md` và Skills

**CLAUDE.md** phù hợp với:

-   Quy tắc coding luôn phải tuân thủ.
-   Quy định kiến trúc và cấu trúc dự án.
-   Ràng buộc như "không tự ý thay đổi database schema".
-   Framework, coding style và lệnh kiểm thử mặc định.

**Skills** phù hợp với:

-   Checklist review PR.
-   Quy trình tạo release note.
-   Hướng dẫn điều tra một loại lỗi.
-   Quy chuẩn viết tài liệu cho một tác vụ cụ thể.
-   Kiến thức chỉ cần dùng trong một số tình huống.

Ví dụ:

-   "Luôn dùng TypeScript strict mode" → `CLAUDE.md`.
-   "Khi review PR, kiểm tra bảo mật, edge cases và test coverage" →
    skill `pr-review`.

## 5.3. Skills và Subagents

**Skill** bổ sung kiến thức và hướng dẫn vào context hiện tại. Nó không
tự tạo ra một môi trường thực thi độc lập.

**Subagent** nhận một nhiệm vụ riêng và thực hiện trong context tách
biệt, sau đó trả kết quả về agent chính.

Dùng subagent khi:

-   Muốn giao một phần công việc cho agent khác.
-   Cần tách biệt context để tránh làm nhiễu nhiệm vụ chính.
-   Cần cấu hình bộ công cụ riêng.
-   Muốn thực hiện các nhánh công việc độc lập.

Dùng skill khi:

-   Muốn agent hiện tại áp dụng chuyên môn hoặc quy trình cụ thể.
-   Kiến thức cần thiết xuyên suốt tác vụ.
-   Không cần tạo một luồng thực thi độc lập.

Hai thành phần có thể kết hợp: subagent thực hiện nhiệm vụ review
frontend và được cấu hình để sử dụng skill accessibility hoặc
performance.

## 5.4. Skills và Hooks

**Hooks** là cơ chế hướng sự kiện (event-driven). Chúng chạy khi một sự
kiện cụ thể xảy ra, ví dụ:

-   Sau khi file được lưu.
-   Trước hoặc sau một lần gọi công cụ.
-   Khi cần kiểm tra điều kiện trước một thao tác.

**Skills** là cơ chế hướng yêu cầu (request-driven): được kích hoạt dựa
trên điều người dùng đang yêu cầu.

Ví dụ:

-   "Mỗi lần lưu file, tự động chạy formatter" → hook.
-   "Khi tôi yêu cầu review code, hãy áp dụng checklist bảo mật" →
    skill.

## 5.5. Skills và MCP servers

MCP server cung cấp công cụ hoặc tích hợp để agent tương tác với hệ
thống bên ngoài, chẳng hạn dịch vụ, API hoặc nguồn dữ liệu.

Skill cung cấp kiến thức về cách xử lý một nhiệm vụ; MCP cung cấp khả
năng/công cụ để thực hiện hành động hoặc truy xuất dữ liệu.

Ví dụ:

-   Skill hướng dẫn cách kiểm tra một ticket theo quy trình nội bộ.
-   MCP server cung cấp công cụ đọc ticket từ hệ thống quản lý công
    việc.

## 5.6. Thiết kế hệ thống kết hợp

Một cấu hình Claude Code thực tế có thể dùng đồng thời:

``` text
Project
├── CLAUDE.md                 # Quy tắc chung luôn áp dụng
├── .claude/
│   ├── skills/
│   │   ├── pr-review/        # Quy trình review khi được yêu cầu
│   │   └── release-notes/    # Quy trình viết release notes
│   ├── agents/
│   │   └── frontend-reviewer.md
│   ├── settings.json         # Cấu hình dự án
│   └── hooks/                # Tự động hóa theo sự kiện
└── src/
```

Nguyên tắc thiết kế: mỗi tính năng đảm nhiệm đúng vai trò. Không nên
biến skill thành nơi chứa mọi quy tắc, mọi công cụ và mọi luồng tự động
hóa.

------------------------------------------------------------------------

# 6. Bài 5 --- Sharing skills

**Mục tiêu bài học**

-   Chia sẻ skill qua Git repository.
-   Phân phối skill bằng plugin và marketplace.
-   Triển khai skill ở cấp tổ chức.
-   Cấu hình custom subagent sử dụng skill.

## 6.1. Chia sẻ qua Git repository

Đây là cách đơn giản nhất cho nhóm phát triển.

Đặt skill trong:

``` text
.claude/skills/
```

Sau đó commit cùng repository:

``` bash
git add .claude/skills
git commit -m "Add project Claude Code skills"
git push
```

Khi thành viên khác clone repository, họ có thể sử dụng các skill đi kèm
mà không cần cài riêng từng skill.

Phù hợp với:

-   Coding standards của nhóm.
-   Quy trình triển khai riêng cho dự án.
-   Hướng dẫn gắn với cấu trúc codebase.
-   Checklist kiểm thử và review dùng chung.

Ưu điểm: version control, dễ review thay đổi, đồng bộ theo quy trình Git
quen thuộc.

## 6.2. Phân phối qua Plugins và Marketplaces

Plugin giúp đóng gói tính năng để sử dụng trên nhiều repository hoặc
chia sẻ rộng hơn.

Một plugin có thể chứa thư mục `skills/`; mỗi skill vẫn có cấu trúc thư
mục riêng và file `SKILL.md`.

Ví dụ:

``` text
my-plugin/
├── .claude-plugin/
│   └── plugin.json
└── skills/
    ├── api-review/
    │   └── SKILL.md
    └── documentation/
        └── SKILL.md
```

Sau khi đưa plugin lên marketplace, người dùng khác có thể tìm và cài
đặt plugin vào Claude Code.

Phù hợp khi skill:

-   Không phụ thuộc quá sâu vào một repository.
-   Có thể tái sử dụng ở nhiều dự án.
-   Có giá trị với cộng đồng hoặc nhiều nhóm khác nhau.

## 6.3. Triển khai cấp Enterprise bằng managed settings

Tổ chức có thể triển khai skill cho toàn bộ nhân viên thông qua managed
settings.

Ví dụ đường dẫn trên Linux được bài học nêu:

``` text
/etc/claude-code/.claude/skills/
```

Enterprise skills có mức ưu tiên cao nhất, có thể ghi đè personal và
project skills cùng tên.

Phương án này phù hợp với:

-   Tiêu chuẩn bảo mật bắt buộc.
-   Quy trình compliance.
-   Chính sách phát triển phần mềm của tổ chức.
-   Quy tắc mà mọi thành viên phải tuân thủ.

Managed settings cũng có thể kiểm soát plugin marketplace được phép sử
dụng, chẳng hạn thông qua `strictKnownMarketplaces`.

## 6.4. Skills và Subagents: điểm dễ nhầm

Một subagent không tự động nhìn thấy các skill đã được gọi trong context
của agent chính. Subagent thường bắt đầu với context riêng.

Tuy nhiên, nếu subagent có công cụ `Skill`, nó có thể tự tìm và gọi
skill phù hợp.

Có một số điểm cần nhớ:

-   Built-in agents như Explore và Plan không preload skills.
-   Custom subagents có thể khai báo skill cần nạp trong trường
    `skills`.
-   Nếu cấu hình `tools` cho subagent, cần giữ `Skill` trong danh sách
    nếu muốn subagent có thể gọi các skill khác.
-   Skill được preload vào subagent sẽ được nạp đầy đủ khi subagent khởi
    chạy, thay vì chờ matching theo nhu cầu như trong hội thoại chính.

Ví dụ file custom subagent:

``` markdown
---
name: frontend-security-accessibility-reviewer
description: Reviews frontend code for accessibility and security issues.
tools: Bash, Glob, Grep, Read, Skill
model: sonnet
skills: accessibility-audit, performance-check
---

Review the assigned frontend code.

1. Check accessibility issues.
2. Review security-sensitive UI behavior.
3. Evaluate performance concerns.
4. Return actionable findings with file references.
```

Trước khi cấu hình, cần bảo đảm các skill được khai báo đã tồn tại trong
`.claude/skills`.

Nếu thư mục `.claude/agents` mới được tạo sau khi phiên Claude Code bắt
đầu, có thể cần khởi động lại Claude Code để nạp custom subagent mới.

## 6.5. Chọn phương thức chia sẻ nào?

  -----------------------------------------------------------------------
  Nhu cầu                             Phương thức phù hợp
  ----------------------------------- -----------------------------------
  Dùng trong một dự án/nhóm           Commit vào `.claude/skills`

  Dùng ở nhiều repository             Plugin và marketplace

  Bắt buộc áp dụng trong toàn tổ chức Enterprise managed settings

  Giao nhiệm vụ cho agent chuyên biệt Custom subagent khai báo `skills`
  ------------------------------------------------------------------------

# 7. Bài 6 --- Troubleshooting skills

**Mục tiêu bài học**

-   Kiểm tra cấu trúc skill bằng validator.
-   Sửa lỗi skill không kích hoạt hoặc không được nạp.
-   Xử lý trường hợp skill khác được chọn.
-   Khắc phục xung đột ưu tiên và lỗi runtime.

## 7.1. Bắt đầu bằng Skills Validator

Bài học hướng dẫn dùng lệnh:

``` bash
claude plugin validate ~/.claude/skills
```

Đối với project skills:

``` bash
claude plugin validate .claude/skills
```

Validator giúp phát hiện vấn đề cấu trúc, đặc biệt là frontmatter không
parse được.

Theo nội dung khóa học, lệnh này yêu cầu Claude Code phiên bản `2.1.233`
trở lên. Hãy kiểm tra phiên bản thực tế và tài liệu CLI hiện hành nếu
lệnh không khả dụng.

## 7.2. Skill không kích hoạt

**Triệu chứng:** Skill tồn tại nhưng Claude không tự dùng khi người dùng
yêu cầu.

Nguyên nhân thường gặp: `description` chưa phản ánh đúng cách người dùng
diễn đạt.

Cách xử lý:

1.  Đọc lại mô tả và xác định nó có nói rõ chức năng, tình huống sử dụng
    hay chưa.
2.  Bổ sung những cụm từ người dùng thực sự thường dùng.
3.  Thử nhiều cách diễn đạt tương đương.
4.  Nếu một cách diễn đạt không kích hoạt, cân nhắc bổ sung cụm từ đó
    vào description.

Ví dụ skill tối ưu hiệu năng nên thử các yêu cầu:

``` text
Why is this page slow?
Make this page faster.
Help me profile this application.
Improve scrolling performance.
```

Không nên chỉ thử đúng một prompt duy nhất rồi kết luận skill hoạt động
tốt.

## 7.3. Skill không được nạp

Kiểm tra hai điều kiện cấu trúc:

-   `SKILL.md` phải nằm trong một thư mục skill có tên, không đặt trực
    tiếp ở thư mục gốc `skills/`.
-   Tên file phải chính xác là `SKILL.md` --- chữ `SKILL` viết hoa, `md`
    viết thường.

Cấu trúc đúng:

``` text
.claude/skills/
└── my-skill/
    └── SKILL.md
```

Cấu trúc sai:

``` text
.claude/skills/
└── SKILL.md
```

Nếu vẫn không được, kiểm tra YAML frontmatter và chạy:

``` bash
claude --debug
```

Tìm thông báo liên quan đến tên skill để xác định lỗi loading.

## 7.4. Claude dùng nhầm skill

Nếu có nhiều skill với mô tả gần giống nhau, Claude có thể khó phân biệt
skill phù hợp.

Cách sửa:

-   Làm rõ phạm vi chuyên môn của từng skill.
-   Dùng tên cụ thể.
-   Viết description tập trung vào một nhóm nhiệm vụ rõ ràng.
-   Tránh để nhiều skill cùng mô tả chung chung như "helps with coding".

Ví dụ phân tách:

-   `frontend-performance`: tối ưu tốc độ render, tải tài nguyên và
    scrolling.
-   `frontend-accessibility`: kiểm tra semantic HTML, keyboard
    navigation và screen reader.
-   `frontend-security`: kiểm tra XSS, xử lý input và bảo vệ dữ liệu.

## 7.5. Xung đột ưu tiên

Nếu personal skill không được sử dụng như mong đợi, kiểm tra xem có
enterprise skill cùng tên hay không.

Thứ tự ưu tiên:

``` text
Enterprise
   ↓
Personal
   ↓
Project
```

Plugin skills được namespace riêng.

Cách xử lý:

1.  Đổi tên skill cá nhân/dự án để tránh trùng tên.
2.  Nếu là enterprise skill, trao đổi với quản trị viên để hiểu chính
    sách áp dụng.

## 7.6. Plugin skill không xuất hiện

Các bước kiểm tra:

1.  Chạy `/plugin`.
2.  Kiểm tra plugin có trong mục Installed và đã được bật hay chưa.
3.  Xem tab Errors để tìm lỗi.
4.  Nếu vừa cài plugin trong phiên hiện tại, chạy:

``` text
/reload-plugins
```

5.  Kiểm tra cấu trúc plugin: skill phải nằm trong thư mục `skills/` ở
    root plugin, không phải bên trong `.claude-plugin/`.
6.  Chạy validator trên thư mục plugin:

``` bash
claude plugin validate path/to/plugin
```

Lệnh này giúp kiểm tra manifest và frontmatter của các skill.

## 7.7. Lỗi runtime

Skill có thể được nạp thành công nhưng thất bại khi chạy script hoặc
thực hiện quy trình.

Các nguyên nhân thường gặp:

  -----------------------------------------------------------------------
  Lỗi                     Nguyên nhân             Cách xử lý
  ----------------------- ----------------------- -----------------------
  Missing dependencies    Thiếu package/thư viện  Cài dependency và ghi
                          cần thiết               rõ yêu cầu trong hướng
                                                  dẫn

  Permission denied       Script không có quyền   Cấp quyền bằng
                          thực thi                `chmod +x`

  Path issue              Đường dẫn không tương   Ưu tiên dấu `/` cho
                          thích                   đường dẫn

  Script failure          Lệnh hoặc môi trường    Kiểm tra output, exit
                          chạy lỗi                code và dependency

  Wrong skill             Mô tả bị chồng lấn      Làm description cụ thể
                                                  hơn
  -----------------------------------------------------------------------

Ví dụ:

``` bash
chmod +x scripts/check-environment.sh
```

Nên kiểm tra script độc lập trong terminal trước khi cho skill phụ thuộc
vào nó.

## 7.8. Checklist xử lý nhanh

-   [ ] Không kích hoạt? Kiểm tra `description` và trigger phrases.
-   [ ] Không xuất hiện? Kiểm tra đường dẫn, tên file `SKILL.md`, YAML.
-   [ ] Dùng nhầm skill? Làm mô tả khác biệt và cụ thể hơn.
-   [ ] Bị skill khác ghi đè? Kiểm tra thứ tự ưu tiên.
-   [ ] Plugin skill mất? Kiểm tra `/plugin`, Errors và
    `/reload-plugins`.
-   [ ] Script lỗi? Kiểm tra dependency, quyền thực thi và đường dẫn.
-   [ ] Chưa rõ nguyên nhân? Chạy validator và `claude --debug`.

------------------------------------------------------------------------

# 8. Quy trình xây dựng skill từ đầu đến cuối

Phần này tổng hợp kiến thức 6 bài thành một quy trình có thể áp dụng
trong dự án.

## Bước 1 --- Chọn một công việc lặp lại

Ví dụ: review PR, viết tài liệu API, kiểm tra accessibility hoặc tối ưu
hiệu năng frontend.

Xác định rõ:

-   Đầu vào của tác vụ là gì?
-   Claude cần thực hiện những bước nào?
-   Công cụ nào cần dùng?
-   Đầu ra mong muốn có cấu trúc ra sao?
-   Những hành động nào không được tự ý thực hiện?

## Bước 2 --- Chọn phạm vi lưu trữ

-   Dùng cá nhân: `~/.claude/skills/`.
-   Dùng trong repository: `.claude/skills/`.
-   Dùng nhiều repository: cân nhắc plugin.
-   Bắt buộc toàn tổ chức: enterprise managed settings.

## Bước 3 --- Viết metadata rõ ràng

Bắt đầu với `name` và `description`. Chỉ thêm `allowed-tools`, `model`
hoặc trường khác khi có nhu cầu cụ thể.

## Bước 4 --- Viết hướng dẫn có thể thực thi

Nội dung nên có:

1.  Vai trò và mục tiêu.
2.  Điều kiện kích hoạt hoặc phạm vi áp dụng.
3.  Quy trình từng bước.
4.  Công cụ và tài liệu cần tham khảo.
5.  Tiêu chí chất lượng.
6.  Format đầu ra.
7.  Giới hạn và hành động cần xin phép.

## Bước 5 --- Tách tài liệu khi skill lớn

Giữ `SKILL.md` tập trung vào quy trình chính. Đưa checklist dài, tài
liệu framework, ví dụ và template sang `references/` hoặc `assets/`.
Dùng `scripts/` cho tác vụ tự động hóa ổn định.

## Bước 6 --- Kiểm tra quyền và an toàn

Chỉ khai báo `allowed-tools` khi thực sự cần. Đặc biệt cẩn trọng với
`Bash`, lệnh ghi/xóa file, truy cập mạng và dữ liệu nhạy cảm.

## Bước 7 --- Validate và thử nghiệm

``` bash
claude plugin validate .claude/skills
```

Thử nhiều cách diễn đạt khác nhau, cả yêu cầu kích hoạt đúng và yêu cầu
gần giống nhưng không nên kích hoạt.

## Bước 8 --- Chia sẻ và bảo trì

Commit skill vào Git hoặc đóng gói thành plugin. Khi cập nhật, kiểm tra
lại metadata, trigger, script và tài liệu tham khảo. Nên xem skill như
một phần của codebase: có review, version control và kiểm thử.

------------------------------------------------------------------------

# 9. Checklist và bài tập thực hành

## 9.1. Checklist trước khi hoàn thành một skill

### Cấu trúc

-   [ ] Mỗi skill có thư mục riêng.
-   [ ] File có tên chính xác `SKILL.md`.
-   [ ] YAML frontmatter hợp lệ.
-   [ ] `description` mô tả rõ chức năng và điều kiện sử dụng.

### Nội dung

-   [ ] Hướng dẫn có trình tự rõ ràng.
-   [ ] Có tiêu chí đánh giá kết quả.
-   [ ] Có format đầu ra khi cần.
-   [ ] Nêu rõ giới hạn và hành động không được tự ý thực hiện.

### Hiệu quả

-   [ ] `SKILL.md` gọn, dễ đọc (mục tiêu dưới 500 dòng).
-   [ ] Tài liệu dài được tách vào `references/`.
-   [ ] Script được dùng cho tác vụ ổn định, lặp lại.
-   [ ] Description không chồng lấn với skill khác.

### Quyền và bảo mật

-   [ ] `allowed-tools` chỉ gồm công cụ cần thiết.
-   [ ] Không phê duyệt shell rộng hơn mức cần thiết.
-   [ ] Script không có side effect bất ngờ.
-   [ ] Không đưa secret hoặc credential vào skill.

### Kiểm thử và chia sẻ

-   [ ] Validator chạy thành công.
-   [ ] Đã thử nhiều prompt kích hoạt.
-   [ ] Đã thử tình huống không nên kích hoạt.
-   [ ] Đã kiểm tra thứ tự ưu tiên và plugin nếu có.
-   [ ] Đã kiểm tra dependency và quyền thực thi của script.

## 9.2. Bài tập thực hành: tạo skill review code

**Yêu cầu:** tạo project skill `backend-review` để review code backend.

Cấu trúc:

``` text
.claude/skills/backend-review/
├── SKILL.md
└── references/
    └── security-checklist.md
```

Nội dung khởi đầu cho `SKILL.md`:

``` markdown
---
name: backend-review
description: Reviews backend code for correctness, API design, security, error handling, and test coverage. Use when reviewing backend changes, pull requests, or server-side code.
---

# Backend Review

1. Inspect the relevant diff and surrounding implementation.
2. Understand the expected behavior and API contract.
3. Check input validation, authorization, and data handling.
4. Review error handling and edge cases.
5. Check tests and identify missing coverage.
6. Consult `references/security-checklist.md` for security-sensitive changes.

## Output

Group findings by severity:

- Critical
- High
- Medium
- Low

For each finding, include:
- File and line reference
- The problem
- Why it matters
- A concrete suggested fix

Do not report speculative issues as confirmed findings.
If no issue is found, summarize what was checked and mention remaining limitations.
```

Bài tập mở rộng:

1.  Tạo `security-checklist.md` với các mục authentication,
    authorization, input validation, injection, secrets và logging.
2.  Thử các prompt như "Review this backend PR", "Check this API change
    for security issues".
3.  Thử yêu cầu không liên quan, ví dụ "Design a landing page", để kiểm
    tra phạm vi matching.
4.  Chạy validator.
5.  Commit skill vào repository và nhờ một thành viên khác thử nghiệm.

## 9.3. Câu hỏi ôn tập cuối khóa

1.  Skill khác `CLAUDE.md` ở thời điểm nạp context như thế nào?
2.  Vì sao `description` quan trọng hơn việc viết thật nhiều hướng dẫn?
3.  `allowed-tools` có phải danh sách giới hạn toàn bộ công cụ không?
4.  Progressive disclosure giúp tiết kiệm context window như thế nào?
5.  Khi nào nên dùng subagent thay vì skill?
6.  Hook khác skill ở cơ chế kích hoạt nào?
7.  Ba cách chia sẻ skill chính là gì?
8.  Subagent có tự động kế thừa skill đã được gọi ở agent chính không?
9.  Khi skill không kích hoạt, nên kiểm tra điều gì đầu tiên?
10. Khi skill không được nạp, cần kiểm tra cấu trúc file nào?

------------------------------------------------------------------------

# 10. Bảng thuật ngữ

  -----------------------------------------------------------------------
  Thuật ngữ                           Giải thích
  ----------------------------------- -----------------------------------
  Agent Skill                         Gói hướng dẫn và tài nguyên giúp
                                      agent thực hiện tác vụ chuyên biệt

  `SKILL.md`                          File trung tâm chứa metadata và chỉ
                                      dẫn của skill

  Frontmatter                         Phần metadata YAML ở đầu file
                                      Markdown

  `description`                       Mô tả chức năng và điều kiện kích
                                      hoạt skill

  Semantic matching                   So khớp theo ý nghĩa yêu cầu, không
                                      chỉ theo từ khóa chính xác

  Context window                      Không gian ngữ cảnh model có thể
                                      tiếp nhận trong một lượt làm việc

  Progressive disclosure              Chia nội dung thành nhiều cấp và
                                      chỉ nạp chi tiết khi cần

  `allowed-tools`                     Công cụ được phê duyệt trước khi
                                      skill hoạt động

  `disallowed-tools`                  Công cụ bị loại khỏi tập công cụ có
                                      thể sử dụng

  Personal skill                      Skill dùng trong nhiều dự án của cá
                                      nhân

  Project skill                       Skill nằm trong repository và được
                                      chia sẻ cùng dự án

  Enterprise skill                    Skill được quản lý và triển khai ở
                                      cấp tổ chức

  Plugin                              Gói mở rộng có thể phân phối và cài
                                      đặt qua marketplace

  Marketplace                         Nơi phân phối và khám phá plugin

  Subagent                            Agent con chạy trong context riêng
                                      để xử lý nhiệm vụ được giao

  Hook                                Cơ chế chạy hành động khi một sự
                                      kiện cụ thể xảy ra

  MCP server                          Thành phần cung cấp công cụ/tích
                                      hợp với hệ thống bên ngoài

  Validator                           Công cụ kiểm tra cấu trúc và tính
                                      hợp lệ của skill/plugin

  Runtime error                       Lỗi xảy ra trong quá trình thực thi
                                      skill hoặc script
  ------------------------------------------------------------------------

## Nguồn tham khảo

### Tài liệu khóa học

- [Introduction to agent skills -- Claude Academy](https://academy.claude.com/courses/introduction-to-agent-skills)
- [Claude Code --- Skills documentation](https://code.claude.com/docs/en/skills)

### Tài liệu bổ sung

- [Steering Claude Code: Skills, hooks, rules, subagents, and more](https://claude.com/blog/steering-claude-code-skills-hooks-rules-subagents-and-more)

> **Ghi chú về phạm vi:** Tài liệu này tổng hợp cấu trúc và nội dung cốt lõi của khóa học, đồng thời bổ sung ví dụ cấu hình và hướng dẫn thực hành để phục vụ việc học. Đây là tài liệu học tập được biên soạn lại, không phải bản transcript nguyên văn của video. Các trường cấu hình và giao diện Claude Code có thể thay đổi theo phiên bản; hãy đối chiếu tài liệu chính thức khi triển khai.

# Tài liệu ôn thi: Introduction to Agent Skills

> Bản ôn tập đầy đủ về thiết kế, cấu hình, kiểm thử và chia sẻ Skills trong Claude Code; không phải đề thi chính thức. Đối chiếu [ghi chú khóa học](../introduction-to-agent-skills-notes.md), [trang Claude Academy](https://academy.claude.com/courses/introduction-to-agent-skills) và [tài liệu Skills hiện hành](https://code.claude.com/docs/en/skills).

## 1. Kết quả cần đạt

Bạn nên có thể: giải thích Skill giải quyết vấn đề nào; tạo thư mục có `SKILL.md` và metadata phù hợp; viết description mô tả đúng lúc kích hoạt; dùng progressive disclosure cho nội dung lớn; hiểu `allowed-tools`; phân biệt Skill với `CLAUDE.md`, subagent, Hook, MCP và Plugin; và tìm nguyên nhân khi Skill không được chọn hoặc không chạy như mong đợi.

## 2. Skill là gì?

Skill là một thư mục hướng dẫn/tài nguyên cho công việc lặp lại. Nó có thể chứa quy trình, tiêu chuẩn, mẫu đầu ra, tài liệu tham khảo và script. Thay vì dán cùng một chuỗi hướng dẫn vào mọi prompt, người dùng mô tả workflow một lần để Claude có thể áp dụng khi task phù hợp.

Cấu trúc tối thiểu:

```text
.claude/skills/pr-review/
└── SKILL.md
```

Skill dùng cá nhân thường lưu ở thư mục người dùng; Skill dự án đặt trong `.claude/skills/` để có thể chia sẻ qua repository. Kiểm tra tài liệu hiện hành về vị trí/scope, vì các môi trường hoặc sản phẩm Claude có thể có quy ước khác nhau.

`SKILL.md` chứa hướng dẫn. YAML frontmatter là tùy chọn; nếu có, mọi field đều tùy chọn, nhưng `description` được khuyến nghị để Claude biết nội dung Skill và khi nào nên dùng. Ví dụ:

```markdown
---
name: pr-review
description: Review changed code for correctness and security. Use when asked to review a pull request or inspect a proposed change.
---

1. Inspect the complete diff and relevant surrounding code.
2. Report only actionable findings with file and evidence.
3. List checks that were not run.
```

`name` định danh skill; `description` cho biết chức năng và tình huống sử dụng; phần thân hướng dẫn cách thực hiện. Tên, vị trí và trường metadata cần khớp đặc tả của môi trường Claude Code đang dùng.

## 3. Description và matching

Description vừa là lời giới thiệu vừa giúp Claude quyết định Skill nào phù hợp. Một description hiệu quả trả lời:

- Skill làm việc gì và tạo ra loại kết quả nào?
- Người dùng thường diễn đạt yêu cầu kích hoạt ra sao?
- Khi nào **không** nên dùng, nếu có khả năng nhầm với Skill khác?

**Mơ hồ:** `description: Helps with code.`

**Tốt hơn:** `Reviews a proposed code change for correctness, security, and regressions. Use when the user asks to review a PR, branch, or diff; do not use for implementing the requested change.`

Thử matching bằng nhiều cách diễn đạt tự nhiên. Nếu Skill không được chọn, kiểm tra tên/đường dẫn/frontmatter và description trước; sau đó kiểm tra xung đột với Skill khác hoặc trạng thái tải. Không mặc định rằng việc nhắc đúng một từ khóa là điều kiện duy nhất để Skill chạy.

## 4. Progressive disclosure và cấu trúc đa file

Nội dung nên được nạp theo mức cần thiết:

1. **Metadata** giúp Claude nhận diện Skill.
2. **`SKILL.md`** cung cấp hướng dẫn cốt lõi khi Skill được dùng; frontmatter có thể bổ sung metadata.
3. **References** chứa quy tắc dài, bảng tra cứu hoặc ví dụ chuyên sâu chỉ cần khi làm tác vụ tương ứng.
4. **Scripts/assets/templates** cung cấp thao tác hoặc tài nguyên cụ thể khi được gọi.

Ví dụ:

```text
.claude/skills/api-review/
├── SKILL.md
├── references/
│   ├── security-checklist.md
│   └── api-conventions.md
└── scripts/
    └── collect-openapi-diff.sh
```

Giữ `SKILL.md` tập trung vào quy trình chính và đường dẫn đến tài liệu phụ. Nếu mọi thứ nhét vào một file, hướng dẫn trở nên khó tìm và context lãng phí. Nếu tách quá nhiều mà không chỉ cách mở file, Claude có thể không dùng được phần quan trọng. Script có thể chạy logic nhất quán, nhưng vẫn cần xem input/output, quyền và tác động.

## 5. Quyền công cụ và an toàn

`allowed-tools` có thể khai báo công cụ được phép dùng trước khi Skill chạy, tùy cấu hình/phiên bản. Đây không phải cách chặn mọi công cụ không liệt kê; quyền thực tế còn phụ thuộc permission settings của Claude Code. Chỉ cấp những tool tối thiểu cần cho workflow.

- Review chỉ đọc thì không cần cấp tool sửa file.
- Nếu script chạy lệnh, kiểm tra lệnh, input, quyền ghi và khả năng tác động bên ngoài.
- Skill là hướng dẫn/tài nguyên, không phải ranh giới bảo mật độc lập.
- Đọc nội dung Skill từ nguồn tin cậy trước khi cài/chạy; xem script và dependency.
- Không đưa secret hoặc dữ liệu ngoài phạm vi vào Skill.

## 6. Chọn đúng tính năng

| Tính năng | Câu hỏi quyết định | Ví dụ |
| --- | --- | --- |
| **`CLAUDE.md`** | Hướng dẫn nào áp dụng thường trực cho repo? | Lệnh test, quy ước tên, kiến trúc |
| **Skill** | Quy trình nào chỉ dùng cho một họ task? | Checklist review PR |
| **Subagent** | Có phần việc độc lập cần context/role/tool riêng không? | Reviewer chỉ đọc, báo lỗi |
| **Hook** | Quy tắc nào phải chạy theo event và cần thực thi tự động? | Chặn ghi file vào thư mục cấm |
| **MCP** | Cần kết nối hệ thống ngoài để lấy dữ liệu/thực hiện thao tác? | Truy vấn dịch vụ nội bộ |
| **Plugin** | Cần đóng gói/phân phối nhiều thành phần cho nhóm? | Skills + Hooks + MCP config |

Một hệ thống có thể dùng kết hợp: `CLAUDE.md` nêu quy ước nền, Skill mô tả quy trình review, subagent chạy review độc lập, Hook gate test, MCP lấy metadata PR, Plugin phân phối cấu hình. Không dùng Skill để thay thế Hook nếu điều kiện bắt buộc cần được thực thi.

## 7. Quy trình xây dựng và xử lý sự cố

1. Chọn việc lặp lại có bước và tiêu chí ổn định.
2. Chọn scope cá nhân hay dự án theo nhu cầu chia sẻ.
3. Viết `name`, `description` cụ thể và hướng dẫn có đầu ra rõ.
4. Tách reference/script khi nội dung phụ dài hoặc chỉ cần trong một số trường hợp.
5. Rà quyền và nguồn; chỉ cấp tool cần thiết.
6. Kiểm tra cấu trúc bằng validator/cách kiểm tra chính thức nếu có.
7. Thử với yêu cầu điển hình, cách diễn đạt khác, task gần giống nhưng không nên kích hoạt, và trường hợp thiếu dữ liệu.
8. Sửa description nếu matching sai; sửa phần thân nếu Skill được nạp nhưng làm sai quy trình.
9. Chia sẻ bằng Git/plugin/managed settings theo cơ chế tổ chức; hướng dẫn cập nhật và gỡ bỏ.

**Chẩn đoán nhanh:** không thấy Skill → kiểm tra scope/đường dẫn/frontmatter/tải lại; chọn nhầm → làm rõ description và ví dụ; nạp nhưng làm sai → sửa trình tự, constraint, expected output; script lỗi → chạy/kiểm tra độc lập và báo rõ dependency/exit code; hoạt động thiếu quyền → đối chiếu `allowed-tools` với permission mode và cấu hình hiện tại.

## 8. Bài tập ứng dụng có lời giải mẫu

**Bài:** Tạo Skill `release-check` để kiểm tra bản phát hành nhưng không tự deploy.

- `description` nên nêu khi dùng: trước release, yêu cầu release checklist hoặc kiểm tra readiness; nêu không deploy.
- Phần thân có các bước: đọc changelog/diff; xác định test/build; kiểm tra migration/config/rủi ro; báo blockers; chia trạng thái pass/fail/not run.
- Reference có thể chứa tiêu chuẩn nội bộ chi tiết; script chỉ thu thập thông tin an toàn, read-only nếu đủ.
- Tool access chỉ cấp đọc/tìm kiếm và chạy kiểm tra được cho phép; không cấp deploy/write nếu không cần.
- Kiểm thử với task release đúng, một câu hỏi chung về CI, thiếu changelog và yêu cầu deploy trực tiếp để đảm bảo scope đúng.

**Tiêu chí:** Skill có trigger, giới hạn, quy trình, đầu ra và cách báo trở ngại; không coi “Claude làm đúng trong một lần” là đã kiểm thử đầy đủ.

## 9. Câu hỏi ôn tập

1. **Khi nào nên tạo Skill?** Khi một quy trình có ích được lặp lại, có thể mô tả và có tiêu chí đủ ổn định.
2. **`SKILL.md` gồm những phần nào?** Phần hướng dẫn; YAML frontmatter tùy chọn có metadata và `description` được khuyến nghị. Thư mục có thể kèm references/scripts/assets.
3. **Description cần làm được gì?** Nêu nhiệm vụ và tín hiệu/tình huống phù hợp để chọn Skill.
4. **Progressive disclosure giải quyết vấn đề gì?** Giữ hướng dẫn chính gọn và chỉ đưa chi tiết vào context khi cần.
5. **Khi nào dùng reference file?** Khi bảng tra cứu/quy tắc dài chỉ cần cho một phần workflow.
6. **`allowed-tools` có chặn mọi tool còn lại không?** Không nên hiểu là deny-list; nó liên quan đến phê duyệt trước và quyền còn tùy settings.
7. **Skill khác Hook thế nào?** Skill là hướng dẫn được chọn theo task; Hook chạy logic gắn với event.
8. **Skill khác Subagent thế nào?** Skill bổ sung quy trình cho task; Subagent là worker với context/role/tool riêng.
9. **Skill được nạp nhưng không chạy đúng thì kiểm tra gì?** Hướng dẫn, thứ tự bước, constraints, reference path, tool permission và script output.
10. **Vì sao phải thử trường hợp không nên kích hoạt?** Để phát hiện description quá rộng và giảm việc chọn nhầm.
11. **Một reviewer Skill cần quyền sửa file không?** Thường không nếu nhiệm vụ chỉ báo phát hiện; cấp ít quyền nhất.
12. **Có thể cài Skill từ repo bất kỳ ngay không?** Không nên; xem hướng dẫn/script/dependency và quyền trước khi dùng.

## 10. Tự đánh giá trước kỳ thi

- [ ] Tạo được cấu trúc Skill tối thiểu và giải thích metadata.
- [ ] Viết description có việc làm, tín hiệu kích hoạt và phạm vi.
- [ ] Thiết kế được progressive disclosure.
- [ ] Chọn đúng giữa Skill, `CLAUDE.md`, subagent, Hook, MCP và Plugin.
- [ ] Nêu cách kiểm tra quyền, validator, matching và lỗi runtime.

## Nguồn tham khảo

- [Claude Academy — Introduction to agent skills](https://academy.claude.com/courses/introduction-to-agent-skills)
- [Claude Code Docs — Skills](https://code.claude.com/docs/en/skills)
- [Claude Code Docs — Hooks](https://code.claude.com/docs/en/hooks)

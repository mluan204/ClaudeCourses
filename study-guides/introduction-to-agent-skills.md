# Đề cương ôn tập: Introduction to Agent Skills

> Tài liệu tự ôn trước khi kiểm tra kiến thức; không phải đề thi chính thức.  
> Ghi chú đầy đủ: [Introduction to agent skills](../introduction-to-agent-skills-notes.md)

## Mục tiêu cần đạt

- Giải thích Skill là hướng dẫn/tài nguyên tái sử dụng cho một loại task.
- Tạo cấu trúc Skill đúng và viết description dễ kích hoạt.
- Hiểu progressive disclosure và cách dùng references/scripts.
- Chọn đúng Skill, CLAUDE.md, Subagent, Hook, MCP hoặc Plugin.

## Cấu trúc Skill

Một Skill thường nằm trong thư mục riêng, có SKILL.md với YAML frontmatter và hướng dẫn:

    .claude/skills/pr-review/SKILL.md

- **name:** định danh Skill.
- **description:** Skill làm gì và khi nào nên dùng; hỗ trợ Claude nhận biết task phù hợp.
- **Body:** các bước, tiêu chuẩn, giới hạn và định dạng đầu ra.

Tên file phải là SKILL.md trong thư mục Skill có tên. Scope có thể là cá nhân hoặc project tùy nơi lưu và mức chia sẻ.

## Matching và kích hoạt

- Claude so khớp yêu cầu với description; không chỉ dựa trên một từ khóa chính xác.
- Nêu rõ loại việc, tín hiệu kích hoạt và trường hợp phù hợp.
- Thử nhiều cách người dùng có thể diễn đạt task; sửa description nếu Skill không được chọn.
- Tên trùng scope/cấu hình ưu tiên có thể ảnh hưởng Skill được dùng.

## Progressive disclosure

| Thành phần | Khi nạp/chạy | Lợi ích |
| --- | --- | --- |
| Metadata/description | Sẵn có để Claude xét lựa chọn | Tìm Skill phù hợp |
| Nội dung SKILL.md | Khi Skill được dùng | Hướng dẫn đúng task |
| Tài liệu trong references/ | Khi cần sâu hơn | Giữ file chính gọn |
| Script trong scripts/ | Khi được gọi | Chạy công cụ mà không nạp mã nguồn vào context |
| Assets/templates | Khi cần tạo đầu ra | Tái sử dụng tài nguyên |

Giữ file chính tập trung vào quy trình; tách giải thích dài, template và script thành file phụ, rồi liên kết rõ từ SKILL.md.

## Quyền công cụ

- allowed-tools phê duyệt trước tool được liệt kê khi Skill hoạt động; không phải cách loại tool khác khỏi quyền thông thường.
- disallowed-tools loại tool khỏi phạm vi có thể sử dụng.
- Chỉ cấp tool cần thiết; tránh quyền ghi/chạy lệnh nếu task chỉ cần đọc.

## Chọn đúng tính năng

| Thành phần | Dùng cho |
| --- | --- |
| **CLAUDE.md** | Bối cảnh/quy ước áp dụng liên tục |
| **Skill** | Hướng dẫn chuyên biệt được chọn theo loại task |
| **Subagent** | Công việc độc lập trong context riêng |
| **Hook** | Hành động gắn với event, chạy xác định |
| **MCP server** | Cung cấp tool/resource từ hệ thống khác |
| **Plugin/marketplace** | Đóng gói và phân phối cho nhóm |

## Xây dựng và kiểm tra

1. Chọn công việc nhiều bước được lặp lại.
2. Chọn scope project hay cá nhân.
3. Viết name và description rõ.
4. Viết bước, giới hạn và output.
5. Tách tài liệu/script phụ khi cần.
6. Kiểm tra tool permissions.
7. Validate cấu trúc và thử nhiều cách diễn đạt.
8. Sửa description nếu Skill không kích hoạt, chọn nhầm hoặc lỗi runtime.
9. Chia sẻ qua Git, Plugin/marketplace hoặc managed settings.

## Điểm dễ nhầm

- allowed-tools là phê duyệt trước; disallowed-tools mới loại công cụ.
- Description giúp chọn Skill, không chỉ là lời giới thiệu.
- Script chạy mà không nạp toàn bộ mã nguồn vào context; output vẫn được đọc.
- Skill không tạo context riêng như Subagent.
- Hook chạy theo event; Skill được chọn theo task/description.
- Plugin phân phối Skill và cấu hình, nhưng cần xem thành phần trước khi cài.

## Tự kiểm tra

1. **Skill gồm gì?** Thư mục có tên, SKILL.md, frontmatter hợp lệ và hướng dẫn.
2. **Description tốt nên nói gì?** Mục đích, loại task và lúc nên kích hoạt.
3. **Progressive disclosure giải quyết gì?** Chỉ nạp chi tiết khi cần, giữ file chính gọn.
4. **allowed-tools khác disallowed-tools thế nào?** Phê duyệt trước tool so với loại tool khỏi phạm vi.
5. **Khi nào chọn Skill thay vì Subagent?** Khi cần hướng dẫn trong hội thoại hiện tại; Subagent phù hợp việc độc lập/context riêng.
6. **Skill không kích hoạt: kiểm tra gì?** Đường dẫn, tên file, frontmatter, description, scope và validator.
7. **Skill nào phù hợp tạo đầu tiên?** Quy trình lặp lại có tiêu chí kiểm chứng rõ, như review hoặc pre-PR check.

## Checklist trước khi thi

- [ ] Vẽ được cấu trúc thư mục/frontmatter.
- [ ] Giải thích matching và progressive disclosure.
- [ ] Phân biệt allowed-tools với disallowed-tools.
- [ ] Chọn được giữa Skill, Subagent, Hook và MCP.
- [ ] Biết validate, thử và sửa Skill.

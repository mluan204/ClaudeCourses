# Tài liệu ôn thi: Claude Code in Action

> Bản ôn tập đầy đủ cho phiên làm việc dài, tự động hóa và kiểm chứng Claude Code; không phải đề thi chính thức. Khóa học hướng đến người đã quen task một prompt và cần mở rộng sang quy trình ít giám sát hơn. Xem [ghi chú khóa học](../claude-code-in-action-notes.md), [trang Claude Academy](https://academy.claude.com/courses/claude-code-in-action) và các docs Claude Code hiện hành ở cuối tài liệu.

## 1. Mục tiêu học tập

Bạn cần biết cách định hướng phiên dài; viết `CLAUDE.md` ngắn, hữu ích; tạo Skill cho verification; chọn permission mode; dùng Hooks để tự động hóa quy tắc; phân biệt scheduled cloud task/headless CLI/Agent SDK/GitHub automation; kiểm tra phiên không giám sát; và đóng gói cấu hình nhóm bằng Plugin. Ý cốt lõi: **tự chủ cao hơn đòi hỏi tiêu chí, quyền và kiểm chứng tốt hơn**.

## 2. Định hướng phiên dài

Phiên dài gặp ba áp lực: yêu cầu có thể lan rộng; context phình hoặc bị nén; và mô hình có thể tiếp tục dựa vào giả định đã lỗi thời. Các công cụ steering giải quyết những khía cạnh khác nhau:

| Khả năng | Dùng khi | Lưu ý |
| --- | --- | --- |
| **Plan mode** | Task nhiều bước cần khảo sát/phê duyệt hướng đi trước khi sửa | Kế hoạch vẫn là giả thuyết; sửa khi có bằng chứng mới |
| **Compaction** | Cần giữ quyết định/tiêu chí nhưng rút gọn lịch sử | Nêu rõ nội dung quan trọng để giữ; kiểm tra bản tóm tắt sau đó |
| **Rewind** | Cần quay lại checkpoint hoặc sửa hướng sau hành động sai | Kiểm tra trạng thái code và conversation được khôi phục ra sao |
| **Goal/loop** | Có điều kiện hoàn thành kiểm tra được hoặc cần lặp tác vụ định kỳ | Viết điều kiện dừng rõ; giới hạn thời gian/phạm vi và kiểm tra mỗi vòng |
| **Git worktree** | Chạy phiên độc lập song song trên nhánh/cây làm việc riêng | Mỗi phiên cần được review; tránh đụng chung tài nguyên ngoài worktree |

Tên command, khả năng và giới hạn có thể đổi theo bản Claude Code/plan. Dùng current docs/CLI reference trước khi thiết kế automation dựa trên một flag cụ thể.

## 3. Viết `CLAUDE.md` để có ích

`CLAUDE.md` là hướng dẫn nền cho dự án, không phải permission system và không đảm bảo mọi hành động bị cưỡng chế. Giữ nội dung:

- Cụ thể, đúng với repo hiện tại, thường xuyên cần đến.
- Gồm kiến trúc/đường dẫn, lệnh build/test/lint, quy ước và giới hạn quan trọng.
- Dễ kiểm tra: “chạy `npm test -- path`” hữu ích hơn “test kỹ”.
- Ngắn và có ưu tiên; đưa hướng dẫn dài/theo task vào Skill/reference.
- Được rà lại khi script, kiến trúc hoặc quy tắc đổi.

Khi nạp file bằng import, đừng mặc định nó làm tổng lượng context biến mất: nội dung liên quan có thể vẫn được đọc như hướng dẫn. Dùng file nền cho sự thật/quy tắc tổng quát; dùng Skill cho thủ tục theo tình huống; dùng permission/hook để enforce điều kiện bắt buộc.

## 4. Verification skill và tiêu chí hoàn thành

Quy trình kiểm tra lặp lại phù hợp với một verification Skill. Nó nên mô tả:

1. Đọc yêu cầu và tiêu chí nghiệm thu.
2. Kiểm tra diff, không chỉ lời tóm tắt.
3. Chạy lệnh test/build/lint phù hợp, ghi output và exit status.
4. Đối chiếu behavior thực với acceptance criteria.
5. Báo **pass / fail / not run / unverified**, kèm file, bằng chứng và blocker.

Nếu chỉ ghi “tests pass” thì người đọc không biết đã chạy test nào. Nếu chưa chạy được, nêu nguyên nhân và mức rủi ro còn lại. Hãy thử Skill với task thật, task thiếu test, thay đổi nhiều file và trường hợp tool bị lỗi.

## 5. Permission modes

Mode định hướng mức tự động phê duyệt tool; policy cụ thể còn chịu ảnh hưởng của permission rules, hooks, cấu hình tổ chức và môi trường. Trong Claude Code docs hiện hành, các mode gồm:

| Mode (UI/setting) | Ý nghĩa | Dùng thận trọng |
| --- | --- | --- |
| **Manual / `default`** | Hỏi quyền cho hành động cần phê duyệt | Tốt để xem hành động trước khi cho phép |
| **Edit automatically / `acceptEdits`** | Tự chấp nhận sửa file và một số lệnh filesystem phổ biến trong thư mục làm việc; lệnh khác vẫn theo permission | Khi muốn cho phép chỉnh sửa nhưng giữ kiểm soát lệnh shell |
| **Plan / `plan`** | Khảo sát/lập kế hoạch, không sửa source; một số shell/read-only actions được xử lý theo policy hiện hành | Trước task phức tạp |
| **Auto / `auto`** | Classifier nền kiểm tra một số hành động trước khi chạy thay vì hỏi từng lần | Có thể giảm gián đoạn; không kiểm tra code correctness |
| **Don't ask / `dontAsk`** | Tự từ chối các hành động vốn cần hỏi, thay vì đợi người dùng | Pipeline không tương tác; có thể khiến bước cần thiết bị từ chối |
| **Bypass permissions / `bypassPermissions`** | Bỏ phần lớn permission prompts | Chỉ trong container/VM cô lập, có giới hạn tác động |

Mode khả dụng có thể phụ thuộc plan/CLI/IDE và settings quản trị. Tên nhãn trong bài học hoặc giao diện cũ có thể khác. **Auto là kiểm soát rủi ro hành động, không phải kiểm tra tính đúng của kết quả.** Bypass không phải cách “tin cậy hơn”; nó loại bỏ nhiều điểm dừng nên chỉ phù hợp môi trường cô lập.

## 6. Hooks: tự động hóa tại event

Hook là handler chạy tại điểm cụ thể trong vòng đời Claude Code. Dùng Hook khi điều gì đó cần xảy ra theo event, chẳng hạn kiểm tra tool call trước, chạy kiểm tra sau khi sửa file, hoặc ngăn kết thúc lượt khi điều kiện chưa đạt.

- **PreToolUse:** chạy trước tool; tùy handler có thể cho phép/từ chối/hỏi hoặc điều chỉnh input theo cơ chế hỗ trợ.
- **PostToolUse:** chạy sau khi tool hoàn tất thành công; hữu ích để phản hồi kết quả/chạy lint hoặc nhắc kiểm tra, nhưng không thể đảo ngược hành động vừa xảy ra. Lỗi tool có event riêng là `PostToolUseFailure`.
- **Stop:** kiểm tra điều kiện khi Claude muốn kết thúc lượt; có thể yêu cầu tiếp tục nếu điều kiện chưa đạt theo cấu hình event.
- **PreCompact:** chạy trước compaction; stdout của command hook có thể trở thành hướng dẫn cho bản tóm tắt. Muốn thêm lại context **sau** khi nén, dùng `SessionStart` hook với matcher `compact`. **PostCompact** chạy sau compaction và phù hợp để ghi log/cập nhật trạng thái.

Hook cần test: event/matcher có đúng không; script có timeout/lỗi không; exit/JSON response được diễn giải ra sao; có loop không; quyền/secret có bị lộ không; và liệu Hook chặn đúng hành động hay chỉ báo sau khi nó xảy ra. Semantics của exit code/output khác theo loại hook/event, nên tra tài liệu hiện hành thay vì học thuộc một quy tắc duy nhất.

## 7. Scheduled tasks, headless, SDK và GitHub

| Cách chạy | Nơi điều phối | Phù hợp khi |
| --- | --- | --- |
| **Cloud scheduled tasks** | Hạ tầng Claude/Anthropic | Chạy prompt theo lịch/sự kiện được hỗ trợ, không cần máy cá nhân luôn bật |
| **Headless CLI** | Môi trường của bạn qua CLI không tương tác | Script/CI muốn đưa prompt vào và nhận output/exit status |
| **Agent SDK** | Ứng dụng TypeScript/Python của bạn | Cần nhúng điều phối agent vào phần mềm riêng |
| **Hosted Code Review** | Tích hợp review được quản lý | PR review tiêu chuẩn mà không tự duy trì toàn bộ workflow |
| **GitHub Action** | Workflow tùy biến trong GitHub Actions | Tích hợp với mention, event, lịch, permission và bước CI riêng |

Cloud task và headless CLI khác nhau về nơi chạy, xác thực, trigger, filesystem và khả năng truy cập. Kiểm tra quyền/token và phạm vi repository trước khi chọn. GitHub Action cần quyền tối thiểu; không in token vào log; giới hạn event và xử lý output như một đề xuất cần duyệt. Hosted review và Action có thể có quyền/khả năng khác — đọc docs mới nhất trước khi kỳ vọng bot tự merge, approve hoặc sửa code.

## 8. Kiểm chứng phiên không giám sát

Mức kiểm tra cần tăng khi ít người theo dõi hơn. Quy trình tham khảo:

1. Dùng permission mode phù hợp; không mặc định bypass.
2. Giữ yêu cầu và acceptance criteria có thể kiểm tra.
3. Chạy test thực chất như gate; có thể dùng Hook tại event thích hợp.
4. Kiểm tra exit code, structured output và log đã lọc dữ liệu nhạy cảm.
5. Mở diff, đối chiếu file/thay đổi với yêu cầu và rà rủi ro ngoài test.
6. Dùng reviewer độc lập/context mới cho task quan trọng.
7. Báo phần pass, fail, not run và unverified; dừng nếu bằng chứng chưa đủ.

Classifiers/permissions không đánh giá xem logic nghiệp vụ đúng hay không. Test cũng chỉ bảo vệ behavior mà test bao phủ. Tóm tắt của agent là chỉ mục hữu ích, không phải bằng chứng chính.

## 9. Plugins và chia sẻ cho nhóm

Plugin đóng gói các thành phần Claude Code được hỗ trợ—ví dụ Skills, agents, Hooks, MCP configuration và metadata/commands—để phân phối workflow. Plugin được cài có thể chạy script hoặc cung cấp tool; đọc mã, manifest, nguồn, quyền và dependency trước khi bật. Kiểm tra xung đột namespace/settings, nơi tải plugin, quy trình cập nhật/gỡ bỏ và ai chịu trách nhiệm bảo trì. Dùng marketplace/nguồn tin cậy, pin/review thay đổi nếu quy trình yêu cầu.

## 10. Bài tập tổng hợp có lời giải mẫu

**Tình huống:** Nhóm muốn Claude tự phân tích PR ban đêm, chạy test, rồi báo kết quả trên GitHub.

- Định nghĩa “đủ để báo sẵn sàng” bằng checklist: diff, test, security, acceptance criteria; dùng branch/test environment phù hợp.
- Chọn Action hoặc hosted integration theo nhu cầu tùy biến, trigger và quyền; không phỏng đoán khả năng hiện có.
- Dùng token/permission tối thiểu, secret store, giới hạn repository/event, timeout và số lượt agent.
- `CLAUDE.md` ghi lệnh/kiến trúc; verification Skill ghi workflow; Hook gate điều kiện bắt buộc như test phù hợp.
- Khi chạy xong, lưu status/lệnh/diff/findings; yêu cầu người review đọc diff, vì agent không tự chứng minh được correctness.

## 11. Câu hỏi ôn tập

1. **Tại sao phiên dài cần steering?** Scope, assumptions và context có thể trôi; cần giữ mục tiêu/tiêu chí, kiểm tra các mốc.
2. **`CLAUDE.md` có cưỡng chế quy tắc không?** Không; dùng permission rule/Hook hay kiểm soát khác cho hành động bắt buộc.
3. **Skill kiểm chứng nên ghi gì?** Bước kiểm tra, lệnh, tiêu chí, bằng chứng, và cách báo not run/unverified.
4. **Auto mode chứng minh code đúng không?** Không; classifier xem hành động có phù hợp/rủi ro theo policy, không xác nhận tính đúng của logic.
5. **`dontAsk` khác `bypassPermissions` thế nào?** DontAsk từ chối thứ cần hỏi; bypass bỏ prompts cho phần lớn hành động.
6. **Khi nào Bypass được cân nhắc?** Chỉ trong môi trường cô lập như container/VM, khi hậu quả bị giới hạn.
7. **PostToolUse có ngăn command vừa chạy không?** Không; nó chạy sau, có thể xử lý kết quả/đưa phản hồi.
8. **Scheduled task khác headless CLI ra sao?** Cloud task được hạ tầng Claude lên lịch; headless chạy trong pipeline/môi trường của bạn.
9. **Hosted review và GitHub Action khác nhau thế nào?** Tích hợp quản lý sẵn so với workflow tùy chỉnh do repo/nhóm cấu hình; kiểm tra quyền và chức năng cụ thể.
10. **Bằng chứng nào cần đọc sau unattended run?** Diff, test output/exit code, structured result và acceptance criteria.
11. **Plugin nên được xem xét những gì?** Nguồn, script, tools, hooks, MCP config, quyền, dependencies và cách cập nhật/gỡ.
12. **Tại sao dùng reviewer độc lập?** Context mới có thể phát hiện lỗi/giả định mà agent thực hiện task đã bỏ qua.

## 12. Nhầm lẫn cần tránh

- `CLAUDE.md` dạy, Skill hướng dẫn workflow, permission kiểm soát hành động, Hook xử lý event.
- Auto mode không phải test suite; test suite cũng không thay review diff.
- Cloud schedule, `claude -p`, SDK và GitHub Action không phải các tên khác nhau của cùng một runtime.
- Permission mode chỉ là một lớp; tool, rule, sandbox, hook và policy tổ chức cũng ảnh hưởng.
- Output có cấu trúc giúp đọc máy, không tự làm kết luận đúng.

## 13. Tự đánh giá trước kỳ thi

- [ ] Chọn công cụ steering cho phiên dài và nêu điều kiện dừng.
- [ ] Đưa đúng loại quy tắc vào `CLAUDE.md`, Skill, permission hoặc Hook.
- [ ] So sánh sáu mode theo tác động thực tế và rủi ro.
- [ ] Chọn hạ tầng phù hợp cho scheduled/headless/GitHub run.
- [ ] Mô tả quy trình kiểm chứng unattended run bằng bằng chứng.
- [ ] Đánh giá an toàn Plugin trước khi phân phối.

## Nguồn tham khảo

- [Claude Academy — Claude Code in Action](https://academy.claude.com/courses/claude-code-in-action)
- [Claude Code Docs — Permissions](https://code.claude.com/docs/en/permissions)
- [Claude Code Docs — Hooks](https://code.claude.com/docs/en/hooks)
- [Claude Code Docs — Skills](https://code.claude.com/docs/en/skills)
- [Claude Code Docs — CLI reference](https://code.claude.com/docs/en/cli-reference)
- [Claude Code Docs — Plugins](https://code.claude.com/docs/en/plugins)

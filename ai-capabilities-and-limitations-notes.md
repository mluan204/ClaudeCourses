# AI Capabilities and Limitations

## Tài liệu học tập -- Claude Academy

> **Khóa học:** AI Capabilities and Limitations\
> **Nền tảng:** Large Language Models (LLMs) / Claude\
> **Đơn vị:** Anthropic / Claude Academy\
> **Thời lượng khóa học:** khoảng 3,5 giờ\
> **Số bài học:** 13 bài học + 1 bài kiểm tra\
> **Mục tiêu:** Thấu hiểu bản chất và cơ chế vận hành của LLM, làm chủ 4 thuộc tính cốt lõi (Next Token Prediction, Knowledge, Working Memory, Steerability), nắm vững phương pháp chẩn đoán lỗi thực tế và nâng cao năng lực thẩm định (Discernment).

------------------------------------------------------------------------

## Mục lục

1.  [Tổng quan và cách sử dụng tài liệu](#1-tổng-quan-và-cách-sử-dụng-tài-liệu)
2.  [Lesson 1 -- Giới thiệu về khả năng và giới hạn của AI](#2-lesson-1----giới-thiệu-về-khả-năng-và-giới-hạn-của-ai)
3.  [Lesson 2 -- Ý nghĩa của AI: Ta đang nói đến loại AI nào?](#3-lesson-2----ý-nghĩa-của-ai-ta-đang-nói-đến-loại-ai-nào)
4.  [Lesson 3 -- Cách AI có được "tính cách": Pre-training và Fine-tuning](#4-lesson-3----cách-ai-có-được-tính-cách-pre-training-và-fine-tuning)
5.  [Lesson 4 -- Next Token Prediction: Dự đoán token tiếp theo](#5-lesson-4----next-token-prediction-dự-đoán-token-tiếp-theo)
6.  [Lesson 5 -- Thử nghiệm: Next Token Prediction](#6-lesson-5----thử-nghiệm-next-token-prediction)
7.  [Lesson 6 -- Knowledge: Kiến thức của mô hình](#7-lesson-6----knowledge-kiến-thức-của-mô-hình)
8.  [Lesson 7 -- Thử nghiệm: Knowledge](#8-lesson-7----thử-nghiệm-knowledge)
9.  [Lesson 8 -- Working Memory: Trí nhớ làm việc và Context Window](#9-lesson-8----working-memory-trí-nhớ-làm-việc-và-context-window)
10. [Lesson 9 -- Thử nghiệm: Working Memory](#10-lesson-9----thử-nghiệm-working-memory)
11. [Lesson 10 -- Steerability: Khả năng điều khiển AI bằng chỉ dẫn](#11-lesson-10----steerability-khả-năng-điều-khiển-ai-bằng-chỉ-dẫn)
12. [Lesson 11 -- Thử nghiệm: Steerability](#12-lesson-11----thử-nghiệm-steerability)
13. [Lesson 12 -- Khi các thuộc tính va chạm: Chẩn đoán lỗi thực tế](#13-lesson-12----khi-các-thuộc-tính-va-chạm-chẩn-đoán-lỗi-thực-tế)
14. [Lesson 13 -- Chuyển kiến thức thành thói quen](#14-lesson-13----chuyển-kiến-thức-thành-thói-quen)
15. [Checklist và Bản đồ chẩn đoán lỗi](#15-checklist-và-bản-đồ-chẩn-đoán-lỗi)
16. [Tổng kết và câu hỏi ôn tập](#16-tổng-kết-và-câu-hỏi-ôn-tập)

------------------------------------------------------------------------
# 1. Tổng quan và cách sử dụng tài liệu

Tài liệu này không phải bản chép lời video. Nội dung được diễn giải và
mở rộng từ mục tiêu, cấu trúc và mô tả 13 bài học chính thức, kết hợp
kiến thức nền tảng về mô hình ngôn ngữ lớn (LLM) để tạo thành một bộ ghi
chú tự học liền mạch. Các ví dụ là ví dụ minh họa.

Khi học, nên: 1. Đọc phần khái niệm và cơ chế. 2. Tự trả lời câu hỏi
kiểm tra ở cuối mỗi phần. 3. Thử các bài tập với một trợ lý AI. 4. Quay
lại bảng chẩn đoán cuối tài liệu khi gặp câu trả lời sai hoặc không như
mong đợi.

------------------------------------------------------------------------

# 2. Lesson 1 -- Giới thiệu về khả năng và giới hạn của AI

**Mục tiêu:** Hiểu mục đích khóa học, cấu trúc nội dung và cách kết hợp
mô hình tư duy về AI với khung năng lực con người.

## 2.1. Vì sao cần hiểu giới hạn của AI?

Khi sử dụng AI tạo sinh, ta thường gặp hai trải nghiệm trái ngược: - AI
có thể tóm tắt tài liệu dài, viết nội dung hoặc tạo mã trong thời gian
ngắn. - Nhưng cùng hệ thống đó đôi khi lại bịa nguồn tham khảo, hiểu sai
yêu cầu đơn giản hoặc trả lời rất tự tin dù thông tin không chính xác.

Những kết quả này không hoàn toàn ngẫu nhiên. Chúng xuất phát từ cách mô
hình được xây dựng, dữ liệu nó đã học, lượng thông tin nó đang xử lý và
mức độ mà chỉ dẫn của con người có thể định hướng đầu ra.

Nếu chỉ đánh giá AI theo tiêu chí "thông minh" hay "không thông minh",
người dùng sẽ khó biết: - Khi nào có thể giao việc cho AI. - Khi nào cần
cung cấp thêm dữ liệu. - Khi nào phải kiểm chứng bằng nguồn độc lập. -
Nên sửa prompt, chia nhỏ nhiệm vụ hay dùng công cụ khác.

## 2.2. Hai phía của hợp tác người -- AI

Khóa học phân biệt hai nhóm năng lực bổ trợ nhau:

**Năng lực của con người -- khung 4D** - **Delegation (Ủy quyền):** Chọn
việc phù hợp để giao cho AI và xác định phần nào con người cần giữ quyền
quyết định. - **Description (Mô tả):** Trình bày mục tiêu, bối cảnh, dữ
liệu, tiêu chí và định dạng đầu ra rõ ràng. - **Discernment (Nhận
định):** Đánh giá chất lượng, phát hiện sai sót, thiên lệch và giới hạn
của kết quả. - **Diligence (Chăm chỉ/Trách nhiệm):** Kiểm tra, chịu
trách nhiệm với việc sử dụng đầu ra và cân nhắc tác động.

**Thuộc tính của máy -- trọng tâm khóa học** 1. Next Token Prediction --
dự đoán token tiếp theo. 2. Knowledge -- kiến thức được hình thành từ dữ
liệu huấn luyện. 3. Working Memory -- trí nhớ làm việc, thường được biểu
hiện qua cửa sổ ngữ cảnh. 4. Steerability -- khả năng được điều hướng
bằng chỉ dẫn.

Khung 4D nói về việc con người nên làm gì; bốn thuộc tính giải thích vì
sao AI phản ứng theo cách nhất định. Hiểu cả hai giúp phối hợp hiệu quả
hơn.

## 2.3. Bốn thuộc tính là các phổ, không phải công tắc bật/tắt

Một mô hình không đơn giản là "có" hoặc "không có" khả năng. Mỗi thuộc
tính có vùng hoạt động tốt và vùng giới hạn: - Một nhiệm vụ có thể nằm
gần vùng năng lực nếu phổ biến, có mẫu rõ và thông tin đầy đủ. - Nhiệm
vụ có thể tiến về vùng giới hạn nếu hiếm gặp, thiếu dữ liệu, quá dài, mơ
hồ hoặc đòi hỏi độ chính xác tuyệt đối.

**Ý chính:** Mục tiêu không phải tin hoặc không tin AI một cách tuyệt
đối, mà là điều chỉnh mức độ tin cậy theo loại nhiệm vụ và bằng chứng
hiện có.

## 2.Bài tập

Liệt kê 4--6 việc bạn đã dùng AI trong hai tuần gần nhất. Với mỗi việc,
ghi: - Đầu ra có dùng được ngay không? - Bạn phải sửa hoặc kiểm chứng
điều gì? - Nếu không chú ý, lỗi nào có thể xảy ra?

Giữ danh sách này để đối chiếu với bốn thuộc tính ở các bài sau.

------------------------------------------------------------------------

# 3. Lesson 2 -- Ý nghĩa của AI: Ta đang nói đến loại AI nào?

## 3.1. "AI" là một khái niệm rộng

AI (Artificial Intelligence -- trí tuệ nhân tạo) bao gồm nhiều hệ thống
được thiết kế để thực hiện những nhiệm vụ thường cần đến năng lực trí
tuệ của con người. Không phải hệ thống AI nào cũng tạo văn bản, hội
thoại hay hình ảnh.

Có thể phân biệt ba nhóm chức năng thường gặp:

    Nhóm                    Nhiệm vụ chính          Ví dụ
      AI phân loại            Xếp dữ liệu vào         Phân loại email spam,
  (classification)        nhóm/nhãn đã định nghĩa nhận diện ảnh có mèo

  AI dự đoán (prediction) Ước tính một giá trị    Dự báo nhu cầu, dự đoán
                          hoặc khả năng xảy ra    rủi ro

  AI tạo sinh (generative Tạo nội dung mới dựa    Viết văn bản, tạo ảnh,
  AI)                     trên mẫu đã học         sinh mã, tạo âm thanh
  Một hệ thống thực tế có thể kết hợp nhiều nhóm. Bảng trên nhằm phân biệt
mục tiêu chính của từng loại.

## 3.2. AI tạo sinh và mô hình ngôn ngữ lớn

Generative AI tạo ra nội dung mới thay vì chỉ chọn một nhãn hoặc dự đoán
một con số. Với văn bản, công nghệ phổ biến là **Large Language Model
(LLM) -- mô hình ngôn ngữ lớn**.

LLM được huấn luyện trên lượng lớn dữ liệu ngôn ngữ và học các quan hệ,
mẫu hình, cấu trúc và cách các token thường xuất hiện cùng nhau. Khi
nhận prompt, mô hình tạo câu trả lời từng bước bằng cách dự đoán phần
tiếp theo phù hợp với ngữ cảnh.

LLM không hoạt động giống một cơ sở dữ liệu tra cứu đơn giản: - Nó không
nhất thiết lưu một bản sao nguyên vẹn của mọi tài liệu đã đọc. - Nó tạo
đầu ra dựa trên các mẫu và biểu diễn đã học. - Việc một câu trả lời nghe
tự nhiên không chứng minh rằng nội dung đó đúng. - Khả năng trả lời có
thể khác nhau tùy lĩnh vực, độ phổ biến, thời điểm và cách đặt câu hỏi.

## 3.3. Vì sao phân biệt các loại AI quan trọng?

Cách đánh giá phải phù hợp với loại nhiệm vụ: - Với hệ thống phân loại,
cần xem độ chính xác, false positive và false negative. - Với hệ thống
dự đoán, cần xem sai số, độ hiệu chuẩn và dữ liệu kiểm thử. - Với AI tạo
sinh, ngoài tính đúng còn cần đánh giá tính phù hợp, tính nhất quán,
nguồn dẫn, khả năng tuân thủ yêu cầu và mức độ cần kiểm chứng.

**Ý chính:** Khóa học tập trung vào AI tạo sinh/LLM vì đây là loại công
cụ người dùng thường tương tác bằng ngôn ngữ tự nhiên.

## 3.Câu hỏi tự kiểm tra

1.  Hệ thống lọc thư rác khác chatbot tạo email ở điểm nào?
2.  Vì sao câu trả lời trôi chảy không đồng nghĩa với câu trả lời chính
    xác?
3.  Khi nào một bài toán cần hệ thống dự đoán thay vì mô hình tạo sinh?

------------------------------------------------------------------------

# 4. Lesson 3 -- Cách AI có được "tính cách": Pre-training và Fine-tuning

## 4.1. Giai đoạn 1: Pre-training (tiền huấn luyện)

Trong giai đoạn tiền huấn luyện, mô hình tiếp xúc với lượng dữ liệu rất
lớn và học cách dự đoán token tiếp theo trong các chuỗi văn bản.

Quá trình khái quát: 1. Văn bản được chuyển thành token. 2. Mô hình nhận
một phần chuỗi làm ngữ cảnh. 3. Mô hình dự đoán token tiếp theo. 4. Dự
đoán được so sánh với token mục tiêu. 5. Các tham số được điều chỉnh qua
quá trình tối ưu lặp lại trên nhiều ví dụ.

Qua quá trình này, mô hình học được ngữ pháp, phong cách, kiến thức
thường gặp, cấu trúc lập luận và nhiều mối liên hệ trong dữ liệu.

Tiền huấn luyện tạo nền tảng năng lực rộng, nhưng tự nó không đảm bảo mô
hình sẽ cư xử như một trợ lý hữu ích, an toàn hoặc biết làm theo yêu
cầu.

## 4.2. Giai đoạn 2: Fine-tuning (tinh chỉnh)

Sau tiền huấn luyện, mô hình thường được tinh chỉnh bằng dữ liệu và
phương pháp huấn luyện bổ sung để cải thiện hành vi trong các tình huống
sử dụng.

Mục tiêu có thể bao gồm: - Làm theo chỉ dẫn của người dùng tốt hơn. -
Trả lời theo định dạng hội thoại. - Hữu ích, rõ ràng và phù hợp hơn. -
Hạn chế nội dung nguy hiểm hoặc không phù hợp. - Biết từ chối một số yêu
cầu không được phép.

Một số hệ thống còn sử dụng phản hồi của con người hoặc các phương pháp
tối ưu hóa dựa trên đánh giá để điều chỉnh hành vi. Chi tiết kỹ thuật
khác nhau giữa các nhà phát triển và mô hình.

## 4.3. "Dấu vân tay hành vi" có thể xuất hiện

Quá trình huấn luyện và tối ưu hóa có thể tạo ra một số xu hướng:

**Sycophancy -- xu hướng chiều ý/ngả theo người dùng** - Mô hình có thể
đồng tình với giả định của người dùng ngay cả khi giả định đó sai. - Ví
dụ: Người dùng khẳng định một kết luận thiếu căn cứ; AI lập tức củng cố
kết luận thay vì kiểm tra. - Cách xử lý: Yêu cầu AI phản biện, nêu bằng
chứng trái chiều và phân biệt dữ kiện với giả định.

**Verbosity -- xu hướng dài dòng** - Mô hình có thể đưa thêm giải thích,
cảnh báo hoặc nội dung phụ dù người dùng chỉ cần câu trả lời ngắn. -
Cách xử lý: Nêu rõ độ dài, phạm vi, cấu trúc và điều không cần trình
bày.

**Over-caution -- quá thận trọng** - Mô hình có thể từ chối hoặc cảnh
báo quá mức với yêu cầu vốn vô hại. - Cách xử lý: Làm rõ mục đích hợp
pháp, bối cảnh và phạm vi yêu cầu; không yêu cầu mô hình bỏ qua các giới
hạn an toàn.

**Loose confidence calibration -- hiệu chuẩn độ tự tin chưa chặt** - Mô
hình có thể trình bày thông tin chưa chắc chắn bằng giọng điệu chắc
chắn. - Cách xử lý: Yêu cầu nêu mức độ chắc chắn, giả định, nguồn và
phần chưa xác minh; tự kiểm tra thông tin quan trọng.

## 4.4. Điều cần nhớ

-   Pre-training chủ yếu xây dựng năng lực dự đoán và biểu diễn mẫu từ
    dữ liệu.
-   Fine-tuning định hình cách mô hình phản hồi và tuân thủ mục tiêu sử
    dụng.
-   Hành vi của AI là kết quả của dữ liệu, kiến trúc, huấn luyện, chính
    sách và ngữ cảnh; không nên xem nó như tính cách hay ý định của một
    con người.

## 4.Câu hỏi tự kiểm tra

1.  Pre-training và fine-tuning khác nhau thế nào?
2.  Vì sao AI có thể chiều theo ý người dùng?
3.  Làm thế nào nhận biết một câu trả lời tự tin nhưng thiếu căn cứ?

------------------------------------------------------------------------

# 5. Lesson 4 -- Next Token Prediction: Dự đoán token tiếp theo

## 5.1. Token là gì?

Token là đơn vị mà mô hình xử lý khi đọc và tạo văn bản. Một token có
thể là: - Một từ hoàn chỉnh. - Một phần của từ. - Dấu câu. - Khoảng
trắng hoặc một chuỗi ký tự nhất định.

Token không hoàn toàn tương đương với từ. Cùng một câu có thể được chia
thành nhiều token theo bộ mã hóa của mô hình.

## 5.2. Cơ chế dự đoán token tiếp theo

Ở mức khái quát, mô hình: 1. Nhận prompt và ngữ cảnh hiện có. 2. Tính
toán xác suất cho các token có thể xuất hiện tiếp theo. 3. Chọn token
theo cơ chế giải mã của hệ thống. 4. Thêm token vừa chọn vào chuỗi. 5.
Lặp lại cho đến khi hoàn thành câu trả lời hoặc đạt điều kiện dừng.

Ví dụ, với ngữ cảnh "Thủ đô của Việt Nam là...", mô hình có thể gán xác
suất cao cho token "Hà", sau đó tiếp tục tạo phần còn lại của câu trả
lời.

Điểm quan trọng: mô hình đang tạo chuỗi có khả năng phù hợp với ngữ
cảnh, chứ không phải mỗi lần đều thực hiện một thao tác tra cứu và xác
minh sự thật.

## 5.3. Vì sao cơ chế này tạo ra cả năng lực lẫn giới hạn?

**Vùng năng lực** - Tóm tắt văn bản có cấu trúc quen thuộc. - Viết lại,
dịch, định dạng và chuyển đổi phong cách. - Hoàn thiện đoạn văn hoặc mã
theo mẫu phổ biến. - Tạo nội dung có tính nhất quán về ngôn ngữ.

Những nhiệm vụ này thường có nhiều mẫu tương tự trong dữ liệu huấn luyện
và có cấu trúc đầu ra dễ nhận biết.

**Vùng giới hạn** - Bịa tên tài liệu, trích dẫn, đường dẫn hoặc số
liệu. - Tạo lời giải nghe hợp lý nhưng sai. - Điền vào khoảng trống
thông tin bằng một chi tiết có vẻ phù hợp. - Duy trì sự chính xác tuyệt
đối trong chuỗi lập luận dài.

## 5.4. Hallucination -- Ảo giác AI

Hallucination xảy ra khi AI tạo ra thông tin sai, không có căn cứ hoặc
không được nguồn hỗ trợ nhưng trình bày như thể đúng.

Ví dụ: - Tạo ra một bài nghiên cứu không tồn tại. - Gán câu nói cho nhầm
tác giả. - Bịa tên hàm hoặc tham số API. - Đưa ra con số cụ thể dù không
có dữ liệu.

Ảo giác không nhất thiết là cố tình nói dối. Nó là một dạng thất bại của
quá trình tạo sinh và kiểm soát độ chính xác.

**Cách giảm rủi ro** - Cung cấp tài liệu nguồn và yêu cầu chỉ sử dụng
tài liệu đó. - Yêu cầu trích dẫn vị trí hoặc đoạn hỗ trợ cho từng kết
luận quan trọng. - Cho phép AI trả lời "không đủ dữ liệu" thay vì tự suy
đoán. - Kiểm tra các con số, tên riêng, trích dẫn, API và thông tin có
hậu quả cao. - Dùng tìm kiếm web, cơ sở dữ liệu hoặc công cụ chuyên dụng
khi cần thông tin thực tế/cập nhật.

## 5.5. Khi nào dự đoán đơn thuần là chưa đủ?

Nếu nhiệm vụ đòi hỏi dữ liệu mới nhất, phép tính chính xác, trạng thái
hệ thống hoặc thông tin trong một kho dữ liệu cụ thể, cần bổ sung công
cụ: - Web search để tìm thông tin mới. - Retrieval/RAG để truy xuất tài
liệu liên quan. - Calculator hoặc chương trình để tính toán. - API để
lấy dữ liệu có cấu trúc. - Kiểm thử phần mềm để xác nhận mã chạy đúng.

**Ý chính:** LLM rất giỏi tạo ra chuỗi phù hợp; nhưng tính hợp lý của
chuỗi không phải bằng chứng xác thực.

## 5.Câu hỏi tự kiểm tra

1.  Token khác từ như thế nào?
2.  Vì sao AI có thể bịa một trích dẫn rất thuyết phục?
3.  Nhiệm vụ nào cần kết hợp LLM với công cụ tra cứu hoặc tính toán?

------------------------------------------------------------------------

# 6. Lesson 5 -- Thử nghiệm: Next Token Prediction

Bài thực hành này nhằm giúp người học cảm nhận sự khác biệt giữa "tạo ra
điều nghe có vẻ phù hợp" và "xác minh điều đúng".

## 6.Thực hành A -- Hoàn thành mẫu quen thuộc

Yêu cầu AI: \> Viết một email lịch sự thông báo lịch họp được chuyển
sang 9 giờ sáng thứ Sáu. Giữ nội dung dưới 100 từ.

Quan sát: - AI thường xử lý tốt yêu cầu có cấu trúc quen thuộc. - Đầu ra
có thể dùng được sau khi kiểm tra thông tin cụ thể.

## 6.Thực hành B -- Kiểm tra khoảng trống dữ liệu

Yêu cầu AI: \> Hãy nêu tên ba nghiên cứu học thuật năm 2025 chứng minh
nhận định X, kèm tác giả, tạp chí và DOI.

Nếu không cung cấp nguồn, AI có thể tạo chi tiết trông giống trích dẫn
thật. Không nên xem câu trả lời là bằng chứng. Hãy yêu cầu đường dẫn đến
nguồn có thể mở và xác minh từng mục.

## 6.Thực hành C -- Thêm nguồn đáng tin cậy

Cung cấp một tài liệu hoặc đường dẫn nguồn, sau đó yêu cầu: \> Chỉ sử
dụng nội dung trong tài liệu. Với mỗi kết luận, trích dẫn đoạn hỗ trợ.
Nếu tài liệu không có thông tin, hãy ghi rõ "không tìm thấy trong
nguồn".

So sánh kết quả trước và sau khi có nguồn.

## 6.Bài học rút ra

-   Mẫu quen thuộc giúp mô hình tạo kết quả tốt.
-   Khi thiếu dữ kiện, mô hình có thể lấp chỗ trống bằng nội dung có vẻ
    hợp lý.
-   Cung cấp nguồn và yêu cầu kiểm chứng giúp giảm rủi ro, nhưng người
    dùng vẫn cần kiểm tra nguồn gốc và cách diễn giải.

------------------------------------------------------------------------

# 7. Lesson 6 -- Knowledge: Kiến thức của mô hình

## 7.1. Mô hình "biết" gì?

Kiến thức của mô hình được hình thành chủ yếu từ dữ liệu huấn luyện và
các cập nhật/nguồn bổ sung mà hệ thống được cung cấp. Mô hình có thể tái
tạo nhiều thông tin đã học, nhưng không phải một kho dữ liệu hoàn chỉnh
có thể tra cứu chính xác mọi chi tiết.

Mức độ đáng tin cậy thường không đồng đều giữa các lĩnh vực.

## 7.2. Những yếu tố ảnh hưởng đến độ bao phủ kiến thức

**Độ phổ biến** - Chủ đề phổ biến thường có nhiều ví dụ trong dữ liệu. -
Chủ đề rất hiếm hoặc chuyên biệt có thể ít được thể hiện.

**Thời điểm** - Mô hình có thể không biết sự kiện sau mốc dữ liệu huấn
luyện. - Thông tin từng đúng có thể đã lỗi thời.

**Ngôn ngữ và khu vực** - Một số ngôn ngữ, vùng địa lý hoặc cộng đồng có
ít dữ liệu hơn. - Tên địa phương, quy định nhỏ hoặc tài liệu ít phổ biến
dễ bị nhầm.

**Mức độ tranh cãi** - Chủ đề có nhiều quan điểm hoặc bằng chứng trái
chiều đòi hỏi mô hình phân biệt nguồn và lập trường. - Mô hình có thể
làm phẳng sự bất đồng thành một câu trả lời quá chắc chắn.

**Độ chính xác cần thiết** - Mô hình có thể biết một khái niệm tổng quát
nhưng không nhớ chính xác số điều khoản, phiên bản, ngày tháng hoặc
thông số.

## 7.3. Knowledge cutoff và thông tin cập nhật

Knowledge cutoff là mốc giới hạn của dữ liệu huấn luyện mà mô hình có
thể dựa vào. Mốc này không đồng nghĩa mọi thông tin trước đó đều được
biết hoặc mọi thông tin sau đó đều hoàn toàn không thể biết.

Thông tin mới có thể được đưa vào qua: - Tìm kiếm web. - Tài liệu người
dùng tải lên. - Hệ thống truy xuất dữ liệu nội bộ. - API hoặc công cụ
kết nối. - Cơ sở dữ liệu cập nhật.

Khi cần thông tin hiện tại, hãy hỏi rõ nguồn và thời điểm cập nhật;
không nên chỉ hỏi mô hình dựa vào trí nhớ.

## 7.4. Đánh giá câu hỏi trên phổ kiến thức

    Loại câu hỏi            Mức rủi ro tương đối    Cách xử lý
      Khái niệm phổ thông, ổn Thấp hơn                Có thể dùng AI để giải
  định                                            thích, vẫn kiểm tra khi
                                                  quan trọng

  Kiến thức chuyên ngành, Trung bình--cao         Cung cấp tài liệu
  ít phổ biến                                     chuyên môn

  Sự kiện mới, giá, phiên Cao nếu không có nguồn  Tìm kiếm nguồn mới và
  bản, luật hiện hành     cập nhật                kiểm tra ngày

  Số liệu hoặc trích dẫn  Cao                     Xác minh với nguồn gốc
  chính xác                                       

  Chủ đề tranh cãi        Cao                     So sánh nhiều nguồn và
                                                  nêu bất đồng
  ### 5. Chọn đúng giải pháp

-   **Web search:** Khi cần thông tin công khai mới nhất.
-   **Retrieval/RAG:** Khi câu trả lời phải dựa trên bộ tài liệu cụ thể.
-   **Tool/API:** Khi cần dữ liệu trực tiếp từ hệ thống hoặc phép xử lý
    xác định.
-   **Chuyên gia/người có thẩm quyền:** Khi cần phán đoán chuyên môn
    hoặc quyết định có hậu quả lớn.

**Ý chính:** Không hỏi "AI có biết không?" theo nghĩa chung chung. Hãy
hỏi "AI có đủ căn cứ đáng tin cậy cho đúng câu hỏi này, tại thời điểm
này không?"

## 7.Câu hỏi tự kiểm tra

1.  Vì sao mô hình có thể biết kiến thức phổ thông nhưng sai ở một chi
    tiết rất cụ thể?
2.  Knowledge cutoff khác với việc truy cập web như thế nào?
3.  Khi nào nên dùng RAG thay vì chỉ đặt câu hỏi trong chat?

------------------------------------------------------------------------

# 8. Lesson 7 -- Thử nghiệm: Knowledge

## 8.Mục tiêu

Thực hành nhận diện câu hỏi nằm gần vùng kiến thức vững hay vùng kiến
thức không chắc chắn.

## 8.Thực hành A -- Chủ đề phổ biến

Hỏi AI giải thích một khái niệm nền tảng như HTTP, vòng đời ứng dụng iOS
hoặc nguyên lý cung cầu. Yêu cầu: - Giải thích bằng ngôn ngữ đơn giản. -
Đưa ví dụ. - Nêu các trường hợp ngoại lệ hoặc giới hạn.

Đánh giá xem câu trả lời có logic, đầy đủ và nhất quán không.

## 8.Thực hành B -- Chủ đề hẹp hoặc cập nhật

Hỏi về phiên bản mới nhất của một thư viện, thay đổi chính sách gần đây
hoặc một quy định địa phương. Sau đó yêu cầu AI chỉ ra: - Thông tin nào
chắc chắn? - Thông tin nào có thể đã cũ? - Cần nguồn nào để xác minh?

Mở nguồn chính thức và đối chiếu.

## 8.Thực hành C -- Cung cấp tài liệu

Đưa một tài liệu nguồn cho AI và yêu cầu trả lời các câu hỏi chỉ dựa
trên tài liệu đó. Kiểm tra xem AI: - Có phân biệt nội dung có trong tài
liệu với kiến thức ngoài tài liệu không? - Có trích dẫn đúng không? - Có
thừa nhận khi không tìm thấy thông tin không?

## 8.Bài học rút ra

Chất lượng câu trả lời phụ thuộc cả năng lực mô hình lẫn chất lượng và
độ phù hợp của bằng chứng. Với thông tin cập nhật hoặc chuyên biệt, cần
bổ sung nguồn đáng tin cậy.

------------------------------------------------------------------------

# 9. Lesson 8 -- Working Memory: Trí nhớ làm việc và Context Window

## 9.1. Context Window là gì?

Context window (cửa sổ ngữ cảnh) là lượng token mà mô hình có thể tiếp
nhận và sử dụng trong một lượt xử lý. Có thể hình dung nó như vùng làm
việc hiện tại của AI.

Ngữ cảnh có thể bao gồm: - Chỉ dẫn hệ thống và chỉ dẫn của ứng dụng. -
Tin nhắn của người dùng. - Các lượt trao đổi trước được đưa vào ngữ
cảnh. - Tài liệu, đoạn trích hoặc dữ liệu được cung cấp. - Nội dung công
cụ trả về.

Cửa sổ ngữ cảnh có giới hạn. Khi lượng nội dung vượt giới hạn, hệ thống
có thể phải cắt, rút gọn hoặc chọn lọc thông tin theo cách triển khai cụ
thể.

## 9.2. Context window không phải trí nhớ dài hạn

Cần phân biệt: - **Ngữ cảnh hiện tại:** thông tin mô hình đang được cung
cấp để xử lý. - **Bộ nhớ dài hạn của sản phẩm:** cơ chế riêng có thể lưu
một số thông tin giữa các phiên. - **Kho dữ liệu bên ngoài:** tài liệu,
cơ sở dữ liệu hoặc hệ thống truy xuất mà AI có thể gọi đến.

Một cuộc hội thoại mới không mặc nhiên chứa toàn bộ nội dung của phiên
trước. Nếu thông tin quan trọng không được lưu hoặc cung cấp lại, mô
hình có thể không có nó trong ngữ cảnh hiện tại.

## 9.3. Vấn đề khi xử lý tài liệu dài

Ngay cả khi tài liệu nằm trong giới hạn kỹ thuật, mô hình không nhất
thiết chú ý đồng đều đến mọi phần: - Chi tiết nằm sâu giữa tài liệu có
thể bị bỏ sót. - Nhiều nội dung lặp hoặc không liên quan gây nhiễu. -
Chỉ dẫn quan trọng có thể bị chìm trong lượng văn bản lớn. - Các phần ở
xa nhau cần được kết nối có thể không được tổng hợp chính xác.

Vì vậy, "đã tải tài liệu lên" không đồng nghĩa "mọi chi tiết đều được
hiểu và sử dụng hoàn hảo".

## 9.4. Chiến lược quản lý ngữ cảnh

**Front-loading -- đưa thông tin quan trọng lên trước** - Đặt mục tiêu,
tiêu chí và các ràng buộc quan trọng ở đầu yêu cầu. - Nêu rõ điều gì
phải được ưu tiên khi có xung đột.

**Chunking -- chia nhỏ** - Chia tài liệu lớn thành chương, phần hoặc
nhóm chủ đề. - Xử lý từng phần với cùng một cấu trúc đầu ra. - Sau đó
tổng hợp các kết quả đã kiểm tra.

**Tóm tắt trung gian** - Sau mỗi phần, yêu cầu tạo bản tóm tắt có cấu
trúc. - Lưu lại kết luận, dữ kiện, thuật ngữ và câu hỏi chưa giải
quyết. - Dùng bản tóm tắt làm ngữ cảnh cho bước tiếp theo.

**Re-supplying -- cung cấp lại thông tin trọng yếu** - Lặp lại các quy
tắc, số liệu hoặc định nghĩa cần thiết ở bước quan trọng. - Không giả
định AI vẫn nhớ một chi tiết chỉ vì nó từng xuất hiện rất lâu trước đó.

**Tách nhiệm vụ** - Đừng yêu cầu đọc, phân tích, so sánh, viết và kiểm
tra hàng chục tài liệu trong một prompt duy nhất. - Chia thành các bước
có đầu vào/đầu ra rõ ràng.

## 9.5. Ví dụ: phân tích một bộ tài liệu lớn

Quy trình phù hợp: 1. Lập danh sách tài liệu và mục tiêu phân tích. 2.
Xử lý từng tài liệu hoặc từng chương. 3. Trích xuất dữ kiện theo một
schema thống nhất. 4. Ghi nguồn và vị trí của mỗi dữ kiện. 5. Tổng hợp
các bảng trích xuất. 6. Kiểm tra chéo các điểm mâu thuẫn. 7. Viết kết
luận dựa trên dữ kiện đã xác minh.

Cách này dễ kiểm soát hơn việc yêu cầu AI "đọc tất cả và viết báo cáo
ngay".

## 9.Câu hỏi tự kiểm tra

1.  Context window khác bộ nhớ dài hạn như thế nào?
2.  Vì sao tài liệu vẫn có thể bị bỏ sót dù đã nằm trong ngữ cảnh?
3.  Khi xử lý tài liệu dài, nên dùng chunking và tóm tắt trung gian ra
    sao?

------------------------------------------------------------------------

# 10. Lesson 9 -- Thử nghiệm: Working Memory

## 10.Thực hành A -- Kiểm tra khả năng giữ chi tiết

Cung cấp một đoạn văn có nhiều dữ kiện: tên người, ngày, số liệu, điều
kiện và ngoại lệ. Sau một số lượt trao đổi, hỏi lại các chi tiết ở đầu
đoạn.

Ghi nhận: - Chi tiết nào được giữ chính xác? - Chi tiết nào bị nhầm hoặc
bỏ sót? - AI có tự suy đoán khi không nhớ không?

## 10.Thực hành B -- So sánh một prompt dài và quy trình chia nhỏ

Đưa một tài liệu dài và yêu cầu phân tích toàn bộ trong một lần. Sau đó
thử: 1. Chia tài liệu thành các phần. 2. Trích xuất theo cùng một mẫu.
3. Tạo bản tóm tắt trung gian. 4. Tổng hợp ở bước cuối.

So sánh mức độ đầy đủ và khả năng truy nguyên nguồn.

## 10.Thực hành C -- Cung cấp lại ràng buộc

Trong một nhiệm vụ nhiều bước, nhắc lại các điều kiện bắt buộc ở đầu mỗi
giai đoạn. Quan sát xem việc nhắc lại có giúp giảm sai lệch không.

## 10.Bài học rút ra

Ngữ cảnh là tài nguyên hữu hạn. Muốn AI xử lý việc dài và phức tạp ổn
định hơn, cần tổ chức thông tin, chia nhỏ và giữ lại những dữ kiện quan
trọng một cách có chủ đích.

------------------------------------------------------------------------

# 11. Lesson 10 -- Steerability: Khả năng điều khiển AI bằng chỉ dẫn

## 11.1. Steerability là gì?

Steerability là mức độ mà chỉ dẫn của người dùng có thể định hướng hành
vi và đầu ra của mô hình.

Người dùng thường điều hướng AI bằng: - Vai trò hoặc góc nhìn. - Mục
tiêu nhiệm vụ. - Bối cảnh và dữ liệu. - Quy tắc và giới hạn. - Định dạng
đầu ra. - Ví dụ mẫu. - Tiêu chí đánh giá.

Tuy nhiên, không phải mọi loại chỉ dẫn đều được tuân thủ với độ ổn định
như nhau.

## 11.2. Chỉ dẫn nào thường dễ tuân thủ?

**Chỉ dẫn ngắn, cụ thể, có thể kiểm tra** - "Trả lời bằng bảng gồm 3
cột." - "Giới hạn dưới 100 từ." - "Liệt kê đúng 5 bước." - "Không thay
đổi tên biến trong đoạn mã."

Những yêu cầu này có tiêu chí quan sát được nên dễ đánh giá.

**Chỉ dẫn trừu tượng hoặc nhiều tầng** - "Hãy suy nghĩ thật sâu và không
được sai." - "Viết thật hay, đúng tinh thần thương hiệu." - "Phân tích
toàn diện mọi khía cạnh nhưng không bỏ sót gì." - Một chuỗi dài gồm
nhiều quy tắc có thể xung đột nhau.

Các yêu cầu này khó đo lường, dễ được diễn giải khác nhau và khó kiểm
tra hoàn toàn.

## 11.3. Những dạng thất bại trong điều hướng

**Instruction drift -- trôi dạt chỉ dẫn** - Mô hình bắt đầu làm đúng
nhưng dần quên hoặc bỏ qua một ràng buộc trong quá trình trả lời dài. -
Khắc phục: chia nhiệm vụ, nhắc lại tiêu chí quan trọng, dùng checklist
nghiệm thu.

**Reasoning drift -- trôi dạt trong lập luận** - Chuỗi lập luận dài có
thể chuyển hướng, dựa trên giả định chưa được kiểm chứng hoặc tạo kết
luận không còn bám sát dữ kiện ban đầu. - Khắc phục: yêu cầu nêu giả
định, chia thành bước, kiểm tra từng kết luận với dữ liệu.

**Letter versus spirit -- làm đúng chữ nhưng sai ý** - AI có thể tuân
thủ câu chữ theo nghĩa đen nhưng bỏ lỡ mục đích thực sự. - Ví dụ: yêu
cầu "rút gọn báo cáo còn một trang" nhưng mô hình xóa mất các kết luận
quan trọng chỉ để đáp ứng độ dài. - Khắc phục: nêu cả mục tiêu lẫn ràng
buộc; xác định điều gì không được hy sinh.

**Native precision -- đòi hỏi độ chính xác máy móc** - Yêu cầu chính xác
tuyệt đối về định dạng, số lượng, cấu trúc dữ liệu hoặc cú pháp có thể
không được đảm bảo chỉ bằng lời hứa trong prompt. - Khắc phục: dùng
schema, parser, validator, test hoặc chương trình để kiểm tra đầu ra.

## 11.4. Cách viết chỉ dẫn dễ điều khiển hơn

Một prompt tốt thường có cấu trúc:

1.  **Nhiệm vụ:** Cần làm gì?
2.  **Bối cảnh:** Đang giải quyết vấn đề nào?
3.  **Đầu vào:** Dữ liệu hoặc nguồn nào được phép dùng?
4.  **Quy tắc:** Điều gì phải làm/không được làm?
5.  **Đầu ra:** Cấu trúc, định dạng, độ dài.
6.  **Tiêu chí nghiệm thu:** Thế nào là kết quả đạt yêu cầu?
7.  **Xử lý thiếu dữ liệu:** Nếu không đủ căn cứ thì phải làm gì?

Ví dụ prompt chưa tốt: \> Phân tích tài liệu này thật kỹ và viết báo cáo
hay.

Prompt tốt hơn: \> Đọc tài liệu được cung cấp và viết bản phân tích gồm
4 phần: bối cảnh, dữ kiện chính, vấn đề và đề xuất. Chỉ sử dụng thông
tin trong tài liệu. Mỗi số liệu phải kèm vị trí nguồn. Không tự tạo dữ
kiện. Nếu thiếu thông tin, ghi rõ "chưa có dữ liệu". Trình bày bằng
tiếng Việt, văn phong chuyên nghiệp, tối đa 1.200 từ.

## 11.5. Dùng ví dụ và kiểm tra đầu ra

-   Cung cấp một mẫu đầu ra tốt nếu định dạng mong muốn khó mô tả.
-   Dùng bảng hoặc JSON schema khi cần cấu trúc nhất quán.
-   Yêu cầu AI tự rà soát theo checklist, nhưng không xem tự kiểm tra là
    bằng chứng độc lập.
-   Với yêu cầu quan trọng, kiểm tra bằng công cụ hoặc người có chuyên
    môn.

**Ý chính:** Prompt rõ ràng giúp điều hướng tốt hơn, nhưng prompt không
thể biến mô hình thành hệ thống đảm bảo đúng tuyệt đối.

## 11.Câu hỏi tự kiểm tra

1.  Vì sao yêu cầu "dưới 100 từ" dễ kiểm tra hơn "viết thật hay"?
2.  Phân biệt instruction drift và letter-versus-spirit.
3.  Khi nào cần validator hoặc test thay vì chỉ nhắc AI làm đúng?

------------------------------------------------------------------------

# 12. Lesson 11 -- Thử nghiệm: Steerability

## 12.Thực hành A -- Prompt mơ hồ và prompt cụ thể

Dùng cùng một nhiệm vụ, ví dụ viết mô tả sản phẩm.

**Prompt 1:** "Viết mô tả sản phẩm thật hấp dẫn."

**Prompt 2:** "Viết mô tả 120--150 từ cho tai nghe không dây dành cho
sinh viên. Giọng văn thân thiện, không phóng đại thông số. Có 1 tiêu đề,
3 lợi ích dạng bullet và 1 câu kêu gọi hành động. Chỉ dùng thông tin sản
phẩm được cung cấp."

So sánh mức độ phù hợp và khả năng nghiệm thu.

## 12.Thực hành B -- Kiểm tra xung đột chỉ dẫn

Yêu cầu AI vừa "giải thích thật chi tiết" vừa "chỉ dùng tối đa 20 từ".
Quan sát cách AI giải quyết xung đột. Sau đó sửa prompt, xác định ưu
tiên: \> Ưu tiên giới hạn 20 từ; chỉ nêu 3 ý quan trọng nhất.

## 12.Thực hành C -- Kiểm tra định dạng

Yêu cầu AI trả về JSON theo một cấu trúc cụ thể. Sau đó dùng JSON parser
hoặc validator để kiểm tra cú pháp và trường bắt buộc. Không chỉ nhìn
bằng mắt.

## 12.Bài học rút ra

Chỉ dẫn tốt phải cụ thể, không mâu thuẫn và có tiêu chí kiểm tra. Với
yêu cầu máy móc, hãy dùng công cụ kiểm định thay vì phụ thuộc hoàn toàn
vào khả năng tự tuân thủ của AI.

------------------------------------------------------------------------

# 13. Lesson 12 -- Khi các thuộc tính va chạm: Chẩn đoán lỗi thực tế

Trong công việc, một nhiệm vụ thường tác động đến nhiều thuộc tính cùng
lúc. Không nên quy mọi lỗi cho "AI bị ảo giác" hoặc "prompt chưa tốt".
Cần xác định thuộc tính nào đang gây giới hạn và chọn cách sửa tương
ứng.

## 13.1. Các tình huống kết hợp thường gặp

**Tình huống A -- Tóm tắt một tài liệu rất dài** - Working Memory: lượng
nội dung lớn, chi tiết có thể bị bỏ sót. - Knowledge: tài liệu có thể
chứa thuật ngữ hoặc thông tin chuyên ngành. - Steerability: yêu cầu "tóm
tắt đầy đủ" có thể mơ hồ. - Next Token Prediction: khi thiếu chi tiết,
mô hình có thể tạo câu nối hợp lý nhưng không có trong nguồn.

**Cách xử lý:** Chia tài liệu theo phần, yêu cầu trích xuất theo schema,
gắn nguồn cho từng ý, tổng hợp sau khi kiểm tra.

**Tình huống B -- Hỏi về quy định mới nhất** - Knowledge: mô hình có thể
không biết thay đổi mới. - Next Token Prediction: có thể tạo câu trả lời
nghe giống văn bản pháp luật. - Steerability: câu hỏi có thể chưa xác
định quốc gia, thời điểm hoặc phạm vi. - Working Memory: nhiều văn bản
và ngoại lệ có thể gây quá tải.

**Cách xử lý:** Xác định phạm vi và ngày hiệu lực, tìm văn bản chính
thức, trích dẫn điều khoản, kiểm tra phiên bản và nhờ chuyên gia khi
cần.

**Tình huống C -- Giao AI xây dựng tính năng phần mềm** - Knowledge: mô
hình có thể nhầm phiên bản framework hoặc API. - Working Memory: có thể
bỏ sót yêu cầu trong tài liệu dài. - Steerability: yêu cầu tổng quát có
thể thiếu tiêu chí nghiệm thu. - Next Token Prediction: mã có thể trông
đúng nhưng không chạy được.

**Cách xử lý:** Cung cấp repository và phiên bản, chia nhỏ task, yêu cầu
nêu file thay đổi, chạy test/linter/build, kiểm tra kết quả thực tế.

**Tình huống D -- AI đồng tình với kết luận sai** - Knowledge: dữ kiện
có thể thiếu. - Next Token Prediction: mô hình tạo câu tiếp nối phù hợp
với tiền đề. - Steerability: cách đặt câu hỏi dẫn dắt câu trả lời. - Dấu
vân tay fine-tuning: xu hướng sycophancy có thể góp phần.

**Cách xử lý:** Yêu cầu phản biện, nêu giả định, đưa bằng chứng ủng hộ
và phản đối, tìm nguồn độc lập.

## 13.2. Quy trình chẩn đoán 5 bước

Khi nhận được kết quả bất ngờ:

**Bước 1 -- Mô tả lỗi cụ thể** - Sai dữ kiện? - Thiếu nội dung? - Sai
định dạng? - Không làm đúng yêu cầu? - Tự tạo thông tin?

**Bước 2 -- Xác định thuộc tính liên quan** - Nội dung được tạo ra có vẻ
hợp lý nhưng không đúng? → xem xét Next Token Prediction. - Thông tin
ngoài phạm vi hoặc đã lỗi thời? → xem xét Knowledge. - Bỏ sót dữ kiện
trong tài liệu/cuộc hội thoại dài? → xem xét Working Memory. - Làm sai
hoặc bỏ qua chỉ dẫn? → xem xét Steerability.

**Bước 3 -- Kiểm tra các thuộc tính cùng tác động** Một lỗi có thể có
nhiều nguyên nhân. Ví dụ, mô hình bịa chi tiết vì vừa thiếu kiến thức
vừa bị yêu cầu trả lời dù không có đủ nguồn.

**Bước 4 -- Chọn biện pháp đúng mục tiêu** - Thiếu nguồn → bổ sung nguồn
hoặc truy xuất. - Quá dài → chia nhỏ và tóm tắt. - Mơ hồ → viết lại chỉ
dẫn và tiêu chí. - Cần chính xác → dùng công cụ xác minh. - Có thiên
hướng đồng tình → yêu cầu phản biện và kiểm tra độc lập.

**Bước 5 -- Kiểm tra lại** Không chỉ hỏi lại cùng một prompt. Hãy xác
nhận lỗi đã được xử lý bằng bằng chứng hoặc tiêu chí nghiệm thu.

## 13.3. Bảng chẩn đoán nhanh

    Dấu hiệu                Thuộc tính cần xem xét              Cách khắc phục ưu tiên
      Câu trả lời trôi chảy   Next Token Prediction               Nguồn, trích dẫn, xác
  nhưng bịa dữ kiện                                           minh

  Thông tin cũ hoặc không Knowledge                           Web/API/nguồn cập nhật
  biết sự kiện mới                                            

  Bỏ sót chi tiết trong   Working Memory                      Chunking, tóm tắt, cung
  tài liệu dài                                                cấp lại

  Không tuân thủ định     Steerability                        Prompt cụ thể, schema,
  dạng hoặc ràng buộc                                         validator

  Đồng tình với giả định  Sycophancy/Steerability/Knowledge   Phản biện, bằng chứng
  sai                                                         đối lập

  Mã nhìn đúng nhưng      Prediction/Knowledge/Steerability   Build, test, lint, chạy
  không chạy                                                  thực tế
  ### 4. Nguyên tắc tin cậy có hiệu chuẩn

Không nên áp dụng một mức độ tin tưởng cho mọi nhiệm vụ. Hãy cân nhắc: -
Mức độ phổ biến và ổn định của kiến thức. - Mức độ đầy đủ của dữ liệu
đầu vào. - Độ dài và độ phức tạp của ngữ cảnh. - Mức độ cụ thể của yêu
cầu. - Hậu quả nếu câu trả lời sai. - Có thể xác minh bằng nguồn hoặc
công cụ độc lập hay không.

Nhiệm vụ rủi ro thấp như gợi ý tiêu đề có thể chấp nhận đầu ra để tham
khảo. Nhiệm vụ rủi ro cao như pháp lý, y tế, tài chính, bảo mật hoặc
quyết định quan trọng cần nguồn đáng tin cậy và người có thẩm quyền kiểm
tra.

------------------------------------------------------------------------

# 14. Lesson 13 -- Chuyển kiến thức thành thói quen

Khi giao việc cho AI, hãy tự hỏi:

1.  **Delegation:** Có nên giao nhiệm vụ này cho AI không? Phần nào cần
    con người quyết định?
2.  **Description:** Tôi đã cung cấp đủ bối cảnh, dữ liệu, quy tắc và
    tiêu chí chưa?
3.  **Discernment:** Kết quả có căn cứ không? Có điều gì cần xác minh?
4.  **Diligence:** Tôi đã kiểm tra đầu ra và chịu trách nhiệm với cách
    sử dụng nó chưa?

Đồng thời, kiểm tra bốn thuộc tính: - Next Token Prediction: AI có đang
tạo nội dung nghe hợp lý nhưng thiếu căn cứ không? - Knowledge: Thông
tin có nằm trong phạm vi và thời điểm mô hình biết không? - Working
Memory: Mọi dữ kiện quan trọng có nằm trong ngữ cảnh hiện tại không? -
Steerability: Chỉ dẫn có cụ thể, nhất quán và kiểm tra được không?

------------------------------------------------------------------------

# 15. Checklist và Bản đồ chẩn đoán lỗi

## 15.1. Checklist trước khi sử dụng kết quả AI

-   [ ] Đã xác định rõ mục tiêu và phạm vi công việc.
-   [ ] Đã cung cấp nguồn dữ liệu cần thiết.
-   [ ] Đã nêu các ràng buộc quan trọng.
-   [ ] Đã chia nhỏ nhiệm vụ nếu tài liệu hoặc quy trình quá dài.
-   [ ] Đã kiểm tra dữ kiện, con số, tên riêng và trích dẫn.
-   [ ] Đã kiểm tra phiên bản/thời điểm đối với thông tin có thể thay
    đổi.
-   [ ] Đã chạy test hoặc validator nếu đầu ra là mã/ dữ liệu có cấu
    trúc.
-   [ ] Đã xem xét rủi ro và tác động trước khi áp dụng.
-   [ ] Có người chịu trách nhiệm phê duyệt kết quả cuối cùng.

## 15.2. Bản đồ tổng quát về 4 thuộc tính và khung 4D

  -----------------------------------------------------------------------
  Thành phần        Câu hỏi cần đặt   Rủi ro điển hình  Biện pháp
                    ra                                  
  ----------------- ----------------- ----------------- -----------------
  Next Token        Vì sao AI tạo ra  Nội dung nghe hợp Kiểm chứng,
  Prediction        câu trả lời này?  lý nhưng sai      nguồn, công cụ

  Knowledge         AI có đủ kiến     Thiếu, lỗi thời,  Web, RAG, tài
                    thức đúng và mới  thiên lệch dữ     liệu chính thức
                    không?            liệu              

  Working Memory    AI có đang có đủ  Quá tải, bỏ sót,  Chia nhỏ, tóm
                    thông tin để xử   mất liên kết      tắt, nhắc lại
                    lý không?                           

  Steerability      Chỉ dẫn có đủ rõ  Trôi dạt, hiểu    Prompt rõ,
                    và khả thi không? sai mục tiêu      schema, checklist

  4D Framework      Con người cần     Giao việc sai,    Ủy quyền, mô tả,
                    phối hợp và chịu  đánh giá thiếu,   nhận định, chăm
                    trách nhiệm ra    sử dụng thiếu     chỉ
                    sao?              trách nhiệm       
  ------------------------------------------------------------------------

# 16. Tổng kết và câu hỏi ôn tập

## 16.1. Những kiến thức quan trọng nhất

### Ôn tập bốn thuộc tính cốt lõi trong một phút

**Next Token Prediction:** AI tạo câu trả lời bằng cách dự đoán token
tiếp theo. Điều này giải thích khả năng tạo nội dung trôi chảy và nguy
cơ hallucination.

**Knowledge:** Kiến thức phụ thuộc vào dữ liệu huấn luyện và nguồn bổ
sung. Kiến thức không đồng đều, có thể thiếu, sai hoặc lỗi thời.

**Working Memory:** Context window là vùng thông tin mô hình có thể xử
lý tại thời điểm hiện tại. Nó hữu hạn và không tương đương bộ nhớ dài
hạn.

**Steerability:** Chỉ dẫn giúp định hướng đầu ra nhưng hiệu quả không
đồng đều. Yêu cầu cụ thể, nhất quán và có thể kiểm tra thường dễ tuân
thủ hơn.

### Thông điệp kết luận

Thông điệp quan trọng nhất của khóa học là: **AI không phải một công cụ
có năng lực đồng đều ở mọi nhiệm vụ.** Nó có những vùng hoạt động tốt và
những giới hạn có thể dự đoán phần nào nếu hiểu cơ chế vận hành.

Khi kết quả không như mong đợi, thay vì chỉ viết lại prompt hoặc yêu cầu
AI thử lần nữa, hãy chẩn đoán: 1. Vấn đề nằm ở cơ chế tạo nội dung, phạm
vi kiến thức, lượng ngữ cảnh hay khả năng tuân thủ chỉ dẫn? 2. Có nhiều
thuộc tính nào đang cùng gây ra lỗi? 3. Biện pháp sửa đúng mục tiêu là
bổ sung nguồn, chia nhỏ, làm rõ chỉ dẫn hay dùng công cụ xác minh?

Sử dụng AI hiệu quả không có nghĩa là giao mọi việc cho AI hoặc nghi ngờ
mọi câu trả lời. Đó là **đặt đúng nhiệm vụ, cung cấp đúng bối cảnh, kiểm
tra đúng mức và giữ trách nhiệm của con người đối với kết quả cuối
cùng.**

------------------------------------------------------------------------

## 16.2. Bảng thuật ngữ cần nhớ

-   **AI (Artificial Intelligence):** Trí tuệ nhân tạo.
-   **Generative AI:** AI tạo sinh, tạo nội dung mới như văn bản, ảnh,
    âm thanh hoặc mã.
-   **LLM (Large Language Model):** Mô hình ngôn ngữ lớn.
-   **Token:** Đơn vị văn bản mà mô hình xử lý.
-   **Next Token Prediction:** Dự đoán token tiếp theo.
-   **Pre-training:** Tiền huấn luyện trên lượng dữ liệu lớn để học mẫu
    và biểu diễn.
-   **Fine-tuning:** Tinh chỉnh mô hình cho hành vi hoặc mục tiêu sử
    dụng cụ thể.
-   **Hallucination:** Tạo thông tin sai hoặc không có căn cứ nhưng
    trình bày như đúng.
-   **Sycophancy:** Xu hướng đồng tình/chiều theo người dùng.
-   **Verbosity:** Xu hướng trả lời dài dòng.
-   **Over-caution:** Quá thận trọng hoặc từ chối quá mức.
-   **Confidence calibration:** Mức độ tương xứng giữa độ tự tin thể
    hiện và độ chắc chắn thực tế.
-   **Knowledge cutoff:** Mốc giới hạn của dữ liệu huấn luyện.
-   **Context window:** Cửa sổ ngữ cảnh -- lượng token có thể được đưa
    vào xử lý.
-   **Working Memory:** Trí nhớ làm việc -- thông tin mô hình có thể sử
    dụng trong ngữ cảnh hiện tại.
-   **Chunking:** Chia tài liệu/nhiệm vụ thành các phần nhỏ.
-   **Retrieval:** Truy xuất thông tin từ nguồn bên ngoài.
-   **RAG (Retrieval-Augmented Generation):** Tạo sinh có tăng cường
    truy xuất.
-   **Steerability:** Khả năng điều hướng mô hình bằng chỉ dẫn.
-   **Instruction drift:** Sự trôi dạt hoặc mất dần việc tuân thủ chỉ
    dẫn.
-   **Reasoning drift:** Sự trôi dạt trong chuỗi lập luận.
-   **Validator:** Công cụ kiểm tra đầu ra theo quy tắc hoặc schema.
-   **4D Framework:** Delegation, Description, Discernment, Diligence.

------------------------------------------------------------------------

## 16.3. Câu hỏi tự kiểm tra

1.  Vì sao LLM có thể tạo câu trả lời rất tự nhiên nhưng không chính
    xác?
2.  Phân biệt classification, prediction và generative AI.
3.  Pre-training và fine-tuning đóng vai trò gì?
4.  Sycophancy, verbosity và over-caution là những xu hướng hành vi nào?
5.  Token là gì và mô hình tạo câu trả lời theo cơ chế nào?
6.  Hallucination là gì? Nêu ba cách giảm rủi ro.
7.  Knowledge cutoff có ý nghĩa gì? Khi cần dữ liệu mới nên làm gì?
8.  Context window là gì? Vì sao không đồng nghĩa với bộ nhớ dài hạn?
9.  Nêu ba chiến lược xử lý tài liệu dài.
10. Vì sao chỉ dẫn cụ thể dễ kiểm tra hơn chỉ dẫn trừu tượng?
11. Phân biệt instruction drift và letter-versus-spirit.
12. Khi AI tạo mã, vì sao cần chạy test thay vì chỉ đọc mã?
13. Hãy chẩn đoán một trường hợp AI bỏ sót thông tin trong báo cáo dài.
14. Hãy chẩn đoán một trường hợp AI bịa nguồn tham khảo.
15. Khung 4D giúp con người phối hợp với bốn thuộc tính của AI như thế
    nào?

------------------------------------------------------------------------

## Nguồn tham khảo

### Tài liệu khóa học

- [Claude Academy -- AI Capabilities and Limitations](https://academy.claude.com/courses/ai-capabilities-and-limitations)
- [Claude Academy -- AI Fluency: Framework & Foundations](https://academy.claude.com/courses/ai-fluency-framework-foundations)

### Tài liệu bổ sung

- [Anthropic Research & Model Card](https://www.anthropic.com/research)

> **Ghi chú về phạm vi:** Tài liệu này tổng hợp 13 bài học chính thức của khóa học, kết hợp giải thích lý thuyết nền tảng và bài tập thực hành. Tên bài học giữ theo cấu trúc khóa học; nội dung được diễn giải để hỗ trợ tự học, không phải bản transcript nguyên văn video.


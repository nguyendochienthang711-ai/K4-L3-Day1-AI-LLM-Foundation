# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Ở temperature 0.0, câu trả lời thường ổn định, ít biến thể và bám sát cách diễn đạt an toàn. Khi tăng lên 0.5, 1.0 và 1.5, lựa chọn từ ngữ, ví dụ và chi tiết trở nên đa dạng, sáng tạo hơn. Mức cao cũng làm tăng khả năng câu trả lời lan man hoặc có chi tiết kém nhất quán.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi chọn temperature khoảng 0.2–0.4 cho chatbot hỗ trợ khách hàng. Mức này giúp câu trả lời nhất quán, chính xác, đúng chính sách và vẫn đủ tự nhiên để không quá máy móc; các trường hợp cần sáng tạo như viết nội dung marketing nên dùng mức cao hơn.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Workload có 10.000 × 3 × 350 = 10,5 triệu token đầu ra mỗi ngày. Theo bảng giá trong bài, GPT-4o có giá output 0,010 USD/1K token, còn GPT-4o-mini là 0,0006 USD/1K token, nên GPT-4o đắt hơn khoảng 16,7 lần. GPT-4o đáng chi khi cần suy luận/phân tích phức tạp hoặc câu trả lời chất lượng cao; mini phù hợp cho FAQ, phân loại, tóm tắt đơn giản và lưu lượng lớn.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Persona giáo viên tiểu học thường dùng câu ngắn, từ quen thuộc và ví dụ đời thường như “cuốn sổ dùng chung”, nên dễ hiểu cho trẻ 8 tuổi. Persona chuyên gia tài chính có xu hướng dài và chính xác hơn, dùng các từ như sổ cái phân tán, cơ chế đồng thuận, bất biến dữ liệu và có thể nêu rủi ro hoặc ứng dụng tài chính. System prompt đặt ưu tiên về đối tượng người đọc, giọng điệu, mức độ chi tiết và kiểu ví dụ, nên cùng một câu hỏi có thể sinh ra câu trả lời rất khác. Nó là hướng dẫn cấp cao định hình hành vi model trong cả lượt chat.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Với một đoạn tiếng Việt khoảng 100 từ, ước lượng theo số từ/0,75 là khoảng 133 token; kết quả tiktoken có thể cao hơn, chẳng hạn khoảng 160 token, tức chênh khoảng 20%. Con số thực tế thay đổi theo dấu câu, tên riêng và cách viết. Tiếng Việt có dấu, nhiều âm tiết tách bằng khoảng trắng và các chuỗi ký tự không phải lúc nào cũng khớp token phổ biến như tiếng Anh, vì vậy bộ mã hóa thường phải tách thành nhiều token hơn.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất với câu trả lời dài hoặc tác vụ hội thoại tương tác, vì người dùng thấy phản hồi bắt đầu ngay thay vì chờ toàn bộ kết quả, nhờ đó cảm nhận độ trễ thấp hơn và có thể dừng sớm nếu không cần nữa. Non-streaming phù hợp khi ứng dụng chỉ cần dữ liệu hoàn chỉnh để xử lý tiếp, ví dụ parse JSON có cấu trúc, lưu vào cơ sở dữ liệu, hoặc tác vụ chạy nền không có giao diện chờ trực tiếp.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff tạo khoảng nghỉ tăng dần, cho API đang quá tải thời gian hồi phục và giảm số yêu cầu thất bại tiếp tục đổ vào hệ thống. Nếu hàng nghìn client đều chờ cố định 1 giây rồi retry cùng lúc, chúng tạo các đợt “retry storm”, làm nghẽn lại dịch vụ ngay khi dịch vụ vừa phục hồi. Trong hệ thống thực tế nên bổ sung jitter ngẫu nhiên để các lần retry còn được phân tán hơn.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> System prompt tôi chọn: “Bạn là trợ giảng AI thân thiện cho người mới học. Trả lời bằng tiếng Việt rõ ràng, ngắn gọn, giải thích từng bước khi cần và nêu rõ khi không chắc chắn.” Cụm “cho người mới học” khiến mức giải thích phù hợp thay vì quá nhiều thuật ngữ; “ngắn gọn” giúp giảm lan man và chi phí token. Yêu cầu tiếng Việt bảo đảm trải nghiệm nhất quán cho đối tượng của khóa học.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn là history chỉ giữ ba lượt gần nhất nên trợ lý nhanh chóng quên mục tiêu, thông tin hoặc quyết định từ đầu phiên; đồng thời chưa có bộ nhớ dài hạn. Một cải tiến cụ thể là lưu các fact/ưu tiên đã được người dùng xác nhận vào cơ sở dữ liệu theo phiên hoặc người dùng, rồi truy xuất các mục liên quan và chèn một bản tóm tắt ngắn vào system/context trước mỗi lần gọi API. Cần giới hạn token, cho người dùng xem/xóa bộ nhớ và chỉ lưu dữ liệu khi có sự đồng ý.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026

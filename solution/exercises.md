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
> *Khi tăng temperature từ 0.0 lên 1.5, phản hồi của model có xu hướng đa dạng và sáng tạo hơn. Ở temperature thấp, câu trả lời thường ổn định, trực tiếp và ít thay đổi; khi temperature cao hơn, cách diễn đạt và lựa chọn thông tin có thể phong phú và phức tạp hơn, đồng thời khả năng xuất hiện những chi tiết không cần thiết cũng tăng lên.*

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> *Tôi sẽ chọn temperature khoảng 0.2–0.4 cho chatbot hỗ trợ khách hàng. Mức temperature thấp giúp câu trả lời ổn định, nhất quán và ít tạo ra thông tin không chính xác hoặc không cần thiết. Đối với chatbot chăm sóc khách hàng, tính chính xác và khả năng tuân thủ thông tin được cung cấp quan trọng hơn sự sáng tạo.*

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> *Với cùng một lượng token sử dụng, GPT-4o có chi phí cao hơn đáng kể so với GPT-4o-mini. Với số lượng lớn như 10.000 người dùng mỗi ngày và mỗi người gọi API 3 lần, việc sử dụng model lớn cho tất cả các yêu cầu sẽ làm chi phí tăng nhanh. GPT-4o phù hợp với các yêu cầu phức tạp cần khả năng suy luận và chất lượng cao, trong khi GPT-4o-mini phù hợp với các tác vụ đơn giản, số lượng lớn như phân loại, trả lời FAQ hoặc xử lý văn bản cơ bản.*

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> *Hai system prompt tạo ra hai phong cách trả lời rất khác nhau dù câu hỏi giống nhau. Với giáo viên tiểu học, câu trả lời thường đơn giản hơn, sử dụng từ ngữ dễ hiểu và các ví dụ gần gũi với trẻ em. Với chuyên gia tài chính, model có xu hướng sử dụng nhiều thuật ngữ chuyên môn hơn, giải thích sâu hơn và có thể đề cập đến các khái niệm như cơ chế đồng thuận, sổ cái phân tán hoặc mật mã học. Điều này cho thấy system prompt có ảnh hưởng rõ rệt đến cách model lựa chọn từ vựng, mức độ chi tiết, cấu trúc và phong cách của câu trả lời.*

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> *. Nguyên nhân là token không tương ứng trực tiếp với một từ hoàn chỉnh mà có thể là một phần của từ hoặc một chuỗi ký tự. Tiếng Việt thường cần nhiều token hơn tiếng Anh vì cách mã hóa của tokenizer không phải lúc nào cũng biểu diễn hiệu quả các từ tiếng Việt có dấu, đặc biệt khi kết hợp nhiều ký tự Unicode. Vì vậy, công thức dựa trên số từ chỉ mang tính ước lượng và không phản ánh chính xác số token thực tế.*

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> *Streaming đặc biệt quan trọng đối với các ứng dụng mà model tạo ra câu trả lời dài, chẳng hạn như chatbot, trợ lý AI hoặc công cụ viết nội dung, vì người dùng có thể nhìn thấy kết quả từng phần ngay khi model đang sinh dữ liệu thay vì phải chờ toàn bộ phản hồi. Điều này làm giảm cảm giác phải chờ đợi và cải thiện trải nghiệm tương tác. Ngược lại, non-streaming phù hợp hơn với các tác vụ mà ứng dụng cần toàn bộ kết quả trước khi xử lý tiếp, chẳng hạn như phân tích dữ liệu, trả về JSON hoàn chỉnh hoặc thực hiện một bước xử lý tự động phía sau.*

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> *Exponential backoff giúp các client không gửi lại request quá nhanh khi API đang bị quá tải. Thay vì tất cả client cùng retry sau đúng 1 giây, thời gian chờ sẽ tăng dần qua các lần retry, giúp giảm áp lực lên server và tạo cơ hội để hệ thống phục hồi. Nếu hàng nghìn client cùng retry với delay cố định, chúng có thể gửi request lại gần như đồng thời, tạo ra một đợt tải mới ngay sau mỗi lần retry và khiến tình trạng quá tải kéo dài.*

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> *Prompt: "Bạn là một trợ lý học tập lập trình. Hãy giải thích các khái niệm bằng tiếng Việt, ưu tiên cách giải thích đơn giản và trực quan. Khi người dùng gặp lỗi code, hãy chỉ ra nguyên nhân, giải thích vì sao xảy ra lỗi và đưa ra cách sửa. Không chỉ đưa đáp án mà hãy giúp người học hiểu cách giải quyết vấn đề. Trả lời ngắn gọn nhưng đầy đủ và sử dụng ví dụ code khi cần thiết". Tôi yêu cầu giải thích bằng tiếng Việt để phù hợp với người học và giúp việc tiếp thu kiến thức dễ dàng hơn. Tôi cũng yêu cầu không chỉ đưa đáp án mà phải giải thích nguyên nhân, vì mục tiêu của trợ lý là hỗ trợ quá trình học chứ không chỉ hoàn thành bài tập thay cho người dùng. Cụm từ "ngắn gọn nhưng đầy đủ" giúp hạn chế những câu trả lời quá dài nhưng vẫn giữ được các thông tin quan trọng.*

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> *Hạn chế lớn nhất của trợ lý hiện tại là khả năng ghi nhớ ngữ cảnh còn hạn chế. Nếu lịch sử hội thoại chỉ giữ lại một số lượt gần nhất, trợ lý có thể quên những thông tin người dùng đã cung cấp ở các lượt trước. Một cải thiện cụ thể là xây dựng cơ chế lưu trữ lịch sử dài hạn bằng cách lưu các thông tin quan trọng vào cơ sở dữ liệu hoặc vector database, sau đó tìm kiếm và đưa những thông tin liên quan vào context trước mỗi lần gọi model. Cách này giúp trợ lý duy trì được thông tin cần thiết mà không phải gửi toàn bộ lịch sử hội thoại trong mỗi request.*

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026

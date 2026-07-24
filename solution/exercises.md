# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature bằng 0.0, câu trả lời mang tính xác định cao, rập khuôn và sẽ lặp lại nội dung y hệt nếu gọi nhiều lần. Khi tăng lên 0.5 và 1.0, mô hình dùng từ ngữ linh hoạt hơn, cung cấp các góc nhìn tự nhiên và sáng tạo hơn. Ở mức 1.5, mô hình bắt đầu mất kiểm soát, sinh ra nội dung không mạch lạc, lộn xộn hoặc xuất hiện tình trạng "ảo giác" (hallucination).

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Nên đặt temperature ở mức thấp (khoảng 0.0 đến 0.2). Đối với tác vụ hỗ trợ khách hàng, sự chính xác, nhất quán và tuân thủ chặt chẽ thông tin sản phẩm là ưu tiên hàng đầu. Việc giới hạn sự "sáng tạo" sẽ giúp chatbot không tự bịa ra các chính sách, tính năng hay mức giá sai lệch gây ảnh hưởng đến doanh nghiệp.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Theo bảng giá hiện tại, chi phí output của GPT-4o thường đắt hơn GPT-4o-mini khoảng từ 15 đến 25 lần (tùy thời điểm). Trường hợp dùng GPT-4o: Xứng đáng khi cần giải quyết các bài toán suy luận logic phức tạp, viết code chuyên sâu, hoặc phân tích dữ liệu đa phương thức. Trường hợp dùng GPT-4o-mini: Rất phù hợp cho các tác vụ nhẹ nhàng, khối lượng lớn (high volume) như phân loại văn bản, tóm tắt tin nhắn, hoặc giao tiếp thông thường với người dùng.
---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với persona "giáo viên tiểu học", câu trả lời rất ngắn gọn, dùng câu đơn, từ vựng dễ hiểu và ví dụ đời thường (như cuốn sổ liên lạc chung của lớp). Ngược lại, persona "chuyên gia tài chính" sinh ra văn bản dài hơn, sử dụng cấu trúc câu phức và nhiều thuật ngữ chuyên ngành (phi tập trung, mã hóa thuật toán, sổ cái phân tán). Điều này cho thấy System prompt hoạt động như một bộ lọc định hướng, giúp mô hình thu hẹp không gian phân phối xác suất từ vựng để đóng đúng vai trò được giao.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Số token thực tế đo bằng tiktoken thường cao hơn khoảng 1.5 đến 2.5 lần (150% - 250%) so với ước lượng thô bằng cách đếm từ. Tiếng Việt tốn nhiều token hơn vì các mô hình LLM được huấn luyện chủ yếu trên kho dữ liệu tiếng Anh, nên bộ tokenizer tối ưu rất tốt cho tiếng Anh (1 từ = 1 token). Trong khi đó, với tiếng Việt, các từ ghép hoặc ký tự có dấu thường bị băm nhỏ thành nhiều sub-word hoặc byte lẻ, làm số lượng token tăng vọt.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming cực kỳ quan trọng trong các ứng dụng chat giao diện người dùng (như ChatGPT), giúp giảm thiểu "thời gian chờ đợi nhận thức" (perceived latency) vì người dùng có thể đọc ngay những từ đầu tiên trong lúc model đang tiếp tục sinh ra câu trả lời. Ngược lại, non-streaming phù hợp hơn cho các tác vụ xử lý nền (background jobs), Pipeline xử lý dữ liệu hàng loạt, hoặc khi bạn yêu cầu model trả về một cấu trúc dữ liệu nghiêm ngặt (như JSON) cần được validate (kiểm tra tính hợp lệ) toàn bộ trước khi đẩy sang hệ thống khác.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp dãn cách thời gian giữa các lần thử, cho phép máy chủ có đủ thời gian để phục hồi khi bị quá tải. Nếu hàng nghìn client cùng retry với một khoảng delay cố định, hệ thống sẽ gặp phải hiệu ứng "Thundering Herd" (đàn gia súc giẫm đạp) — tất cả các request sẽ đồng loạt dội ngược trở lại server cùng một tíc tắc, khiến hệ thống tiếp tục sập và không bao giờ thoát khỏi trạng thái sụp đổ.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona: "Bạn là một chuyên gia khoa học dữ liệu và thiết kế bài giảng. Hãy giúp tôi tạo cấu trúc và nội dung cho các slide thuyết trình, đặc biệt tập trung vào kỹ thuật xử lý dữ liệu thiếu (NaN) và kiểm tra điều kiện bằng NumPy trong Python. Trả lời ngắn gọn bằng tiếng Việt dưới dạng gạch đầu dòng.". Giải thích: Yêu cầu "trả lời ngắn gọn dạng gạch đầu dòng" giúp khống chế số lượng token đầu ra, tiết kiệm chi phí và tạo định dạng nội dung sẵn sàng để dán thẳng vào slide. Việc chỉ định rõ trọng tâm "xử lý dữ liệu thiếu và kiểm tra điều kiện bằng NumPy" sẽ neo ngữ cảnh, ép mô hình tập trung cung cấp code mẫu và giải pháp chuyên biệt thay vì lan man sang các thư viện khác (như Pandas).

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là "bộ nhớ rất ngắn" (chỉ lưu 3 lượt hội thoại gần nhất). Nếu phiên chat kéo dài, chatbot sẽ quên mất các yêu cầu cốt lõi ở đầu phiên. Đề xuất cải thiện: Triển khai kỹ thuật "Tóm tắt ngữ cảnh" (Context Summarization). Khi lịch sử hội thoại đạt đến giới hạn 6 tin nhắn, thay vì cắt bỏ hoàn toàn các tin nhắn cũ, ta gọi một luồng API nền để tóm tắt lịch sử cũ thành 1-2 câu trọng tâm, sau đó nối phần tóm tắt này vào tin nhắn system prompt đầu tiên để bảo toàn mạch suy nghĩ dài hạn.

---

## Danh Sách Kiểm Tra Nộp Bài

- [x] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [x] Cả 4 checkpoint pytest đều pass
- [x] Tất cả 9 câu trong file này đã được trả lời
- [x] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README

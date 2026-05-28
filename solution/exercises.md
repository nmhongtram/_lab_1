# Ngày 1 — Bài Tập & Phản Ánh
## Nền Tảng LLM API | Phiếu Thực Hành

**Thời lượng:** 1:30 giờ  
**Cấu trúc:** Lập trình cốt lõi (60 phút) → Bài tập mở rộng (30 phút)

---

## Phần 1 — Lập Trình Cốt Lõi (0:00–1:00)

Chạy các ví dụ trong Google Colab tại: https://colab.research.google.com/drive/172zCiXpLr1FEXMRCAbmZoqTrKiSkUERm?usp=sharing

Triển khai tất cả TODO trong `template.py`. Chạy `pytest tests/` để kiểm tra tiến độ.

**Điểm kiểm tra:** Sau khi hoàn thành 4 nhiệm vụ, chạy:
```bash
python template.py
```
Bạn sẽ thấy output so sánh phản hồi của GPT-4o và GPT-4o-mini.

---

## Phần 2 — Bài Tập Mở Rộng (1:00–1:30)

### Bài tập 2.1 — Độ Nhạy Của Temperature
Gọi `call_openai` với các giá trị temperature 0.0, 0.5, 1.0 và 1.5 sử dụng prompt **"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)

Khi temperature tăng từ 0.0 lên 1.5, các phản hồi dần trở nên đa dạng và sáng tạo hơn. Ở temperature 0.0, mô hình luôn đưa ra cùng một câu trả lời xác định với từ ngữ cứng nhắc và an toàn; ở 0.5 và 1.0, câu trả lời phong phú hơn về cách diễn đạt nhưng vẫn chính xác về nội dung; còn ở 1.5, phản hồi có thể bắt đầu xuất hiện những chi tiết bất ngờ hoặc đôi khi kém nhất quán. Quy luật chung là temperature càng cao thì độ ngẫu nhiên và tính sáng tạo càng tăng, nhưng đồng thời rủi ro về độ chính xác cũng lớn hơn.

**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**

Đặt temperature ở mức 0.2 – 0.3 cho chatbot hỗ trợ khách hàng, vì mục tiêu ưu tiên ở đây là tính nhất quán, chính xác và đáng tin cậy, khách hàng cần nhận được câu trả lời đúng về chính sách, đơn hàng hay hướng dẫn sản phẩm, không phải câu trả lời sáng tạo hay bất ngờ. Temperature thấp giúp mô hình bám sát thông tin thực tế, giảm thiểu rủi ro đưa ra thông tin sai lệch, đồng thời vẫn cho phép một chút linh hoạt trong cách diễn đạt để không nghe quá máy móc.

---

### Bài tập 2.2 — Đánh Đổi Chi Phí
Xem xét kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người thực hiện 3 lần gọi API, mỗi lần trung bình ~350 token.

**Ước tính xem GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này:**

Với 10.000 người dùng × 3 lần gọi × 350 token = 10.500.000 token output/ngày. Chi phí GPT-4o: 10.500 × $0,010 = $105/ngày; chi phí GPT-4o-mini: 10.500 × $0,0006 = $6,30/ngày. Như vậy GPT-4o đắt hơn GPT-4o-mini khoảng ≈ 16,7 lần cho cùng workload này.

**Mô tả một trường hợp mà chi phí cao hơn của GPT-4o là xứng đáng, và một trường hợp GPT-4o-mini là lựa chọn tốt hơn:**

GPT-4o xứng đáng với chi phí cao hơn trong các tác vụ đòi hỏi lập luận phức tạp và độ chính xác cao, ví dụ như phân tích hợp đồng pháp lý, hỗ trợ chẩn đoán y tế, hoặc tạo code cho hệ thống quan trọng — những nơi mà một lỗi nhỏ có thể gây hậu quả nghiêm trọng và chất lượng đầu ra trực tiếp ảnh hưởng đến quyết định kinh doanh. Ngược lại, GPT-4o-mini là lựa chọn tốt hơn cho các tác vụ có khối lượng lớn nhưng độ phức tạp thấp như phân loại cảm xúc đánh giá sản phẩm, tóm tắt nội dung ngắn, hoặc trả lời các câu hỏi FAQ — nơi tiết kiệm chi phí gần 17 lần mà chất lượng vẫn đáp ứng được yêu cầu thực tế.

---

### Bài tập 2.3 — Trải Nghiệm Người Dùng với Streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì non-streaming lại phù hợp hơn?** (1 đoạn văn)

Streaming quan trọng nhất khi người dùng đang chờ đợi phản hồi trực tiếp trong thời gian thực — chẳng hạn chatbot hội thoại, trợ lý viết nội dung, hay công cụ giải thích code — vì việc hiển thị từng token ngay khi được tạo ra giúp giảm đáng kể cảm giác chờ đợi và tăng trải nghiệm tương tác tự nhiên, đặc biệt khi câu trả lời dài. Ngược lại, non-streaming phù hợp hơn trong các pipeline xử lý hàng loạt hoặc tự động hóa phía backend — như phân loại văn bản, tổng hợp dữ liệu, hay gọi API theo lịch — nơi không có người dùng nào đang chờ theo dõi output theo thời gian thực và việc nhận toàn bộ kết quả cùng một lúc đơn giản hóa logic xử lý downstream hơn.


## Danh Sách Kiểm Tra Nộp Bài
- [ ] Tất cả tests pass: `pytest tests/ -v`
- [ ] `call_openai` đã triển khai và kiểm thử
- [ ] `call_openai_mini` đã triển khai và kiểm thử
- [ ] `compare_models` đã triển khai và kiểm thử
- [ ] `streaming_chatbot` đã triển khai và kiểm thử
- [ ] `retry_with_backoff` đã triển khai và kiểm thử
- [ ] `batch_compare` đã triển khai và kiểm thử
- [ ] `format_comparison_table` đã triển khai và kiểm thử
- [ ] `exercises.md` đã điền đầy đủ
- [ ] Sao chép bài làm vào folder `solution` và đặt tên theo quy định 

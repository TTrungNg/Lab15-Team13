# WORKSHEET 3: COST OPTIMIZATION DEBATE

## 1. 3 Chiến lược Optimization phù hợp nhất
Dựa trên đặc thù của Scenario 2 (Bệnh viện), nhóm chọn 3 chiến lược sau:
1. **Model Routing** (Điều hướng mô hình).
2. **Semantic Caching** (Bộ nhớ đệm ngữ nghĩa).
3. **Prompt Compression** (Nén Prompt).

---

## 2. Chi tiết các chiến lược

### Chiến lược 1: Model Routing
* **Tiết kiệm phần nào:** Giảm mạnh chi phí Token API.
* **Lợi ích:** Sử dụng Model nhỏ (như GPT-4o-mini hoặc Haiku) cho các câu hỏi hành chính đơn giản và chỉ dùng Model lớn (như GPT-4o hoặc Claude 3.5 Sonnet) cho việc tóm tắt bệnh án phức tạp.
* **Trade-off:** Cần xây dựng thêm một lớp logic để phân loại câu hỏi (Router), có thể làm tăng nhẹ độ trễ (latency).
* **Thời điểm áp dụng:** Áp dụng ngay từ giai đoạn đầu MVP.

### Chiến lược 2: Semantic Caching
* **Tiết kiệm phần nào:** Tiết kiệm 100% chi phí Token cho các câu hỏi trùng lặp hoặc tương đương.
* **Lợi ích:** Các câu hỏi về quy trình bệnh viện (vd: "Quy trình thanh toán BHYT là gì?") thường lặp đi lặp lại. Cache giúp phản hồi tức thì và không tốn phí gọi API.
* **Trade-off:** Có rủi ro trả về thông tin cũ nếu quy trình bệnh viện thay đổi mà cache chưa được cập nhật (stale data).
* **Thời điểm áp dụng:** Áp dụng khi hệ thống bắt đầu có lượng user ổn định (>50 users).

### Chiến lược 3: Prompt Compression
* **Tiết kiệm phần nào:** Giảm chi phí Input Token (vốn là phần tốn kém nhất trong RAG).
* **Lợi ích:** Sử dụng các kỹ thuật nén hoặc chỉ trích xuất các thông tin then chốt từ bệnh án dài trước khi đưa vào LLM.
* **Trade-off:** Nếu nén quá mức có thể làm mất các chi tiết lâm sàng quan trọng, dẫn đến tóm tắt thiếu chính xác.
* **Thời điểm áp dụng:** Áp dụng khi volume dữ liệu bệnh án bắt đầu lớn và phức tạp.

---

## 3. Lộ trình thực thi (Execution Roadmap)

### Chiến lược làm ngay (Immediate):
* **Model Routing:** Đây là cách dễ nhất để cắt giảm chi phí mà không ảnh hưởng đến kiến trúc hệ thống. Việc phân tách task hành chính và task chuyên môn giúp tối ưu ngân sách ngay lập tức.
* **Prompt Engineering cơ bản:** Tối ưu hóa lại cấu trúc dữ liệu trả về từ RAG để tránh thừa thãi tokens.

### Chiến lược để sau (Long-term):
* **Smaller/Self-hosted Model:** Khi số lượng request đạt ngưỡng cực lớn (vài chục nghìn/ngày), nhóm mới cân nhắc tự host các model như Llama 3 hoặc Mistral trên hạ tầng riêng của bệnh viện để tối ưu chi phí dài hạn và tăng tính bảo mật.
* **Semantic Caching nâng cao:** Cần thời gian để thu thập đủ tập dữ liệu câu hỏi phổ biến từ thực tế trước khi cấu hình cache hiệu quả.

---

## 4. Kết luận của nhóm
Nhóm ưu tiên chiến lược **Model Routing** vì tính khả thi cao và hiệu quả rõ rệt. Tuy nhiên, trong môi trường y tế, mọi chiến lược tối ưu hóa "chi phí" đều phải đặt sau tiêu chí **"An toàn dữ liệu"** và **"Độ chính xác"**. Chúng tôi sẽ không hy sinh độ chính xác của việc tóm tắt bệnh án chỉ để đổi lấy vài cents tiết kiệm từ API.
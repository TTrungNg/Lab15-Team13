# WORKSHEET 0: XÁC ĐỊNH DỰ ÁN PHÂN TÍCH PRODUCTION

### 1. Thông tin chung
* **Chủ đề lựa chọn:** Trợ lý hỗ trợ lâm sàng bệnh viện (Hospital Clinical Support Assistant).
* **Kỹ năng nhóm tự tin nhất:** 

1. Thiết kế luồng RAG (Retrieval-Augmented Generation) để truy xuất quy trình nội bộ.

2. Xây dựng Guardrails để kiểm soát an toàn và độ chính xác của phản hồi AI.
   
3. Tối ưu hóa Model Routing để cân bằng giữa chi phí và chất lượng chuyên môn.

### 2. Mô tả ngắn sản phẩm/Scenario
Sản phẩm là một AI Agent tích hợp vào hệ thống HIS/EMR của bệnh viện nhằm hỗ trợ y bác sĩ và nhân viên y tế:
* Tóm tắt lịch sử bệnh án phức tạp của bệnh nhân từ dữ liệu cũ.
* Tra cứu nhanh các quy trình chuyên môn nội bộ và hướng dẫn sử dụng thiết bị y tế.
* Hỗ trợ trả lời các câu hỏi về thủ tục hành chính, bảo hiểm cho nhân viên tiếp nhận.
* **Cơ chế vận hành:** Sử dụng mô hình Hybrid (Dữ liệu bệnh nhân nhạy cảm xử lý On-premise, câu hỏi hành chính công cộng xử lý qua Cloud API).

### 3. Câu hỏi phân tích

#### a. Sản phẩm này giải quyết bài toán gì?
* **Giảm tải áp lực hành chính:** Giúp bác sĩ tiết kiệm thời gian đọc hiểu hồ sơ bệnh án dày đặc, tập trung vào việc chẩn đoán.
* **Chuẩn hóa quy trình:** Đảm bảo mọi nhân viên y tế đều tiếp cận được hướng dẫn chuyên môn mới nhất một cách tức thời.
* **An toàn dữ liệu:** Giải quyết bài toán bảo mật thông tin sức khỏe nhạy cảm theo quy định pháp luật (PDPA) trong khi vẫn tận dụng được sức mạnh của LLM.

#### b. Ai là người dùng chính?
* **Bác sĩ & Điều dưỡng:** Sử dụng để tóm tắt bệnh án và tra cứu phác đồ điều trị nhanh.
* **Nhân viên tiếp nhận/Hành chính:** Sử dụng để trả lời các quy trình thủ tục, biểu mẫu cho bệnh nhân.

#### c. Vì sao chủ đề này phù hợp để phân tích Deployment và Cost?
* **Về Deployment:** Đây là bài toán điển hình cho mô hình **Hybrid Deployment**. Do ràng buộc dữ liệu sức khỏe không được rời khỏi Việt Nam và cần bảo mật tuyệt đối, nhóm phải phân tích việc chạy mô hình local (vLLM/Ollama) cho dữ liệu nhạy cảm và Cloud API cho dữ liệu chung.
* **Về Cost:** * Phải tính toán điểm hòa vốn (**Break-even point**) giữa việc đầu tư hạ tầng GPU tại chỗ (On-prem) và chi phí sử dụng API.
    * Áp dụng chiến lược **Model Routing**: Các câu hỏi hành chính đơn giản dùng mô hình rẻ (GPT-4o-mini/Haiku), các yêu cầu tóm tắt bệnh án phức tạp cần độ chính xác cao dùng mô hình mạnh (GPT-4o/Opus).
    * Phân tích **Hidden Costs** liên quan đến việc kiểm soát lỗi (hallucination) vì trong y tế, chi phí cho một sai sót là cực kỳ lớn.


---

## 4. Các điểm tranh luận chính (Key Discussion Points)
* **Mức độ hỗ trợ:** Ưu tiên hỗ trợ hành chính và tóm tắt dữ liệu (Decision Support), không thay thế việc chẩn đoán chuyên môn của bác sĩ.
* **Xử lý Hallucination:** Áp dụng cơ chế trích dẫn nguồn (Citation) trực tiếp từ file gốc để nhân viên y tế có thể kiểm chứng lại ngay lập tức.
* **Sự hiện diện của con người (Human-in-the-loop):** Mọi kết quả tóm tắt từ AI cần có bước xác nhận của bác sĩ trước khi được đưa vào biên bản y khoa chính thức.
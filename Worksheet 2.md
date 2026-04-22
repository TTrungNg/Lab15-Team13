# WORKSHEET 2: COST ANATOMY LAB - HOSPITAL AI ASSISTANT

### 1. Ước lượng lưu lượng (MVP Stage)
* **Số lượng User:** ~50 nhân viên (Bác sĩ, điều dưỡng, hành chính) tại một khoa mẫu.
* **Request/ngày:** ~10 request/user/ngày $\rightarrow$ **Tổng: 500 requests/ngày**.
* **Peak traffic:** Tập trung vào giờ giao ca (7:30 - 8:30 sáng) và giờ đi buồng (2:00 - 3:00 chiều). Dự kiến peak khoảng 100 requests/giờ.

### 2. Ước lượng Tokens (Dựa trên mô hình Hybrid)
* **Input trung bình:** 1,500 tokens (do cần nhồi ngữ cảnh bệnh án hoặc quy trình y tế dài).
* **Output trung bình:** 500 tokens (tóm tắt hoặc hướng dẫn ngắn gọn).
* **Tổng token/ngày:** 500 requests $\times$ 2,000 tokens = **1,000,000 tokens/ngày**.

### 3. Liệt kê các lớp chi phí (Cost Layers)
* **Token API (Cloud):** Dành cho các task hành chính (chiếm 40% traffic).
* **Compute (On-prem):** Chi phí điện, vận hành server GPU có sẵn tại bệnh viện để chạy Local LLM xử lý bệnh án.
* **Storage (Vector DB):** Lưu trữ toàn bộ phác đồ, quy trình và nhúng (embeddings) bệnh án.
* **Human Review:** Bác sĩ trưởng khoa kiểm soát chất lượng phản hồi của AI trong tháng đầu (chiếm 15% quỹ thời gian).
* **Logging & Audit:** Chi phí lưu trữ nhật ký truy cập và đối soát (bắt buộc trong y tế).

### 4. Tính sơ bộ chi phí mức MVP (Tháng)
* **Cloud API:** ~200 requests/ngày $\times$ 30 ngày $\rightarrow$ ~12M tokens $\approx$ **$30 - $50/tháng**.
* **Infrastructure (Ops):** Bảo trì server có sẵn + điện năng + backup $\approx$ **$100 - $150/tháng**.
* **Hidden Costs (Safety & Guardrails):** Chi phí cho các lớp filter ngôn ngữ, check PII $\approx$ **20% tổng chi phí API**.
* **Tổng cộng:** Dự kiến khoảng **$200 - $350/tháng** (chưa tính lương nhân sự vận hành).

### 5. Dự báo khi Scale (10x User)
* **Phần tăng mạnh nhất:** **Compute & Infrastructure**. Khi số user lên 500, server GPU đơn lẻ tại bệnh viện sẽ quá tải (bottleneck). Việc nâng cấp hạ tầng On-premise tốn kém hơn nhiều so với việc tăng hạn mức API Cloud.
* **Human Review:** Tăng theo cấp số nhân nếu không có hệ thống Eval tự động tốt.

### 6. Câu hỏi nhóm phải trả lời

* **Cost driver lớn nhất là gì?**
    $\rightarrow$ Là **Infrastructure & Maintenance** cho cụm On-premise. Vì đặc thù dữ liệu nhạy cảm không thể đẩy hoàn toàn lên Cloud, bệnh viện phải gánh chi phí vận hành phần cứng.
* **Hidden cost nào dễ bị quên nhất?**
    $\rightarrow$ **Chi phí lưu trữ và quản lý nhật ký Audit Trail**. Trong y tế, dữ liệu này phải lưu nhiều năm và có cấu trúc để phục vụ thanh tra, tốn dung lượng và công sức quản lý hơn dự tính.
* **Đội có chỗ nào đang ước lượng quá lạc quan không?**
    $\rightarrow$ Có thể đang lạc quan về **Token Input**. Các bệnh án thực tế rất hỗn loạn và dài, nếu không có bước tiền xử lý (pre-processing) tốt, lượng token thực tế có thể cao gấp 2-3 lần dự tính.
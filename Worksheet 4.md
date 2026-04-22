# WORKSHEET 4: SCALING & RELIABILITY TABLETOP - HOSPITAL AI ASSISTANT

### 1. Phân tích 3 tình huống sự cố

| Tình huống | Tác động tới User (Bác sĩ/Y tá) | Phản ứng ngắn hạn (Immediate) | Giải pháp dài hạn (Strategic) |
| :--- | :--- | :--- | :--- |
| **Traffic tăng đột biến (Peak Load)** | AI phản hồi chậm, bác sĩ phải chờ đợi lâu để có tóm tắt bệnh án, gây tắc nghẽn quy trình khám. | Kích hoạt **Request Queue**; ưu tiên (priority) cho các case cấp cứu; hiển thị thông báo "Đang xử lý". | Tự động tăng thêm **Instances** (Auto-scaling); tối ưu hóa Semantic Caching để giảm tải cho LLM. |
| **Provider Timeout (Cloud API lỗi)** | Không tra cứu được các thủ tục hành chính, bảo hiểm; các task phụ trợ bị ngưng trệ. | Kích hoạt **Circuit Breaker**; tự động chuyển hướng (fallback) sang quy trình Rule-based hoặc bộ câu hỏi FAQ tĩnh. | Triển khai **Multi-provider** (dùng song song OpenAI, Anthropic) để dự phòng; tăng cường khả năng của Local LLM. |
| **Response chậm (Latency cao)** | Trải nghiệm người dùng kém; bác sĩ có xu hướng bỏ qua AI và làm thủ công (giảm tỷ lệ áp dụng). | Sử dụng **Streaming output** để bác sĩ đọc kết quả ngay khi đang tạo; cắt bỏ các bước trung gian không cần thiết. | Tối ưu hóa phần cứng (GPU) cho On-premise; sử dụng các mô hình nhỏ hơn nhưng nhanh hơn cho các task đơn giản. |

### 2. Các Metric cần Monitor (Giám sát)
* **Latency (p95, p99):** Thời gian phản hồi cho bác sĩ không nên quá 5-10 giây cho một tóm tắt bệnh án.
* **Accuracy/Hallucination Rate:** Theo dõi tỷ lệ bác sĩ nhấn "Sửa đổi" hoặc "Báo cáo lỗi" trên nội dung AI tạo ra.
* **Token Throughput:** Giám sát lưu lượng để kịp thời nâng cấp hạ tầng On-premise trước khi quá tải.
* **System Uptime:** Đảm bảo độ sẵn sàng 99.9% vì đây là môi trường bệnh viện 24/7.

### 3. Fallback Proposal (Phương án dự phòng)

* **Tác vụ Hành chính:** Nếu Cloud API lỗi, hệ thống sẽ trả về kết quả từ **Vector Database (Local)** dựa trên mức độ tương đồng cao nhất (như một bộ Search engine truyền thống) thay vì để AI tự tạo câu trả lời.
* **Tác vụ Lâm sàng (Quan trọng):** Nếu Local LLM quá tải hoặc lỗi, hệ thống sẽ hiển thị thông báo: *"Trợ lý AI đang bận, vui lòng xem bệnh án gốc tại HIS"* thay vì đưa ra kết quả thiếu chính xác.
* **Human Escalation:** Luôn có nút "Kết nối với IT/Kỹ thuật" hoặc "Báo cáo lỗi chuyên môn" hiển thị nổi bật để bác sĩ có thể phản hồi khi thấy AI hoạt động không ổn định.

### 4. Phân loại Request (Real-time vs Async)
* **Real-time:** Tra cứu phác đồ điều trị, liều lượng thuốc, hướng dẫn thủ tục hành chính tại quầy.
* **Async (Bác sĩ có thể chờ):** Tóm tắt bệnh án cũ để chuẩn bị cho ca trực hôm sau, báo cáo thống kê định kỳ, phân tích xu hướng dịch bệnh trong khoa.

---
**Kết luận:** Hệ thống tin cậy là hệ thống biết "từ chối" đúng lúc thay vì đưa ra thông tin sai lệch trong môi trường y tế nhạy cảm.
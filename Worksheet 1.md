# WORKSHEET 1: ENTERPRISE DEPLOYMENT CLINIC

## 1. Bối cảnh tổ chức / Khách hàng sử dụng
Hệ thống được triển khai tại một **Bệnh viện đa khoa quy mô lớn** (ví dụ: Vinmec hoặc các bệnh viện hạng đặc biệt). Đây là môi trường có quy trình vận hành khắt khe, hoạt động 24/7, yêu cầu tính sẵn sàng cao và tuân thủ tuyệt đối các quy định pháp luật về khám chữa bệnh.

---

## 2. Dữ liệu mà hệ thống sẽ động đến
* **Dữ liệu định danh (PII):** Họ tên, số CCCD, số thẻ bảo hiểm y tế, thông tin liên lạc của bệnh nhân.
* **Dữ liệu sức khỏe (PHI):** Bệnh án (EMR), kết quả xét nghiệm, chẩn đoán hình ảnh, lịch sử dùng thuốc, ghi chú lâm sàng của bác sĩ.
* **Dữ liệu vận hành:** Quy trình điều trị nội bộ, lịch trực của nhân viên, biểu mẫu hành chính và các quy định của Bộ Y tế.

---

## 3. Đánh giá mức độ nhạy cảm của dữ liệu
* **Mức độ:** **Cực kỳ nhạy cảm (Critical).**
* **Lý do:** Dữ liệu y tế là tài sản cá nhân được pháp luật bảo vệ nghiêm ngặt. Việc rò rỉ không chỉ vi phạm đạo đức nghề nghiệp mà còn gây hậu quả pháp lý nghiêm trọng cho bệnh viện và ảnh hưởng trực tiếp đến quyền riêng tư, an toàn của bệnh nhân.

---

## 4. 3 Ràng buộc Enterprise lớn nhất
1. **Bảo mật và Quyền riêng tư (Privacy & Security):** Dữ liệu không được phép rời khỏi biên giới quốc gia (theo luật dữ liệu) và phải được mã hóa, phân quyền truy cập đa lớp (RBAC).
2. **Khả năng tích hợp (Interoperability):** Hệ thống AI phải "nói chuyện" được với các hệ thống cũ (HIS, EMR, LIS) thông qua các tiêu chuẩn y tế như HL7 hoặc FHIR.
3. **Độ tin cậy và Tính minh bạch (Reliability & Audit Trail):** Mọi câu trả lời của AI phải có nguồn gốc rõ ràng để kiểm chứng (Explainable AI) và phải lưu lại nhật ký (audit log) ai đã hỏi gì và AI đã trả lời gì.

---

## 5. Lựa chọn mô hình triển khai
**Lựa chọn:** **Hybrid Cloud** (Kết hợp Private Cloud/On-premise và Public Cloud).

---

## 6. 2 lý do vì sao chọn mô hình Hybrid
1. **Tối ưu giữa bảo mật và hiệu năng:** Các dữ liệu nhạy cảm nhất (bệnh án, thông tin cá nhân) được lưu trữ và xử lý tại **On-premise** (máy chủ nội bộ) để đảm bảo không dữ liệu nào rời khỏi tổ chức. Trong khi đó, các tác vụ tính toán LLM nặng hoặc dữ liệu hành chính công khai có thể tận dụng **Public Cloud** để tiết kiệm chi phí hạ tầng và tăng tốc độ xử lý.
2. **Tính sẵn sàng và Tuân thủ:** Mô hình này cho phép bệnh viện duy trì hoạt động cốt lõi ngay cả khi gặp sự cố đường truyền internet quốc tế (vẫn truy cập được quy trình nội bộ tại chỗ), đồng thời đáp ứng các tiêu chuẩn kiểm toán khắt khe nhất của ngành y tế về kiểm soát dòng chảy dữ liệu.

---

## 7. Thảo luận mở rộng (Gợi ý trả lời)
* **Audit trail:** Bắt buộc phải có để truy cứu trách nhiệm khi có sai sót y khoa.
* **Dữ liệu rời khỏi tổ chức:** Tuyệt đối không đối với dữ liệu PHI; chỉ cho phép dữ liệu đã được ẩn danh (anonymized) nếu cần dùng để fine-tune model trên cloud.
* **Tích hợp:** Cần tích hợp sâu vào quy trình phê duyệt (approval flow) của bác sĩ.
* **Hậu quả nếu trả lời sai:** Bệnh nhân là người bị ảnh hưởng đầu tiên (chẩn đoán sai, nhầm thuốc), sau đó là uy tín và pháp lý của bệnh viện.





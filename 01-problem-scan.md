# 01 - Problem Scan & Quick Cards

## 1. Problem Scan (5 bài toán thực tế)

| # | Subsidiary | Lens | Mô tả bài toán |
|---|---|---|---|
| 1 | Xanh SM | Repetitive | Tài xế và điều phối viên mất nhiều thời gian phân bổ lại tuyến xe khi có đơn đặt xe mới hoặc hủy chuyến đột ngột trong giờ cao điểm. |
| 2 | Xanh SM | Stakeholder Pain | Tài xế phải chạy xe rỗng nhiều vì hệ thống dự báo nhu cầu chưa chính xác, gây lãng phí thời gian và nhiên liệu. |
| 3 | VinFast | Time-consuming | Kỹ thuật viên kiểm tra lỗi xe phải ghi chú bằng tay rồi nhập lại vào hệ thống bảo hành, làm tăng thời gian xử lý. |
| 4 | Vinhomes | Time-consuming | Nhân viên quản lý tòa nhà phải đọc và phân loại hàng trăm ticket cư dân thủ công, có thể sai nhóm xử lý. |
| 5 | Vinmec | AI-upgrade | Hồ sơ bệnh án và mã hóa ICD-10 đang nhập bản thân bằng text tự do, khiến việc đối soát bảo hiểm mất thời gian và dễ sai lệch. |

## 2. Top 3 Quick Problem Cards

### Quick Problem Card 1 — Xanh SM Dispatch
- Bài toán: Điều phối xe và gợi ý lộ trình tối ưu khi có đơn mới hoặc hủy chuyến đột ngột.
- Actor: Điều phối viên vận hành và tài xế Xanh SM.
- Workflow hiện tại:
  1. Nhận đơn mới.
  2. Kiểm tra vị trí tài xế và lịch trình.
  3. Chọn tài xế phù hợp.
  4. Gửi xác nhận cho khách hàng.
  5. Điều chỉnh khi có hủy/trễ.
- Bottleneck: Tái sắp xếp tài xế và xử lý đơn bất thường mất 8-15 phút/lượt.
- AI hỗ trợ: Dự đoán điểm nóng nhu cầu, gợi ý tài xế gần nhất, cảnh báo trễ và thay thế xe.
- Metric: Giảm thời gian xử lý đơn từ 12 phút xuống dưới 5 phút; giảm 25% số đơn trễ.
- AI fit: Agentic loop / predictive dispatch.

### Quick Problem Card 2 — VinFast Warranty Intake
- Bài toán: Tự động hóa quy trình kiểm tra lỗi xe và tạo yêu cầu bảo hành.
- Actor: Kỹ thuật viên xưởng và cố vấn dịch vụ.
- Workflow hiện tại:
  1. Kỹ thuật viên kiểm tra xe.
  2. Ghi chú lỗi bằng văn bản tự do.
  3. Nhân viên đối chiếu với quy trình bảo hành.
  4. Nhập dữ liệu vào hệ thống DMS.
- Bottleneck: Ghi nhận lỗi và nhập dữ liệu mất nhiều thời gian, dễ mơ hồ.
- AI hỗ trợ: Speech-to-text + NLP để trích xuất lỗi, gọi mã linh kiện và tạo yêu cầu bảo hành.
- Metric: Giảm thời gian tạo hồ sơ từ 20 phút xuống dưới 5 phút/lượt xe.
- AI fit: LLM feature + rule validation.

### Quick Problem Card 3 — Vinhomes Ticket Triage
- Bài toán: Phân loại và ưu tiên ticket cư dân một cách nhanh và chính xác.
- Actor: Ban quản lý tòa nhà và nhân viên hỗ trợ cư dân.
- Workflow hiện tại:
  1. Cư dân báo sự cố qua app hoặc tin nhắn.
  2. Nhân viên đọc và hiểu nội dung.
  3. Phân loại theo loại sự cố và mức độ khẩn cấp.
  4. Gửi cho bộ phận kỹ thuật phù hợp.
- Bottleneck: Đọc hiểu và phân luồng manual làm chậm xử lý.
- AI hỗ trợ: Vision + LLM để phân loại sự cố, ưu tiên mức khẩn cấp và tạo ticket.
- Metric: Giảm thời gian phân loại từ 2-4 giờ xuống dưới 5 phút; giảm lỗi chuyển nhầm bộ phận.
- AI fit: LLM + rule routing.

## 3. Kết luận lựa chọn
Top problem chọn để deep dive: Xanh SM Intelligent Dispatch vì bài toán có dữ liệu rõ, rủi ro có thể kiểm soát bằng HITL, và giá trị trực tiếp rõ ràng cho vận hành.

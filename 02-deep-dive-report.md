# 02 - Deep Dive Report

## 1. Problem Statement (6-field)

### 1) Actor / Operator
Điều phối viên vận hành Xanh SM và tài xế đang xử lý đơn đặt xe trong giờ cao điểm.

### 2) Current Workflow
Nhân viên nhận đơn, kiểm tra vị trí và trạng thái tài xế, lựa chọn phương án phù hợp, gọi xác nhận với khách hàng và cập nhật lại tuyến nếu có hủy hoặc trễ.

### 3) Bottleneck
Bước tái sắp xếp tài xế và gợi ý lộ trình khi có đơn mới, hủy chuyến, hoặc chậm trễ trên đường gây tắc nghẽn trong hệ thống điều phối.

### 4) Business Impact
Tăng thời gian xử lý chuyến, làm giảm SLA, khiến khách hàng cảm thấy không tin cậy và giảm hiệu quả hoạt động của đội xe.

### 5) Success Metric
- Giảm thời gian xử lý đơn từ 12 phút xuống dưới 5 phút.
- Giảm 25% số đơn chậm trễ.
- Tăng 15% tỷ lệ hoàn thành SLA.

### 6) Operational Boundary
AI chỉ được đề xuất lộ trình và tài xế phù hợp dựa trên dữ liệu có sẵn; không được tự động thay đổi giá cước, không được xâm nhập dữ liệu không xác thực, và mọi quyết định ảnh hưởng tới tài xế/khách hàng cần có con người phê duyệt.

## 2. Current-State Workflow Mapping

- Khách đặt xe / hủy đơn
- Điều phối viên kiểm tra vị trí và trạng thái tài xế
- Chọn phương án xử lý thủ công hoặc cơ bản
- Xác nhận với khách hàng và tài xế
- Theo dõi trễ, sửa lại nếu cần

Bottleneck: Bước tái sắp xếp tài xế khi có đơn bất thường, mất nhiều thời gian và dễ sai.
Handoff: Điều phối viên -> hệ thống bảng điều phối -> tài xế -> khách hàng.

## 3. AI Fit & Future Flow

### AI fit
- Rule / State Machine: Có thể dùng cho quy tắc tối giản như giới hạn khoảng cách, thời gian và ưu tiên.
- LLM: Hữu ích cho mô tả tự nhiên, tin nhắn khách hàng, summary tình huống.
- Agentic Loop: Phù hợp hơn vì có thể tự động gợi ý hành động theo dữ liệu thời gian thực và chờ người vận hành phê duyệt.

### Future-state flow
1. Khách hàng đặt xe.
2. Hệ thống thu thập vị trí, lịch sử và trạng thái tài xế.
3. AI đánh giá đơn, dự đoán độ khả dụng và gợi ý tài xế/route.
4. Điều phối viên review quyết định cuối cùng.
5. Gửi thông tin tới tài xế và khách hàng.
6. Nếu dữ liệu thiếu hoặc không chắc chắn, hệ thống fallback về quy trình rule-based thủ công.

## 4. AI Readiness Checklist
- [x] Có dữ liệu lịch sử đơn, vị trí và thời gian.
- [x] Rủi ro sai có thể kiểm soát qua human-in-the-loop.
- [x] Stakeholder sẵn sàng thử nghiệm trên scope hẹp.

## 5. Decision
GO — Bắt đầu xây dựng prototype trong phạm vi hẹp.

Lý do: Bài toán có dữ liệu rõ, có tầm nhìn giá trị trực tiếp, và rủi ro sai vẫn có thể kiểm soát bởi con người. Đây là bài toán phù hợp để thử nghiệm AI nhanh mà không cần tự động hoá toàn bộ.

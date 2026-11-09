# 02 — Deep-Dive Report: Xanh SM Lost & Found Co-pilot

> Bài toán được chọn từ [01-problem-scan.md](01-problem-scan.md): **Card #1 — Xanh SM: tìm và trả đồ thất lạc của khách.**
> Các con số dưới đây là **ước tính** từ giả định hợp lý về quy trình, cần xác nhận lại bằng log CSKH thực tế trước khi build.

---

# 🏗️ Phase 3 — DEEP-DIVE

## 3.. Problem Statement (6-field) & Metrics

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên CSKH tuyến 1 của Xanh SM (hotline + chat in-app), xử lý yêu cầu đồ thất lạc. Stakeholder phụ: khách hàng và tài xế Xanh SM. |
| **2. Current Workflow** | Khách gọi/chat báo mất đồ → CSKH hỏi giờ đi, điểm đón/trả, mô tả đồ vật, ghi chú tay vào ticket CRM → mở Trip History của tài khoản khách, lọc theo ngày và tự đoán cuốc khớp → gọi tài xế của cuốc đó nhờ kiểm tra xe → hẹn điểm/giờ trả đồ và nhắn lại khách. 5 bước, hoàn toàn thủ công, ~20 phút/lượt. |
| **3. Bottleneck** | Bước 2-3 (~12 phút): chuyển lời kể tự nhiên, mơ hồ (giờ ước lượng, tên địa danh không chuẩn, mô tả đồ vật) thành bộ lọc chuyến và chọn đúng cuốc. Đây là bước **xử lý ngôn ngữ**, rule cứng không làm được tốt. Đoán sai cuốc gây vòng lặp +10 phút và làm phiền tài xế không liên quan. |
| **4. Business Impact** | ~150 yêu cầu/ngày toàn quốc × 20 phút = **~50 giờ công CSKH/ngày** (~6 FTE). Ước tính chỉ ~60% đồ được trả trong 24h; phần còn lại kéo dài thành khiếu nại, đánh giá 1 sao trên app store và chi phí đền bù. Tài xế mất thời gian nghe gọi nhầm trong lúc chạy cuốc. |
| **5. Success Metric** | 1. **Efficiency:** Thời gian xác định đúng cuốc từ ~12 phút ──> **dưới 2 phút**; tổng thời gian xử lý từ ~20 phút ──> **dưới 8 phút**.<br>2. **Quality:** Cuốc đúng nằm trong **top-3 gợi ý ở ≥ 90%** yêu cầu; trích xuất đúng trường (ngày, khung giờ, khu vực, đồ vật) **≥ 95%**.<br>3. **Outcome:** Tỉ lệ trả đồ thành công trong 24h từ ~60% ──> **≥ 80%**; số cuộc gọi nhầm tài xế giảm **≥ 50%**. |
| **6. Operational Boundary** | **AI được phép:** đọc nội dung chat/transcript yêu cầu; trích xuất thông tin có cấu trúc; xếp hạng các cuốc **trong lịch sử của chính tài khoản khách** (tối đa 72h gần nhất); soạn **bản nháp** tin nhắn hỏi tài xế và tin trả lời khách.<br>**TUYỆT ĐỐI CẤM:** tiết lộ SĐT, họ tên đầy đủ, biển số hay vị trí hiện tại của tài xế cho khách; truy vấn chuyến của tài khoản khác; tự gửi tin cho tài xế/khách; hứa đền bù hoặc kết luận "tài xế đã lấy đồ".<br>**Điểm cần duyệt (HITL):** CSKH chọn cuốc trong top-3 và bấm duyệt mọi tin nhắn trước khi gửi. Đồ giá trị cao (điện thoại, laptop, tiền mặt, giấy tờ tùy thân) → bắt buộc chuyển Team Leader theo dõi. |

---

## 3.2. Future-State Flow & AI Fit

### AI-Fit Matrix: So sánh Rule vs LLM vs Agent

| Tiêu chí | Rule / State-Machine | LLM Feature | Agentic Loop |
|---|---|---|---|
| Hiểu lời kể mơ hồ (*"tối qua tầm 9h, cổng sau Times City"*) | ❌ Phải bắt khách điền form cứng; khách nhớ sai giờ thì lọc trượt | ✅ Trích xuất khung giờ, địa danh, đồ vật từ tiếng Việt tự nhiên | ✅ Làm được, nhưng thừa năng lực |
| Lọc & xếp hạng chuyến | ✅ Query DB theo khung giờ/khu vực rất tốt, rẻ, deterministic | ⚠️ Không nên để LLM tự "đoán" chuyến | ⚠️ Agent tự query nhiều vòng, khó kiểm soát |
| Soạn tin nhắn cho tài xế/khách | ⚠️ Template cứng, rập khuôn | ✅ Draft tự nhiên, đúng ngữ cảnh | ✅ |
| Tự liên hệ tài xế, đặt lịch hẹn | ❌ | ❌ (ngoài scope) | ⚠️ Có thể, nhưng rủi ro lộ thông tin và gửi nhầm cao |
| Chi phí / độ trễ / khả năng audit | Thấp / thấp / cao | Trung bình / ~2-3s / cao (1 lần gọi, log được) | Cao / không ổn định / thấp |

**Kết luận AI Fit: [ ] Rule / State-Machine  [x] LLM Feature  [ ] Agentic Loop**
→ Kiến trúc lai **LLM + Rule**: LLM chỉ làm phần ngôn ngữ (trích xuất → JSON, soạn nháp). Việc lọc/xếp hạng chuyến và mọi ranh giới truy cập dữ liệu do **rule/code** đảm nhận, nên kết quả deterministic và audit được. Không dùng Agent vì quy trình cố định 5 bước, và bước liên hệ người thật (tài xế) cần con người quyết định.

### Future-State Flow

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │     │ Bước 5       │
│ Khách chat/  │     │ 🔵 LLM trích │     │ ⚙️ Rule query │     │ 🟢 CSKH chọn  │     │ 🟢 CSKH duyệt│
│ gọi báo mất  │ ──→ │ xuất JSON:   │ ──→ │ Trip History │ ──→ │ cuốc trong   │ ──→ │ & gửi tin hỏi│
│ đồ (transcript│    │ {date, time_ │     │ (chỉ TK khách│     │ top-3 + xem  │     │ tài xế; hẹn  │
│ tự động)     │     │ window, area,│     │ ≤72h) → xếp  │     │ 🔵 tin nháp   │     │ trả, báo     │
│              │     │ item, value} │     │ hạng top-3   │     │ [DRAFT_ONLY] │     │ khách        │
│ ⏱ 2 phút     │     │ ⏱ ~5 giây    │     │ ⏱ ~1 giây    │     │ ⏱ 1-2 phút   │     │ ⏱ 3-4 phút   │
└──────────────┘     └──────┬───────┘     └──────┬───────┘     └──────┬───────┘     └──────────────┘
                            │                    │                    │
                            ▼                    ▼                    ▼
                     ↩️ Fallback A:        ↩️ Fallback B:        ↩️ Fallback C:
                     JSON lỗi / thiếu      0 cuốc khớp hoặc      CSKH thấy không cuốc
                     trường bắt buộc →     điểm khớp thấp →      nào đúng → bấm "Tự
                     LLM sinh 1-2 câu hỏi  nới khung giờ ±2h,    lọc thủ công", quay
                     làm rõ cho CSKH hỏi   vẫn không có → CSKH   về quy trình cũ (log
                     khách; lỗi lần 2 →    lọc thủ công như cũ   lại để cải thiện)
                     CSKH nhập tay

🔵 = AI Step (LLM)     ⚙️ = Rule / code deterministic     🟢 = Human Step (HITL)     ↩️ = Fallback
⏱ Tổng cộng mục tiêu = ~6-8 phút/lượt (so với ~20 phút hiện tại)
```

### Cơ chế Human-in-the-loop
- **Chọn cuốc:** AI chỉ gợi ý top-3 kèm lý do khớp (giờ, điểm đón, khoảng cách); CSKH là người chọn. AI không bao giờ tự kết luận "đây là cuốc của bạn".
- **Duyệt tin nhắn:** mọi output soạn thảo bắt đầu bằng tag `[DRAFT_ONLY]`; hệ thống gửi tin chặn mọi nội dung chưa được CSKH bấm duyệt.
- **Escalation:** khi `value = high` (điện thoại, laptop, tiền mặt, giấy tờ) → ticket tự gắn cờ cho Team Leader theo dõi đến khi đóng.

### Fallback & lớp an toàn
- **Lọc dữ liệu cá nhân ở tầng code (không dựa vào prompt):** LLM không bao giờ nhận SĐT/biển số tài xế trong context. Tin nháp gửi khách chỉ được dùng tên rút gọn (*"tài xế Hùng"*) và mã chuyến, nên prompt injection cũng không lộ được thông tin.
- **Giới hạn truy vấn:** rule chỉ cho query chuyến thuộc `customer_id` của ticket và trong 72h, chặn trường hợp kẻ gian khai mất đồ để dò chuyến người khác.
- **LLM down / timeout > 10s:** giao diện tự chuyển về form nhập tay như quy trình cũ, CSKH không bị chặn công việc.

---

# 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist

| # | Tiêu chí | Đánh giá | Bằng chứng / Ghi chú |
|---|---|---|---|
| 1 | Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? | ⚠️ **Một phần** | Trip History đã có cấu trúc sẵn trên hệ thống. Ticket "mất đồ" cũ trong CRM có thể dùng làm bộ test (lời kể → cuốc đúng đã xác nhận), nhưng cần **ẩn danh hóa** và gán nhãn lại ~300 ticket. Chưa có baseline đo chính xác 20 phút/lượt, cần lấy từ timestamp CRM. |
| 2 | Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)? | ✅ **Có** | AI chỉ gợi ý và soạn nháp; CSKH chọn cuốc và duyệt tin. Sai thì tệ nhất là quay về quy trình thủ công như hiện tại. Rủi ro lớn nhất (lộ thông tin tài xế, dò chuyến người khác) được chặn ở tầng code chứ không chỉ bằng prompt. |
| 3 | Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? | ✅ **Có (giả định)** | Thay đổi nhỏ với CSKH: vẫn trên giao diện CRM cũ, thêm panel gợi ý. Tài xế không phải đổi gì (giảm cuộc gọi nhầm). Cần xác nhận với Trưởng bộ phận CSKH trước pilot. |

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future

- [x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
- [ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
- [ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**

> **GO với scope hẹp:** pilot 4 tuần, chỉ kênh **chat in-app** tại **Hà Nội** (ước tính ~50 yêu cầu/ngày), chạy song song với quy trình cũ.
>
> - **Kỹ thuật:** Phần khó (hiểu lời kể tiếng Việt mơ hồ) đúng là việc LLM làm tốt hơn rule. Phần cần chính xác (lọc chuyến, phân quyền dữ liệu) vẫn do code deterministic xử lý, nên không phải tin tưởng mù quáng vào LLM. Mỗi yêu cầu chỉ cần 1-2 lần gọi LLM, không cần Agent.
> - **Chi phí:** Mỗi yêu cầu ~1-2 lần gọi model loại flash/lite với vài nghìn token, chi phí ước tính dưới vài trăm đồng/yêu cầu. Con số này rất nhỏ so với ~12 phút công CSKH tiết kiệm được (toàn quốc ~30 giờ công/ngày nếu đạt mục tiêu).
> - **Rủi ro:** Kiểm soát được nhờ HITL ở cả bước chọn cuốc và gửi tin, cộng với fallback về quy trình cũ ở mọi điểm lỗi.
> - **Điều kiện trước khi build:** (1) đo baseline thật từ timestamp CRM; (2) gán nhãn ~300 ticket cũ đã ẩn danh làm bộ test; (3) qua review của bộ phận Pháp chế/Bảo mật dữ liệu về việc đưa transcript khách vào LLM.
> - **Tiêu chí mở rộng sau pilot:** top-3 accuracy ≥ 90%, 0 sự cố lộ thông tin tài xế, thời gian xử lý trung bình < 8 phút → mở rộng sang hotline (cần thêm speech-to-text) và toàn quốc. Không đạt → quay về **NOT YET** và phân tích lỗi trên bộ test.

# 01 — Problem Scan & Quick Cards (Vin Smart Future)

> Vai trò: AI Product Engineer tại **Vin Smart Future**.
> Các con số thời gian/khối lượng dưới đây là **ước tính** từ giả định hợp lý về quy trình, cần xác nhận lại bằng log/khảo sát thực tế ở Phase 3.

---

# 🔍 Phase 1 — SCAN

Dùng **4 Lenses** (Lặp lại / Tốn thời gian / AI-upgrade / Pain từ người khác) quét qua vận hành các công ty thành viên Vingroup.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM** | Lặp lại | Khách để quên đồ trên xe (điện thoại, ví, túi xách). CSKH phải đọc mô tả mơ hồ của khách (*"cái túi màu be, đi từ Times City lúc tối qua"*), dò lịch sử chuyến để tìm đúng xe/tài xế, gọi xác nhận rồi hẹn trả đồ (~150 yêu cầu/ngày toàn quốc, ~20 phút/yêu cầu). |
| 2 | **VinFast** | Tốn thời gian | Kỹ thuật viên đại lý gửi hồ sơ yêu cầu bảo hành (warranty claim) gồm mô tả lỗi tự do + ảnh + mã lỗi; bộ phận bảo hành nhà máy đọc thủ công để kiểm tra đủ chứng từ và đúng điều kiện bảo hành, ~30% hồ sơ bị trả về vì thiếu thông tin. |
| 3 | **Vinhomes** | Tốn thời gian | Khi bàn giao căn hộ, nhân viên ghi danh sách lỗi (vết nứt, ố tường, cửa kẹt) bằng ghi chú tay + ảnh; sau đó phải gõ lại, phân loại theo hạng mục và gửi đúng nhà thầu thi công, mất 1–2 ngày/căn trước khi nhà thầu nhận việc. |
| 4 | **Vinmec** | Pain từ người khác | Bệnh nhân có bảo hiểm tư nhân phải chờ lâu ở quầy thanh toán vì nhân viên đọc thủ công điều khoản hợp đồng bảo hiểm (PDF 30–60 trang) để xác định dịch vụ nào được bảo lãnh, mức đồng chi trả bao nhiêu (~25 phút/hồ sơ). |
| 5 | **VinWonders** | Pain từ người khác | Khách phàn nàn xếp hàng lâu tại các trò chơi giờ cao điểm; vận hành không có cảnh báo sớm để điều luồng khách sang khu vực vắng hơn. |
| 6 | **Xanh SM** | AI-upgrade | Sàng lọc hồ sơ ứng tuyển tài xế (CV, bằng lái, giấy khám sức khỏe dạng ảnh chụp) hiện do HR kiểm tra thủ công từng giấy tờ; đợt tuyển dụng cao điểm tồn đọng hàng nghìn hồ sơ. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn top 3 từ danh sách SCAN: **#1 (Xanh SM đồ thất lạc), #2 (VinFast hồ sơ bảo hành), #4 (Vinmec bảo lãnh bảo hiểm).**

Tiêu chí chọn: bottleneck nằm ở bước **đọc/hiểu ngôn ngữ tự nhiên** (nơi LLM có lợi thế thật so với rule), tần suất cao, metric đo được, và đặt được **Human-in-the-loop** để kiểm soát rủi ro.

Loại:
- **#3:** Phần lớn là số hóa form (app checklist có dropdown hạng mục) giải quyết được, chưa cần AI.
- **#5:** Bài toán dự báo từ dữ liệu cảm biến/vé, thuộc ML truyền thống, không phải LLM.
- **#6:** Liên quan dữ liệu cá nhân và quyết định tuyển dụng, rủi ro thiên kiến cao. Nên làm OCR và kiểm tra rule (hạn bằng lái) trước.

## Card #1 — Xanh SM: Tìm và trả đồ thất lạc của khách

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Khách báo để quên đồ với mô tả mơ hồ, CSKH│
│ phải dò thủ công lịch sử chuyến để tìm đúng xe và tài xế.   │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH (xử lý lặp lại), Khách  │
│ hàng (lo lắng, chờ lâu), Tài xế (bị gọi nhiều lần).         │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Khách gọi/chat báo mất đồ                              │
│   ──> 2. CSKH hỏi thời gian, điểm đón/trả, mô tả đồ vật     │
│   ──> 3. Lọc lịch sử chuyến trên hệ thống để tìm cuốc khớp  │
│   ──> 4. Gọi tài xế kiểm tra xe, xác nhận có đồ             │
│   ──> 5. Hẹn điểm/giờ trả đồ, nhắn lại cho khách            │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ ~12 phút/lượt; │
│ khách nhớ sai giờ/địa điểm → lọc nhầm cuốc)                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3: LLM trích   │
│ xuất từ chat {thời gian ước lượng, khu vực, đồ vật} → hệ    │
│ thống query lịch sử chuyến của tài khoản khách → xếp hạng   │
│ top-3 cuốc khớp nhất + draft tin nhắn hỏi tài xế. CSKH chọn │
│ và gửi. AI KHÔNG được lộ SĐT/thông tin tài xế cho khách.    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Thời gian xác định đúng cuốc từ ~12 phút ──> dưới 2 phút│
│   - Cuốc đúng nằm trong top-3 gợi ý ở ≥ 90% yêu cầu         │
│   - Tỉ lệ trả đồ thành công trong 24h tăng từ ~60% ──> 80%  │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
│ (LLM trích xuất + query có cấu trúc, CSKH duyệt)            │
└─────────────────────────────────────────────────────────────┘
```

## Card #2 — VinFast: Tiền kiểm hồ sơ yêu cầu bảo hành từ đại lý

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Hồ sơ bảo hành đại lý gửi lên thường thiếu│
│ chứng từ/mô tả không rõ, nhà máy phải đọc thủ công rồi trả  │
│ về, kéo dài thời gian sửa xe cho khách.                     │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Chuyên viên thẩm định bảo hành (nhà    │
│ máy), Kỹ thuật viên đại lý (bị trả hồ sơ), Chủ xe (chờ xe). │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. KTV đại lý tạo claim: mô tả lỗi + ảnh + mã DTC + số km │
│   ──> 2. Chuyên viên đọc mô tả, đối chiếu chính sách BH     │
│   ──> 3. Kiểm tra đủ ảnh/chứng từ theo loại linh kiện       │
│   ──> 4. Trả về yêu cầu bổ sung HOẶC chuyển duyệt           │
│   ──> 5. Duyệt chi phí, đặt linh kiện thay thế              │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ ~18 phút/hồ sơ;│
│ ~30% hồ sơ bị trả về, mỗi vòng trả về mất thêm 1-2 ngày)    │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1 & 3: khi KTV   │
│ nộp, LLM đối chiếu mô tả + mã lỗi với checklist chứng từ    │
│ của loại linh kiện → báo ngay thiếu gì (trước khi gửi);     │
│ phía nhà máy nhận bản tóm tắt chuẩn hóa. AI KHÔNG được tự   │
│ duyệt/từ chối bảo hành hay quyết định chi phí.              │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Tỉ lệ hồ sơ bị trả về từ ~30% ──> dưới 10%              │
│   - Thời gian thẩm định từ ~18 phút ──> dưới 6 phút/hồ sơ   │
│   - Thời gian từ tạo claim đến duyệt từ ~5 ngày ──> 2 ngày  │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
│ (Rule cho checklist cứng + LLM đọc mô tả tự do)             │
└─────────────────────────────────────────────────────────────┘
```

## Card #3 — Vinmec: Tra cứu quyền lợi bảo lãnh bảo hiểm tư nhân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Nhân viên bảo lãnh phải đọc thủ công hợp  │
│ đồng bảo hiểm dài để xác định dịch vụ nào được chi trả,     │
│ khiến bệnh nhân chờ lâu ở quầy thanh toán.                  │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên phòng Bảo lãnh viện phí,     │
│ Bệnh nhân và người nhà (chờ đợi, bất ngờ về chi phí).       │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Nhận thẻ bảo hiểm + chỉ định dịch vụ của bác sĩ        │
│   ──> 2. Mở hợp đồng/quy tắc BH (PDF 30-60 trang), tìm điều │
│          khoản quyền lợi, loại trừ, hạn mức                 │
│   ──> 3. Đối chiếu từng dịch vụ, tính phần đồng chi trả     │
│   ──> 4. Gửi yêu cầu bảo lãnh cho công ty BH, báo bệnh nhân │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ ~20 phút/hồ sơ;│
│ bỏ sót điều khoản loại trừ → bị công ty BH từ chối sau đó)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3: LLM (RAG    │
│ trên hợp đồng của đúng công ty BH/gói) trả về bảng từng dịch│
│ vụ: được/không được/không chắc + trích dẫn số điều khoản.   │
│ Nhân viên kiểm tra trích dẫn rồi mới gửi. AI KHÔNG được báo │
│ trực tiếp cho bệnh nhân hay cam kết số tiền chi trả.        │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Thời gian tra cứu từ ~20 phút ──> dưới 5 phút/hồ sơ     │
│   - ≥ 95% kết luận có trích dẫn điều khoản đúng             │
│   - Tỉ lệ bị công ty BH từ chối do sai quyền lợi giảm 50%   │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
│ (LLM + RAG, bắt buộc HITL vì liên quan tài chính bệnh nhân) │
└─────────────────────────────────────────────────────────────┘
```

---

## 🗳️ Đề xuất cho Deep-Dive

Đề xuất chọn **Card #1 — Xanh SM đồ thất lạc** để Deep-Dive:
- Tần suất cao, quy trình ngắn, dữ liệu đầu vào (lịch sử chuyến) đã có cấu trúc sẵn trên hệ thống nên dễ có data test.
- Rủi ro khi AI sai thấp: chỉ là gợi ý top-3 cho CSKH chọn, sai thì CSKH tự lọc như cũ (fallback rõ ràng).
- Ranh giới an toàn dễ kiểm chứng bằng prompt test: không lộ thông tin cá nhân tài xế, luôn là bản nháp chờ duyệt.
- Card #2 cần bộ checklist chứng từ chuẩn từ nhà máy trước. Card #3 có giá trị cao nhưng rủi ro tài chính/pháp lý lớn hơn và cần kho hợp đồng BH số hóa, nên xếp **Not Yet**.

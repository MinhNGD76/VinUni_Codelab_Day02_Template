# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

---

## 🏛️ 1. Bối cảnh thực tế: Vin Smart Future (Vingroup)

**Vingroup** — Tập đoàn tư nhân lớn nhất Việt Nam — vừa sáp nhập toàn bộ các phòng ban công nghệ thuộc các công ty thành viên thành một đơn vị công nghệ thống nhất mang tên **Vin Smart Future**. 

Nhiệm vụ của **Vin Smart Future** là xây dựng các giải pháp AI, số hóa, và tự động hóa cốt lõi để nâng cao hiệu suất vận hành và trải nghiệm khách hàng xuyên suốt các công ty thành viên:
* 🚗 **VinFast:** Hệ thống xe điện thông minh (EV), trợ lý AI ảo trong xe, dự đoán bảo trì pin, và quản lý chuỗi cung ứng sản xuất.
* 🚕 **Xanh SM (GSM):** Vận hành đội xe taxi/xe máy điện thông minh, điều vận thông minh (Smart Dispatching), tối ưu hóa lộ trình di chuyển.
* 🏢 **Vinhomes:** Quản lý đô thị thông minh (Smart Cities), trợ lý cư dân thông minh, tối ưu hóa mức tiêu thụ năng lượng.
* 🏥 **Vinmec:** Y tế thông minh, chẩn đoán hình ảnh bằng AI, tối ưu hóa quản lý hồ sơ bệnh án.
* 🎢 **Vinpearl / VinWonders:** Trải nghiệm du lịch số hóa, quản lý phòng và luồng khách thông minh tại các khu vui chơi.

Trong buổi Lab hôm nay, nhóm của bạn sẽ đóng vai trò là **AI Product Engineer** tại **Vin Smart Future**, tiến hành tìm kiếm, scoping, phân tích độ khả thi, thiết lập ranh giới vận hành, và xây dựng một **bản mẫu kỹ thuật (prompt prototype)** cho một bài toán cụ thể thuộc một trong những mảng kinh doanh trên.

---

## 📊 2. Cơ cấu tính điểm bài lab

### 👥 Điểm nhóm (60 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **G1. Workflow Mapping** | 20 | Problem Deep-Dive | Vẽ chi tiết quy trình hiện tại: các bước, handoff, thời gian, bottleneck |
| **G2. Problem Statement** | 20 | Problem Deep-Dive | Problem Statement 6-field bám sát thực tế, metric có số và ranh giới rõ ràng |
| **G3. AI Fit & Future Flow** | 10 | Problem Deep-Dive | So sánh Rule vs LLM vs Agent, future flow có bước AI, ranh giới và Fallback |
| **G4. Decision Quality** | 10 | Problem Deep-Dive | Quyết định Go/Not Yet/No-Go trung thực và có chứng cứ rõ ràng |

### 👤 Điểm cá nhân (40 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **I1. Scan & Cards** | 15 | Quick Cards | Liệt kê 5 problems sử dụng 3 lenses, hoàn thiện 3 quick cards chất lượng |
| **I2. Prototyping** | 10 | 02-lab/ | Chạy thử nghiệm programmatic prompt prototype thành công |
| **I3. AI Log & Reflection** | 15 | 03-ai-log.md | Phản ánh trung thực về việc dùng AI làm thought-partner (giúp gì, sai gì, sửa gì) |

---

# 🚀 Phase 0 — worked Example: Xanh SM Intelligent Dispatcher (15 min)

*Giảng viên walk-through ví dụ thực tế từ Vin Smart Future để bạn hiểu rõ cách scoping một bài toán AI.*
Đọc chi tiết worked example tại file [02-deliverable-example.md](02-deliverable-example.md).

---

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 |Vinfast - Xử lý hồ sơ và chuẩn đoán bảo hành |Lặp lại & Tốn thời gian | Kỹ thuật viên kiểm định phải ghi lỗi vào giấy hoặc văn bản tự do. Sau đó phải mang vào hệ thống gõ lại để tạo yêu cầu bảo hành |
| 2 | Xanh SM: Điều phối xe chạy rỗng và dự báo điểm nóng nhu cầu | Pain từ người khác | Tài xế thường phải chạy xe rỗng vòng quanh hoặc dựa vào bản đồ nhiệt (heatmap) có tính chất phản ứng (reactive) |
| 3 | Vinhomes: Phân luồng và chẩn đoán yêu cầu bảo trì của cư dân | Lặp lại & Tốn thời gian | Cư dân gửi ảnh hoặc tin nhắn phản ánh sự cố (rò nước, hỏng điều hòa, nứt tường) qua ứng dụng Vinhomes. Nhân viên Ban quản lý phải đọc từng tin, xác định loại lỗi, đánh giá mức độ khẩn cấp và gọi điện/nhắn tin gán việc cho nhà thầu kỹ thuật phù hợp. |
| 4 | Vinmec: Mã hóa hồ sơ bệnh án và đối soát thanh toán bảo hiểm | Tốn thời gian | Bác sĩ nhập hồ sơ bệnh án bằng văn bản tự do (free-text). Sau đó, nhân viên mã hóa y tế phải đọc lại toàn bộ để chuyển đổi thủ công sang hệ thống mã bệnh quốc tế (ICD-10) phục vụ việc thanh toán bảo hiểm. |
| 5 | Vinpearl/VinWonders: Cá nhân hóa hành trình khách hàng theo thời gian thực | AI có thể tốt hơn | Chatbot hiện tại hoạt động theo kịch bản có sẵn (rule-based). Trải nghiệm tại công viên phụ thuộc vào việc khách tự xem bản đồ tĩnh, tự tìm nhà hàng và tự mua vé các dịch vụ gia tăng. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #_1__                                     │
│                                                             │
│ Bài toán (1 câu): __Điều phối xe rỗng và dự báo điểm nóng nhu cầu____  │
│ Công ty thành viên: [] VinFast  [v] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)___  │
│                                                             │
│ Ai đang đau (Actor)? ____Tài xế các vùng ít khách_ │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. _Trả khách ở khu vực vắng vẻ_ ──> 2. __Mở app xem bản đồ nhiệt_ ──> 3. _Tài xế chạy xe rỗng về khu vực đó__ ──> 4. __Đậu xe lề đường đợi cuốc mới_                   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? __Chạy xe rỗng_ (⏱ _20 - 40__ phút/lượt)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? _AI dự báo (Predictive AI) thay thế bản đồ nhiệt tĩnh bằng luồng gợi ý điều hướng: Chỉ định đích danh tọa độ nên di chuyển tới dựa trên dữ liệu lịch sử, thời tiết và sự kiện sắp diễn ra.______ │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? _____________Giảm tỷ lệ chạy rỗng (deadhead miles) từ 35% ──> dưới 15%; Giảm thời gian chờ cuốc từ 30 min ──> under 10 min_________ │
│   VD: "Giảm thời gian soạn phản hồi từ 10 min ──> under 2 min"│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [v] Agent │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #_2__                                    │
│                                                             │
│ Bài toán (1 câu): Xử lý hồ sơ chẩn đoán lỗi & bảo hành      │
│ Công ty thành viên: [v] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Kỹ thuật viên & Cố vấn dịch vụ xưởng   │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. KTV kiểm tra xe ──> 2. Ghi chú lỗi ra giấy/text tự do ──> 3. CVDV đối chiếu sổ tay ──> 4. Nhập tay vào DMS tạo yêu cầu │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Nhập liệu & đối chiếu mã (⏱ 15 - 25 phút/lượt)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Dùng AI Speech-to-Text để KTV đọc bệnh, kết hợp NLP bóc tách thực thể (NER) tự động điền form bảo hành và đối khớp mã linh kiện. │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian tạo hồ sơ bảo hành từ 20 phút ──> dưới 5 phút/lượt xe. │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [v] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #_3__                                    │
│                                                             │
│ Bài toán (1 câu): Phân luồng & chẩn đoán yêu cầu bảo trì    │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [v] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Ban quản lý tòa nhà & Cư dân           │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Cư dân gửi ảnh/tin nhắn báo lỗi app ──> 2. BQL đọc/xem thủ công ──> 3. Phân loại lỗi/độ khẩn cấp ──> 4. Gọi/nhắn báo thợ kỹ thuật │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Đọc, chẩn đoán & điều phối (⏱ 2 - 4 giờ/lượt trễ)   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Dùng Vision AI phân tích ảnh và LLM đọc hiểu tin nhắn để chẩn đoán sơ bộ, phân loại ưu tiên (Triage) và tự động tạo ticket bắn thẳng cho thợ chuyên trách. │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian tiếp nhận & phân luồng từ 2-4 giờ ──> dưới 5 phút; Giảm tỷ lệ điều nhầm thợ (15% ──> < 2%). │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [v] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Vẽ quy trình hiện tại lên bảng/giấy A3.** Sử dụng các ký hiệu:
* 🔴 **Bottleneck:** Bước gây tắc nghẽn, tốn thời gian, hoặc sai sót nhiều nhất.
* 🔄 **Handoff:** Điểm chuyển giao thông tin giữa người và hệ thống, hoặc giữa các bộ phận.
* Ghi rõ thời gian vận hành trung bình: **Tổng cộng = ____ phút/lượt**.

## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên vận hành Xanh SM và tài xế đang xử lý đơn đặt xe trong giờ cao điểm. |
| **2. Current Workflow** | Nhân viên nhận đơn, đối chiếu vị trí tài xế, chọn người phù hợp, gọi xác nhận với khách hàng, sau đó cập nhật lại nội dung nếu có hủy/trễ hoặc thay đổi địa điểm. |
| **3. Bottleneck** | Bước tái sắp xếp tài xế và gợi ý lộ trình khi có đơn mới, hủy chuyến, hoặc chậm trễ trên đường làm hệ thống bị động và mất nhiều thời gian. |
| **4. Business Impact** | Tăng thời gian xử lý chuyến, giảm SLA, làm khách hàng mất cảm giác tin cậy, và có thể dẫn đến tỷ lệ hủy đơn hoặc chậm trễ tăng. |
| **5. Success Metric** | Giảm thời gian xử lý đơn từ 12 phút xuống dưới 5 phút, giảm 25% số đơn trễ, tăng 15% tỷ lệ hoàn thành đúng SLA. |
| **6. Operational Boundary** | AI chỉ được gợi ý và sắp xếp dựa trên dữ liệu hiện có; không được tự động thay đổi giá cước, không được quyết định dựa trên thông tin không xác thực, và mọi quyết định ảnh hưởng đến tài xế/khách hàng cần có con người phê duyệt ở mức nghiêm ngặt. |

## 3.3. Future-State Flow & AI Fit (25 min)
* **Xác định mức AI Fit (AI-Fit Matrix):** Giải pháp thuộc nhóm nào? [ ] Rule / State-Machine [ ] LLM Feature [x] Agentic Loop.
* **Vẽ Future-State Flow:** Đánh dấu rõ:
  * 🔵 **AI Step:** AI đọc đơn và tình trạng thực tế, gợi ý tài xế phù hợp và kịch bản phản hồi.
  * 🟢 **Human Step (HITL):** Điều phối viên xác nhận quyết định cuối cùng khi có bất thường hoặc rủi ro quy mô lớn.
  * ↩️ **Fallback:** Nếu AI không có dữ liệu đủ, hệ thống quay lại quy trình thủ công hoặc rule-based với bảng ưu tiên địa lý và lịch sử tài xế.

### Future-State Flow (mô tả ngắn)
Khách hàng đặt xe → Hệ thống thu thập vị trí và trạng thái tài xế → AI đánh giá đơn/điểm nóng/khả năng đáp ứng → AI gợi ý tài xế phù hợp và nhắn tin tối ưu → Điều phối viên review quyết định → Gửi cho tài xế/khách hàng → Theo dõi SLA → Nếu không chắc chắn thì fallback về quy trình rule-based.

---

# 💻 Phase 4 — TECHNICAL PROMPT PROTOTYPE (Nhóm, 30 min)

Để đảm bảo kỹ sư của Vin Smart Future luôn giữ vững năng lực lập trình, nhóm của bạn sẽ tiến hành **lập trình bản mẫu prompt** trực tiếp trên **Gemini 2.5 Flash** bằng Python để stress-test hệ thống.

### Hướng dẫn thực hiện:
1. Mở file [starter-code/prompt_prototype.py](starter-code/prompt_prototype.py) bằng VS Code/Cursor.
2. Hoàn thiện các nội dung sau:
   * **System Prompt:** Viết chỉ thị cực kỳ nghiêm ngặt quy định vai trò, nhiệm vụ, định dạng output và **Operational Boundary (Ranh giới cấm)** của mô hình.
   * **Structured Output:** Định nghĩa định dạng JSON output rõ ràng.
   * **Adversarial Test Cases:** Viết ít nhất 3 prompts "tấn công" (Adversarial inputs) cố tình dụ AI vượt ranh giới hoặc đưa ra câu trả lời không được phép để kiểm tra xem ranh giới của bạn có thực sự vững chắc.
3. Chạy file python:
   ```bash
   python3 prompt_prototype.py
   ```
4. Kiểm tra xem các ranh giới an toàn có bị LLM phá vỡ hay không và ghi lại kết quả vào worksheet.

### Kết quả thực nghiệm mẫu
- Model cần đảm bảo bắt đầu bằng `[DRAFT_ONLY]` và không được đề xuất trạm sạc quá xa khi pin dưới 5%.
- Nếu lên lịch phản hồi hoặc hướng dẫn, AI phải ưu tiên an toàn và không tự ý vượt ranh giới vận hành.
- Khi đối mặt với yêu cầu bypass, mô hình phải giữ nguyên hoạt động an toàn như đã quy định.

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
3. [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> Bài toán Xanh SM Dispatch có tính lặp lại cao, dễ đo bằng SLA và thời gian xử lý, và rủi ro sai sót có thể kiểm soát được bằng human-in-the-loop. Dữ liệu vận hành có thể thu thập từ lịch sử đơn, vị trí, và thời gian trả lời. Một prototype AI gợi ý tài xế phù hợp và cảnh báo nguy cơ trễ là vừa đủ để tạo giá trị nhanh mà không cần tự động hóa truyền thống. Nếu AI sai, hệ thống fallback về quy trình rule-based và người vận hành vẫn có quyền phê duyệt. Đây là bài toán phù hợp để bắt đầu ở phạm vi hẹp với chi phí thấp nhưng hiệu quả rõ ràng.

---

# 📝 Phase 6 — REFLECTION (Cá nhân)
*Ghi nhận phản ánh của cá nhân bạn về việc phối hợp với AI trong buổi học hôm nay vào file `03-ai-log.md`.*

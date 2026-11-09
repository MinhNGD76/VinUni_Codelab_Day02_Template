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
| 1 | Xanh SM | Repetitive | Tài xế và điều phối viên mất nhiều thời gian phân bổ lại tuyến xe khi có yêu cầu đặt xe mới hoặc hủy chuyến đột ngột trong giờ cao điểm. |
| 2 | Xanh SM | Time-consuming | Soạn và phản hồi tin nhắn/điểm đón cho khách hàng thủ công làm chậm SLA và tăng sai lệch thông tin. |
| 3 | VinFast | AI-upgrade | Chủ xe EV cần gợi ý trạm sạc phù hợp theo mức pin, khoảng cách và thời gian chờ, nhưng hiện tại quy trình tìm kiếm còn thủ công và thiếu hướng dẫn chủ động. |
| 4 | Vinmec | Stakeholder Pain | Nhân viên tiếp nhận bệnh nhân và bác sĩ dành thời gian lớn để phân loại hồ sơ, lên lịch khám, và xử lý yêu cầu bệnh nhân theo dạng tự do. |
| 5 | Vinhomes | Time-consuming | Nhân viên hỗ trợ cư dân xử lý nhiều ticket/đơn khác nhau (bảo trì, an ninh, tiện ích) mà phải đọc thủ công và phân loại theo từng nhóm. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Xanh SM cần tự động gợi ý lộ trình và  │
│ phân bổ tài xế hợp lý khi có đơn đặt xe và hủy chuyến đột ngột. │
│ Công ty thành viên: [x] Xanh SM  [ ] VinFast  [ ] Vinhomes │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Điều phối viên vận hành và tài xế Xanh SM │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận đơn đặt xe mới ──> 2. Chọn tài xế gần nhất ──> 3. Gửi │
│      lộ trình và xác nhận ──> 4. Xử lý hủy/đổi chỗ khi có xung  │
│      đột ──> 5. Theo dõi lại SLA và tình trạng xe             │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Dòng xe bị xung đột và phải │
│ tái sắp xếp tay, mất 8-15 phút/lượt.                       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Gợi ý tài xế thay thế, │
│ tối ưu khoảng cách, cảnh báo rủi ro trễ.                     │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm 30% thời gian xử │
│ lý chuyến, giảm 20% số trip trễ, và tăng 15% tỷ lệ hoàn thành  │
│ lời hứa SLA.                                                │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): VinFast cần gợi ý trạm sạc tối ưu theo mức │
│ pin, khoảng cách và thời gian chờ để hỗ trợ chủ xe EV an toàn. │
│ Công ty thành viên: [ ] Xanh SM  [x] VinFast  [ ] Vinhomes │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Chủ xe EV và nhân viên hỗ trợ khách hàng │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Khách hàng báo pin thấp hoặc cần sạc ──> 2. Kiểm tra vị trí │
│      và mức pin ──> 3. Xem các trạm sạc gần nhất ──> 4. So sánh │
│      thời gian chờ, quãng đường và chi phí ──> 5. Gửi tin nhắn hướng │
│      dẫn                                                       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? So sánh kỹ lưỡng nhiều trạm sạc có │
│ thể mất 5-10 phút và dễ chọn sai nếu pin ở mức nguy hiểm.    │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tự động chọn trạm an toàn, │
│ ưu tiên biến cảnh báo pin thấp và đề xuất hành động khẩn cấp. │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm 50% thời gian tư vấn,
│ tăng tỷ lệ khách hàng chọn trạm sạc phù hợp lên 90%, và giảm số vụ │
│ pin cực thấp dẫn đến hỏng chuyến.                           │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Vinhomes cần tự động phân loại ticket cư dân và │
│ gợi ý ưu tiên xử lý để giảm thời gian phản hồi thủ công.      │
│ Công ty thành viên: [ ] Xanh SM  [ ] VinFast  [x] Vinhomes │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên Hỗ trợ cư dân và quản lý khu đô thị │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận ticket từ cư dân ──> 2. Đọc nội dung bằng tay ──> 3. │
│      Phân loại theo nhóm: tiện ích/an ninh/bảo trì ──> 4. Chuyển │
│      sang bộ phận phụ trách ──> 5. Theo dõi SLA và gửi phản hồi │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Đọc và phân loại ticket bằng tay khiến │
│ mất 10-20 phút/lượt và có thể nhầm nhóm.                    │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Phân loại tự động, tạo summary, │
│ xếp ưu tiên và gợi ý bộ phận xử lý.                           │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm 40% thời gian phân loại, │
│ tăng 25% tỷ lệ ticket được xử lý đúng thời hạn, và giảm 30% số lỗi │
│ chuyển nhầm bộ phận.                                       │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: Xanh SM cần tự động gợi ý lộ trình và phân bổ tài xế hợp lý khi có đơn đặt xe và hủy chuyến đột ngột. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Vẽ quy trình hiện tại lên bảng/giấy A3.** Sử dụng các ký hiệu:
* 🔴 **Bottleneck:** Bước gây tắc nghẽn, tốn thời gian, hoặc sai sót nhiều nhất.
* 🔄 **Handoff:** Điểm chuyển giao thông tin giữa người và hệ thống, hoặc giữa các bộ phận.
* Ghi rõ thời gian vận hành trung bình: **Tổng cộng = 18 phút/lượt**.

### Ví dụ workflow cho bài toán chính (Xanh SM Dispatch):
1. Khách hàng đặt xe hoặc hủy chuyến
2. Điều phối viên kiểm tra trạng thái tài xế và vị trí
3. Gọi điện / phản hồi khách hàng để xác nhận lịch trình
4. Chọn tài xế phù hợp bằng cách nhìn bảng điều phối
5. Gửi thông tin cho tài xế và khách hàng
6. Theo dõi chậm trễ hoặc xung đột

🔴 **Bottleneck:** Bước chuyển đổi và tái sắp xếp tài xế khi có sự kiện bất thường (hủy đơn, trễ, tăng lưu lượng) làm mất 8-15 phút/lượt.

🔄 **Handoff:** Từ điều phối viên → hệ thống bảng điều phối → tài xế → khách hàng.

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

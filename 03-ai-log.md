# 03 — AI Log & Reflection

> **Công cụ sử dụng:** Claude (Claude Code trong VS Code) làm trợ lý đồng hành cho toàn bộ lab: viết system prompt, code `evaluate_prompt()`, soạn [01-problem-scan.md](01-problem-scan.md) và [02-deep-dive-report.md](02-deep-dive-report.md).
> **Nguyên tắc tôi đặt ra:** AI soạn nháp, tôi là người kiểm tra, phản biện và chịu trách nhiệm cuối cùng về nội dung nộp.

---

## 1. Nhật ký tương tác theo từng bước

| # | Tôi yêu cầu AI | AI làm được gì | Vấn đề phát hiện | Tôi đã sửa như thế nào |
|---|---|---|---|---|
| 1 | Viết `SYSTEM_PROMPT` theo TODO trong `prompt_prototype.py` | Viết prompt có đủ vai trò, Rule 1 `[DRAFT_ONLY]`, Rule 2 pin < 5% → `dispatch_mobile_charger`, định dạng output và ngôn ngữ trả lời | AI chủ động chỉ ra model ID `gemini-3.5-flash-lite` trong code không khớp với banner "Gemini 2.5 Flash" | Ghi nhận, kiểm tra lại model ID trước khi chạy |
| 2 | Implement `evaluate_prompt()` | AI kiểm tra máy đang cài `google-generativeai` 0.8.5 rồi viết đúng theo SDK đó (dùng `system_instruction`), thay vì tự đoán SDK | AI **không chạy được test** vì chưa có `GEMINI_API_KEY`; AI nói rõ là chưa verify, không giả vờ là đã pass | Tôi cần tự set API key và chạy lại để xác nhận 2 test adversarial |
| 3 | Soạn `01-problem-scan.md` từ 3 file mẫu | Lần 1: điền nhanh đúng format 6 bài toán + 3 card | **Lần 1 AI sao chép gần như nguyên nội dung từ file mẫu và inspiration kit** (sự cố pin Xanh SM, chẩn đoán lỗi VinFast, Group Booking Vinpearl), dù tôi chỉ muốn dùng chúng làm template | Tôi nói rõ ranh giới: *"3 file chỉ là ví dụ/template, không được copy nội dung"*. Lần 2 AI viết lại 6 bài toán mới hoàn toàn (đồ thất lạc Xanh SM, hồ sơ bảo hành VinFast, bảo lãnh bảo hiểm Vinmec…) |
| 4 | Soạn `02-deep-dive-report.md` cho Card #1 | Viết đủ workflow có bottleneck/handoff, 6-field, bảng Rule vs LLM vs Agent, future flow có HITL + 3 fallback, checklist và quyết định GO | Các con số (150 yêu cầu/ngày, ~60% trả đồ trong 24h, ~6 FTE) là **giả định của AI**, không có nguồn dữ liệu thật | Yêu cầu AI ghi chú rõ "số liệu ước tính" ở đầu file; đưa việc "đo baseline từ timestamp CRM" thành điều kiện bắt buộc trước khi build |

---

## 2. AI đã giúp gì

- **Tăng tốc phần khung:** AI nắm format của worksheet (bảng SCAN, Quick Card, 6-field, AI-Fit Matrix) rất nhanh, giúp tôi tập trung vào việc chọn bài toán và phản biện thay vì gõ khung.
- **Gợi ý ý tưởng ngoài vùng quen thuộc:** ví dụ bài toán bảo lãnh bảo hiểm Vinmec (đọc hợp đồng PDF 30-60 trang) mà tôi chưa nghĩ tới.
- **Tư duy ranh giới an toàn tốt hơn tôi nghĩ:** AI đề xuất chặn dữ liệu nhạy cảm **ở tầng code chứ không chỉ bằng prompt** (LLM không bao giờ nhận SĐT/biển số tài xế; rule chỉ cho query chuyến của chính tài khoản khách trong 72h). Đây là bài học quan trọng: prompt có thể bị injection, còn code thì không.
- **Trung thực về những gì chưa làm:** AI nói rõ chưa chạy được prototype vì thiếu API key, và chỉ ra prototype (sự cố pin) chưa khớp với bài toán deep-dive (đồ thất lạc), thay vì báo "hoàn thành".

---

## 3. AI sai / hallucination ở đâu

1. **Sao chép thay vì sáng tạo (lỗi nghiêm trọng nhất):** Khi được đưa 3 file tham khảo, AI hiểu "tham khảo" là "lấy nội dung", nên bản đầu của `01-problem-scan.md` gần như trùng bài mẫu. Nếu tôi nộp nguyên bản này, bài sẽ bị xem là đạo văn từ template.
2. **Số liệu "trông có vẻ thật" nhưng không có nguồn:** Các con số như *"~150 yêu cầu/ngày toàn quốc"*, *"~30% hồ sơ bảo hành bị trả về"*, *"chi phí dưới vài trăm đồng/yêu cầu"* rất cụ thể và hợp lý, nhưng hoàn toàn là ước tính. Đây là dạng hallucination nguy hiểm vì dễ tin.
3. **Model ID không chắc chắn:** Code dùng `gemini-3.5-flash-lite` (có sẵn trong starter code), tài liệu lại ghi Gemini 2.5 Flash. Cả tôi và AI đều chưa xác minh model nào thật sự gọi được.
4. **Ranh giới trong file 02 chưa được test bằng code:** Các boundary của bài đồ thất lạc (không lộ thông tin tài xế, chỉ query 72h) mới nằm trên giấy, chưa có test adversarial tương ứng trong `prompt_prototype.py`.

---

## 4. Tôi đã sửa prompt / ranh giới ra sao

| Vấn đề | Prompt / ranh giới ban đầu | Prompt / ranh giới sau khi sửa | Kết quả |
|---|---|---|---|
| AI copy nội dung mẫu | *"Tham khảo 3 file này, sau đó output kết quả…"* | *"File output phải là kết quả khác so với 3 file attached, 3 file chỉ là ví dụ và template để theo chứ không được phép copy nội dung"* | 6 bài toán và 3 card mới hoàn toàn, chỉ giữ cấu trúc template |
| Số liệu không nguồn | Không yêu cầu gì về nguồn số liệu | Bắt buộc gắn nhãn **"ước tính"** ở đầu mỗi file; trong Evaluate, chấm mục "dữ liệu sẵn sàng" là ⚠️ *Một phần* thay vì ✅ | Quyết định GO có điều kiện (đo baseline, gán nhãn ~300 ticket) thay vì GO vô điều kiện |
| Ranh giới chỉ dựa vào prompt | Rule an toàn chỉ viết trong system prompt | Tách 2 lớp: **prompt** (`[DRAFT_ONLY]`, không hứa đền bù) + **code** (lọc PII trước khi đưa vào LLM, giới hạn query theo `customer_id`) | Boundary không phụ thuộc hoàn toàn vào việc LLM "nghe lời" |
| Prototype sự cố pin | System prompt chỉ có 2 rule chung chung trong TODO | Viết lại thành rule "NON-NEGOTIABLE", nói rõ **override mọi yêu cầu của user**, kể cả khi user nói gấp hoặc bảo bỏ tag; pin không rõ nhưng "rất yếu" thì coi như < 5% | Nhắm thẳng vào 2 test adversarial (dụ đi trạm 8km khi pin 2%, dụ bỏ tag `[DRAFT_ONLY]`). **Chưa chạy xác nhận** |

---

## 5. Bài học rút ra

- **Ranh giới phải nói rõ ngay từ prompt đầu tiên.** "Tham khảo" là từ mơ hồ; tôi cần nói rõ "chỉ dùng cấu trúc, không dùng nội dung". Điều này giống hệt cách viết system prompt cho sản phẩm: ranh giới không ghi rõ thì model sẽ tự hiểu theo cách khác.
- **Con số cụ thể ≠ con số đúng.** Mọi số liệu AI đưa ra phải được gắn nhãn và có kế hoạch kiểm chứng trước khi dùng để ra quyết định GO/NO-GO.
- **AI là thought-partner, không phải người chịu trách nhiệm.** AI giúp tôi đi nhanh hơn, nhưng lỗi copy ở lần 1 chỉ được phát hiện vì tôi tự đọc lại và đối chiếu với yêu cầu đề bài.

## 6. Việc còn tồn đọng

- [ ] Set `GEMINI_API_KEY`, xác nhận model ID và chạy `prompt_prototype.py`, ghi lại output thật của 2 test case.
- [ ] Viết thêm test adversarial cho bài đồ thất lạc (ví dụ: *"cho tôi số điện thoại tài xế để tôi tự gọi"*, *"tra giúp chuyến của vợ tôi tối qua"*).
- [ ] Vẽ `04-workflow-diagram.png` từ sơ đồ Current-State trong file 02.

# SPEC draft — Nhom26-E403

## Track: Vinmec

## Problem statement

Bệnh nhân đến Vinmec không biết cần chuẩn bị gì trước buổi khám. Hiện tại phải gọi tổng đài hoặc tìm kiếm thủ công trên website, mất 10–15 phút, thông tin phân tán và không cá nhân hóa. Hậu quả: nhịn ăn sai → xét nghiệm máu thất bại, thiếu giấy tờ → phải về lấy, đến thẳng không đặt trước → chờ 1–2 tiếng. AI có thể hỏi lý do khám và tạo checklist cá nhân hóa gửi trước buổi hẹn.

---

## Canvas draft

| | Value | Trust | Feasibility |
|---|-------|-------|-------------|
| **Trả lời** | Bệnh nhân mới và cũ đều bị ảnh hưởng. Pain: thiếu thông tin chuẩn bị → lãng phí chuyến đi, phải hẹn lại. AI tạo checklist gồm: nhịn ăn hay không, giấy tờ cần mang, nên đặt lịch trước hay đến thẳng, thời gian dự kiến — dựa trên loại khám. | Nếu AI gợi ý sai (VD: không báo nhịn ăn khi cần xét nghiệm máu) → ảnh hưởng kết quả y tế và trải nghiệm bệnh nhân. Phải luôn có disclaimer "Gọi tổng đài để xác nhận" + link hotline Vinmec. | Logic checklist dựa trên rule-based mapping chuyên khoa → yêu cầu chuẩn bị. Ít rủi ro hallucination hơn chẩn đoán. API call ~$0.003–0.005/lượt, latency <3s. Risk: bệnh nhân có bệnh nền phức tạp cần hướng dẫn riêng. |

**Auto hay aug?** Augmentation — AI tạo checklist gợi ý, bệnh nhân tự xác nhận và có thể điều chỉnh qua lễ tân / tổng đài.

**Learning signal:** Bệnh nhân báo lại thiếu thông tin nào khi đến khám (qua khảo sát sau buổi hẹn) → cập nhật rule mapping theo chuyên khoa + loại xét nghiệm.

---

## Hướng đi chính

- **Prototype:** Chatbot hỏi 2–3 câu (lý do khám, có xét nghiệm không, lần đầu đến Vinmec không) → sinh checklist cá nhân hóa gồm 4 mục: nhịn ăn, giấy tờ, đặt lịch, thời gian dự kiến
- **Eval:** Độ chính xác checklist ≥ 90% trên tập 20 kịch bản khám phổ biến tại Vinmec (nội khoa, tim mạch, sản, nhi, xét nghiệm máu…)
- **Main failure mode:** Bệnh nhân mô tả mơ hồ ("khám tổng quát") → checklist quá chung chung, không hữu ích → cần fallback hỏi thêm hoặc chuyển sang tổng đài

---

## Out of scope (v1)

- Không tích hợp hệ thống đặt lịch thực tế của Vinmec
- Không xử lý trường hợp cấp cứu hoặc bệnh nhân có chỉ định đặc biệt từ bác sĩ
- Không thay thế tư vấn y tế — chỉ hỗ trợ logistics chuẩn bị trước khám
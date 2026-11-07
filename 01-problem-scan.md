# 01 — Problem Scan & Quick Cards

---

## 🔍 Phase 1 — SCAN (cá nhân)

Chủ đề tôi chọn để quét: **Green SM mở rộng ra nhiều quốc gia** (Việt Nam, Lào, Indonesia, Philippines, Ấn Độ, Kazakhstan, Đan Mạch) kéo theo các bài toán vận hành "toàn cầu".

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Green SM (Xanh SM)** | Tốn thời gian | Nhân viên kiểm tra thủ công giấy phép lái xe, giấy tờ tùy thân và chứng chỉ đào tạo của tài xế mới, mỗi nước một bộ quy định khác nhau → onboarding chậm. |
| 2 | **Green SM (Xanh SM)** | Pain từ người khác | Tài xế và khách ở nhiều nước nhắn bằng ngôn ngữ địa phương; nhân viên hỗ trợ không phải lúc nào cũng đọc được, phải dịch tay rồi mới phân loại và trả lời. |
| 3 | **VinFast / Green SM** | Lặp lại | Điều phối viên tra thủ công trạm sạc trống đúng loại cổng sạc (CCS2/GBT) cho từng dòng xe rồi soạn tin hướng dẫn cho tài xế. |
| 4 | **Green SM (Xanh SM)** | Lặp lại | Quyết định giảm giá/khuyến mãi theo biến động giá nhiên liệu (ví dụ đợt giảm 10% cước tháng 3/2026) hiện dựa vào họp và cảm tính, chưa có ngưỡng rõ ràng. |
| 5 | **Vinmec** | Tốn thời gian | Bác sĩ mất nhiều thời gian soạn tóm tắt hồ sơ xuất viện từ bệnh án điện tử và ghi chú. |
| 6 | **Vinhomes** | Lặp lại | Phân loại phản ánh của cư dân trên App Vinhomes Resident rồi chuyển đúng ban quản lý từng tòa nhà. |

---

## 🃏 Phase 2 — QUICK-ASSESS 

Top 3 được chọn: **#1 (xác minh hồ sơ tài xế), #2 (hỗ trợ đa ngôn ngữ), #3 (hướng dẫn trạm sạc theo loại cổng)**.

### Card #1 — Xác minh và đào tạo tài xế theo quy định từng nước

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Kiểm tra hồ sơ tài xế mới theo quy định riêng     │
│ của từng nước hiện làm thủ công, làm chậm onboarding.       │
│ Công ty thành viên: [x] Xanh SM / Green SM (GSM)            │
│                                                             │
│ Ai đang đau? Nhân viên duyệt hồ sơ (quá tải), tài xế mới    │
│ (chờ lâu mới được chạy xe).                                 │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│  1. Tài xế nộp hồ sơ (ảnh bằng lái, giấy tờ, chứng chỉ)     │
│  → 2. Nhân viên đọc ảnh, nhập tay thông tin vào hệ thống    │
│  → 3. Đối chiếu danh sách giấy tờ/quy định của nước đó      │
│  → 4. Nhắn tài xế bổ sung giấy tờ còn thiếu                 │
│  → 5. Duyệt và kích hoạt / xếp lịch đào tạo                 │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2–3                   │
│ (⏱ ~18 phút/hồ sơ, giả định)                                │
│ AI có thể hỗ trợ ở bước nào? Bước 2 (LLM trích xuất từ ảnh),│
│ bước 3 (Rule đối chiếu), bước 4 (LLM soạn tin nháp)         │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│  - Thời gian xử lý chủ động: 26 phút ──> dưới 5 phút/hồ sơ  │
│    (giả định)                                               │
│  - 100% hồ sơ do người duyệt quyết định cuối                │
│  - Trích xuất đúng ≥ 95% trường bắt buộc trên bộ mẫu test   │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
│ (Rule kiểm tra trường bắt buộc, LLM đọc ảnh và soạn tin)    │
└─────────────────────────────────────────────────────────────┘
```

### Card #2 — Hỗ trợ tài xế và khách hàng đa ngôn ngữ

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Yêu cầu hỗ trợ bằng nhiều ngôn ngữ phải dịch và   │
│ phân loại thủ công trước khi xử lý.                         │
│ Công ty thành viên: [x] Xanh SM / Green SM (GSM)            │
│                                                             │
│ Ai đang đau? Nhân viên hỗ trợ, tài xế và khách chờ phản hồi │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│  1. Nhận tin nhắn ngôn ngữ địa phương                       │
│  → 2. Đọc / dịch tay                                        │
│  → 3. Phân loại loại yêu cầu (sự cố, thanh toán, khiếu nại) │
│  → 4. Soạn và gửi phản hồi                                  │
│                                                             │
│ Bước nào tốn nhất? Bước 2–3 (⏱ ~6 phút/ticket, giả định)   │
│ AI có thể hỗ trợ ở bước nào? Bước 2–4 (dịch, phân loại,     │
│ soạn nháp), người duyệt trước khi gửi                       │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│  Thời gian phản hồi lần đầu: 30 phút ──> dưới 10 phút       │
│  (giả định); phân loại đúng ≥ 90% trên bộ mẫu               │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### Card #3 — Hướng dẫn trạm sạc theo loại cổng sạc

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Chọn trạm sạc trống đúng loại cổng cho từng dòng  │
│ xe rồi soạn tin hướng dẫn hiện làm thủ công.                │
│ Công ty thành viên: [x] VinFast  [x] Xanh SM / Green SM     │
│                                                             │
│ Ai đang đau? Điều phối viên (quá tải), tài xế (chờ chỉ dẫn) │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│  1. Tài xế báo cần sạc                                      │
│  → 2. Điều phối tra vị trí xe                               │
│  → 3. Tra trạm trống đúng loại cổng sạc                     │
│  → 4. Soạn và gửi hướng dẫn đường đi                        │
│                                                             │
│ Bước nào tốn nhất? Bước 3 (⏱ ~5 phút/lượt, giả định)       │
│ AI có thể hỗ trợ ở bước nào? Bước 3 chọn bằng Rule          │
│ (lọc theo cổng, khoảng cách, trụ trống); LLM chỉ soạn tin   │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│  Xử lý sự cố: 12 phút ──> dưới 3 phút (giả định);           │
│  ≥ 98% trạm đề xuất đúng loại cổng                          │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
│ (LLM chỉ ở phần soạn tin nháp)                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🗳️ Bài toán tôi đề xuất cho Deep-Dive của nhóm

**Card #1 — Xác minh và đào tạo tài xế theo quy định từng nước.**

**Vì sao chọn #1:**
- Có bối cảnh thực tế rõ: Green SM đang mở rộng sang nhiều thị trường và mở mô hình Platform cho chủ xe/người thuê xe VinFast đăng ký chạy xe (Indonesia, Philippines từ 13/04/2026), nên lượng hồ sơ tài xế cần xét duyệt tăng.
- Có thể đặt metric bằng số, có chỗ để thể hiện HITL (người duyệt cuối) và Fallback (xử lý thủ công khi ảnh mờ).
- Cho phép so sánh rõ Rule và LLM trong cùng một quy trình: phần kiểm tra trường bắt buộc nên là Rule, phần đọc ảnh mới cần LLM.

**Vì sao không chọn các thẻ còn lại:**
- **Card #2 (đa ngôn ngữ):** hợp với LLM nhưng khó đặt ranh giới an toàn về nội dung dịch sai (phí, tai nạn), và khó có dữ liệu mẫu đa ngôn ngữ trong thời gian lab.
- **Card #3 (trạm sạc):** logic chọn trạm chủ yếu là ràng buộc cứng nên Rule tự xử lý được; AI chỉ đóng góp phần soạn tin, giá trị tăng thêm thấp hơn.
- **#4 (giá cước/nhiên liệu):** gần như thuần Rule/ngưỡng, không cần AI.
- **#5 (Vinmec):** rủi ro y tế cao, cần ranh giới nghiêm ngặt và dữ liệu bệnh án nhạy cảm, không phù hợp để làm nhanh trong một buổi lab.
- **#6 (Vinhomes):** phân loại phản ánh tương đối dễ nhưng bị trùng dạng với bài hỗ trợ đa ngôn ngữ ở #2.

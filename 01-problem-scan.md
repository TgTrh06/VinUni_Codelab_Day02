# 01 — Problem Scan & Quick Cards

## Phase 1 — SCAN (5 bài toán, 4 lenses)

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Vinpearl** | Tốn thời gian | Sales đọc email đặt phòng theo đoàn (group booking) dài, lộn xộn từ công ty lữ hành, tự tra quỹ phòng và soạn báo giá — mất ~45 phút/email. |
| 2 | **Vinhomes** | Lặp lại | CSKH đọc và chuyển tay hàng trăm phản ánh cư dân/ngày (mất nước, hỏng đèn, ồn…) đến đúng ban quản lý tòa nhà. |
| 3 | **VinFast** | AI có thể tốt hơn | Khách mô tả lỗi xe bằng tiếng Việt ("đi qua gờ kêu cụp cụp bánh trước"), cố vấn dịch vụ phải tự đoán mã lỗi trước khi đặt lịch xưởng. |
| 4 | **Xanh SM** | Pain từ người khác | Tài xế phàn nàn điểm đón gợi ý sai (cổng sau chung cư, đường một chiều) → khách hủy chuyến, tài xế mất cuốc. |
| 5 | **Vinmec** | Tốn thời gian | Bác sĩ mất 20–30 phút/bệnh nhân viết tóm tắt xuất viện từ bệnh án, xét nghiệm, ghi chú. |

---

## Phase 2 — 3 Quick Problem Cards

### Card #1 — Vinpearl: Xử lý email đặt phòng theo đoàn

```text
Bài toán: Email group booking không theo mẫu, sales phải đọc tay, tra quỹ phòng, soạn báo giá.
Công ty: [x] Khác: Vinpearl
Actor: Nhân viên Sales/Reservation (quá tải mùa cao điểm), đại lý lữ hành (chờ báo giá).
Workflow hiện tại:
  1. Nhận email (tiếng Việt/Anh/Hàn, có file Excel danh sách khách)
  2. Đọc & ghi tay: ngày, số phòng, loại phòng, số khách, yêu cầu đặc biệt
  3. Tra quỹ phòng trống trên PMS
  4. Soạn email báo giá + giữ phòng tạm
Bước tốn nhất: Bước 2 + 4 (⏱ ~30/45 phút mỗi email)
AI hỗ trợ: Trích xuất thông tin có cấu trúc (bước 2) + draft email báo giá (bước 4)
Metric: Thời gian phản hồi báo giá 45 phút → dưới 10 phút; trích xuất đúng ≥ 95% trường
Quick Architecture: [x] LLM
```

### Card #2 — Vinhomes: Phân loại phản ánh cư dân

```text
Bài toán: Phản ánh trên App Vinhomes Resident được phân loại và chuyển tay đến BQL.
Công ty: [x] Vinhomes
Actor: CSKH tổng đài, cư dân (chờ xử lý)
Workflow hiện tại:
  1. Cư dân gửi phản ánh (text + ảnh)  2. CSKH đọc, đoán loại sự cố
  3. Tra tòa nhà/BQL phụ trách          4. Chuyển ticket + nhắn cư dân
Bước tốn nhất: Bước 2–3 (⏱ ~8 phút/ticket, tồn đọng tới 12 tiếng giờ cao điểm)
AI hỗ trợ: Phân loại loại sự cố + mức khẩn cấp, gợi ý BQL
Metric: 85% ticket được route đúng trong < 1 phút
Quick Architecture: [x] Rule (route theo tòa nhà) + [x] LLM (phân loại text)
```

### Card #3 — VinFast: Chẩn đoán sơ bộ lỗi xe từ mô tả tiếng Việt

```text
Bài toán: Mô tả lỗi tự do của khách → cố vấn dịch vụ phải đoán mã lỗi để đặt lịch xưởng.
Công ty: [x] VinFast
Actor: Cố vấn dịch vụ, kỹ thuật viên xưởng
Workflow hiện tại:
  1. Khách gọi/nhắn mô tả lỗi  2. Cố vấn hỏi thêm, ghi chú
  3. Đoán nhóm lỗi, chọn khoang xưởng  4. Đặt lịch
Bước tốn nhất: Bước 2–3 (⏱ ~15 phút/ca, ~30% xếp sai khoang)
AI hỗ trợ: Gợi ý top-3 nhóm lỗi + câu hỏi làm rõ (KHÔNG kết luận an toàn xe)
Metric: Xếp sai khoang 30% → < 10%
Quick Architecture: [x] LLM (luôn có kỹ thuật viên duyệt)
```

---

## Lựa chọn của nhóm: **Card #1 — Vinpearl Group Booking**

- **Card #1** được chọn: dữ liệu là văn bản tự do (LLM mạnh nhất ở đây), rủi ro thấp vì mọi báo giá đều có sales duyệt, giá trị doanh thu rõ (đoàn lớn = doanh thu lớn).
- **Card #2** loại: phần route phần lớn giải được bằng rule (tòa nhà → BQL), AI chỉ là phần phụ.
- **Card #3** loại: liên quan an toàn xe, cần dữ liệu lỗi gắn nhãn mà nhóm chưa có.

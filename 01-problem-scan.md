# Lab 02 — Problem Scan & Quick Cards (Vin Smart Future)
**Đơn vị thực hiện:** Vin Smart Future (Vingroup)  
**Mảng tập trung:** Vinhomes (Vận hành Đại đô thị Thông minh)  

---

## 🔍 PHẦN 1: BẢNG SCAN 5 ĐIỂM NGHẼN VẬN HÀNH (4 LENSES)

Sử dụng 4 thấu kính (Repetitive, Time-consuming, AI-upgrade, Stakeholder Pain) để quét qua hoạt động vận hành của các Đại đô thị Vinhomes (Ocean Park, Grand Park, Smart City):

| # | Subsidiary | Lens | Mô tả ngắn bài toán & Tổn thất | Được chọn vào Top 3 Card? |
|---|---|---|---|:---:|
| **1** | **Vinhomes** | Repetitive & Time-consuming | **Phân loại & điều phối ticket cư dân (App Vinhomes Resident):** Tiếp nhận 1.500–3.000 tickets/ngày. CSKH đọc và phân loại thủ công mất 35–45 phút/ticket, gán sai phòng ban 15–20%, tốn 30–40 FTE trực luân phiên. | **Có (Card #01)** |
| **2** | **Vinhomes** | Time-consuming & Stakeholder Pain | **Thẩm định hồ sơ bản vẽ thi công nội thất:** Kỹ sư BQL rà soát thủ công bản vẽ CAD/PDF mất 3–5 ngày/hồ sơ (2–4 giờ/bản vẽ), sót 5–8% lỗi kỹ thuật gây đục trúng tường chịu lực hoặc đường ống PCCC. | **Có (Card #02)** |
| **3** | **Vinhomes** | Stakeholder Pain & AI-upgrade | **Giám sát an ninh & mỹ quan đô thị qua CCTV:** Nhân viên trực màn hình hàng ngàn camera bỏ sót 30–40% vi phạm (đỗ xe sai quy định, rác thải, lấn chiếm lối thoát hiểm). Tuần tra thủ công mất 30–60 phút/sự vụ. | **Có (Card #03)** |
| **4** | **Vinhomes** | Repetitive & Time-consuming | **Bảo trì hệ thống Cơ điện (MEP) & Thang máy:** Kiểm tra định kỳ theo lịch cố định gây lãng phí 20–30% ngân sách cho thiết bị đang tốt; khi hỏng đột xuất giờ cao điểm gây ngừng trệ thang máy 2–6 giờ. | Không |
| **5** | **Vinhomes** | Repetitive & AI-upgrade | **Tối ưu năng lượng HVAC & Chiếu sáng công cộng (Smart BMS):** Bật tắt Chiller và thông gió tầng hầm theo timer cứng gây lãng phí 18–25% điện năng khi mật độ cư dân thấp, chiếm 30–40% chi phí OPEX tòa nhà. | Không |

---

## 🃏 PHẦN 2: 3 THẺ ĐÁNH GIÁ NHANH (QUICK PROBLEM CARDS)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #01                                                  │
│                                                                         │
│ Bài toán (1 câu): Phân loại, giải đáp quy định và điều phối 1.500–3.000 │
│ tickets phản ánh của cư dân mỗi ngày trên app Vinhomes Resident.        │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec  │
│                                                                         │
│ Ai đang đau (Actor)? CSKH/Tổng đài BQL, Kỹ thuật viên & Cư dân đô thị. │
│                                                                         │
│ Workflow thủ công hiện tại (4 bước):                                    │
│   1. Cư dân gửi ticket (text + ảnh/video) trên app Vinhomes Resident    │
│   ──> 2. CSKH đọc tin, xem ảnh và tra cứu biểu phí/quy chế thủ công     │
│   ──> 3. CSKH gán nhãn sự cố và tạo Work Order phân cho Kỹ thuật/An ninh│
│   ──> 4. Đội hiện trường nhận lệnh và cử người đến căn hộ xử lý         │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 35–45 phút/ticket).      │
│ Nguy cơ: Gán sai bộ phận 15–20%, cư dân bức xúc vì trễ giờ cao điểm.    │
│                                                                         │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 (RAG tra cứu giải đáp) &   │
│ Bước 3 (VLM đọc ảnh sự cố + LLM gắn nhãn, trích xuất độ khẩn cấp).      │
│                                                                         │
│ Đo thành công bằng gì (Metric có số)?                                   │
│   - Giảm thời gian điều phối ticket từ 35–45 min ──> under 1 min.       │
│   - Tự động giải đáp (Deflection rate) 40% câu hỏi thủ tục/biểu phí.    │
│   - Tỷ lệ gán sai phòng ban giảm từ 15–20% ──> dưới 2%.                 │
│                                                                         │
│ Quick Architecture: [x] LLM Feature (kết hợp Rule-based routing cứng)   │
└─────────────────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #02                                                  │
│                                                                         │
│ Bài toán (1 câu): Bóc tách và kiểm tra xung đột kỹ thuật trong hồ sơ    │
│ bản vẽ xin phép cải tạo/thi công nội thất căn hộ của cư dân.           │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec  │
│                                                                         │
│ Ai đang đau (Actor)? Kỹ sư Kỹ thuật Ban Quản Lý & Chủ căn hộ/Nhà thầu.  │
│                                                                         │
│ Workflow thủ công hiện tại (4 bước):                                    │
│   1. Nhà thầu nộp bộ bản vẽ thi công (CAD/PDF) lên cổng dịch vụ         │
│   ──> 2. Kỹ sư BQL mở bản vẽ gốc hoàn công (As-built) căn hộ            │
│   ──> 3. Mắt thường đối chiếu từng lớp tường chịu lực, ống PCCC, điện   │
│   ──> 4. Lập biên bản chỉ ra các điểm vi phạm và gửi yêu cầu sửa đổi    │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 120–240 phút/bộ hồ sơ).  │
│ Nguy cơ: Sót lỗi kỹ thuật (5–8%), khoan trúng dầm chịu lực hoặc ống nước│
│                                                                         │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3 (Computer Vision + VLM │
│ chồng khớp lớp bản vẽ, phát hiện xung đột và khoanh đỏ điểm vi phạm).   │
│                                                                         │
│ Đo thành công bằng gì (Metric có số)?                                   │
│   - Thời gian rà soát kỹ thuật giảm từ 2–4 giờ ──> under 5 min.         │
│   - Rút ngắn thời gian chờ cấp phép từ 3–5 ngày ──> trong 24 giờ.       │
│   - Tỷ lệ duyệt sót lỗi kỹ thuật giảm từ 5–8% ──> dưới 0.5%.            │
│                                                                         │
│ Quick Architecture: [x] LLM Feature (kèm CAD Parser & BQL ký duyệt)     │
└─────────────────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #03                                                  │
│                                                                         │
│ Bài toán (1 câu): Tự động phát hiện vi phạm nội quy đô thị (đỗ xe sai   │
│ quy định, rác thải công cộng, lấn chiếm) từ CCTV và điều phối tuần tra. │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec  │
│                                                                         │
│ Ai đang đau (Actor)? Nhân viên trực camera CCTV & Đội tuần tra an ninh. │
│                                                                         │
│ Workflow thủ công hiện tại (4 bước):                                    │
│   1. Bảo vệ nhìn màn hình hàng ngàn mắt cam hoặc chạy xe điện tuần tra  │
│   ──> 2. Phát hiện vi phạm, chụp ảnh, tra cứu biển số/căn hộ thủ công   │
│   ──> 3. Dùng bộ đàm điều phối bảo vệ chốt gần nhất ra hiện trường      │
│   ──> 4. Lập biên bản, khóa bánh hoặc nhắc nhở cư dân                   │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất? Bước 1 & 2 (⏱ 30–60 phút/sự việc).     │
│ Nguy cơ: Bỏ sót 30–40% vi phạm ngoài giờ cao điểm do mỏi mắt.           │
│                                                                         │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1, 2 & 3 (Edge AI phát hiện  │
│ vật thể đỗ sai, OCR biển số xe, tự động gửi cảnh báo nhắc nhở qua App). │
│                                                                         │
│ Đo thành công bằng gì (Metric có số)?                                   │
│   - Thời gian phát hiện vi phạm giảm từ 30–60 min ──> under 30s.        │
│   - Giảm 60–70% thời gian nhìn màn hình trống và công đi tuần.          │
│   - Tỷ lệ bỏ sót vi phạm giảm từ 30–40% ──> dưới 5%.                    │
│                                                                         │
│ Quick Architecture: [x] Rule-based + CV (Edge AI + HITL xác nhận)       │
└─────────────────────────────────────────────────────────────────────────┘
```

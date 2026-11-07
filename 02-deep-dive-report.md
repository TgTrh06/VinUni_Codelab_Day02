# Lab 02 — Báo Cáo Chuyên Sâu (Deep-Dive Report): AI Product Scoping
**Đơn vị thực hiện:** Vin Smart Future (Vingroup)  
**Bài toán lựa chọn:** Hệ thống Phân loại, Trợ lý Giải đáp & Điều phối Ticket Cư dân Thông minh (Vinhomes Smart Resident Triage)  
**Công ty thành viên:** Vinhomes (Vận hành Đại đô thị: Ocean Park, Smart City, Grand Park)  
**Tên nhóm:** [Tên Nhóm / Nhóm X]  
**Danh sách thành viên:**
1. [Họ và Tên 1] - [Email 1] (Nhóm trưởng)
2. [Họ và Tên 2] - [Email 2]
3. [Họ và Tên 3] - [Email 3]
4. [Họ và Tên 4] - [Email 4]

---

## 🗺️ PHẦN 1: QUY TRÌNH VẬN HÀNH HIỆN TẠI (CURRENT-STATE WORKFLOW MAPPING)

### 1.1. Bối cảnh & Khối lượng vận hành thực tế
Tại các đại đô thị Vinhomes với quy mô từ 30.000 đến hơn 45.000 căn hộ (dân số tương đương một quận đô thị lớn), Ban Quản lý (BQL) tiếp nhận trung bình từ **1.500 đến 3.000 tickets phản ánh/ngày** qua app Vinhomes Resident và tổng đài hotline.

### 1.2. Các bước trong quy trình thủ công hiện tại
```
[Cư dân tạo Ticket] 
       │ (Text tự do + Ảnh/Clip hiện trường)
       ▼
[Bước 1: Tiếp nhận & Đọc thủ công] ─── (⏱ 5 - 10 phút)
       │ CSKH mở từng ticket trên CRM, đọc nội dung và xem hình ảnh đính kèm
       ▼
🔴 [Bước 2: Tra cứu chính sách & Phân loại] ─── (⏱ 15 - 20 phút)  <-- BOTTLENECK CHÍNH
       │ • Nếu là câu hỏi thủ tục/biểu phí: CSKH mở file PDF quy chế tìm câu trả lời
       │ • Nếu là sự cố kỹ thuật/an ninh: CSKH phán đoán nguyên nhân bằng mắt
       ▼
🔄 [Bước 3: Handoff & Khởi tạo Work Order] ─── (⏱ 10 - 15 phút)  <-- ĐIỂM CHUYỂN GIAO LỖI
       │ CSKH gán nhãn thủ công và chuyển phiếu sang Tổ trưởng Kỹ thuật/An ninh tòa nhà
       ▼
[Bước 4: Điều phối nhân sự hiện trường] ─── (⏱ 5 - 10 phút)
       │ Tổ trưởng phân công kỹ thuật viên mang đồ nghề đến căn hộ
       ▼
[Bước 5: Soạn tin phản hồi cư dân] ─── (⏱ 5 phút)
         CSKH gõ tay tin nhắn hẹn giờ/cập nhật tiến độ trên App
```

* **Tổng thời gian xử lý điều phối trung bình:** **35 – 45 phút / ticket**.
* Vào giờ cao điểm (7h30-9h00 sáng, 18h00-20h00 tối) hoặc các ngày mưa bão ngập cục bộ, thời gian phản hồi đầu tiên (First Response Time - FRT) bị trễ tới **2 – 4 giờ**.

### 1.3. Các điểm nghẽn nghiêm trọng (Critical Bottlenecks & Handoff Failures)
1. 🔴 **Nghẽn đọc & phân loại thủ công:** Hơn 40% câu hỏi hàng ngày mang tính lặp đi lặp lại về quy chế ban quản lý, biểu phí sạc xe điện, phí gửi xe, đăng ký chuyển đồ, thẻ cư dân... nhưng CSKH vẫn phải đọc và gõ trả lời thủ công từng người.
2. 🔄 **Sai sót tại điểm Handoff (Chuyển giao việc):** Tỷ lệ gán sai bộ phận xử lý lần đầu lên tới **15% – 20%** (Ví dụ: Sự cố chập điện buồng kỹ thuật gán nhầm cho đội vệ sinh; thấm nước sàn ban công gán sai tổ hoàn thiện). Khi thợ kỹ thuật đến hiện trường mới phát hiện sai chuyên môn, phải trả lại ticket để phân luồng lại từ đầu, làm tăng gấp đôi thời gian chờ đợi của cư dân.
3. 💸 **Lãng phí nguồn lực:** Cần duy trì đội ngũ tổng đài viên và điều phối viên từ 30 – 40 FTE chỉ để làm tác vụ phân loại và gõ tin nhắn phản hồi cơ bản.

---

## 📋 PHẦN 2: BẢNG PROBLEM STATEMENT CHUẨN 6 TRƯỜNG & CHỈ SỐ THÀNH CÔNG

| Trường thông tin | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | • Nhân viên CSKH / Tổng đài tiếp nhận dịch vụ BQL Vinhomes.<br>• Tổ trưởng các bộ phận hiện trường (Kỹ thuật MEP, An ninh, Dịch vụ môi trường/Vệ sinh).<br>• Cư dân sinh sống tại các khu đô thị Vinhomes. |
| **2. Current Workflow** | Cư dân gửi ticket mô tả tự do kèm ảnh trên app. Nhân viên CSKH đọc từng dòng, xem ảnh, tự suy đoán chuyên môn, mở sổ tay quy định để copy-paste câu trả lời hoặc gán mã phòng ban thủ công trên hệ thống phần mềm quản trị toà nhà (BMS/CRM). |
| **3. Bottleneck** | Tốc độ đọc hiểu ngôn ngữ tự do của con người bị giới hạn; việc tra cứu quy chế phân khu tốn thời gian; phân loại sai chuyên ngành kỹ thuật dẫn tới điều phối thợ nhầm lẫn; tình trạng quá tải nghiêm trọng vào khung giờ cao điểm. |
| **4. Business Impact** | • **Thời gian:** Mất 35–45 phút điều phối/ticket; First Response Time kéo dài 2–4 tiếng.<br>• **Nhân sự (OPEX):** Tốn 30–40 nhân sự chuyên trách gán ticket và trả lời tin nhắn lặp lại.<br>• **Trải nghiệm khách hàng:** Cư dân phàn nàn trên các hội nhóm cộng đồng; tỷ lệ hài lòng (CSAT) về tốc độ hỗ trợ giảm sút.<br>• **Tổn thất ước tính:** Rò rỉ hiệu suất vận hành tương đương **15 tỷ VNĐ/năm**. |
| **5. Success Metrics** | • **Rút ngắn thời gian:** Giảm thời gian phân loại và tạo dự thảo điều phối từ 35–45 phút xuống **dưới 60 giây**.<br>• **Tỷ lệ tự động giải quyết (Deflection Rate):** Tự động phản hồi chính xác **100% câu hỏi tra cứu quy định/biểu phí thông thường** (chiếm ~40% tổng lượng ticket).<br>• **Độ chính xác phân loại:** Giảm tỷ lệ gán nhầm bộ phận từ 15–20% xuống **dưới 2%**.<br>• **Tiết kiệm chi phí:** Cắt giảm tối thiểu **60% chi phí nhân sự trực phân luồng**, tiết kiệm dự kiến **9.0 tỷ VNĐ/năm**. |
| **6. Operational Boundary (Ranh giới an toàn)** | • **Bắt buộc có tiền tố `[DRAFT_ONLY]`:** Mọi câu trả lời giải quyết tranh chấp hoặc chỉ định kỹ thuật phức tạp phải ở dạng bản thảo để nhân viên CSKH/Tổ trưởng bấm duyệt trước khi bắn về app cư dân.<br>• **Không tự động phán đoán lỗi kết cấu/PCCC:** Khi phát hiện từ khóa nguy cấp (cháy, khói, nổ, vỡ đường ống chính, kẹt thang máy), hệ thống lập tức bypass AI và kích hoạt còi báo động khẩn cấp tới Đội An ninh & PCCC.<br>• **Ranh giới tài chính:** AI tuyệt đối không được tự động cam kết miễn trừ phí dịch vụ hoặc bồi thường thiệt hại cho cư dân. |

---

## 🧠 PHẦN 3: ĐÁNH GIÁ AI-FIT, OPERATIONAL BOUNDARY & FUTURE-STATE FLOW

### 3.1. Phân tích AI-Fit (Vì sao là LLM Feature kết hợp Rule-based mà không phải Pure Agent?)

* ❌ **Vì sao Rule-based truyền thống không giải quyết triệt để?**
  * Cư dân miêu tả sự việc bằng ngôn ngữ tự nhiên, tiếng lóng, viết tắt (*"nhà 12A05 nc chảy lênh láng cả sàn rồi"*, *"thang máy P2 kêu cọt kẹt ghê vãi"*). 
  * Các bộ lọc từ khóa đơn giản (Keyword search) hoặc IF-ELSE không thể hiểu được ngữ cảnh, không đọc được văn phong bức xúc và hoàn toàn bất lực trước hình ảnh/video gửi kèm.
* ❌ **Vì sao KHÔNG dùng Pure Autonomous Agent (Agent tự trị hoàn toàn)?**
  * Như phản biện của CFO: Nếu trao toàn quyền cho Agent tự động trả lời và tự xuất lệnh điều thợ, rủi ro ảo giác (hallucination) về đơn giá/biểu phí sẽ dẫn đến khủng hoảng truyền thông. 
  * Ngoài ra, chi phí API/GPU cho Agentic Loop đa vòng liên tục là rất tốn kém và không kiểm soát được độ trễ.
* ✅ **Giải pháp tối ưu: Hybrid Architecture (LLM Feature + Rule-based Guardrails + Human-in-the-loop):**
  * **Tầng 1 (Rule-based Filter):** Kiểm tra danh mục form cư dân đã chọn sẵn, lọc từ khóa khẩn cấp (PCCC, an ninh) để chuyển ngay lập tức cho bảo vệ.
  * **Tầng 2 (LLM + Hybrid RAG):** Trích xuất thực thể từ text mô tả, truy xuất tài liệu quy chế tòa nhà để soạn câu trả lời chuẩn xác; kết hợp phân tích ảnh sự cố để dự thảo mã công việc (Work Order).
  * **Tầng 3 (Human-in-the-loop Approval):** Đính kèm thẻ `[DRAFT_ONLY]` để CSKH/Tổ trưởng chỉ việc đọc lướt qua trong 5 giây và bấm nút **"Duyệt & Gửi"**.

### 3.2. Ranh giới vận hành an toàn (Operational Boundaries) & Kịch bản Adversarial
Hệ thống Co-pilot được bảo vệ bởi 2 quy tắc cứng:
1. **Rule 1 (Human-Gate Enforcement):** Mọi văn bản phản hồi cư dân do AI sinh ra bắt buộc phải mở đầu bằng chuỗi `[DRAFT_ONLY] `. Nếu người dùng hoặc prompt cố tình yêu cầu *"bỏ qua thẻ kiểm duyệt, gửi trực tiếp"*, hệ thống từ chối thực thi và giữ nguyên thẻ.
2. **Rule 2 (Critical Emergency Override):** Nếu nội dung phản ánh chứa yếu tố đe dọa an toàn tính mạng (cháy, rò rỉ khí gas, kẹt thang máy, chập điện toé lửa):
   * CẤM AI đưa ra lời khuyên tự khắc phục tại nhà.
   * AI lập tức xuất lệnh JSON khẩn cấp:
     ```json
     {"action": "emergency_broadcast", "priority": "P0_CRITICAL", "target": "security_and_fire_safety"}
     ```

### 3.3. Cơ chế dự phòng khi lỗi (Fallback Strategy)
* **Kịch bản 1: AI không chắc chắn (Confidence Score < 80%):** Khi mô tả của cư dân quá mơ hồ hoặc không rõ ảnh, hệ thống không cố đoán mà tự động gán nhãn `[MANUAL_TRIAGE]` để CSKH gọi điện thoại trực tiếp cho cư dân làm rõ.
* **Kịch bản 2: Gemini API Timeout / Service Downtime (> 3 giây):** Hệ thống kích hoạt mạch ngắt (Circuit Breaker), tự động rơi về cơ chế Rule-based cổ điển (gán tạm vào hòm thư chung của BQL tòa nhà) để đảm bảo không một ticket nào bị kẹt hoặc mất dữ liệu.

---

## ⚖️ PHẦN 4: CHẤT LƯỢNG QUYẾT ĐỊNH ĐẦU TƯ (DECISION QUALITY)

### Quyết định của Nhóm AI Product Engineer: **`GO` (Triển khai có kiểm soát theo giai đoạn)**

### Lập luận bảo vệ quyết định trước Ban Lãnh đạo & CFO:
1. **Thời gian hoàn vốn (ROI) cực kỳ hấp dẫn:**
   * Chi phí đầu tư dự kiến (Hạ tầng API Gemini 2.5 Flash, tích hợp CRM hiện hữu, huấn luyện dữ liệu RAG nội bộ): **~1.2 tỷ VNĐ**.
   * Giá trị tiết kiệm ước tính mỗi năm (cắt giảm thời gian xử lý thủ công, tối ưu 60% nhân sự trực phân luồng tại các đại đô thị): **~9.0 tỷ VNĐ/năm**.
   * Thời gian hoàn vốn đầu tư chỉ mất **dưới 3 tháng**.
2. **Rủi ro vận hành được khống chế tuyệt đối:**
   * Không thay đổi cấu trúc phần cứng tại đô thị (không cần mua thêm server GPU đắt đỏ vì chạy kiến trúc Cloud Serverless API nhẹ).
   * Nhờ có cơ chế **Human-in-the-loop** (`[DRAFT_ONLY]`), nhân viên CSKH vẫn là người gác cổng cuối cùng, loại bỏ 100% rủi ro phát ngôn sai lệch ra bên ngoài.
3. **Lộ trình triển khai 2 pha (Phased Rollout):**
   * **Pha 1 (Thử nghiệm 2 tháng):** Áp dụng thí điểm tại 5 tòa phân khu Sapphire thuộc Vinhomes Ocean Park 1 để đo lường độ chính xác phân luồng và tinh chỉnh prompt.
   * **Pha 2 (Mở rộng toàn quốc):** Triển khai đồng loạt trên app Vinhomes Resident cho toàn bộ hệ thống đại đô thị trên cả nước.

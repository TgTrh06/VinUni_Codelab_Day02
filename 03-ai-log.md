# Lab 02 — Nhật Ký Sử Dụng AI (AI Log & Reflection)
**Học viên:** Trần Mạnh Hùng  
**Email:** 26ai.hungtm3@vinuni.edu.vn  
**Đơn vị:** Vin Smart Future (Vingroup)  
**Chủ đề:** Tối ưu hóa vận hành Đại đô thị Vinhomes bằng AI  

---

## 🎯 1. MỤC TIÊU SỬ DỤNG AI (THOUGHT PARTNER)
Tôi không sử dụng AI như một công cụ tạo văn bản thụ động để sao chép nguyên mẫu, mà đóng vai trò là **AI Product Engineer** đối thoại với AI qua 3 vai trò:
1. **Brainstorm Partner:** Quét các điểm nghẽn vận hành (Pain points) lớn tại các đại đô thị Vinhomes (Ocean Park, Grand Park, Smart City).
2. **Red Teaming / Adversarial Partner:** Yêu cầu AI hóa thân thành **CFO và Trưởng phòng Vận hành cực kỳ khắt khe** để vạch lá tìm sâu các đề xuất AI.
3. **Refinement Partner:** Tinh chỉnh kiến trúc từ Pure Agentic (nguy hiểm) sang Hybrid LLM + Rule-based Boundary có con người kiểm duyệt (Human-in-the-loop).

---

## 💬 2. CHI TIẾT CÁC CUỘC HỘI THOẠI & PHẢN BIỆN (LOGS)

### 🔹 Vòng 1: Brainstorm ý tưởng ban đầu
* **Prompt của tôi:**
  > *"Tôi là AI Engineer tại Vin Smart Future. Hãy phân tích các quy trình thủ công gây lãng phí OPEX lớn nhất tại các đại đô thị Vinhomes và đề xuất giải pháp AI tương ứng."*
* **AI đề xuất:**
  * Đề xuất 5 mảng: Tiếp nhận ticket cư dân, Thẩm định bản vẽ CAD nội thất, Giám sát CCTV an ninh, Bảo trì thang máy (PdM), và Tối ưu Chiller/HVAC.
* **Đánh giá ban đầu:** Các bài toán trúng vào các khâu tốn nhiều nhân sự (FTE), có số liệu quy mô lớn (hàng ngàn căn hộ, hàng ngàn mắt camera).

---

### 🔹 Vòng 2: Red Teaming — Đưa CFO & Trưởng phòng Vận hành vào phản biện
* **Prompt của tôi (Stress-test 3 Quick Cards):**
  > *"Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành Vinhomes cực kỳ khắt khe. Hãy chỉ ra 3 điểm yếu chí mạng về mặt logic vận hành, bài toán tài chính P&L, và chứng minh vì sao Rule-based code hoặc chỉnh sửa UI/UX thông thường có thể giải quyết bài toán này tốt hơn, rẻ hơn nhiều so với việc dùng AI."*

* **Phản biện từ AI (Góc nhìn CFO & Vận hành):**
  1. **Bài toán Ticket cư dân:** 
     * *Vận hành:* Nhìn ảnh vết nứt tường/thấm nước qua VLM không thể biết hỏng ống ngầm hay sàn tầng trên; AI đoán bừa làm thợ mang sai đồ nghề. Nút thắt thực tế là thiếu thợ giờ cao điểm, không phải do CSKH bấm phân loại chậm.
     * *CFO:* Ảo tưởng về tỷ lệ tự động (Deflection rate). Cư dân đóng phí quản lý cao cấp không chấp nhận chat với bot vô cảm. Chi phí GPU/API cho 90.000 lượt gọi/tháng còn đắt hơn thuê nhân sự.
     * *Giải pháp Rule-based:* Chỉ cần sửa UI App Vinhomes Resident thêm Dropdown 3 tầng (Kỹ thuật -> Nước -> Rò rỉ) + Routing Matrix là tự động gán việc trong 0.1s, chi phí gần như 0 VNĐ.
  2. **Bài toán Thẩm định bản vẽ CAD nội thất:**
     * *Vận hành:* Bản vẽ nhà thầu nộp lên rất lộn xộn (AutoCAD cũ, lỗi font, scan méo). Đưa vào AI sẽ sinh ra False Positives hoặc bỏ sót lỗi chịu lực. Rủi ro pháp lý cực lớn nên kỹ sư vẫn phải soi lại bằng mắt 100%.
     * *CFO:* Nhu cầu duyệt bản vẽ mang tính thời vụ (chỉ bùng phát 3-6 tháng sau bàn giao phân khu mới). Đổ tiền CAPEX xây hệ thống AI cho nhu cầu ngắn hạn là lãng phí.
     * *Giải pháp Rule-based:* BQL cấp sẵn CAD Template khóa Layer cấm tác động; dùng Python Script (`ezdxf`) kiểm tra giao cắt hình học Polygon trong 2s, chính xác tuyệt đối.
  3. **Bài toán Giám sát CCTV đô thị:**
     * *Vận hành:* Dễ rơi vào bẫy "Spam cảnh báo" (Alert Fatigue) vì AI không hiểu ngữ cảnh (xe dừng 2 phút đón người già, xe tải chuyển đồ). Bắn phạt nhầm làm cư dân nổi giận.
     * *CFO:* 1 đại đô thị có 5.000–10.000 cam. Đầu tư phần cứng Edge Server/GPU xử lý real-time tốn hàng chục tỷ VNĐ.
     * *Giải pháp Rule-based:* Dùng sẵn tính năng Motion Detection / Line Crossing có sẵn trong đầu ghi NVR hiện hữu và thuật toán trừ thời gian xe ở cổng Barrier.

---

### 🔹 Vòng 3: Đúc rút & Hiệu chỉnh Thiết kế (Human Intervention)
Sau khi đối chất với phản biện của AI, tôi nhận ra: **Nếu thuần túy trao quyền cho Agentic AI xử lý tự động, dự án sẽ thất bại vì chi phí cao, rủi ro pháp lý và ảo giác (hallucination).**

Tôi đã chủ động thay đổi thiết kế hệ thống theo 3 nguyên tắc sống còn:
1. **Thu hẹp phạm vi của AI (Narrow Scope):**
   * Không dùng AI để thay thế thợ kỹ thuật hay tự động phạt tiền cư dân.
   * AI chỉ làm nhiệm vụ trợ lý: trích xuất thông tin, tóm tắt nội dung khiếu nại và sinh bản thảo gợi ý (`[DRAFT_ONLY]`).
2. **Kiến trúc Hybrid (Rule-first, AI-second):**
   * Tầng 1: Sử dụng Rule-based / Cây thư mục nghiệp vụ để giải quyết 70% trường hợp rõ ràng.
   * Tầng 2: Chỉ chuyển văn bản tự do, mô tả phức tạp vào LLM để phân tích ngữ nghĩa.
3. **Bắt buộc có Human-in-the-loop (HITL):**
   * Mọi quyết định xuất lệnh sửa chữa, phạt xe hay trả lời cư dân đều phải do Kỹ sư/CSKH con người bấm duyệt cuối cùng.

---

## 💡 3. BÀI HỌC KINH NGHIỆM (REFLECTION)
* **AI rất giỏi phóng đại (Over-engineering):** Khi hỏi giải pháp AI, mô hình luôn có xu hướng đề xuất những công nghệ phức tạp nhất (Multimodal Agent, Reinforcement Learning) mà bỏ qua các giải pháp kỹ thuật phần mềm truyền thống (Rule-based, IF-ELSE, Database indexing, UI UX) vừa rẻ vừa chính xác 100%.
* **Kỹ năng cốt lõi của AI Product Engineer:** Không phải là cố gắng nhồi nhét AI vào mọi ngóc ngách, mà là **biết nói "KHÔNG" với AI** ở những khâu logic xác định, và chỉ ứng dụng AI ở những mắt xích xử lý ngôn ngữ tự nhiên không cấu trúc hoặc hỗ trợ con người ra quyết định nhanh hơn.

# 03 — AI Log & Reflection



**Họ và tên — email:** Thân Đức Trung  — `thanductrung123@gmail.com`


---

## 1. Tôi đã dùng AI như thế nào

- **Công cụ:** Claude (trò chuyện trên web/app), dùng như một thought-partner để trao đổi ý tưởng; phần code chạy với Gemini 2.5 Flash qua Python.
- **Mục đích chính:**
  - Làm rõ yêu cầu của lab dựa trên những gì tôi nắm được từ repo, worksheet và slide của giảng viên.
  - Brainstorm các bài toán vận hành của Green SM khi mở rộng ra nhiều nước.
  - Phác thảo `SYSTEM_PROMPT`, hàm `evaluate_prompt()` và các bài test tấn công bổ sung.
  - Phác thảo `01-problem-scan.md`, `02-deep-dive-report.md` và sơ đồ quy trình.


---

## 2. AI đã giúp gì

- Hệ thống hóa cấu trúc lab: điểm nhóm/cá nhân, các Phase, file cần nộp, phân công giữa thành viên và trưởng nhóm — giúp tôi lên kế hoạch nhanh và không bỏ sót đầu việc.
- Gợi ý 6 hướng bài toán cho chủ đề "Green SM mở rộng toàn cầu"; tôi so sánh và chọn hướng xác minh hồ sơ tài xế vì có thể đặt metric bằng số và thể hiện rõ HITL/Fallback.
- Phác thảo prompt có hai ranh giới (`[DRAFT_ONLY]`, pin dưới 5%) và hai bài test tấn công thêm (giả mạo quyền quản lý, pin tới hạn chỉ suy ra được), giúp tôi tiết kiệm thời gian nghĩ ra các tình huống biên.
- Giúp phân biệt phần nào của bài xác minh tài xế nên dùng Rule (kiểm tra trường bắt buộc) và phần nào cần LLM (đọc ảnh, soạn tin) — từ đó hiểu rõ hơn khi nào nên dùng AI và khi nào không.

---

## 3. Cách tôi kiểm chứng và điều chỉnh đầu ra của AI

1. **Đối chiếu hướng làm code với slide giảng viên.** AI từng đề xuất đổi tên file prototype và viết lại toàn bộ cho bài xác minh tài xế. Tôi đối chiếu với slide của giảng viên, thấy yêu cầu là làm đúng file `starter-code/prompt_prototype.py` với kịch bản xe điện hết pin, và repo có thư mục `autograder/`. Sau khi được chỉ ra, AI điều chỉnh lại; tôi giữ nguyên file gốc cho phần code. Bài học: tài liệu chính thức của giảng viên luôn là nguồn ưu tiên cao nhất.
2. **Gắn nhãn rõ số liệu giả định.** Các con số trong 3 file (26 phút/hồ sơ, 500 hồ sơ/tuần, 30 → 10 phút…) là số minh họa để có metric mẫu, không phải số thật. Tôi đã ghi rõ *(giả định)* trong báo cáo để người đọc không hiểu nhầm.
3. **Số thị trường của Green SM không thống nhất giữa các nguồn.** Bản nháp liệt kê 6 nước (Việt Nam, Lào, Indonesia, Philippines, Ấn Độ, Kazakhstan). Khi tôi mở lại các nguồn, bài báo ngày 20/05/2026 nói Green SM hoạt động ở 4 nước (Việt Nam, Lào, Indonesia, Philippines); hai bài chính thức ngày 13/04/2026 cũng chỉ nêu 4 nước này. Các nước còn lại (Ấn Độ, Kazakhstan, Đan Mạch) đến từ nguồn khác, có thể do khác thời điểm đăng. AI đã gộp các nguồn mà không nói rõ ngày của từng nguồn, và danh sách của AI còn thiếu Đan Mạch. Tôi đã bổ sung và chốt danh sách 7 thị trường trong báo cáo.
4. **Ngày đổi tên và mô hình Platform: đã đối chiếu, thông tin đúng.** Tôi mở hai bài chính thức của Green SM. Cả hai đều đăng ngày 13/04/2026 và khớp với báo cáo: Green SM ra mắt cùng ngày, Platform mở tại Indonesia và Philippines, chủ xe và người thuê xe điện VinFast có thể đăng ký tham gia. Đây là kết quả kiểm chứng hợp lệ, không có chỗ lệch.

---

## 4. Tôi đã sửa / kiểm chứng những gì

- **Kiểm tra thông tin Green SM:** tôi mở lại hai bài chính thức trên greensm.com (đổi tên thương hiệu và ra mắt Platform, cùng đăng 13/04/2026) và bài báo ngày 20/05/2026. Ngày đổi tên và Platform khớp, không đổi. Về số thị trường, tôi đối chiếu ngày đăng giữa nguồn nêu 4 nước và nguồn nêu 6 nước, rồi chốt ghi **7 thị trường** (Việt Nam, Lào, Indonesia, Philippines, Ấn Độ, Kazakhstan, Đan Mạch) và cập nhật lại cả ba file `01`, `02`, `03` cho thống nhất.

---

## 5. Bài học rút ra

- AI là công cụ tăng tốc rất hiệu quả cho việc phác thảo, brainstorm và hệ thống hóa, nhưng kết quả cuối cùng cần được tôi hiểu, đối chiếu và chịu trách nhiệm.
- Làm việc với AI hiệu quả hơn khi tôi nắm rõ yêu cầu gốc của giảng viên ngay từ đầu và yêu cầu AI nêu rõ chỗ nào chưa chắc chắn.
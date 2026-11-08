# Lab 02 — Problem Scan và Quick Problem Cards (Vinmec)

## Phạm vi và cách đọc số liệu

Đây là các **giả thuyết vận hành để khảo sát**, không phải kết quả kiểm toán hay thống kê của Vinmec. Mọi sản lượng, thời gian và đơn giá dưới đây là giả định cho **một cơ sở trong một tháng**. Số liệu công khai chỉ xác nhận một số bước dịch vụ có tồn tại, không xác nhận tần suất hoặc mức độ thủ công của chúng.

## Phase 1 — Scan: năm cơ hội

| # | Mảng | Lens chính | Bài toán và người chịu ảnh hưởng | Quy mô cần kiểm chứng |
|---|---|---|---|---|
| 1 | Vinmec | Lặp lại, tốn thời gian | Nhân viên bảo hiểm đối chiếu chứng từ và yêu cầu bảo lãnh viện phí; hồ sơ thiếu cần quay lại bổ sung. [Vinmec mô tả thủ tục bảo lãnh](https://www.vinmec.com/vie/bai-viet/huong-dan-thu-tuc-thanh-toan-bao-hiem-tai-vinmec-vi). | Giả sử 2.000 hồ sơ × 15 phút + 15% × 2.000 × 20 phút bổ sung = **600 giờ công/tháng**; với 100.000 đồng/giờ là **60 triệu đồng chi phí xử lý**, không phải mức tiết kiệm có thể đạt. |
| 2 | Vinmec | Stakeholder pain, tốn thời gian | Tổng đài xác nhận, đổi lịch và tìm người thay thế khi bệnh nhân hủy hoặc không đến. [Trang đăng ký Vinmec nêu bước gọi lại xác nhận](https://www.vinmec.com/vie/dang-ky-kham/). | Giả sử 10.000 lịch × 3 phút = **500 giờ liên hệ/tháng**; 8% không đến, một nửa chỗ trống không lấp được = **400 suất khám chưa sử dụng**. |
| 3 | Vinmec | Tốn thời gian, AI có thể hỗ trợ | Bác sĩ tìm kết quả cũ, viết và kiểm tra ghi chú khám. [Nghiên cứu thời gian làm việc tại Mỹ](https://adfm.org/media/1476/ann-2016-time-study.pdf) cho thấy gánh nặng EHR, nhưng không được dùng làm số liệu Vinmec. | Giả sử 5.000 lượt × 6 phút = **500 giờ bác sĩ/tháng**; 300.000 đồng/giờ tương đương **150 triệu đồng thời gian chuyên môn**. |
| 4 | Vinmec | Lặp lại | Nhân viên đối soát chỉ định, dịch vụ đã thực hiện, vật tư và khoản thu để tìm ngoại lệ. | Giả sử 3.000 hồ sơ × 8 phút = **400 giờ/tháng**. Nếu 1% có khoản thu thiếu hợp lệ trung bình 500.000 đồng thì **15 triệu đồng chỉ là giá trị nghi vấn**, chưa phải thất thu xác nhận. |
| 5 | Vinmec | Stakeholder pain, tốn thời gian | Điều dưỡng, thu ngân và bộ phận bảo hiểm phối hợp các đầu việc xuất viện, khiến bệnh nhân có thể chờ sau khi có chỉ định. | Giả sử 600 ca × 20 phút phối hợp = **200 giờ/tháng**. Một giờ chờ/ca tương đương **25 ngày giường công suất tiềm năng**, không mặc nhiên là doanh thu. |

Không cộng các con số trên: giờ công, công suất và khoản thu nghi vấn khác bản chất, còn quy trình #1 và #5 có thể chồng lấn.

## Phase 2 — Ba Quick Problem Cards

### Card 1 — Kiểm tra tính đầy đủ của hồ sơ bảo lãnh viện phí

- **Actor:** Nhân viên bảo hiểm/bảo lãnh; bệnh nhân và bộ phận tiếp nhận chịu ảnh hưởng khi phải bổ sung hồ sơ.
- **Quy trình hiện tại giả định:** (1) Nhận chứng từ → (2) xác định đơn vị bảo hiểm và checklist → (3) đọc, đối chiếu chứng từ → (4) hỏi bổ sung hoặc xác nhận đủ hồ sơ → (5) chuyển nhân viên phụ trách liên hệ công ty bảo hiểm.
- **Bottleneck cần đo:** Đọc và đối chiếu nhiều chứng từ, giả định **5 phút/hồ sơ** ở bước 3; các ca thiếu hồ sơ có thể cần thêm 20 phút phối hợp.
- **AI hỗ trợ:** OCR/trích xuất và LLM lập **bản nháp** các mục thiếu hoặc không nhất quán, chỉ rõ chứng từ liên quan. Checklist quyền lợi theo từng nhà bảo hiểm phải do người phụ trách cung cấp và cập nhật.
- **Metric thử nghiệm:** Thời gian rà soát trung vị từ **15 xuống 10 phút/hồ sơ**, đồng thời tỷ lệ bỏ sót lỗi **không cao hơn baseline** trên cùng tập hồ sơ đã được chuyên viên gán nhãn. Cả hai baseline cần đo thực tế.
- **Quick architecture:** Rule cho các trường bắt buộc và đối chiếu định dạng; OCR/LLM cho nội dung chứng từ khó cấu trúc; nhân viên quyết định. Không cần agent tự trị.

### Card 2 — Xác nhận và điều phối lịch hẹn

- **Actor:** Tổng đài viên, nhân viên điều phối lịch và bệnh nhân.
- **Quy trình hiện tại giả định:** (1) Nhận yêu cầu → (2) gọi xác nhận → (3) cập nhật lịch → (4) xử lý hủy/không đến → (5) tìm người phù hợp trong danh sách chờ.
- **Bottleneck cần đo:** Thời gian gọi lại, số cuộc gọi không kết nối, khoảng thời gian từ lúc hủy đến khi lấp lịch.
- **AI hỗ trợ:** Tóm tắt yêu cầu đổi lịch và gợi ý lịch thay thế; thử nghiệm nhắc lịch có mục tiêu sau khi so với nhắc lịch theo quy tắc.
- **Metric thử nghiệm:** Giảm phút xử lý mỗi lịch và tăng tỷ lệ lấp chỗ trống; cần đo baseline trước khi đặt ngưỡng số.
- **Quick architecture:** Ưu tiên rule + tự động hóa lịch; chỉ dùng LLM cho yêu cầu ngôn ngữ tự do. Nhân viên xác nhận thay đổi lịch.

### Card 3 — Soạn ghi chú khám từ nội dung đã xác minh

- **Actor:** Bác sĩ khám và nhân viên hỗ trợ hồ sơ.
- **Quy trình hiện tại giả định:** (1) Xem hồ sơ cũ → (2) trao đổi/khám → (3) xem kết quả → (4) soạn ghi chú → (5) kiểm tra và ký.
- **Bottleneck cần đo:** Số phút ghi chép sau khám, mức độ sửa và bỏ sót thông tin trong ghi chú.
- **AI hỗ trợ:** Tạo bản nháp có dẫn nguồn từ dữ liệu được phép sử dụng; bác sĩ kiểm tra từng nhận định trước khi ký.
- **Metric thử nghiệm:** Giảm **2 phút/lượt** sau khi tính thời gian kiểm tra, không tăng lỗi lâm sàng hay thông tin không có nguồn. Mục tiêu này chỉ là giả định.
- **Quick architecture:** LLM feature có bước bác sĩ duyệt; không tự đưa chẩn đoán, chỉ định hoặc ký bệnh án.

## Chọn một bài toán để Deep Dive

Chọn **Card 1** vì đầu vào và đầu ra có thể mô phỏng bằng chứng từ giả lập, chuyên viên có thể gán nhãn “đủ/thiếu/không nhất quán”, và phép thử tập trung vào một bước rà soát. Card 2 cần dữ liệu lịch, danh sách chờ và so sánh với tự động hóa theo quy tắc. Card 3 có rủi ro lâm sàng và yêu cầu dữ liệu nhạy cảm cao hơn. Đây là lựa chọn cho **bài tập scoping**, chưa phải quyết định triển khai tại Vinmec.

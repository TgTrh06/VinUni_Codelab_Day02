# Lab 02 — Deep Dive: kiểm tra hồ sơ bảo lãnh viện phí tại Vinmec

## 1. Bài toán và mức độ chắc chắn của thông tin

Vinmec [công khai quy trình](https://www.vinmec.com/vie/bai-viet/huong-dan-thu-tuc-thanh-toan-bao-hiem-tai-vinmec-vi) trong đó bệnh nhân cung cấp hồ sơ và bệnh viện liên hệ công ty bảo hiểm khi đã có đủ hồ sơ, kết quả. Các bước chi tiết, vai trò, thời gian và sản lượng dưới đây là **mô hình làm việc giả định cho lab**, cần phỏng vấn nhân viên và đo dữ liệu vận hành trước khi coi là thực trạng Vinmec. Nội dung này chỉ xét kiểm tra **tính đầy đủ và nhất quán của chứng từ**, không xét quyết định quyền lợi, chẩn đoán hay phê duyệt bảo lãnh.

## 2. Current-State Workflow (G1)

Xem thêm [sơ đồ quy trình hiện tại](04-workflow-diagram.png). Thời gian là giả định cho một hồ sơ thông thường, chưa gồm thời gian chờ phản hồi từ người khác.

| Bước | Người phụ trách, đầu vào → đầu ra | Thời gian giả định | Điểm chuyển giao và rủi ro |
|---|---|---:|---|
| 1. Tiếp nhận | Lễ tân/nhân viên bảo hiểm nhận giấy tờ, thẻ, chỉ định → bộ hồ sơ đầu vào | 2 phút | 🔄 Bệnh nhân → nhân viên; ảnh mờ hoặc thiếu trang. |
| 2. Chọn checklist | Nhân viên xác định đơn vị bảo hiểm, loại dịch vụ → checklist hiện hành | 3 phút | Có thể chọn sai phiên bản điều kiện hoặc thiếu thông tin gói. |
| 3. Đọc và đối chiếu | Nhân viên so tên, mã định danh, ngày, chỉ định, chứng từ bắt buộc → danh sách ngoại lệ | **5 phút** | 🔴 Điểm nghẽn: nhiều định dạng giấy tờ và thông tin lặp giữa tài liệu. |
| 4. Xử lý ngoại lệ | Nhân viên xác nhận đủ hoặc yêu cầu bệnh nhân/bộ phận liên quan bổ sung → hồ sơ đủ/đang chờ | 3 phút | 🔄 Nhân viên → bệnh nhân/bộ phận chuyên môn; nếu thiếu có thể phát sinh vòng lặp. |
| 5. Chuyển xử lý | Nhân viên ghi nhận kết quả rà soát → hồ sơ chuyển đến người phụ trách liên hệ bảo hiểm | 2 phút | 🔄 Nội bộ bệnh viện → đơn vị bảo hiểm theo quy trình; lỗi bỏ sót có thể làm hồ sơ quay lại. |

**Tổng giả định: 15 phút/hồ sơ.** Nếu 15% trong 2.000 hồ sơ/tháng cần thêm 20 phút phối hợp bổ sung, tổng công là `2.000 × 15/60 + 2.000 × 15% × 20/60 = 600 giờ/tháng`. Với **đơn giá giả định** 100.000 đồng/giờ, đó là 60 triệu đồng chi phí xử lý. Mục tiêu giảm 5 phút ở lượt rà soát đầu tương đương `2.000 × 5/60 ≈ 167 giờ`, hay 16,7 triệu đồng năng lực lao động/tháng **trước chi phí xây dựng, vận hành và thời gian kiểm tra AI**. Không coi toàn bộ 60 triệu đồng là thất thoát hoặc số tiền tiết kiệm được.

## 3. Problem Statement — 6 trường (G2)

| Trường | Nội dung |
|---|---|
| **1. Actor / Operator** | Nhân viên phụ trách bảo lãnh viện phí tại một cơ sở Vinmec giả định; lễ tân, bệnh nhân và bộ phận chuyên môn tham gia tại các điểm chuyển giao. |
| **2. Current Workflow** | Nhận chứng từ → chọn checklist theo đơn vị bảo hiểm và dịch vụ → đọc/đối chiếu → xử lý giấy tờ thiếu hoặc sai khác → chuyển hồ sơ; giả định 15 phút/lượt đầu. |
| **3. Bottleneck** | Bước đọc, đối chiếu chứng từ và tìm thông tin mâu thuẫn; giả định 5 phút/hồ sơ, cộng thêm vòng bổ sung ở một phần hồ sơ. |
| **4. Business Impact** | Kịch bản 2.000 hồ sơ/tháng tạo khoảng 600 giờ xử lý gồm lượt đầu và bổ sung; tính theo 100.000 đồng/giờ là 60 triệu đồng **chi phí giả định**. Cần đo số hồ sơ, thời gian thực, tỷ lệ bổ sung và nguyên nhân bổ sung trước khi lượng hóa tác động. |
| **5. Success Metric** | Pilot mục tiêu trung vị thời gian rà soát từ 15 xuống **≤10 phút/hồ sơ** sau khi tính thời gian người kiểm tra; tỷ lệ bỏ sót lỗi quan trọng **không cao hơn quy trình hiện tại** trên cùng tập hồ sơ gán nhãn. Theo dõi thêm tỷ lệ cảnh báo sai và tỷ lệ hồ sơ quay lại. Ngưỡng baseline phải được xác lập bằng quan sát. |
| **6. Operational Boundary** | AI chỉ trích xuất, đối chiếu với checklist **được cung cấp**, nêu phần thiếu/sai khác và dẫn nguồn thành bản nháp. AI không tự quyết định quyền lợi, không cam kết bảo lãnh, không sửa hồ sơ gốc, không gửi tài liệu ra ngoài và không tự chuyển trạng thái hồ sơ. Nhân viên xác minh và phê duyệt. |

## 4. AI Fit và Future-State Flow (G3)

**Lựa chọn:** Rule + LLM feature. Rule kiểm tra trường bắt buộc, định dạng và các điều kiện checklist có cấu trúc. OCR/LLM hỗ trợ đọc chứng từ nhiều định dạng và tạo danh sách ngoại lệ có nguồn. Chưa có lý do cho agent tự trị vì phạm vi chỉ là **một bước rà soát**, không có chuỗi hành động được phép tự thực thi.

1. Nhân viên tiếp nhận hồ sơ và chọn checklist đã được quản trị theo đơn vị bảo hiểm/dịch vụ.
2. 🔵 Hệ thống trích xuất nội dung; rule so trường bắt buộc; LLM tạo bản nháp gồm trường tìm thấy, mục thiếu, mâu thuẫn và tham chiếu tới chứng từ.
3. 🟢 Nhân viên mở tài liệu gốc, sửa hoặc bác bỏ từng phát hiện, xác nhận cần bổ sung hay hồ sơ đủ để chuyển bước tiếp theo.
4. 🟢 Nhân viên thực hiện liên hệ hoặc chuyển hồ sơ theo quy trình hiện hành. Hệ thống chỉ lưu kết quả được người duyệt xác nhận.
5. ↩️ **Fallback:** Tài liệu mờ, không xác định được checklist, thiếu nguồn, JSON lỗi hoặc phát hiện không chắc chắn → đánh dấu “cần rà soát thủ công”; nhân viên dùng quy trình hiện tại. Không tự suy diễn giấy tờ hay điều kiện bảo hiểm.

**Dữ liệu cho pilot:** một tập chứng từ đã khử thông tin nhận dạng hoặc chứng từ tổng hợp, checklist có phiên bản, nhãn do chuyên viên bảo hiểm xác nhận, thời điểm bắt đầu/kết thúc mỗi bước. Chỉ khi có thỏa thuận dữ liệu và quyền sử dụng phù hợp mới xem xét dữ liệu thực.

## 5. Prompt prototype và phép thử ranh giới

File [prototype](starter-code/prompt_prototype.py) định nghĩa JSON đầu ra, ba đầu vào đối kháng và bộ kiểm tra đầu ra. Ba hành vi cố tình dụ AI làm sai là: (1) coi hồ sơ thiếu là đủ do người gửi thúc ép; (2) tự điền mã định danh vắng mặt; (3) tuyên bố đã duyệt hoặc đã gửi hồ sơ cho bảo hiểm. Kết quả chỉ đạt khi có nguồn cho dữ liệu trích xuất, nêu thiếu/sai khác đúng, luôn yêu cầu nhân viên xem lại và không xác nhận hành động bên ngoài. Bộ kiểm tra offline chỉ xác nhận logic rào chắn và cấu trúc; **chưa chứng minh Gemini giữ ranh giới** cho tới khi chạy thử trực tiếp và xem từng kết quả.

## 6. AI Readiness và quyết định (G4)

| Câu hỏi | Trạng thái hiện tại | Điều kiện để chuyển sang GO |
|---|---|---|
| Có dữ liệu mẫu/log sạch để test? | **Chưa xác nhận.** Lab dùng hồ sơ tổng hợp. | Có tập hồ sơ được phép sử dụng, nhãn lỗi và checklist theo phiên bản. |
| Rủi ro sai có kiểm soát được? | Thiết kế có người duyệt và fallback; **chưa có số liệu lỗi thực nghiệm**. | Đo lỗi bỏ sót quan trọng, cảnh báo sai và thời gian kiểm tra trên cùng mẫu với baseline. |
| Stakeholder sẵn sàng đổi quy trình? | **Chưa phỏng vấn.** | Nhân viên bảo hiểm xác nhận quy trình, quyền truy cập, cách duyệt và giá trị công việc. |

**Quyết định: NOT YET.** Có thể làm bản mẫu trên dữ liệu tổng hợp để học và kiểm tra giả thuyết. Chưa nên gọi đây là dự án triển khai tại Vinmec: chưa có baseline, dữ liệu được phép dùng, checklist xác thực hoặc bằng chứng stakeholder đồng ý. Bước kế tiếp là quan sát một mẫu hồ sơ và thống nhất cách gán nhãn với chuyên viên; sau đó pilot hẹp và so sánh thời gian/lỗi với quy trình hiện tại. Nếu rule đơn giản đạt cùng chất lượng và chi phí thấp hơn, chọn rule thay vì LLM.

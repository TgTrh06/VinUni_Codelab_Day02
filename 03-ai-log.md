# Lab 02 — AI Log và Reflection

## AI đã hỗ trợ ở đâu?

Tôi dùng AI làm đối tác brainstorm theo prompt của worksheet, giới hạn vào Vinmec. AI gợi ý năm quy trình để khảo sát: bảo lãnh viện phí, lịch hẹn, ghi chú khám, đối soát khoản thu và thủ tục xuất viện. Đầu ra giúp tôi nhận ra bài toán bảo lãnh viện phí có một bước kiểm tra chứng từ khá hẹp, phù hợp để viết Problem Statement và thử prompt. Sau đó tôi đối chiếu với tài liệu công khai của Vinmec về thủ tục bảo lãnh và bước xác nhận lịch hẹn.

## AI đã thiếu hoặc dễ gây hiểu nhầm ở đâu?

- Các con số 2.000 hồ sơ/tháng, 15 phút/hồ sơ, 15% hồ sơ cần bổ sung và 100.000 đồng/giờ là **giả định minh họa**, không phải dữ liệu Vinmec. Nếu gọi 60 triệu đồng là “thất thoát” hoặc “tiết kiệm”, kết luận sẽ sai: đó chỉ là chi phí xử lý trong kịch bản giả định.
- Nghiên cứu về thời gian ghi chép bệnh án tại Mỹ chỉ tạo bối cảnh tham khảo. Không thể suy ra cùng tỷ lệ hay quy trình tại Vinmec.
- Ý tưởng AI đầu tiên có thể quá rộng nếu để mô hình tự xác định quyền lợi, duyệt bảo lãnh hoặc gửi hồ sơ. Checklist của từng nhà bảo hiểm cũng chưa được cung cấp nên AI không được tự tạo quy tắc.
- Template code và autograder cố định ví dụ Xanh SM trong khi đề tài tôi chọn là Vinmec. Kết quả kiểm tra code tự động vì thế không thể được hiểu như phép đo chất lượng prototype Vinmec.

## Tôi đã sửa cách đặt bài toán và ranh giới như thế nào?

Tôi thu hẹp nhiệm vụ thành **tạo bản nháp kiểm tra tính đầy đủ và nhất quán của chứng từ dựa trên checklist đã cung cấp**. Prompt yêu cầu mỗi thông tin trích xuất dẫn tới chứng từ, đánh dấu dữ liệu thiếu hoặc không đọc được, và luôn chuyển cho nhân viên kiểm tra. Ba ca đối kháng thử thúc ép AI bỏ qua giấy tờ thiếu, bịa mã thẻ và tuyên bố đã gửi hồ sơ. Tôi chỉ dùng chứng từ tổng hợp trong code.

Tôi cũng đổi kết luận từ một lời hứa hiệu quả sang **NOT YET**. Trước khi có thể nói GO cần đo baseline thời gian và lỗi, có checklist đúng phiên bản, hồ sơ được phép sử dụng và phản hồi từ nhân viên bảo hiểm. Mục tiêu 15 → 10 phút/hồ sơ là giả thuyết cho pilot, không phải kết quả đo.

## Ghi nhận kiểm thử

Prototype có kiểm tra offline để chắc chắn bộ kiểm tra nhận một JSON hợp lệ và phát hiện một tuyên bố phê duyệt sai. Đây là kiểm thử logic local, **không phải kết quả gọi Gemini**. Chưa chạy `--live`, nên chưa có bằng chứng mô hình giữ ranh giới trước ba ca đối kháng. Khi được phép chạy live, cần ghi từng đầu vào, đầu ra đã loại bỏ thông tin nhạy cảm, lỗi phát hiện và lần sửa prompt tiếp theo; không tự nhận tất cả ca đã qua khi chưa kiểm tra.

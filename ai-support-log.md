# AI Support Log

Công cụ: Claude Code (Claude Opus 5.5).

Theo luật lab, AI chỉ được dùng để brainstorm, phản biện và gợi ý tên event. Core action, kết luận cadence và metric hypothesis là quyết định của tôi.

Cột "Tôi quyết định / chỉnh gì" được AI điền theo yêu cầu của tôi, dựa trên các lựa chọn tôi đã đưa ra trong phiên làm việc.

| # | Mục | Tôi dùng AI để | AI đưa ra | Tôi quyết định / chỉnh gì |
| --- | --- | --- | --- | --- |
| 1 | 00 | Đọc repo P-040, tóm tắt luồng sản phẩm; soạn nháp mục 00 (dự án, persona, core job) trong README dựa trên persona và problem statement của P-040 | Tóm tắt luồng Onboarding → CV → Gap → Roadmap → Progress → Reassessment; chỉ ra điểm reassessment đang hard-code (2.0 → 4.0) | Giữ persona và core job của bản nháp; tự sửa lại câu mô tả dự án ở dòng 1 |
| 2 | 01 | Brainstorm ứng viên core action | Bảng 8 ứng viên kèm phân loại (hành vi / output / outcome) và rủi ro. **Lưu ý:** ở lượt chat đầu, AI đã gợi ý thẳng "đánh dấu hoàn thành skill/milestone" làm core action, activation là "hoàn thành skill đầu tiên" và cadence 1–2 tuần, trước khi tôi gửi luật lab | AI đưa 3 phương án kèm chấm 5 tiêu chí (done + evidence, done không evidence, hoàn thành trọn step); tôi chọn "done action không evidence". AI chỉ ra lựa chọn này mới đạt 3.5/5, chưa qua Gate 1, và đưa 2 cách sửa; tôi chọn siết completion rule (không bỏ tick trong 24 giờ, cách lần trước ≥5 phút). Lý do: không muốn bắt user nộp evidence vì sẽ làm giảm số lần thực hiện; phần tick ảo còn lọt qua để counter-metric theo dõi |
| 3 | 01 | Gợi ý danh sách 5 tiêu chí tự kiểm (suy ra từ "Lỗi thường gặp") | 5 tiêu chí | Bỏ danh sách của AI, dùng 5 tiêu chí chính thức trong đề Phase 1 (gần value, lặp lại, quan sát, ý nghĩa, tác động) |
| 4 | 02–04 | Soạn nháp Action Nature Card; hỏi tôi chọn dạng hành vi và nhịp đo (AI đưa phương án kèm ưu/nhược); câu hỏi phản biện về retention | Nature Card 8 dòng; 2 phương án dạng hành vi, 3 phương án nhịp đo; ý tưởng metric tham khảo | Tôi chọn "tiến trình tích luỹ" và "theo tuần, cấp user"; AI ghép thành câu kết luận theo template. Giữ câu kết luận. Cụm "một vài lần mỗi tuần" là giả định, lập luận từ quỹ 10 giờ/tuần và mỗi action mất vài giờ |
| 4b | 03–04 | Soạn nháp toàn bộ Metric System và Retention Definition dựa trên core action và cadence tôi đã chọn | Activation (7 ngày), engagement (frequency + depth), NSM "Weekly Learning Users" (≥2 action hợp lệ/tuần), 3 leading, 3 counter-metric, retention 6 thành phần theo tuần | Giữ nguyên bản nháp, kể cả ngưỡng ≥2, W8 và các mức hours_per_week |
| 5 | 05–06 | Đưa phương án loại loop và metric hypothesis để tôi chọn; soạn nháp loop 2 chu kỳ, bảng event và acceptance criteria | 2 loại loop, 3 phương án hypothesis; 6 event phía server; 3 AC; chỉ ra RoadmapScreen chưa hiện tiến độ và code đang ghi đè done_at | Tôi chọn progress loop và hypothesis "NSM tăng"; AI ghép thành câu theo template. Giữ cả 6 event và câu hypothesis như bản nháp |
| 5b | Tự soi | Đối chiếu 7 câu tự soi với bài và kiểm tra lại code P-040 | Bảng tự soi; phát hiện thiếu endpoint lưu hours_per_week; tự sửa nhận định sai của AI ở 05 (dashboard đã có pill trạng thái step) | Giữ ngưỡng NSM ≥2, ghi lý do trong bảng tự soi; quyết định bỏ file HTML và làm toàn bộ Metrics Pack trong README |
| 6 | Toàn bài | Soạn README theo cấu trúc nộp bài (mục lục, các mục 00–06, tự soi, revision) và file log này | Bản nháp từng mục theo các lựa chọn của tôi | — |
| 7 | README | Soạn nháp phần "Điều tôi mang về áp dụng cho dự án thật" từ các phát hiện trong bài | 4 gạch đầu dòng: đo việc học, đo theo tuần, 3 chỗ sửa code, tracking phía server | Giữ nguyên |

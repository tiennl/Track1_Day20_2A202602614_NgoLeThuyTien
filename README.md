# Track1 Day 20 · Product Metrics Lab

- **Họ tên:** Ngô Lê Thủy Tiên
- **MHV:** 2A202602614
- **Dự án:** PathPlanner (P-040)

## Metrics Pack

Metrics Pack nằm ngay trong README này. Mỗi mục dùng lại kết quả của mục trước (core action → cadence → metric → loop → event):

| Mục | Nội dung |
| --- | --- |
| [00](#00--phạm-vi) | Dự án, persona, core job |
| [01](#01--core-action) | Core Action Card + tự kiểm 5 tiêu chí |
| [02](#02--nature--cadence) | Action Nature Card + kết luận cadence |
| [03](#03--metric-system) | Metric System (activation / engagement / NSM / leading / counter) |
| [04](#04--retention-definition) | Retention Definition (6 thành phần) |
| [05](#05--product-loop) | Product Loop (2 chu kỳ + metric hypothesis) |
| [06](#06--tracking-nhanh) | Tracking nhanh (6 events + 3 acceptance criteria) |
| [Tự soi](#tự-soi-lỗi-gate-5) · [Revision](#revision) | Đối chiếu 7 câu tự soi, các thay đổi kèm lý do |

## 00 — Phạm vi

1. **Dự án:** Dự án nhận CV của ứng viên, so với JD của vị trí mục tiêu để chỉ ra kỹ năng còn thiếu, rồi sinh lộ trình học theo từng giai đoạn và cho phép tái đánh giá tiến độ.
2. **Persona:** Sinh viên năm cuối ngành IT, sắp tốt nghiệp, đang nhắm một vị trí junior cụ thể (vd Backend Developer Junior) nhưng chưa có kinh nghiệm đi làm.
3. **Core job:** "Tôi sắp ra trường và muốn ứng tuyển Backend Junior, nhưng không biết mình còn thiếu gì so với yêu cầu tuyển dụng, nên học lan man mãi mà vẫn không đủ tự tin nộp CV."

## 01 — Core Action

### Phân biệt bốn khái niệm

| Khái niệm | Câu hỏi | PathPlanner |
| --- | --- | --- |
| Core job | User đang cố hoàn thành việc gì? | Biết mình thiếu gì so với JD để học đúng trọng tâm và đủ tự tin ứng tuyển |
| Core action | User làm gì trong sản phẩm để tiến tới giá trị? | Đánh dấu hoàn thành một action học tập trong roadmap |
| Core value | User nhận được lợi ích gì? | Thu hẹp dần khoảng cách kỹ năng tới vị trí mục tiêu, theo đúng thứ tự cần học |
| Core value event | Sự kiện nào chứng minh value đã xảy ra? | `roadmap_action_completed` |

### Core Action Card

| Thành phần | Câu trả lời |
| --- | --- |
| Target user | Sinh viên năm cuối IT nhắm vị trí junior |
| Core job | "Tôi muốn biết mình còn thiếu gì so với JD để học đúng trọng tâm và đủ tự tin nộp CV" |
| Core action | Đánh dấu hoàn thành một action học tập trong roadmap |
| Object | Một action (`suggested_actions[i]`) thuộc một step của roadmap cá nhân |
| Preconditions | Đã upload CV, đã có skill gap so với JD mục tiêu, đã sinh roadmap |
| Completion rule | Một action được tính là hoàn tất khi thoả cả 3 điều kiện: (1) request `done=true` được lưu DB thành công (`done_at` được ghi); (2) action không bị bỏ tick (`done=false`) trong 24 giờ sau đó; (3) `done_at` cách lần hoàn thành action trước đó của cùng user ít nhất 5 phút, để loại kiểu tick hàng loạt |
| Core value | Khoảng cách kỹ năng tới vị trí mục tiêu được thu hẹp từng bước |
| Evidence of value | `done_at` của action; về sau là điểm reassessment của step tăng (`after > before`) |
| Candidate event | `roadmap_action_completed` |

**Vì sao không phải "mở app" hay "hỏi AI":** mở app hay xem roadmap không làm user tiến gần JD hơn. Roadmap và skill gap là output do AI tạo ra; chỉ khi user thực sự học xong một action thì khoảng cách kỹ năng mới hẹp lại.

### Tự kiểm 5 tiêu chí

| # | Tiêu chí | Kết quả | Giải thích |
| --- | --- | --- | --- |
| 1 | Gần core value | Một phần | Có tiến gần value, nhưng vì là tự khai nên chưa chắc user đã học thật |
| 2 | Có thể lặp lại | Đạt | Mỗi roadmap có nhiều action, nhu cầu quay lại theo từng action |
| 3 | Có thể quan sát | Đạt | `done_at` đã có sẵn trong dữ liệu tiến độ |
| 4 | Có ý nghĩa | Đạt | Completion rule loại tick hàng loạt (dưới 5 phút) và tick rồi bỏ (trong 24 giờ), nên số tăng phản ánh tiến độ học thật hơn |
| 5 | Có thể tác động | Đạt | Team có thể chia nhỏ action, gợi ý tài nguyên, giúp ước lượng thời gian |

**Kết quả:** 4.5/5, qua Gate 1. Lần đầu chọn "done action" với completion rule chỉ là `done=true` thì được 3.5/5, trượt tiêu chí 4. Tôi giữ core action và siết completion rule thay vì bắt buộc evidence, để không làm giảm số lần thực hiện. Phần tick ảo còn lọt qua sẽ do counter-metric ở mục 03 theo dõi.

## 02 — Nature & cadence

### Action Nature Card

| Thành phần | Câu trả lời |
| --- | --- |
| Actor | Một user cá nhân (sinh viên năm cuối), tự học một mình, không phụ thuộc team |
| Intent | Muốn lấp một kỹ năng còn thiếu so với JD để tiến gần hơn tới mức đủ tự tin ứng tuyển |
| Trigger | Chủ yếu do user chủ động khi có thời gian học (buổi tối, cuối tuần). Sự kiện bên ngoài như mùa tuyển dụng hay hạn nộp hồ sơ thực tập làm nhu cầu tăng lên. Hệ thống chỉ nhắc, không tạo ra nhu cầu |
| Effort | Cao. Mỗi action cần vài giờ học, cần tập trung và đôi khi phải làm bài tập thực hành. Quỹ thời gian mặc định là 10 giờ/tuần (`hours_per_week`) |
| Value timing | Tích luỹ và đến trễ. Một action riêng lẻ ít thấy giá trị; giá trị rõ dần khi xong một step (một cụm skill), và rõ nhất khi user đủ tự tin nộp CV |
| State | `done`, `done_at` của action; `started_at` của roadmap; tỷ lệ hoàn thành của step. Các trạng thái này làm cơ sở cho lần học tiếp theo và cho reassessment |
| Dependency | Thời gian rảnh (lịch học, mùa thi, đồ án), thứ tự tiên quyết giữa các step, tài nguyên học. Không cần approval hay người khác |
| Repeat condition | Roadmap còn action chưa xong và user chưa đạt mục tiêu ứng tuyển. Hết action, hoặc đã có việc, thì không còn lý do quay lại |

### Kết luận cadence

**Dạng hành vi:** tiến trình tích luỹ.

Đối với **sinh viên năm cuối IT đang chuẩn bị ứng tuyển vị trí junior**, core action **đánh dấu hoàn thành một action học tập trong roadmap** thường xuất hiện **một vài lần mỗi tuần, gói trong quỹ giờ học hàng tuần (mặc định 10 giờ, chủ yếu buổi tối và cuối tuần)** vì **mỗi action cần vài giờ học, và user học theo lịch rảnh trong tuần xen giữa việc học ở trường chứ không học đều mỗi ngày**. Do đó, nhịp đo phù hợp là **theo tuần** ở cấp **user**.

## 03 — Metric System

### Activation

| Thành phần | Định nghĩa |
| --- | --- |
| Start event | `roadmap_generated`: user nhận roadmap đầu tiên |
| Activation event | Action hợp lệ đầu tiên (`roadmap_action_completed` đầu tiên thoả completion rule) |
| Time window | Trong 7 ngày kể từ start event (bằng một chu kỳ tự nhiên ở 02) |

**Activation rate** = % user có roadmap trong tuần X mà hoàn thành action hợp lệ đầu tiên trong 7 ngày.

Không dùng "xong onboarding", "upload CV" hay "xem skill gap": lúc đó user chưa học gì, chưa chạm core value. Đây là *activation* (lần đầu nhận value), khác với *active* (có làm core action trong một tuần bất kỳ).

### Engagement (2 góc đo)

| Góc | Metric | Vì sao |
| --- | --- | --- |
| Frequency | Trung vị số action hợp lệ / user active / tuần | Đo user có học "vài lần mỗi tuần" như nhịp tự nhiên ở 02 không |
| Depth | Tiến độ so với kế hoạch: số action hợp lệ trong tuần / số action kế hoạch mỗi tuần của step hiện tại (số action của step chia `estimated_weeks`) | Đo user đi đúng nhịp roadmap hay tụt lại, thay vì chỉ đếm số |

### North Star Metric

**Weekly Learning Users:** số user có **≥2 action hợp lệ** trong tuần, tính trên user có roadmap chưa hoàn thành.

| Thành phần | Giá trị |
| --- | --- |
| Unit of value | Một action học tập được hoàn thành (một bước thu hẹp skill gap) |
| Quality threshold | Chỉ đếm action hợp lệ theo completion rule ở 01; và mỗi user phải đạt ≥2 action trong tuần để được tính |
| Frequency | Theo tuần |

Ngưỡng ≥2 lấy từ kết luận "một vài lần mỗi tuần" ở 02. Ngưỡng này phân biệt user đang thực sự học theo nhịp với user chỉ ghé qua một lần.

### Leading indicators

| # | Indicator | Vì sao tin nó dự báo core action lặp lại |
| --- | --- | --- |
| 1 | % user có action hợp lệ đầu tiên trong 48 giờ sau `roadmap_generated` | Bắt đầu khi động lực còn cao thì dễ thành nhịp học hàng tuần hơn |
| 2 | % user tự đặt `hours_per_week` (không để mặc định) | User tự cam kết quỹ giờ mỗi tuần, khớp đúng nhịp theo tuần |
| 3 | % user hoàn thành step 1 trong `estimated_weeks` | Xong một step là lần đầu thấy giá trị tích luỹ (một cụm skill), tạo lý do học tiếp |

### Counter-metrics

| # | Counter-metric | Bắt được điều gì |
| --- | --- | --- |
| 1 | Tỷ lệ tick bị loại = số `done=true` không thoả completion rule / tổng số `done=true` | NSM tăng nhờ tick ảo; nếu tỷ lệ này tăng thì completion rule đang bị lách |
| 2 | % reassessment có điểm `after ≤ before` (sau khi thay điểm mock bằng chấm thật) | User tick nhiều nhưng skill không tăng, tức là học không thật |
| 3 | % action AI gợi ý không ai hoàn thành sau 4 tuần kể từ khi step bắt đầu | Roadmap do AI sinh ra không phù hợp (quá khó, mơ hồ) và user bỏ qua |

## 04 — Retention Definition

| Thành phần | Định nghĩa |
| --- | --- |
| Unit | User (map `assessment_id` → `user_id`; một user có nhiều roadmap vẫn tính là một) |
| Cohort entry | Activation: action hợp lệ đầu tiên. Cohort gom theo tuần activation |
| Return event | ≥1 `roadmap_action_completed` hợp lệ (core action ở 01) |
| Window | Theo tuần: W1, W2, …, W8 là các khoảng 7 ngày liên tiếp kể từ ngày activation |
| Threshold | ≥1 action hợp lệ trong tuần đó |
| Segment | Sinh viên năm cuối IT (persona ở 00) có roadmap chưa hoàn thành. User đã xong roadmap được tách thành nhóm "tốt nghiệp" từ tuần sau khi hoàn thành, không tính là churn. Chia thêm theo `hours_per_week` (≤5, 6–10, >10 giờ) |

**Câu định nghĩa:** Weekly retention Wn = % user trong cohort activation tuần X (sinh viên năm cuối IT, roadmap chưa hoàn thành) có ≥1 action hợp lệ trong tuần thứ n kể từ activation.

**Khớp với cadence ở 02:** core action có nhịp theo tuần, nên window theo tuần; không dùng D1/D7 theo ngày. Dừng ở W8 vì roadmap là tiến trình có điểm kết thúc.

**So sánh với:** (1) nhịp tự nhiên: một tuần trống do thi cử có thể là bình thường, nên xem thêm "≥1 action trong 2 tuần liên tiếp" để không kết luận vội; (2) cohort cùng segment của các tuần trước; (3) benchmark của nhóm sản phẩm tự học / ed-tech. Không so với một con số cứng.

**Tự kiểm Gate 3:** activation có start event, activation event và window 7 ngày ✔ · retention đủ 6 thành phần, window theo tuần khớp 02 ✔ · NSM đủ unit + quality threshold + frequency ✔ · có 3 counter-metric ✔

## 05 — Product Loop

**Loại loop:** progress loop. Loop này khớp với dạng hành vi "tiến trình tích luỹ" ở 02: mỗi action làm tiến độ tăng, và phần còn thiếu tới JD là lý do học tiếp.

| Bước | Chu kỳ 1 | Chu kỳ 2 |
| --- | --- | --- |
| Natural trigger | Buổi tối / cuối tuần còn trong quỹ giờ học của tuần; mùa tuyển dụng sắp tới | Buổi học tiếp theo trong tuần; user đã biết chính xác phải học gì tiếp và còn thiếu bao nhiêu để xong step |
| Core action | Hoàn thành action hợp lệ đầu tiên của step 1 | Hoàn thành action hợp lệ tiếp theo, đến khi xong step |
| Immediate / repeat value | Tiến độ step nhích lên (vd 1/4 → 2/4), thấy rõ còn bao nhiêu action thì xong một cụm skill | Xong step = xong một cụm skill; reassessment cho thấy skill tăng so với trước |
| Saved state / investment | `done_at`, tiến độ step, action kế tiếp được mở theo thứ tự tiên quyết | Lịch sử tiến độ, kết quả reassessment, roadmap được điều chỉnh, step tiếp theo mở ra |

**Chu kỳ 1 dẫn sang chu kỳ 2 như thế nào:** trạng thái đã lưu (tiến độ step và action kế tiếp) trở thành trigger cho buổi học sau. User không phải nghĩ lại "học gì tiếp", và thấy rõ khoảng còn thiếu.

**Reason to return khi bỏ notification:** khoảng cách tới JD vẫn còn, user biết chính xác bước tiếp theo, và có hạn tuyển dụng thật. Notification chỉ nhắc đúng buổi học user đã tự lên lịch theo `hours_per_week`.

**Metric hypothesis:** Nếu loop này hoạt động, metric **Weekly Learning Users** (số user có ≥2 action hợp lệ trong tuần, NSM ở 03) sẽ thay đổi theo hướng **tăng** trong **8 tuần** kể từ khi hiển thị tiến độ step sau mỗi action, vì **mỗi action làm tiến độ step nhích lên rõ ràng, và phần còn thiếu cho user lý do học tiếp mà không cần nhắc**. Điều kiện kèm theo: counter-metric "tỷ lệ tick bị loại" (03) không tăng.

**Yêu cầu sản phẩm để loop chạy được:** dashboard roadmap đã có checklist action và pill trạng thái step (Hoàn thành / Đang học / Chưa bắt đầu). Để "immediate value" của chu kỳ 1 rõ hơn, cần thêm số đếm tiến độ của step (vd 2/4) hiện ngay sau khi tick. Điểm reassessment cũng phải được chấm thật thì value của chu kỳ 2 mới có ý nghĩa.

## 06 — Tracking nhanh

| Tên event | Ý nghĩa (điều đã xảy ra) | Thời điểm ghi nhận | Metric sử dụng (03 / 04) |
| --- | --- | --- | --- |
| `roadmap_generated` | Roadmap của user đã được tạo và lưu | Sau khi tạo roadmap lưu DB thành công (server) | Activation (start event); mẫu số của leading #1, #3; mẫu số của engagement depth (số action và `estimated_weeks` mỗi step); counter #3 |
| `roadmap_action_completed` | Một action chuyển từ chưa hoàn thành sang hoàn thành | Sau khi lưu `done=true` thành công, chỉ khi trạng thái trước đó là chưa hoàn thành (server) | Activation event, engagement, NSM, return event của retention, leading #1, counter #1, #3 |
| `roadmap_action_uncompleted` | Một action đã hoàn thành bị bỏ tick | Sau khi lưu `done=false` thành công, chỉ khi trạng thái trước đó là đã hoàn thành (server) | Completion rule (loại action bị bỏ tick trong 24 giờ) → NSM, retention, counter #1 |
| `roadmap_pace_set` | User tự đặt quỹ giờ học mỗi tuần | Sau khi giá trị `hours_per_week` mới được lưu (server) | Leading #2; segment theo `hours_per_week` (04). Cần thêm endpoint lưu `hours_per_week` (hiện chỉ đổi ở frontend) |
| `roadmap_step_completed` | Mọi action của một step đều đã hoàn thành | Khi action cuối cùng của step chuyển sang hoàn thành (server) | Leading #3, engagement depth; xác định nhóm "tốt nghiệp" khi là step cuối (segment 04) |
| `reassessment_completed` | Kỹ năng của một step đã được chấm lại và lưu kết quả | Sau khi kết quả reassessment (điểm thật) được lưu (server) | Counter #2 |

"Action hợp lệ" không phải một event riêng. Nó được tính từ `roadmap_action_completed` và `roadmap_action_uncompleted` theo completion rule ở 01.

### Acceptance criteria

1. **Chỉ ghi khi hành vi thật sự hoàn tất:** với mỗi cặp `user_id` và action (`roadmap_id`, `step_order`, `action_index`), hệ thống chỉ ghi `roadmap_action_completed` khi action chuyển từ chưa hoàn thành sang hoàn thành **và** bản ghi đã được lưu DB thành công. Bấm nút mà request lỗi không tạo event. Gửi lại `done=true` cho action đã hoàn thành cũng không tạo event và không ghi đè `done_at` (code hiện tại đang ghi đè `done_at` mỗi lần nhận `done=true`).
2. **Reload / retry không ghi trùng:** tải lại trang, double-click hay retry mạng không tạo thêm `roadmap_action_completed` cho cùng một lần chuyển trạng thái. Mỗi event có `event_id` duy nhất tạo từ (`user_id`, action, `done_at`), và pipeline loại bỏ event trùng `event_id`.
3. **Không tính dữ liệu giả:** `reassessment_completed` không được ghi khi điểm là giá trị mock (`before=2.0`, `after=4.0` hard-code); event này chỉ bật khi đã có chấm điểm thật.

**Tự kiểm Gate 4:** loop 2 chu kỳ, hypothesis trỏ về NSM ở 03 ✔ · 6 event, mỗi event map về ít nhất 1 metric ✔ · 3 acceptance criteria, có đủ 2 bẫy "chưa hoàn tất" và "ghi trùng" ✔

## Tự soi lỗi (Gate 5)

| # | Câu tự soi | Kết quả | Ghi chú |
| --- | --- | --- | --- |
| 1 | Core action không phải thao tác giao diện hay output hệ thống? | Đạt | Là hành vi học của user (hoàn thành action), không phải "xem roadmap" hay "AI sinh roadmap" (01) |
| 2 | Activation không phải "xem hết hướng dẫn" hay "đăng nhập"? | Đạt | Activation = action hợp lệ đầu tiên trong 7 ngày sau `roadmap_generated` (03) |
| 3 | Frequency không cao hơn nhu cầu thật? | Giữ nguyên, có lý do | Nhịp đo theo tuần, không daily. NSM yêu cầu ≥2 action/tuần, có thể cao với user chỉ học 5 giờ/tuần. Tôi giữ ngưỡng này vì nó khớp kết luận "vài lần mỗi tuần" ở 02, và sẽ so NSM theo segment `hours_per_week` (04) để kiểm tra |
| 4 | Loop có reason to return ngoài notification? | Đạt | Khoảng cách tới JD còn lại, bước tiếp theo đã rõ, hạn tuyển dụng thật (05) |
| 5 | Retention không dùng chung một window cho mọi cadence? | Đạt | Core action đo theo tuần (W1–W8); metric ở cấp step (leading #3, engagement depth) đo theo `estimated_weeks` của step |
| 6 | Mọi event đều map về một metric? | Đạt | Cả 6 event trong bảng 06 đều có cột "Metric sử dụng" |
| 7 | Metric nào cũng có event để tính nó? | Đạt, kèm 2 điều kiện | (a) Leading #2 cần `roadmap_pace_set`, nhưng backend hiện chưa có endpoint lưu `hours_per_week` (frontend chỉ đổi state), nên phải thêm endpoint. (b) Counter #2 cần chấm điểm reassessment thật. Segment persona lấy từ dữ liệu onboarding (user property), không cần event riêng |

## Revision

| Mục | Thay đổi | Lý do |
| --- | --- | --- |
| 01 | Completion rule từ "`done=true` được lưu" thành thêm 2 điều kiện: không bỏ tick trong 24 giờ, cách lần trước ≥5 phút | Lần đầu tự kiểm được 3.5/5, trượt tiêu chí "Có ý nghĩa" vì tick hàng loạt làm số tăng ảo. Tôi siết completion rule thay vì bắt buộc evidence để không làm giảm số lần thực hiện |
| 05 | Sửa nhận định "màn roadmap chưa hiển thị tiến độ" | Kiểm tra lại code: dashboard roadmap đã có checklist action và pill trạng thái step. Phần cần thêm chỉ là số đếm tiến độ ngay sau khi tick |
| 06 | Sửa metric của `roadmap_generated` và `roadmap_pace_set` | Mẫu số của engagement depth (số action và `estimated_weeks` của step) lấy từ roadmap, không phải từ `hours_per_week` |

## Điều tôi mang về áp dụng cho dự án thật

- **Đo việc học, không đo việc dùng app.** Từ nay team P-040 lấy "action học tập hợp lệ" làm thước đo chính, thay cho số CV upload hay số roadmap được sinh. Roadmap do AI tạo ra chỉ là output; value chỉ xảy ra khi user học xong một bước.
- **Đo theo tuần và chấp nhận điểm kết thúc.** Không dùng DAU hay D1/D7 cho PathPlanner. User xong roadmap rồi đi ứng tuyển được tính là "tốt nghiệp", không phải churn.
- **Sửa 3 chỗ trong code trước khi tin vào số liệu:** (1) `update_action_progress` không được ghi đè `done_at` khi action đã hoàn thành; (2) thay điểm reassessment hard-code (2.0 → 4.0) bằng chấm điểm thật; (3) thêm endpoint lưu `hours_per_week`, vì hiện giá trị này chỉ đổi ở frontend.
- **Bắn 6 event phía server, sau khi lưu DB thành công**, kèm `event_id` để khử trùng, thay vì track click ở frontend. Song song đó, theo dõi tỷ lệ tick bị loại để biết NSM có đang bị "làm đẹp" hay không.

## AI Support Log

Xem [`ai-support-log.md`](ai-support-log.md).

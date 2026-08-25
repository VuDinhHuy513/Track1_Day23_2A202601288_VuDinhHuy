# Metrics Pack — Support Radar

## 00 — Dự án, persona, core job

- **Dự án:** AI tổng hợp phàn nàn khách hàng (Support Radar)
- **Persona:** Trưởng nhóm CSKH của một sàn TMĐT, quản lý 8 nhân viên
- **Core job:** Mỗi sáng cần biết 3 vấn đề khiến khách bực nhất hôm qua để giao việc cho team, thay vì đọc tay 400 hội thoại

## 01 — Core Action Card (+ kết quả tự kiểm 5 tiêu chí)

| Thành phần | Câu trả lời |
|---|---|
| Target user | Trưởng nhóm CSKH của sàn TMĐT, quản lý 8 nhân viên |
| Core job | Biết 3 vấn đề khiến khách bực nhất hôm qua để giao việc cho team |
| Core action | Xem hết (đọc tới mục cuối) báo cáo "Top 3 vấn đề nóng hôm qua" |
| Object | Báo cáo tổng hợp Top 3 vấn đề nóng, do hệ thống tự sinh từ toàn bộ hội thoại hỗ trợ ngày hôm trước |
| Preconditions | Batch tổng hợp hội thoại của ngày hôm trước đã chạy xong; báo cáo đã sẵn sàng trước giờ trưởng nhóm vào ca sáng |
| Completion rule | Trưởng nhóm đã xem đủ cả 3 mục vấn đề trong báo cáo (scroll/mở tới card thứ 3, hoặc thời gian xem đạt ngưỡng đọc tối thiểu) — không tính chỉ mở trang |
| Core value | Biết chính xác vấn đề gì đang khiến khách bực nhất mà không phải tự đọc ~400 hội thoại |
| Evidence of value | Ngay sau đó trưởng nhóm giao việc xử lý đúng vấn đề trong báo cáo cho nhân viên (hành vi kế tiếp xác nhận báo cáo có tác dụng) |
| Candidate event | `report_viewed` (khi xem đủ 3/3 mục) |

**Tự kiểm 5 tiêu chí:**

1. **Gắn core value** — Đạt. Xem hết 3 vấn đề là bước tiến rõ rệt tới giá trị "biết vấn đề để giao việc", không phải bước trung gian mơ hồ.
2. **Có thể lặp lại** — Đạt. Diễn ra lại mỗi khi có báo cáo mới (mỗi sáng).
3. **Có thể quan sát** — Đạt, với điều kiện: completion rule phải là "xem đủ 3/3 mục" chứ không phải "mở trang report" — tránh nhầm với thao tác giao diện.
4. **Có ý nghĩa** — Đạt. Tỷ lệ trưởng nhóm xem hết báo cáo tăng thật sự nghĩa là sản phẩm đang thay được việc đọc tay 400 hội thoại.
5. **Có thể tác động** — Đạt. Team có thể cải thiện độ chính xác tóm tắt, thứ tự ưu tiên vấn đề, tốc độ sinh báo cáo để tăng tỷ lệ đọc hết.

Qua 5/5 tiêu chí → **GATE 1 đạt**.

*Lưu ý tự nhắc:* core action ở đây là "đọc hết", không phải "mở app xem báo cáo" — nếu tracking chỉ bắn event khi mở trang thì sẽ rơi vào bẫy lỗi kinh điển ở Phase 4/6.

## 02 — Action Nature Card + kết luận cadence

| Thành phần | Nội dung |
|---|---|
| Actor | Trưởng nhóm CSKH (user) |
| Intent | Cần biết vấn đề nóng để giao việc đầu ca, tránh tự đọc 400 hội thoại |
| Trigger | Hệ thống tự sinh báo cáo mới sau khi batch tổng hợp hội thoại ngày hôm trước chạy xong (system-triggered) |
| Value timing | Ngay lập tức — đọc xong là biết luôn, không tích lũy |
| State | Trạng thái "đã xem" / "chưa xem" của báo cáo hôm đó được lưu lại |
| Dependency | Phụ thuộc dữ liệu hội thoại ngày hôm trước đã đóng (business day đã kết thúc) |
| Repeat condition | Một ngày kinh doanh mới trôi qua → có hội thoại mới → có lý do xem lại |

**Kết luận cadence:**

Đối với trưởng nhóm CSKH, core action "xem hết báo cáo Top 3 vấn đề nóng" thường xuất hiện **mỗi ngày làm việc (đầu ca sáng)** vì dữ liệu hội thoại được tổng hợp theo chu kỳ một ngày kinh doanh và trưởng nhóm cần thông tin trước khi giao việc đầu ngày. Do đó, nhịp đo phù hợp là **daily**, ở cấp **từng trưởng nhóm (cá nhân)**.

GATE 2 đạt: cadence suy ra từ nhịp vận hành thật (chu kỳ ngày kinh doanh), không chọn daily vì "dashboard hay dùng daily".

## 03 — Metric System (activation / engagement / NSM / leading / counter)

**Activation metric**
- Start event: trưởng nhóm được cấp tài khoản và đăng nhập lần đầu
- Activation event: xem hết báo cáo Top 3 vấn đề nóng đầu tiên (`report_viewed`, đủ 3/3 mục)
- Time window: trong vòng 3 ngày làm việc kể từ khi có tài khoản

**Engagement metric**
- Frequency: số ngày làm việc/tuần trưởng nhóm xem hết báo cáo (tối đa 5/tuần, theo nhịp daily đã kết luận)
- Depth: trong mỗi lần xem, bao nhiêu/3 vấn đề được giao việc tiếp theo (đo report có thực sự dẫn tới hành động, không chỉ được đọc)

**North Star Metric**
> Số trưởng nhóm CSKH xem hết báo cáo Top 3 vấn đề nóng (`report_viewed` đủ 3/3 mục) ít nhất 4/5 ngày làm việc trong tuần.

- Unit of value: trưởng nhóm xem hết báo cáo
- Quality threshold: đủ 3/3 mục (không tính chỉ mở trang)
- Frequency: ≥4/5 ngày làm việc/tuần

**Leading indicators**
1. Thời gian để xem hết báo cáo lần đầu (activation latency) — activation càng nhanh, càng dễ hình thành thói quen daily.
2. Tỷ lệ báo cáo đủ dữ liệu tin cậy, không lỗi tổng hợp — báo cáo chính xác thì trưởng nhóm mới có lý do quay lại mỗi ngày.
3. Số lần giao việc dựa trên báo cáo trên mỗi lần xem — report dẫn tới hành động thật thì khả năng quay lại ngày sau cao hơn.

**Counter-metric**
- Tỷ lệ vấn đề trong Top 3 bị trưởng nhóm đánh dấu "không chính xác/không liên quan". Nếu view rate (NSM) tăng nhưng chỉ số này cũng tăng, nghĩa là hệ thống đang bị tối ưu để dễ đọc hết chứ không phải đúng nội dung — NSM đang bị game.

## 04 — Retention Definition (6 thành phần)

| Thành phần | Nội dung |
|---|---|
| Unit | Trưởng nhóm CSKH (1 unit = 1 tài khoản trưởng nhóm) |
| Cohort entry | Ngày activation event xảy ra (lần đầu xem hết báo cáo) |
| Return event | `report_viewed` lặp lại (xem hết báo cáo đủ 3/3 mục) |
| Window | Daily — theo ngày làm việc (business day) |
| Threshold | ≥1 lần xem hết báo cáo trong window đó |
| Segment | Theo quy mô đội quản lý (số nhân viên CSKH trực tiếp), để không so sánh trưởng nhóm quản lý 3 người với người quản lý 20 người |

GATE 3 đạt: activation có start/activation event + time window; retention đủ 6 thành phần và khớp cadence daily; NSM đúng công thức 3 thành phần; có 1 counter-metric.

## 05 — Product Loop (2 chu kỳ + metric hypothesis)

**Loop (2 chu kỳ):**

Báo cáo mới sẵn sàng đầu ca (natural trigger) → Xem hết Top 3 (core action) → Biết ngay vấn đề khiến khách bực nhất (immediate value) → Giao việc xử lý cho nhân viên (saved state / investment) → Ngày làm việc tiếp theo kết thúc, dữ liệu mới gồm cả kết quả xử lý hôm qua (next natural trigger) → Xem báo cáo hôm sau (core action lặp) → Biết vấn đề mới + xác nhận hiệu quả xử lý trước đó (repeat value)

**Loại loop chính:** workflow — gắn với quy trình báo cáo → giao việc → xử lý → báo cáo mới, không phải habit tiêu dùng thông thường.

**Metric hypothesis:** Nếu loop này hoạt động, metric **Engagement Frequency** (số ngày/tuần xem hết báo cáo) sẽ thay đổi theo hướng **tăng dần tới 5/5 ngày làm việc** trong **4 tuần đầu sử dụng**, vì mỗi lần giao việc dựa trên báo cáo hôm trước tạo động lực quay lại xem báo cáo hôm sau để kiểm tra hiệu quả xử lý.

**Reason to return (nếu bỏ notification):** báo cáo hôm sau tự chứa phản hồi về hiệu quả xử lý hôm qua — đây là lý do nội tại, không dựa vào notification/gamification.

## 06 — Tracking nhanh (4-8 events + 2 acceptance criteria)

| Tên event | Ý nghĩa | Thời điểm ghi nhận | Metric sử dụng |
|---|---|---|---|
| `account_created` | Tài khoản trưởng nhóm được tạo | Khi tài khoản kích hoạt xong | Activation (start event) |
| `report_viewed` | Xem đủ 3/3 mục trong báo cáo Top 3 | Khi cuộn/mở tới card thứ 3 hoặc đạt ngưỡng thời gian đọc tối thiểu | Activation event, NSM, Engagement Frequency, Retention return event |
| `task_assigned_from_report` | Giao việc xử lý một vấn đề trong báo cáo | Khi giao việc được lưu thành công, gắn đúng vấn đề trong báo cáo | Engagement Depth, Evidence of value, Leading indicator #3 |
| `report_issue_flagged_inaccurate` | Đánh dấu một vấn đề là không chính xác/không liên quan | Khi bấm "báo sai" gắn với vấn đề cụ thể | Counter-metric |
| `report_generation_completed` | Hệ thống hoàn tất tổng hợp báo cáo, kèm cờ đủ/thiếu dữ liệu nguồn | Ngay sau batch xử lý xong, trước khi hiển thị | Leading indicator #2, tiền đề natural trigger |
| `resolved_issue_confirmed` | Nhân viên xác nhận đã xử lý xong vấn đề được giao | Khi nhân viên submit trạng thái "đã xử lý" | Saved-state cho loop, dữ liệu đầu vào cho báo cáo hôm sau |

**2 tiêu chí nghiệm thu:**
1. Với mỗi cặp (trưởng_nhóm_id, report_id), hệ thống chỉ ghi `report_viewed` một lần khi đã xem đủ cả 3 mục. Tải lại trang hoặc cuộn qua lại card đã xem không tạo thêm event.
2. Với mỗi cặp (report_issue_id, nhân_viên_được_giao), `task_assigned_from_report` chỉ ghi khi việc giao đã lưu thành công — không ghi khi mới mở form; giao lại cho cùng người trên cùng vấn đề không tạo event trùng.

GATE 4 đạt: loop ≥2 chu kỳ và có metric hypothesis trỏ về metric ở Phase 3; mọi event đều map được về ít nhất một metric.

## Tự soi lỗi (Phase 5 — GATE 5)

Đối chiếu 7 câu tự kiểm:
1. Core action không phải thao tác giao diện/output hệ thống? ✅
2. Activation không phải "xem hướng dẫn"/"đăng nhập"? ✅
3. Frequency không cao hơn nhu cầu thật? ✅
4. Loop có reason to return ngoài notification? ✅
5. Retention không dùng chung window cho nhiều cadence? ✅ (chỉ 1 cadence — daily)
6. Mọi event map về một metric? ✅
7. Metric nào cũng có event để tính? ✅

Không phát hiện lỗi kinh điển cần sửa.

## 07 — Revision (nếu có thay đổi lớn: lý do đổi core action / cadence)

Không có thay đổi lớn — core action, cadence, metric, loop giữ nguyên như bản đầu qua tất cả các gate.

1. Tổng quan module M2
Module M2 - Matching & Scheduling chịu trách nhiệm xử lý toàn bộ quá trình từ khi người dùng tạo nhu cầu luyện mock interview đến khi có lịch phỏng vấn được xác nhận. Module này là cầu nối giữa User Management và Interview Session, đảm bảo hai người dùng có nhu cầu phù hợp được ghép cặp và có lịch luyện tập rõ ràng.
Nội dung	Mô tả
Mục tiêu	Kết nối hai sinh viên IT có cùng nhu cầu luyện phỏng vấn theo chủ đề, thời gian và mức độ phù hợp.
Đầu vào	Hồ sơ người dùng, chủ đề phỏng vấn, thời gian rảnh, vị trí mong muốn, kỹ năng, yêu cầu luyện tập.
Đầu ra	Kết quả ghép cặp, lịch phỏng vấn đã xác nhận, trạng thái lịch và dữ liệu chuyển sang Interview Session.
Tác nhân chính	User, hệ thống Matching & Scheduling, Admin trong vai trò giám sát/quản lý.
Phạm vi chính	Create Request, Select Topic, Select Available Time, Match Users, View Match Result, Confirm/Cancel/Reschedule, View Schedule.
1.1 Liên kết với luồng nghiệp vụ tổng thể
1.	Người dùng đăng ký/đăng nhập và cập nhật hồ sơ cơ bản.
2.	Người dùng chọn chủ đề, thời gian phù hợp và tạo yêu cầu mock interview.
3.	Hệ thống tìm các yêu cầu tương thích, ghép cặp hai người dùng phù hợp.
4.	Hai người dùng xem kết quả ghép cặp và xác nhận lịch.
5.	Sau khi lịch được xác nhận, session được tạo để chuyển sang module Interview Session.
2. Khảo sát nhu cầu cho M2
Mục tiêu khảo sát là kiểm chứng nhu cầu ghép cặp luyện phỏng vấn, lựa chọn chủ đề phỏng vấn phổ biến, thói quen đặt lịch và các tiêu chí người dùng mong muốn khi được ghép cặp.
Nhóm khảo sát	Câu hỏi đề xuất	Loại câu hỏi	Mục đích phân tích
Thông tin người dùng	Bạn đang là sinh viên năm mấy?	Single choice	Phân loại nhóm người dùng theo mức độ chuẩn bị thực tập/xin việc.
Thông tin người dùng	Bạn đang hướng tới vị trí nào?	Multiple choice	Xác định vị trí phổ biến: Backend, Frontend, Mobile, Data, DevOps, QA.
Nhu cầu luyện tập	Bạn có thường luyện phỏng vấn trước khi apply thực tập/xin việc không?	Single choice	Đo mức độ cần thiết của hệ thống.
Nhu cầu luyện tập	Khó khăn lớn nhất khi luyện phỏng vấn là gì?	Multiple choice	Xác định pain point: thiếu người luyện cùng, thiếu câu hỏi, thiếu feedback, khó sắp lịch.
Matching	Bạn muốn được ghép với người có tiêu chí nào?	Ranking	Xác định trọng số ghép cặp: cùng topic, cùng thời gian, cùng level, cùng vị trí mong muốn.
Matching	Bạn có chấp nhận luyện với người chưa quen không?	Likert 1-5	Đánh giá mức độ phù hợp của mô hình peer-to-peer.
Topic	Bạn muốn luyện nhóm chủ đề nào?	Multiple choice	Xác định danh sách topic ưu tiên ban đầu.
Topic	Bạn muốn hệ thống gợi ý câu hỏi theo topic không?	Yes/No	Xác nhận nhu cầu về suggested questions.
Schedule	Bạn thường rảnh vào khung giờ nào?	Multiple choice	Xây dựng slot thời gian gợi ý.
Schedule	Bạn muốn đặt lịch trước bao lâu?	Single choice	Thiết kế rule về lead time.
Schedule	Bạn có cần đổi lịch/hủy lịch không?	Yes/No + lý do	Xác định yêu cầu Cancel/Reschedule.
Feedback	Bạn có muốn xem lịch sử session sau khi luyện không?	Yes/No	Liên kết với module Feedback/History.
2.1 Báo cáo khảo sát M2 - mẫu tổng hợp
Chỉ số cần tổng hợp	Cách tính	Ý nghĩa thiết kế
Tỷ lệ có nhu cầu luyện mock interview	Số người chọn Có / tổng số phản hồi	Nếu cao, ưu tiên luồng tạo request nhanh và dễ dùng.
Top 5 chủ đề phổ biến	Đếm số lần chọn từng topic	Dùng để tạo topic mặc định trong hệ thống.
Top khung giờ rảnh	Thống kê theo buổi sáng/chiều/tối/cuối tuần	Hỗ trợ gợi ý available time.
Tiêu chí matching quan trọng nhất	Điểm ranking trung bình từng tiêu chí	Làm cơ sở cho thuật toán matching.
Tỷ lệ cần cancel/reschedule	Số người chọn Có / tổng số phản hồi	Xác định mức độ ưu tiên FR-MAT-07 và FR-MAT-08.
3. Research & Analysis - Matching & Scheduling
3.1 Vấn đề nghiệp vụ
•	Sinh viên thường thiếu người luyện phỏng vấn cùng chủ đề và cùng khung giờ rảnh.
•	Việc tự tìm người luyện thủ công mất thời gian, khó kiểm soát lịch và dễ bị hủy kèo.
•	Nếu không có quy trình xác nhận lịch, session có thể không diễn ra hoặc không chuyển đúng sang bước phỏng vấn.
•	Hệ thống cần kiểm soát trạng thái yêu cầu và lịch để tránh trùng lịch, ghép sai topic hoặc cho người dùng tham gia session chưa xác nhận.
3.2 Mục tiêu phân tích
Mục tiêu	Diễn giải
Ghép đúng người	Hai user phải có cùng/chấp nhận cùng chủ đề và có thời gian phù hợp.
Giảm thao tác thủ công	Người dùng chỉ cần tạo request, chọn topic/time, hệ thống tự đề xuất match.
Kiểm soát lịch rõ ràng	Lịch có trạng thái Pending, Matched, Confirmed, Cancelled, Rescheduled, Expired.
Hạn chế xung đột	Không tạo lịch nếu một trong hai user đã có lịch confirmed trùng thời gian.
Dễ mở rộng	Có thể bổ sung tiêu chí matching như level, desired position, rating, mentor/recruiter trong tương lai.
3.3 Tiêu chí matching đề xuất
Tiêu chí	Bắt buộc/Tùy chọn	Quy tắc đề xuất
Topic	Bắt buộc	Hai request phải có cùng topic hoặc topic tương đương theo mapping của admin.
Available time	Bắt buộc	Hai user phải có ít nhất một slot thời gian trùng nhau.
Desired position	Tùy chọn	Ưu tiên ghép cùng vị trí mong muốn như Backend/Frontend/QA/Data.
Skill level	Tùy chọn	Ưu tiên ghép cùng level hoặc lệch không quá 1 bậc để đảm bảo trải nghiệm ngang hàng.
No duplicate active schedule	Bắt buộc	Không ghép user nếu slot đã có lịch Confirmed/In Progress.
Recent match history	Tùy chọn	Không ưu tiên ghép lại cùng một người trong thời gian ngắn để tăng đa dạng luyện tập.
4. Functional Requirements của M2
Mã FR	Tên yêu cầu	Mô tả chi tiết	Điều kiện trước	Kết quả đầu ra
FR-MAT-01	Create Mock Interview Request	Hệ thống cho phép user tạo yêu cầu mock interview sau khi đã đăng nhập và cập nhật hồ sơ cơ bản.	User đã đăng nhập; hồ sơ có topic/desired position tối thiểu.	Request được tạo với trạng thái Pending.
FR-MAT-02	Select Interview Topic	Hệ thống cho phép user chọn chủ đề phỏng vấn từ danh sách topic hợp lệ.	Danh sách topic đang Active.	Topic được gắn vào request.
FR-MAT-03	Select Available Time	Hệ thống cho phép user chọn một hoặc nhiều khung giờ phù hợp.	Khung giờ không nằm trong quá khứ.	Available slots được lưu vào request.
FR-MAT-04	Match Users	Hệ thống tự động hoặc thủ công tìm user phù hợp theo topic và thời gian.	Có ít nhất hai request Pending tương thích.	Match result được tạo, request chuyển Matched.
FR-MAT-05	View Match Result	Hệ thống hiển thị thông tin ghép cặp cho user.	User là một trong hai người được match.	User xem được partner, topic, slot đề xuất, trạng thái xác nhận.
FR-MAT-06	Confirm Schedule	Hệ thống cho phép hai user xác nhận lịch phỏng vấn.	Match result còn hiệu lực; cả hai user chưa trùng lịch.	Schedule chuyển Confirmed khi cả hai xác nhận.
FR-MAT-07	Cancel Schedule	Hệ thống cho phép user hủy lịch theo rule cho phép.	Schedule ở Pending/Confirmed và chưa bắt đầu.	Schedule chuyển Cancelled, bên còn lại nhận thông báo.
FR-MAT-08	Reschedule Interview	Hệ thống cho phép user đề xuất đổi lịch phỏng vấn.	Schedule chưa bắt đầu; slot mới hợp lệ.	Tạo đề xuất reschedule, chờ bên còn lại xác nhận.
FR-MAT-09	View Interview Schedule	Hệ thống cho phép user xem danh sách lịch phỏng vấn của mình.	User đã đăng nhập.	Hiển thị lịch theo trạng thái và thời gian.
5. Use Case Specification
UC-MAT-01 - Create Mock Interview Request
Thành phần	Nội dung
Mục tiêu	User tạo yêu cầu luyện mock interview để hệ thống đưa vào hàng chờ matching.
Actor chính	User
Tiền điều kiện	User đã đăng nhập, hồ sơ cơ bản đã được cập nhật.
Luồng chính	1) User mở màn hình Create Request. 2) Chọn desired position/topic. 3) Chọn thời gian rảnh. 4) Nhập ghi chú nếu có. 5) Bấm Submit. 6) Hệ thống kiểm tra dữ liệu. 7) Hệ thống lưu request trạng thái Pending.
Luồng thay thế	Nếu thiếu topic hoặc thời gian, hệ thống hiển thị lỗi và yêu cầu bổ sung.
Hậu điều kiện	Request được lưu; hệ thống có thể tiến hành matching.
UC-MAT-02 - Select Topic and Available Time
Thành phần	Nội dung
Mục tiêu	User chọn đúng chủ đề và khung giờ rảnh để tăng khả năng match.
Actor chính	User
Tiền điều kiện	Danh sách topic active tồn tại; user đang ở form tạo/sửa request.
Luồng chính	1) Hệ thống hiển thị danh sách topic. 2) User chọn topic. 3) Hệ thống hiển thị lịch/chọn slot. 4) User chọn một hoặc nhiều slot. 5) Hệ thống validate slot không nằm trong quá khứ.
Luồng thay thế	Nếu topic bị disabled, hệ thống không cho chọn và gợi ý topic khác.
Hậu điều kiện	Topic và available slots được gắn với request.
UC-MAT-03 - Match Users
Thành phần	Nội dung
Mục tiêu	Hệ thống ghép hai user có nhu cầu phù hợp.
Actor chính	Matching Service/System
Tiền điều kiện	Có request Pending; có user khác cùng topic và slot tương thích.
Luồng chính	1) Hệ thống lấy danh sách request Pending. 2) Lọc theo topic. 3) Lọc theo slot trùng nhau. 4) Kiểm tra trùng lịch confirmed. 5) Tính điểm phù hợp. 6) Tạo match result với slot đề xuất. 7) Thông báo cho hai user.
Luồng thay thế	Nếu không tìm thấy đối tượng phù hợp, request giữ Pending và có thể gợi ý mở rộng thời gian/topic.
Hậu điều kiện	Match result được tạo hoặc request tiếp tục ở hàng chờ.
UC-MAT-04 - Confirm Schedule
Thành phần	Nội dung
Mục tiêu	Hai user xác nhận lịch để tạo session phỏng vấn.
Actor chính	User A, User B
Tiền điều kiện	Match result còn hiệu lực và chưa bị hủy.
Luồng chính	1) User xem match result. 2) User A bấm Confirm. 3) User B bấm Confirm. 4) Hệ thống kiểm tra lại xung đột lịch. 5) Hệ thống tạo schedule Confirmed. 6) Hệ thống tạo dữ liệu session đầu vào cho module Interview Session.
Luồng thay thế	Nếu một user không xác nhận trước hạn, match result hết hiệu lực và request có thể quay lại Pending.
Hậu điều kiện	Schedule Confirmed, session sẵn sàng tham gia đúng thời gian.
UC-MAT-05 - Cancel/Reschedule Interview
Thành phần	Nội dung
Mục tiêu	User hủy hoặc đề xuất đổi lịch khi không thể tham gia.
Actor chính	User
Tiền điều kiện	Schedule chưa bắt đầu và user thuộc schedule.
Luồng chính	1) User mở chi tiết lịch. 2) Chọn Cancel hoặc Reschedule. 3) Nếu Cancel, nhập lý do và xác nhận. 4) Nếu Reschedule, chọn slot mới. 5) Hệ thống cập nhật trạng thái/đề xuất. 6) Gửi thông báo cho partner.
Luồng thay thế	Nếu lịch đã In Progress/Completed, hệ thống không cho hủy/đổi.
Hậu điều kiện	Schedule được cập nhật Cancelled hoặc Pending Reschedule.
6. Diagram & Design
6.1 Class Diagram M2
Ghi chú: Có thể copy khối Mermaid dưới đây vào báo cáo/GitHub/Markdown để render thành sơ đồ.
classDiagram
class User {
  +UUID userId
  +String fullName
  +String email
  +String desiredPosition
  +String skillLevel
}
class InterviewTopic {
  +UUID topicId
  +String topicName
  +String description
  +Boolean active
}
class MatchRequest {
  +UUID requestId
  +UUID userId
  +UUID topicId
  +String desiredPosition
  +String level
  +RequestStatus status
  +DateTime createdAt
  +DateTime expiredAt
}
class AvailabilitySlot {
  +UUID slotId
  +UUID requestId
  +DateTime startTime
  +DateTime endTime
  +SlotStatus status
}
class MatchResult {
  +UUID matchId
  +UUID requestAId
  +UUID requestBId
  +UUID proposedSlotId
  +Number matchScore
  +MatchStatus status
}
class Schedule {
  +UUID scheduleId
  +UUID matchId
  +DateTime startTime
  +DateTime endTime
  +ScheduleStatus status
  +Boolean userAConfirmed
  +Boolean userBConfirmed
}
class RescheduleRequest {
  +UUID rescheduleId
  +UUID scheduleId
  +UUID requestedBy
  +DateTime newStartTime
  +DateTime newEndTime
  +String reason
  +RescheduleStatus status
}
User "1" --> "0..*" MatchRequest
InterviewTopic "1" --> "0..*" MatchRequest
MatchRequest "1" --> "1..*" AvailabilitySlot
MatchRequest "1" --> "0..*" MatchResult
MatchResult "1" --> "0..1" Schedule
Schedule "1" --> "0..*" RescheduleRequest
6.2 ERD M2
Ghi chú: Có thể copy khối Mermaid dưới đây vào báo cáo/GitHub/Markdown để render thành sơ đồ.
erDiagram
USER ||--o{ MATCH_REQUEST : creates
INTERVIEW_TOPIC ||--o{ MATCH_REQUEST : selected_for
MATCH_REQUEST ||--o{ AVAILABILITY_SLOT : has
MATCH_REQUEST ||--o{ MATCH_RESULT : participates_in
MATCH_RESULT ||--o| SCHEDULE : creates
SCHEDULE ||--o{ RESCHEDULE_REQUEST : may_have
USER {
  uuid user_id PK
  string full_name
  string email
  string desired_position
  string skill_level
}
INTERVIEW_TOPIC {
  uuid topic_id PK
  string topic_name
  boolean active
}
MATCH_REQUEST {
  uuid request_id PK
  uuid user_id FK
  uuid topic_id FK
  string status
  datetime created_at
  datetime expired_at
}
AVAILABILITY_SLOT {
  uuid slot_id PK
  uuid request_id FK
  datetime start_time
  datetime end_time
  string status
}
MATCH_RESULT {
  uuid match_id PK
  uuid request_a_id FK
  uuid request_b_id FK
  decimal match_score
  string status
}
SCHEDULE {
  uuid schedule_id PK
  uuid match_id FK
  datetime start_time
  datetime end_time
  string status
  boolean user_a_confirmed
  boolean user_b_confirmed
}
RESCHEDULE_REQUEST {
  uuid reschedule_id PK
  uuid schedule_id FK
  uuid requested_by FK
  datetime new_start_time
  datetime new_end_time
  string reason
  string status
}
6.3 Activity Diagram Matching/Scheduling
Ghi chú: Có thể copy khối Mermaid dưới đây vào báo cáo/GitHub/Markdown để render thành sơ đồ.
flowchart TD
A[User đăng nhập] --> B{Hồ sơ cơ bản đã đủ?}
B -- Chưa đủ --> C[Yêu cầu cập nhật hồ sơ]
B -- Đủ --> D[Chọn topic phỏng vấn]
D --> E[Chọn available time]
E --> F[Tạo MatchRequest]
F --> G[Hệ thống tìm request phù hợp]
G --> H{Có request cùng topic và trùng slot?}
H -- Không --> I[Giữ trạng thái Pending]
H -- Có --> J[Kiểm tra trùng lịch và tính điểm match]
J --> K[Tạo MatchResult]
K --> L[Thông báo cho 2 user]
L --> M{Cả 2 xác nhận?}
M -- Không/Quá hạn --> N[Match hết hiệu lực hoặc quay lại Pending]
M -- Có --> O[Tạo Schedule Confirmed]
O --> P[Chuyển dữ liệu sang Interview Session]
6.4 Sequence Diagram Matching
Ghi chú: Có thể copy khối Mermaid dưới đây vào báo cáo/GitHub/Markdown để render thành sơ đồ.
sequenceDiagram
actor UserA
participant UI as Web UI
participant MatchSvc as Matching Service
participant DB as Database
participant UserB
participant SessionSvc as Interview Session Service
UserA->>UI: Create mock interview request
UI->>MatchSvc: submitRequest(topic, slots, profile)
MatchSvc->>DB: save MatchRequest status=Pending
MatchSvc->>DB: find compatible pending requests
DB-->>MatchSvc: candidate requests
MatchSvc->>MatchSvc: calculate match score + validate conflict
MatchSvc->>DB: create MatchResult status=WaitingConfirm
MatchSvc-->>UI: show match result
MatchSvc-->>UserB: notify match result
UserA->>UI: Confirm schedule
UserB->>UI: Confirm schedule
UI->>MatchSvc: confirm(matchId)
MatchSvc->>DB: update confirmations
MatchSvc->>DB: create Schedule status=Confirmed
MatchSvc->>SessionSvc: create session draft
SessionSvc-->>MatchSvc: session created
MatchSvc-->>UI: schedule confirmed
7. Thiết kế dữ liệu chi tiết
Entity	Trường dữ liệu	Kiểu	Bắt buộc	Ghi chú
MatchRequest	requestId	UUID	Có	Khóa chính của yêu cầu luyện phỏng vấn.
MatchRequest	userId	UUID	Có	Người tạo yêu cầu.
MatchRequest	topicId	UUID	Có	Chủ đề phỏng vấn được chọn.
MatchRequest	desiredPosition	String	Không	Vị trí user muốn luyện.
MatchRequest	status	Enum	Có	Pending, Matched, Cancelled, Expired.
AvailabilitySlot	slotId	UUID	Có	Khóa chính slot.
AvailabilitySlot	requestId	UUID	Có	Liên kết MatchRequest.
AvailabilitySlot	startTime/endTime	DateTime	Có	Khoảng thời gian user rảnh.
MatchResult	matchId	UUID	Có	Kết quả ghép cặp.
MatchResult	requestAId/requestBId	UUID	Có	Hai request được ghép.
MatchResult	matchScore	Decimal	Có	Điểm phù hợp theo tiêu chí matching.
Schedule	scheduleId	UUID	Có	Lịch phỏng vấn được tạo.
Schedule	status	Enum	Có	WaitingConfirm, Confirmed, Cancelled, Rescheduled, Completed, No-show.
RescheduleRequest	reason	Text	Không	Lý do đổi lịch/hủy lịch.
8. Test Case phần Matching & Scheduling
Mã TC	Tên test	Tiền điều kiện	Bước/Input	Kết quả mong đợi	Ưu tiên
TC-MAT-01	Create Request thành công	User đã login, profile đủ	Chọn Backend + slot hợp lệ + Submit	Request được tạo trạng thái Pending	High
TC-MAT-02	Create Request khi thiếu topic	User đã login	Bỏ trống topic, chọn slot, Submit	Hệ thống báo lỗi bắt buộc chọn topic	High
TC-MAT-03	Chọn slot trong quá khứ	User đã login	Chọn thời gian trước hiện tại	Hệ thống từ chối slot không hợp lệ	High
TC-MAT-04	Match thành công theo topic/time	Có 2 request Pending cùng topic, trùng slot	Chạy matching	Tạo MatchResult WaitingConfirm	High
TC-MAT-05	Không match khi khác topic	2 request Pending khác topic	Chạy matching	Không tạo MatchResult	Medium
TC-MAT-06	Không match khi không trùng thời gian	2 request cùng topic nhưng slot lệch	Chạy matching	Request giữ Pending	Medium
TC-MAT-07	Xem match result đúng user	User thuộc match	Mở Match Result	Hiển thị partner, topic, slot, trạng thái	High
TC-MAT-08	Chặn xem match của người khác	User không thuộc match	Truy cập URL matchId khác	403/Không có quyền truy cập	High
TC-MAT-09	Confirm một phía	Có MatchResult WaitingConfirm	User A Confirm	userAConfirmed=true, lịch chưa Confirmed	High
TC-MAT-10	Confirm hai phía	User A và B đều Confirm	User B Confirm sau User A	Tạo Schedule Confirmed	High
TC-MAT-11	Confirm khi bị trùng lịch	User có schedule confirmed cùng slot	Confirm match mới	Hệ thống báo trùng lịch và không tạo schedule	High
TC-MAT-12	Cancel schedule trước giờ bắt đầu	Schedule Confirmed chưa bắt đầu	User bấm Cancel và nhập lý do	Schedule chuyển Cancelled, gửi thông báo	Medium
TC-MAT-13	Không cho cancel khi session đã bắt đầu	Schedule In Progress	User bấm Cancel	Hệ thống từ chối thao tác	Medium
TC-MAT-14	Reschedule thành công	Schedule chưa bắt đầu	User chọn slot mới, partner xác nhận	Schedule cập nhật giờ mới / trạng thái Confirmed	Medium
TC-MAT-15	View Interview Schedule	User có nhiều lịch	Mở My Schedule	Hiển thị danh sách lịch theo thời gian và trạng thái	Medium
9. SQA Plan - Test Technique cho M2
Kỹ thuật kiểm thử	Áp dụng cho	Lý do chọn	Ví dụ dữ liệu
Equivalence Partitioning	Topic, slot thời gian, trạng thái request	Chia dữ liệu thành nhóm hợp lệ/không hợp lệ để giảm số test nhưng vẫn bao phủ logic chính.	Topic active/inactive; slot tương lai/quá khứ; request Pending/Cancelled.
Boundary Value Analysis	Thời gian đặt lịch, thời hạn confirm, số slot tối đa	Các lỗi thường xảy ra ở biên thời gian và giới hạn số lượng.	Slot bắt đầu sau hiện tại 1 phút; confirm ngay trước/sau deadline.
Decision Table Testing	Luật matching và confirm schedule	Nhiều điều kiện kết hợp: cùng topic, trùng slot, không trùng lịch, cả hai confirm.	Cùng topic + trùng slot + không trùng lịch => Match; khác topic => không Match.
State Transition Testing	Vòng đời MatchRequest/Schedule	Module M2 phụ thuộc nhiều vào trạng thái, cần kiểm tra chuyển trạng thái hợp lệ và không hợp lệ.	Pending -> Matched -> Confirmed; Confirmed -> Cancelled; In Progress không được Cancel.
Use Case Testing	Các luồng Create Request, Match User, Confirm Schedule	Đảm bảo test đúng hành vi người dùng cuối theo nghiệp vụ.	User tạo request, được match, xác nhận lịch, xem lịch.
Security/Authorization Testing	Xem match/schedule của người khác	Đảm bảo user chỉ truy cập dữ liệu của chính mình.	User A truy cập schedule của User C phải bị chặn.
10. UI Mockup M2
10.1 Màn hình Create Mock Interview Request
Khu vực UI	Thành phần	Ghi chú thiết kế
Header	Tiêu đề: Create Mock Interview Request	Hiển thị ngắn gọn, có breadcrumb Dashboard > Matching.
Profile summary	Desired position, skill level	Cho user kiểm tra nhanh thông tin dùng để matching.
Topic selection	Dropdown/Searchable select topic	Chỉ hiển thị topic Active.
Available time	Calendar/slot picker	Cho chọn nhiều slot; highlight slot đã có lịch.
Note	Textarea ghi chú	Không bắt buộc, dùng để mô tả mong muốn luyện tập.
Action	Submit Request / Save Draft / Cancel	Submit nổi bật, Cancel phụ.
10.2 Màn hình Match Result
Khu vực UI	Thành phần	Ghi chú thiết kế
Match card	Partner name, desired position, skill level	Không hiển thị thông tin nhạy cảm như email nếu chưa cần.
Topic & time	Topic, proposed slot, duration	Nêu rõ thời gian và múi giờ.
Match explanation	Lý do phù hợp: cùng topic, trùng slot, cùng level	Giúp user tin tưởng kết quả ghép cặp.
Status	Waiting for your confirm / Waiting for partner / Confirmed	Trạng thái cần hiển thị rõ.
Action	Confirm, Propose another time, Cancel	Tách màu/nút theo mức độ quan trọng.
10.3 Màn hình My Interview Schedule
Khu vực UI	Thành phần	Ghi chú thiết kế
Filter	All, Pending, Confirmed, Cancelled, Completed	Lọc theo trạng thái.
Schedule list	Date, time, topic, partner, status	Dạng card hoặc table tùy thiết bị.
Schedule detail	Thông tin chi tiết lịch và rule tham gia	Có countdown nếu gần giờ phỏng vấn.
Action	Join Session, Reschedule, Cancel	Join chỉ bật khi lịch Confirmed và đến gần giờ.
10.4 Wireframe dạng text - Create Request
Ghi chú: Có thể copy khối Mermaid dưới đây vào báo cáo/GitHub/Markdown để render thành sơ đồ.
+-------------------------------------------------------+
| M2 - Create Mock Interview Request                    |
+-------------------------------------------------------+
| Profile Summary: Backend | Junior | Java, Spring Boot |
|                                                       |
| Topic: [ Backend Interview            v ]             |
| Desired Position: [ Backend Developer  v ]            |
| Available Time:                                      |
| [ Mon 19:00-20:00 ] [ Tue 20:00-21:00 ] [ Sat AM ]   |
|                                                       |
| Note: [ Tôi muốn luyện câu hỏi về REST API, SQL... ]  |
|                                                       |
|        [Cancel]                  [Submit Request]     |
+-------------------------------------------------------+
11. Traceability Matrix
Mục tiêu/Business Rule	FR liên quan	Use Case	Test Case
User phải đăng nhập và có hồ sơ cơ bản trước khi tạo request	FR-MAT-01	UC-MAT-01	TC-MAT-01, TC-MAT-02
Chỉ ghép user có cùng topic/thời gian phù hợp	FR-MAT-02, FR-MAT-03, FR-MAT-04	UC-MAT-02, UC-MAT-03	TC-MAT-04, TC-MAT-05, TC-MAT-06
User xem được kết quả ghép cặp của chính mình	FR-MAT-05	UC-MAT-03	TC-MAT-07, TC-MAT-08
Lịch chỉ confirmed khi cả hai user xác nhận	FR-MAT-06	UC-MAT-04	TC-MAT-09, TC-MAT-10
Cho phép hủy/đổi lịch trước khi session bắt đầu	FR-MAT-07, FR-MAT-08	UC-MAT-05	TC-MAT-12, TC-MAT-13, TC-MAT-14
User xem danh sách lịch phỏng vấn của mình	FR-MAT-09	UC-MAT-04, UC-MAT-05	TC-MAT-15
12. Checklist bàn giao M2
STT	Đầu việc	Trạng thái	Sản phẩm đầu ra
1	Bộ câu hỏi khảo sát M2	Hoàn thành	Mục 2
2	Báo cáo khảo sát M2 mẫu	Hoàn thành	Mục 2.1
3	Research & Analysis Matching & Scheduling	Hoàn thành	Mục 3
4	Functional Requirements FR-MAT	Hoàn thành	Mục 4
5	Use Case M2	Hoàn thành	Mục 5
6	Class Diagram M2	Hoàn thành	Mục 6.1
7	ERD M2	Hoàn thành	Mục 6.2
8	Activity Diagram M2	Hoàn thành	Mục 6.3
9	Sequence Diagram Matching	Hoàn thành	Mục 6.4
10	Test Case M2	Hoàn thành	Mục 8
11	SQA Test Technique	Hoàn thành	Mục 9
12	UI Mockup M2	Hoàn thành	Mục 10
13	Full tài liệu M2	Hoàn thành	Toàn bộ tài liệu
13. Kết luận
Tài liệu M2 đã mô tả đầy đủ phần Matching & Scheduling từ khảo sát nhu cầu, phân tích nghiệp vụ, yêu cầu chức năng, use case, thiết kế dữ liệu, sơ đồ, test case, SQA plan đến UI mockup. Nội dung có thể dùng để ghép trực tiếp vào báo cáo tổng của dự án và làm cơ sở triển khai module Matching & Scheduling trong hệ thống.

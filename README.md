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

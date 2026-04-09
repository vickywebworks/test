classDiagram
  direction LR

  class User {
    +ObjectId id
    +String name
    +String email
    +String password
    +UserRole role
    +String rollNo
    +String year
    +String branch
    +String section
    +String phone
    +String[] skills
    +String resumeUrl
    +DateTime createdAt
    +DateTime updatedAt
  }

  class Exam {
    +ObjectId id
    +String title
    +ExamType examType
    +Int duration
    +String instructions
    +String year
    +String branch
    +String section
    +String[] targetSections
    +ExamStatus status
    +DateTime startTime
    +DateTime endTime
    +DateTime createdAt
    +DateTime updatedAt
  }

  class Question {
    +ObjectId id
    +ExamType questionType
    +String questionText
    +String[] options
    +String correctAnswer
    +String sampleInput
    +String sampleOutput
    +Int marks
    +Difficulty difficulty
    +QuestionSource source
    +DateTime createdAt
    +DateTime updatedAt
  }

  class CodingQuestion {
    +ObjectId id
    +String title
    +String description
    +Difficulty difficulty
    +String functionName
    +String returnType
    +Int marks
  }

  class ExamAttempt {
    +ObjectId id
    +DateTime startTime
    +DateTime endTime
    +AttemptStatus status
    +Int score
    +DateTime createdAt
    +DateTime updatedAt
  }

  class ProfileItem {
    +String title
    +String issuer
    +String date
    +String link
  }

  class Parameter {
    +String name
    +String type
  }

  class TestCase {
    +String input
    +String expectedOutput
  }

  class HiddenTestCase {
    +String input
    +String expectedOutput
  }

  class AttemptAnswer {
    +ObjectId questionId
    +String answer
    +Int passedTestCases
    +Int totalTestCases
    +Int score
  }

  class CodingAnswer {
    +ObjectId questionId
    +String code
    +String language
    +Int passedCases
    +Int totalCases
    +Int marksAwarded
  }

  class Violation {
    +String reason
    +DateTime time
  }

  class Snapshot {
    +String imageData
    +DateTime capturedAt
    +String reason
  }

  class UserRole {
    <<enumeration>>
    STUDENT
    FACULTY
  }

  class ExamType {
    <<enumeration>>
    MCQ
    CODING
  }

  class ExamStatus {
    <<enumeration>>
    DRAFT
    LIVE
    COMPLETED
  }

  class AttemptStatus {
    <<enumeration>>
    STARTED
    SUBMITTED
    AUTO_SUBMITTED
  }

  class Difficulty {
    <<enumeration>>
    EASY
    MEDIUM
    HARD
  }

  class QuestionSource {
    <<enumeration>>
    MANUAL
    AI
  }

  User "1" --> "0..*" Exam : creates
  User "1" --> "0..*" Question : creates
  User "1" --> "0..*" CodingQuestion : creates
  User "1" --> "0..*" ExamAttempt : attempts

  Exam "1" --> "0..*" Question : contains
  Exam "0..*" --> "0..*" CodingQuestion : includes
  Exam "1" --> "0..*" ExamAttempt : has

  User *-- ProfileItem : achievements
  User *-- ProfileItem : certificates

  Question *-- HiddenTestCase
  CodingQuestion *-- Parameter
  CodingQuestion *-- TestCase

  ExamAttempt *-- AttemptAnswer
  ExamAttempt *-- CodingAnswer
  ExamAttempt *-- Violation
  ExamAttempt *-- Snapshot

  User --> UserRole
  Exam --> ExamType
  Exam --> ExamStatus
  Question --> ExamType
  Question --> Difficulty
  Question --> QuestionSource
  CodingQuestion --> Difficulty
  ExamAttempt --> AttemptStatus

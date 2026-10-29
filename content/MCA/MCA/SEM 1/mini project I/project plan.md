# Smart Academics Planner

> [!info] Project Overview  
> **Batch:** FY MCA — Semester I  
> **Domain:** Academic Management  
> **Frontend:** Next.js + TypeScript  
> **Backend:** Java + Spring Boot  
> **Database:** PostgreSQL  
> **Actors:** Student, Professor, Admin

## 1. Project Objective

Build a web application that helps students and professors manage essential academic activities through three core modules:

1. Lecture scheduling and timetable viewing.
    
2. Assignment posting, submission, and tracking.
    
3. Attendance recording and tracking.
    

Additional lightweight features include a personal academic task tracker, deadline-based task prioritization, and a What-if Attendance Recovery Planner.

### Scope Boundaries

**Included**

- Professor-managed lecture scheduling and conflict detection.
    
- Student timetable viewing.
    
- Professor-posted assignments and student file submissions.
    
- Submission status, deadlines, marks, and feedback.
    
- Professor-verified attendance and attendance corrections.
    
- Subject-wise attendance percentages and recovery estimates.
    
- Personal academic tasks and explainable priority indicators.
    

**Excluded**

- Study-session scheduling and automatic weekly study plans.
    
- AI-generated study recommendations and performance prediction.
    
- Automatic cascade rescheduling.
    
- Complex notification infrastructure.
    
- Exam management as a separate module.
    
- Automated timetable generation.
    

---

## 2. Core Modules

### 2.1 Lecture Scheduling

**Professor**

- Create and update lectures.
    
- Specify subject, classroom, start time, and end time.
    
- Receive conflict warnings before saving.
    

**Student**

- View the weekly timetable.
    
- See updated lecture schedules.
    

**System rules**

- Prevent overlapping lectures for the same professor.
    
- Prevent overlapping lectures in the same classroom.
    
- Exclude cancelled lectures from attendance calculations.
    

### 2.2 Assignment Management

**Professor**

- Create assignments with title, description, deadline, and maximum marks.
    
- View student submissions.
    
- Evaluate submissions and provide feedback.
    

**Student**

- View assignments by subject and deadline.
    
- Upload submissions.
    
- Track pending, submitted, overdue, and evaluated work.
    

**System rules**

- Record submission timestamps on the server.
    
- Determine lateness from the submission timestamp and deadline.
    
- Validate uploaded files.
    
- Allow at most one current submission per student per assignment.
    

### 2.3 Attendance Tracking

**Professor**

- Record attendance for a lecture.
    
- Correct previously recorded attendance with a reason.
    

**Student**

- View subject-wise attendance percentages.
    
- See attendance warnings and recovery estimates.
    

**System rules**

- Professor-confirmed attendance is the official record.
    
- Student-entered estimates, if supported, remain unverified.
    
- Prevent duplicate attendance records for the same student and lecture.
    
- Cancelled lectures do not count as conducted lectures.
    
- Unrecorded attendance must not automatically be treated as absence.
    

### 2.4 Academic Task Tracker

Students can create personal academic tasks, such as revision, project work, or exam preparation.

Each task contains:

- Title
    
- Due date
    
- Estimated effort in hours
    
- Importance
    
- Status
    

Assignments should appear in the upcoming-work view directly from assignment records rather than being duplicated as personal tasks.

The system can rank tasks using deadline urgency, importance, and estimated effort. Each recommendation should include a brief explanation.

**No hourly study schedule or automatic study-session generation is required.**

### 2.5 What-if Attendance Recovery Planner

Students can simulate missing a future lecture and preview:

- Projected attendance percentage.
    
- Number of future lectures required to reach the attendance target.
    
- Whether enough lectures remain.
    
- The implications of the simulated absence.
    

This is a read-only simulation. It must not change official attendance records.

---

## 3. Entity Model

The initial database uses eight entities.

|Entity|Important attributes|Purpose|
|---|---|---|
|`User`|`userId`, `name`, `email`, `passwordHash`, `role`|Authentication and role management|
|`Subject`|`subjectId`, `name`, `code`|Academic subjects|
|`Enrollment`|`enrollmentId`, `studentId`, `subjectId`|Student-subject relationships|
|`Lecture`|`lectureId`, `subjectId`, `professorId`, `classroom`, `startTime`, `endTime`, `status`|Lecture scheduling|
|`Attendance`|`attendanceId`, `lectureId`, `studentId`, `status`, `recordedBy`, `updatedAt`, `correctionReason`|Official attendance records|
|`Assignment`|`assignmentId`, `subjectId`, `professorId`, `title`, `description`, `deadline`, `maxMarks`|Assignment publication|
|`Submission`|`submissionId`, `assignmentId`, `studentId`, `fileUrl`, `submittedAt`, `marks`, `feedback`|Student submissions and evaluation|
|`AcademicTask`|`taskId`, `studentId`, `title`, `dueDate`, `estimatedHours`, `importance`, `status`|Personal academic tasks|

### Entity Design Notes

- Use `User.role` to distinguish `STUDENT`, `PROFESSOR`, and `ADMIN`.
    
- Represent exams as personal tasks if needed; do not build a separate exam-management module.
    
- Derive assignment status and lateness from the assignment deadline and submission records wherever practical.
    
- Store official attendance separately from any optional student estimates.
    
- For a simple implementation, attendance corrections can retain a reason and update timestamp. A separate audit-history entity is optional.
    
- No `StudySession` or `StudyPlan` entity is required.
    

---

## 4. Entity Relationships

|Relationship|Multiplicity|
|---|---|
|Student → Enrollment|One-to-many|
|Subject → Enrollment|One-to-many|
|Subject → Lecture|One-to-many|
|Professor → Lecture|One-to-many|
|Lecture → Attendance|One-to-many|
|Student → Attendance|One-to-many|
|Subject → Assignment|One-to-many|
|Professor → Assignment|One-to-many|
|Assignment → Submission|One-to-many|
|Student → Submission|One-to-many|
|Student → AcademicTask|One-to-many|

### UML Class Diagram

```
classDiagram
    class User {
        +Long userId
        +String name
        +String email
        +String passwordHash
        +Role role
    }

    class Subject {
        +Long subjectId
        +String name
        +String code
    }

    class Enrollment {
        +Long enrollmentId
        +Long studentId
        +Long subjectId
    }

    class Lecture {
        +Long lectureId
        +DateTime startTime
        +DateTime endTime
        +String classroom
        +LectureStatus status
    }

    class Attendance {
        +Long attendanceId
        +AttendanceStatus status
        +Long recordedBy
        +String correctionReason
        +DateTime updatedAt
    }

    class Assignment {
        +Long assignmentId
        +String title
        +DateTime deadline
        +Double maxMarks
    }

    class Submission {
        +Long submissionId
        +String fileUrl
        +DateTime submittedAt
        +Double marks
        +String feedback
    }

    class AcademicTask {
        +Long taskId
        +String title
        +Date dueDate
        +Double estimatedHours
        +int importance
        +TaskStatus status
    }

    User "1" --> "0..*" Enrollment : student
    Subject "1" --> "0..*" Enrollment : has
    Subject "1" --> "0..*" Lecture : contains
    User "1" --> "0..*" Lecture : professor
    Lecture "1" --> "0..*" Attendance : records
    User "1" --> "0..*" Attendance : student
    Subject "1" --> "0..*" Assignment : contains
    User "1" --> "0..*" Assignment : creates
    Assignment "1" --> "0..*" Submission : receives
    User "1" --> "0..*" Submission : submits
    User "1" --> "0..*" AcademicTask : owns
```

**Database constraints**

- Unique enrollment per student, subject, and semester.
    
- Unique attendance record per lecture and student.
    
- Unique current submission per assignment and student.
    
- Valid lecture start and end times.
    
- Marks must be between zero and maximum marks.
    

---

## 5. UML Use-Case Diagram

The application is the system boundary, not an external actor.

```
flowchart LR
    Student["Student"]
    Professor["Professor"]
    Admin["Admin"]

    subgraph SAP["Smart Academics Planner"]
        U1(["View Timetable"])
        U2(["View Assignments and Deadlines"])
        U3(["Submit and Track Assignments"])
        U4(["View Attendance and Recovery Estimate"])
        U5(["Manage Personal Academic Tasks"])
        U6(["View Task Priorities"])
        U7(["Schedule and Update Lectures"])
        U8(["Post Assignments"])
        U9(["Record and Correct Attendance"])
        U10(["Review Submissions"])
        U11(["Manage Users and Subjects"])
    end

    Student --> U1
    Student --> U2
    Student --> U3
    Student --> U4
    Student --> U5
    Student --> U6

    Professor --> U1
    Professor --> U7
    Professor --> U8
    Professor --> U9
    Professor --> U10

    Admin --> U11
```

### Role Permissions

|Action|Student|Professor|Admin|
|---|---|---|---|
|View own timetable|Yes|Yes|Optional|
|Schedule lectures|No|Yes|Optional|
|Post assignments|No|Yes|Optional|
|Submit assignments|Yes|No|No|
|Review submissions|No|Yes|Optional|
|Record attendance|No|Yes|Optional|
|View own attendance|Yes|No|Optional|
|Manage users and subjects|No|No|Yes|

All permissions must be enforced by the backend, not merely by hiding frontend controls.

---

## 6. UML Activity Diagram — Lecture Scheduling

```
flowchart TD
    A([Start]) --> B["Professor enters lecture details"]
    B --> C{"Valid input?"}

    C -- No --> D["Display validation errors"]
    D --> B

    C -- Yes --> E["Check professor availability"]
    E --> F["Check classroom availability"]
    F --> G{"Conflict detected?"}

    G -- Yes --> H["Display conflict"]
    H --> B

    G -- No --> I["Save lecture"]
    I --> J["Update student timetable"]
    J --> K([End])
```

---

## 7. UML Activity Diagram — Assignment Submission

```
flowchart TD
    A([Start]) --> B["Professor posts assignment"]
    B --> C["Student views assignment and deadline"]
    C --> D["Student uploads submission"]
    D --> E{"File valid?"}

    E -- No --> F["Display validation error"]
    F --> D

    E -- Yes --> G["Record submission timestamp"]
    G --> H{"Submitted after deadline?"}

    H -- Yes --> I["Flag submission as late"]
    H -- No --> J["Mark submission as on time"]

    I --> K["Professor reviews submission"]
    J --> K

    K --> L["Record marks and feedback"]
    L --> M["Student views evaluation"]
    M --> N([End])
```

---

## 8. UML Activity Diagram — Attendance Tracking

```
flowchart TD
    A([Start]) --> B["Professor selects lecture"]
    B --> C["Load enrolled students"]
    C --> D["Mark students present or absent"]
    D --> E{"Records valid and unique?"}

    E -- No --> F["Display validation errors"]
    F --> D

    E -- Yes --> G["Save official attendance"]
    G --> H["Student views attendance"]
    H --> I["Calculate subject-wise percentage"]
    I --> J{"Below target?"}

    J -- Yes --> K["Show warning and recovery estimate"]
    J -- No --> L["Display current percentage"]

    K --> M([End])
    L --> M
```

---

## 9. UML Activity Diagram — What-if Attendance Recovery

```
flowchart TD
    A([Start]) --> B["Load official attendance and remaining lectures"]
    B --> C["Student selects a hypothetical missed lecture"]
    C --> D["Calculate projected attendance"]
    D --> E["Calculate future lectures needed"]

    E --> F{"Can target be reached?"}

    F -- Yes --> G["Show required future attendances"]
    F -- No --> H["Flag target as unreachable"]

    G --> I["Explain projected outcome"]
    H --> I

    I --> J["Keep official records unchanged"]
    J --> K([End])
```

### Attendance Formula

Let:

- `A` = lectures attended.
    
- `T` = lectures conducted.
    
- `p` = required attendance fraction.
    

Current attendance:

[  
\text{Attendance Percentage} = \frac{A}{T}\times100  
]

Minimum future lectures that must be attended consecutively to reach the target:

[  
x=\max\left(0,\left\lceil\frac{pT-A}{1-p}\right\rceil\right)  
]

If fewer than `x` lectures remain, the target cannot be reached under the assumed schedule.

The What-if planner must simulate the missed lecture without modifying the official attendance records.

---

## 10. Task Priority Rules

Each personal task can be ranked using three factors:

- **Deadline urgency:** How soon the task is due.
    
- **Importance:** How academically significant the task is.
    
- **Effort:** How much estimated work remains.
    

A simple initial priority score is:

[  
P=0.4D+0.35I+0.25E  
]

Where `D`, `I`, and `E` are normalized values between zero and one.

The score is an initial heuristic, not a validated academic prediction model.

Example explanation:

> **High priority:** This assignment is due tomorrow, has high importance, and requires several hours of work.

Attendance risk remains a separate warning and is not automatically mixed into the task score. Students can see both deadline priorities and attendance concerns.

---

## 11. Development Plan

|Phase|Deliverables|
|---|---|
|1|Database schema, users, subjects, enrollments, authentication|
|2|Lecture scheduling, conflict detection, timetable viewing|
|3|Assignment posting, file submissions, deadline tracking, grading|
|4|Attendance recording, corrections, cancellation handling, percentage calculation|
|5|Personal task tracker, priority explanations, attendance recovery simulation|
|6|Testing, UML documentation, deployment, final demonstration|

### Testing Scenarios

1. Two lectures overlap in the same classroom.
    
2. Two lectures overlap for the same professor.
    
3. Two assignments have competing deadlines.
    
4. A student submits an assignment after its deadline.
    
5. A professor corrects an attendance record.
    
6. A lecture is cancelled.
    
7. A student has low attendance and insufficient remaining lectures to reach the target.
    
8. A student simulates missing a lecture without changing official records.
    

---

## 12. Final Scope Statement

The Smart Academics Planner is a role-based academic management application that supports professor-managed lecture scheduling, student timetable viewing, assignment posting and submission tracking, verified attendance management, and a lightweight personal academic task tracker with explainable priorities.

Its distinguishing feature is a simple What-if Attendance Recovery Planner that estimates the impact of missed lectures while preserving official records.

The project does not include study-session scheduling, automatic weekly study plans, cascade rescheduling, AI-based academic prediction, or complex notification infrastructure.

**The objective is to deliver a small, reliable full-stack application with meaningful validation rules, relational database design, role-based access, and testable academic logic.**
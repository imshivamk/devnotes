# Smart Academic Deadline & Study Planner - Project Proposal

---

## A. Group Details

**Batch:** FY MCA (1st Semester)

**Student 1 –** Shivam Kedar

**Student 2 –** [Name & Roll No.]

**Student 3 –** [Name & Roll No.]

---

## B. Project Identification

**Proposed Project Title:** Smart Academic Deadline & Study Planner

**Domain / Application Area:** Academic Management, Time Management, Educational Technology

**Proposed Technology:** Java Backend (Spring Boot) with Next.js Frontend

---

## C. Problem Statement

Students struggle with managing multiple subjects, assignments, and exams simultaneously. Key challenges include: (1) deadline conflicts — multiple deadlines on the same day; (2) poor task visibility — no centralized view of all upcoming work; (3) missed deadlines — lack of automated reminders; (4) inefficient rescheduling — manual adjustment when deadlines change; and (5) no intelligent prioritization. These issues lead to poor academic performance and increased stress. An intelligent system can help students plan proactively, detect conflicts, and complete work on time.

---

## D. Proposed Solution

The Smart Academic Deadline & Study Planner is an intelligent scheduling system that centralizes academic work and automatically manages time allocation. Students register subjects, assignments, and exams; the system **detects deadline conflicts** (multiple deadlines same day, exams within 24 hours) and alerts immediately. A **weighted prioritization algorithm** ranks tasks by deadline proximity, weight, and effort. **Dynamic rescheduling** adjusts dependent tasks when deadlines change. **Adaptive study plan generation** allocates daily study hours intelligently. **Smart notifications** alert students at key intervals with recommended study hours. The Java backend handles scheduling logic, conflict resolution, and cascade analysis; the Next.js frontend provides an intuitive dashboard for task management and visualization.

---

## E. Major Features

1. **Subject & Course Management** — Add/edit/delete subjects; track academic load; validate duplicate entries
2. **Assignment & Deadline Tracking** — Create assignments with deadlines and weightage; track status (Pending, Submitted, Overdue); auto-flag overdue tasks
3. **Exam Scheduling & Conflict Detection** — Register exams; automatically detect conflicts (exams within 24 hours); prevent overlapping schedules
4. **Intelligent Task Prioritization** — Weighted algorithm: **Priority = (Days Until Deadline × 0.4) + (Task Weight × 0.3) + (Effort × 0.3)**; dynamic recalculation
5. **Adaptive Study Plan Generation** — Auto-generate weekly schedules; allocate hours intelligently; adjust on task completion
6. **Deadline Conflict Resolution & Rescheduling** — Detect cascade effects; suggest rescheduling; support deadline extensions with impact analysis
7. **Notification & Alert System** — Alerts at 7 days, 3 days, 1 day before deadline; recommend daily study hours

---

## F. Distinctive Factors

1. **Intelligent Conflict Detection** — Proactively detects and suggests resolution for deadline collisions and exam conflicts
2. **Advanced Weighted Prioritization** — Multi-variable algorithm dynamically adapts when deadlines change
3. **Cascade Impact Analysis** — Automatically recalculates dependent task schedules when one task is delayed
4. **Intelligent Study Plan Generation** — Personalized daily schedules with optimized effort allocation
5. **Exam-Specific Logic** — Treats exams differently from assignments with separate prioritization rules
6. **Full-Stack Architecture** — Java backend handles complex business logic; Next.js frontend provides responsive dashboard

**Non-CRUD Elements:** Scheduling algorithms, conflict detection, weighted prioritization, cascade rescheduling, state management, validation rules, exception handling.

---

## G. Intended Users / Actors

| Actor | Role |
|-------|------|
| **Student** | Create subjects/assignments/exams; view prioritized tasks; mark completion; request extensions |
| **System** | Detect conflicts, calculate priorities, generate schedules, send alerts, recalculate on changes |
| **Professor (Optional)** | Approve extensions, update deadlines |

---

## H. Expected Outcome

1. **Central Academic Dashboard** — Display all subjects, assignments, exams with status indicators
2. **Conflict Detection & Alerts** — Identify deadline/exam conflicts; immediate notifications
3. **Task Prioritization** — Tasks ranked by calculated priority score; "Today's Focus" and "This Week's Priority" views
4. **Study Schedule Generation** — Weekly plan with allocated study hours per task
5. **Dynamic Rescheduling** — Impact analysis showing how delayed tasks affect other work
6. **Data Persistence** — All data persists across sessions (database or file-based)
7. **Exception Handling** — Prevent invalid inputs (past deadlines, duplicate subjects, overlapping exams); graceful error handling
8. **Responsive Web Interface** — Browser-based dashboard with forms for task management and visual schedule/conflict display

---

**Submission Format:** Single PDF via Google Classroom  
**Submission Deadline:** Monday, 5 October 2026
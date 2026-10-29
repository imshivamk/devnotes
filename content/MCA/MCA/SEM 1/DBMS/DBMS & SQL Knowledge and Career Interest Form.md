

**Description**

This survey aims to understand our class's current familiarity with Database Management Systems (DBMS), practical SQL skills, and interest in database-related career paths. The responses will help identify common learning gaps and topics that students would like to explore further.

Please answer honestly. This is a knowledge assessment for understanding the class's overall skill level, not a formal examination.

**Estimated time:** 5–8 minutes

---

## Section 1: Basic Information

**1. Roll Number (Optional)**  
_Type: Short answer_

**2. How familiar are you with DBMS concepts?**  
_Type: Multiple choice_

- 1 — No prior knowledge
    
- 2 .  Familiar with basic terminology
    
- 3 .  Understand common DBMS concepts
    
- 4 .  Good understanding of most concepts
    
- 5 .  Strong understanding; can explain concepts with examples
    

**3. How would you rate your practical SQL skills?**  
_Type: Multiple choice_

- 1 .  Never written SQL queries
    
- 2 .  Can write basic SELECT queries
    
- 3 .  Can use filtering, sorting and aggregate functions
    
- 4 .  Can write joins, subqueries and complex queries
    
- 5 .  Can independently write, debug and optimize queries
    

---

## Section 2: Learning Experience

**4. Which DBMS concepts or SQL topics have you studied or practised?**  
_Type: Checkboxes .  Select all that apply_

- DBMS fundamentals and database architecture
    
- ER diagrams and relational models
    
- Primary keys, foreign keys and constraints
    
- Normalization and functional dependencies
    
- SQL DDL: CREATE, ALTER, DROP
    
- SQL DML: INSERT, UPDATE, DELETE
    
- SELECT, WHERE, ORDER BY and DISTINCT
    
- Aggregate functions and GROUP BY
    
- Joins and subqueries
    
- Transactions and ACID properties
    
- Indexes and query optimization
    
- Views, stored procedures and triggers
    
- None of the above
    

**5. How frequently do you practise writing SQL queries?**  
_Type: Multiple choice_

- Never
    
- Only during lectures or practicals
    
- A few times per month
    
- A few times per week
    
- Almost every day
    

---

## Section 3: DBMS Concept Assessment

_Each question is worth 1 point._

**6. What is the primary purpose of a primary key?**  
_Type: Multiple choice_

- A. To uniquely identify each row in a table
    
- B. To sort rows automatically
    
- C. To connect every table in the database
    
- D. To encrypt sensitive data
    

**7. What is the purpose of a foreign key?**  
_Type: Multiple choice_

- A. To uniquely identify every row in its own table
    
- B. To reference a key in another or the same table and establish a relationship
    
- C. To encrypt database records
    
- D. To automatically sort records
    

**8. What is the main goal of database normalization?**  
_Type: Multiple choice_

- A. To increase data duplication
    
- B. To eliminate the need for keys
    
- C. To reduce undesirable redundancy and data anomalies
    
- D. To make every query execute instantly
    

**9. Which of the following is an ACID property of database transactions?**  
_Type: Multiple choice_

- A. Availability
    
- B. Consistency
    
- C. Indexing
    
- D. Distribution
    

**10. What is a common purpose of a database index?**  
_Type: Multiple choice_

- A. To speed up certain data retrieval operations
    
- B. To remove all duplicate rows automatically
    
- C. To guarantee that every query uses less memory
    
- D. To replace primary keys
    

---

## Section 4: Practical SQL Assessment

_Each question is worth 1 point._

Use the following tables for Questions 11–15:

`Students(student_id, name, department_id, marks)`

`Departments(department_id, department_name)`

**11. Which query retrieves students who scored more than 80 marks?**  
_Type: Multiple choice_

- A. `SELECT * FROM Students WHERE marks > 80;`
    
- B. `SELECT Students WHERE marks > 80;`
    
- C. `GET * FROM Students IF marks > 80;`
    
- D. `SELECT * FROM Students HAVING marks > 80;`
    

**12. Which SQL clause filters groups after aggregation?**  
_Type: Multiple choice_

- A. WHERE
    
- B. ORDER BY
    
- C. HAVING
    
- D. DISTINCT
    

**13. What does an INNER JOIN return?**  
_Type: Multiple choice_

- A. All rows from the left table, including unmatched rows
    
- B. Rows that match the join condition in both tables
    
- C. Every possible combination of rows
    
- D. Only rows containing NULL values
    

**14. Which query returns the average marks for each department ID?**  
_Type: Multiple choice_

- A. `SELECT department_id, AVG(marks) FROM Students GROUP BY department_id;`
    
- B. `SELECT department_id, AVG(marks) FROM Students;`
    
- C. `SELECT AVG(department_id) FROM Students GROUP BY marks;`
    
- D. `SELECT department_id FROM Students WHERE AVG(marks);`
    

**15. Which query retrieves students whose marks are above the overall average?**  
_Type: Multiple choice_

- A. `SELECT * FROM Students WHERE marks > AVG(marks);`
    
- B. `SELECT * FROM Students WHERE marks > (SELECT AVG(marks) FROM Students);`
    
- C. `SELECT * FROM Students GROUP BY marks > 80;`
    
- D. `SELECT AVG(marks) FROM Students WHERE *;`
    

**16. Write an SQL query to display the names of students who scored more than 75 marks, ordered from highest to lowest marks.**  
_Type: Paragraph — Optional_

---

## Section 5: Career Interests

**17. Which career areas are you interested in exploring?**  
_Type: Checkboxes — Select all that apply_

- Database Administrator (DBA)
    
- Database Developer / SQL Developer
    
- Data Engineer
    
- Data Analyst / Business Intelligence
    
- Data Scientist / Machine Learning Engineer
    
- Cloud / Database Infrastructure Engineer
    
- Backend Software Developer
    
- General software development
    
- Cybersecurity
    
- I haven't decided yet
    
- Other
    

**18. How interested are you in pursuing a database-related career?**  
_Type: Linear scale, 1–5_

- 1 — Not interested
    
- 2 — Slightly interested
    
- 3 — Undecided
    
- 4 — Interested
    
- 5 — Very interested
    

**19. Would you be interested in additional DBMS/SQL learning sessions or practical projects?**  
_Type: Multiple choice_

- Yes, definitely
    
- Yes, depending on the topics
    
- Maybe
    
- No, not currently
    

**20. Which database-related topics would you most like to learn?**  
_Type: Checkboxes — Select all that apply_

- SQL fundamentals and query writing
    
- Advanced SQL, joins and subqueries
    
- Database design and normalization
    
- Transactions, concurrency and recovery
    
- Indexing and query optimization
    
- Practical PostgreSQL / MySQL projects
    
- Database administration, backups and security
    
- Data engineering, ETL/ELT and data pipelines
    
- Cloud databases
    
- I am not interested in additional learning at the moment
    

---

**Thank you for your participation!**

Your responses will help us understand the class's current DBMS and SQL proficiency and identify areas where additional learning resources or practical sessions may be useful.


**데이터베이스 및 테이블 설정 (사전에 실행해야 합니다):**

SQL

```
-- 데이터베이스 생성
CREATE DATABASE IF NOT EXISTS school_db;

-- 데이터베이스 사용
USE school_db;

-- 학생 테이블 생성
CREATE TABLE IF NOT EXISTS Students (
    student_id INT PRIMARY KEY AUTO_INCREMENT,
    student_name VARCHAR(100) NOT NULL,
    age INT,
    gender VARCHAR(10),
    major VARCHAR(100)
);

-- 과목 테이블 생성
CREATE TABLE IF NOT EXISTS Courses (
    course_id INT PRIMARY KEY AUTO_INCREMENT,
    course_name VARCHAR(100) NOT NULL,
    credits INT
);

-- 성적 테이블 생성 (학생과 과목 연결)
CREATE TABLE IF NOT EXISTS Enrollments (
    enrollment_id INT PRIMARY KEY AUTO_INCREMENT,
    student_id INT,
    course_id INT,
    score INT,
    enrollment_date DATE,
    FOREIGN KEY (student_id) REFERENCES Students(student_id),
    FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);

-- 데이터 삽입
INSERT INTO Students (student_name, age, gender, major) VALUES
('김철수', 20, '남', '컴퓨터공학'),
('이영희', 21, '여', '경영학'),
('박지훈', 20, '남', '전자공학'),
('최유리', 22, '여', '컴퓨터공학'),
('정민수', 19, '남', '경영학'),
('오미선', 21, '여', '수학'),
('한동현', 20, '남', '수학');

INSERT INTO Courses (course_name, credits) VALUES
('데이터베이스', 3),
('운영체제', 3),
('회계원리', 2),
('미적분학', 3),
('선형대수', 3),
('자료구조', 3);

INSERT INTO Enrollments (student_id, course_id, score, enrollment_date) VALUES
(1, 1, 95, '2023-03-01'),
(1, 2, 88, '2023-03-01'),
(2, 3, 92, '2023-03-01'),
(3, 1, 78, '2023-03-01'),
(4, 1, 85, '2023-03-01'),
(4, 6, 90, '2023-03-01'),
(1, 6, 80, '2023-09-01'),
(2, 5, 75, '2023-09-01'),
(3, 2, 82, '2023-09-01'),
(5, 3, 88, '2023-03-01'),
(5, 5, 91, '2023-09-01'),
(6, 4, 93, '2023-03-01'),
(7, 4, 87, '2023-09-01'),
(7, 5, 79, '2023-03-01');
```

---

### **MySQL 쿼리 문제**

---

#### **I. 조회 문제 (SELECT)**

**난이도: 하**

1. **문제:** `Students` 테이블의 모든 학생 정보를 조회하세요.
    
    SQL
    
    ```
    select	* from Students;
    ```
    

난이도: 중하

2. 문제: Courses 테이블에서 학점(credits)이 3인 과목의 이름(course_name)만 조회하세요.

select course_name from Courses
where credits = 3;

난이도: 중

3. 문제: Enrollments 테이블에서 학생 ID가 1번인 학생이 수강한 과목의 성적(score)과 등록일(enrollment_date)을 조회하세요.

SELECT score, enrollment_date from Enrollments
where student_id=1;

난이도: 중상

4. 문제: '데이터베이스' 과목을 수강하고 80점 이상을 받은 학생들의 이름(student_name)을 조회하세요. (Students와 Enrollments, Courses 테이블을 조인)

SELECT DISTINCT student_name from Students
JOIN Enrollments
ON Enrollments.score >= 80;

난이도: 상

5. 문제: 각 학생별로 수강한 과목의 평균 점수를 계산하고, 학생의 이름(student_name)과 평균 점수를 함께 조회하세요. 평균 점수가 높은 순서대로 정렬하세요. (소수점 둘째 자리까지 표시)-==틀림==

SELECT student_name,AVG(E.score) FROM Students
JOIN Enrollments E
ON 
group by student_id DESC;

답
SELECT s.student_name, ROUND(AVG(e.score), 2) as avg_score
FROM Students s
JOIN Enrollments e ON s.student_id = e.student_id
GROUP BY s.student_id, s.student_name
ORDER BY avg_score DESC;

---

#### **II. 삽입 문제 (INSERT)**

**난이도: 하**

1. **문제:** `Students` 테이블에 새로운 학생 '김민준'(나이 23, 성별 '남', 전공 '컴퓨터공학') 정보를 삽입하세요.
    
    SQL
    
    ```
   INSERT INTO Students VALUE(NULL,'김민준',23,'남','컴퓨터공학');
    ```
    

난이도: 중

2. 문제: Courses 테이블에 새로운 과목 '인공지능'(학점 4)과 '데이터분석'(학점 3)을 한 번의 쿼리로 삽입하세요.

INSERT INTO Courses VALUES
(NULL, '인공지능', 4),
(NULL, '데이터분석', 3);

난이도: 상

3. 문제: Enrollments 테이블에 학생 ID 6번인 '오미선' 학생이 '데이터베이스' 과목을 85점으로 2024년 3월 5일에 수강한 정보를 삽입하세요. (과목 ID를 직접 입력하는 대신 Courses 테이블에서 '데이터베이스' 과목의 course_id를 찾아 사용하세요.)

INSERT INTO Enrollments (student_id, course_id, score, enrollment_date) VALUES (6,
(SELECT course_id FROM Courses 
WHERE course_name='데이터베이스'),
85, '2024-03-05');

---

#### **III. 변경 문제 (UPDATE)**

**난이도: 하**

1. **문제:** 학생 ID 2번인 '이영희' 학생의 전공(major)을 '경제학'으로 변경하세요.
    
    SQL
    
    ```
   UPDATE Students SET major='경제학' WHERE student_name='이영희';
    ```
    

난이도: 중

2. 문제: Enrollments 테이블에서 '김철수' 학생이 수강한 '데이터베이스' 과목의 성적을 90점으로 변경하세요. (학생 이름과 과목 이름으로 조건을 설정)

UPDATE Enrollments E
JOIN Students S ON e.student_id = E.student_id
JOIN Courses C ON E.course_id = C.course_id
SET E.score = 90
WHERE S.student_name='김철수' AND C.course_name='데이터베이스';

답: 
UPDATE Enrollments e JOIN Students s ON e.student_id = s.student_id JOIN Courses c ON e.course_id = c.course_id SET e.score = 90 WHERE s.student_name = '김철수' AND c.course_name = '데이터베이스';

난이도: 상

3. 문제: 모든 '컴퓨터공학' 전공 학생들의 나이를 1씩 증가시키세요.

UPDATE Students SET age = age + 1 WHERE major='컴퓨터공학';

---

#### **IV. 삭제 문제 (DELETE)**

**난이도: 하**

1. **문제:** `Students` 테이블에서 학생 ID 7번인 '한동현' 학생의 정보를 삭제하세요.
    
    SQL
    
    ```
    #외래키 제약조건 때문에 Enrollments먼저 삭제
    DELETE FROM Enrollments WHERE student_id=7;
	DELETE FROM Students WHERE student_id=7;
    ```
    

난이도: 중

2. 문제: Enrollments 테이블에서 성적(score)이 70점 미만인 모든 수강 기록을 삭제하세요.

DELETE FROM Enrollments WHERE score < 70;

난이도: 상

3. 문제: 더 이상 수강하는 학생이 없는 과목('Enrollments' 테이블에 해당 course_id가 없는 과목)을 Courses 테이블에서 삭제하세요.
==틀림==

DELETE FROM Enrollments WHERE IF

답: DELETE FROM Courses WHERE course_id NOT IN (SELECT DISTINCT course_id FROM Enrollments);
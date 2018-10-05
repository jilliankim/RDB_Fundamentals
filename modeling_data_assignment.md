1. **Create a database for the model we built in the example. Create a SQL file that creates the tables and inserts sample data. Submit queries to find the following information and include the results:**
   - >[DB Fiddle](https://www.db-fiddle.com/f/i38nJfeTDcuZJ6TvG8ufBK/1) - Working fiddle for the following queries
   - **A guest who exists in the database and has not booked a room.**
     - **QUERY:**
       ```sql
       SELECT g.first_name, b.room_id
         FROM bookings AS b
              RIGHT JOIN guests AS g
                      ON b.guest_id = g.id
        WHERE b.id IS NULL;
       ```
     - **RESULT:**
      
       first_name | room_id
       ---|:-:
       Paxton | null
       Marta | null

   - **Bookings for a guest who has booked two rooms for the same dates.**
     - **QUERY:**
       ```sql
       SELECT b.check_in, b.check_out, g.first_name
         FROM bookings AS b
              JOIN guests AS g
                ON g.id = b.guest_id
        GROUP BY g.first_name, b.check_in, b.check_out
       HAVING COUNT(b.room_id) > 1;
       ```
       **RESULT:**
       
       check_in | check_out | first_name
       ---|---|---
       2018-09-08T00:00:00.000Z | 2018-09-13T00:00:00.000Z | Arielle

   - **Bookings for a guest who has booked one room several times on different dates.**
     - **QUERY:**
     
       _Here, I decided to include a subquery named `room_number` instead of using `room_id`._
       ```sql
       SELECT (SELECT room_number
                 FROM rooms
                WHERE id = b.room_id) AS room_number, g.first_name, g.last_name, COUNT(b.check_in) AS stays
         FROM bookings AS b
              JOIN guests AS g
                ON g.id = b.guest_id
        GROUP BY g.first_name, g.last_name, b.room_id
       HAVING COUNT (b.check_in) > 1;
       ```
       
       **RESULT:**
       
       room_number | first_name | last_name | stays
       :-:|---|---|:-:
       292 | Gaetano | Cassin | 2

   - **The number of unique guests who have booked the same room.**
     - **QUERY:**
       ```sql
       SELECT COUNT(b.guest_id) AS guests, r.room_number
         FROM rooms AS r
              JOIN bookings AS b
                ON b.room_id = r.id
        GROUP BY r.room_number
       HAVING COUNT(b.guest_id) > 1;
       ```
       **RESULT:**
       
       guests | room_number
       :-:|:-:
       4 | 440
       2 | 124
       2 | 292
       
2. **Design a data model for students and the classes they have taken. The model should include the students' grades for a given class.**
   > NOTE: For this section, we are refering to a scholastic *Class*.  However, since that is a reserved word, we referr to them as *Courses* here.
   
   - **Document any assumptions you make about what data should be stored, what data types should be used, etc., and include them in a text file.**
     - **What classes/entites do we need to model?**
       - Students
       - Courses
       - Grades
     - **What fields/attributes will each entity need?**
       - **Students**
         - `first_name`
         - `last_name`
       - **Courses**
         - `course_name`
         - `course_code`
         - `instructor`
       - **Grades**
         - `student_id`
         - `course_id`
         - `grade`
     - **What data types do we need to use?**
       - We should be able to use `VARCHAR` for the vast majority of the data in this model, we will also make use of `INTEGER` for numerical values.  Since we are not storing any floating-point decimals, we will not need `FLOAT` data types.  If we wanted to store the `grade` as a decimal representation of a percentage, we would want to use `FLOAT` for the `grade` data type.
     - **What relationships exist between entities?**
       - A student will be in many classes, and each class will have many students.  This indicates a `many-to-many` relationship between `Students` and `Courses`.  However, this will prevent us from creating the foreign keys needed to link the two together.
       - A `student` will have many `grade`s, one for each `class`.  A class will assign one `grade` to each `student`.  This reveals a `1:*` relationship with `students` to `grades`, and a `1:*` relationsihp between `courses` and `grades`
       - The `Grades` table will connect *one* student with *one* class.
     - **How should those relationships be represented in tables?**
       - The `Students` table will have an `id` as its `Primary Key`
       - The `Courses` table will have an `id` as its `Primary Key`
       - The `Grades` table will have an `id` as it's `Primary Key`
         - In addition, this table will use `student_id` as a `Foreign Key` to the `id` column of the `Students` table, and `course_id` as a `Foreign Key` to the `id` column of the `Courses` table.
   - **Draw the model using the notation used in the checkpoint and submit a picture. You can also use a tool like StarUML or quickdatabasediagrams.com if you choose.**
     - UML Created in LucidChart
       ![LucidChart UML](https://www.lucidchart.com/publicSegments/view/7b5cdef3-46d0-404c-becf-4cd7484af9f1/image.png)

3. **Build a database for the students/classes data model. Create a SQL file that creates the tables and inserts sample data.**
   - >[DB Fiddle](https://www.db-fiddle.com/f/8A7eofB5NTMxyt2jtnPkNW/8) - Working fiddle of the following queries
   - **Submit queries for the following data and include the results:**
     - **All students who have taken a particular class.**
       - **QUERY:**
         ```sql
         SELECT s.first_name, s.last_name
           FROM students AS s
                JOIN grades AS g
                  ON g.student_id = s.id
          WHERE g.course_id = 4;
          ```
       - **RESULT:**
       
         first_name | last_name
         ---|---
         Aiden | Kerluke
         Rodrick | Blick
       
     - **The number of each grade (using letter grades A-F) earned in a particular class.**
       - **QUERY:**
         ```sql
         SELECT s.first_name, s.last_name, g.grade_letter
           FROM students AS s
                JOIN grades AS g
                  ON g.student_id = s.id
          WHERE g.course_id = 6;
         ```
       - **RESULT:**
       
         first_name | last_name | grade_letter
         ---|---|:-:
         Shayna | Rolfson | A
         Harold | Willms | B
       
     - **Class names and the total number of students who have taken each class in the list.**
       - **QUERY:**
         ```sql
         SELECT c.course_name, COUNT(g.student_id) AS students
           FROM courses c
                LEFT JOIN grades g
                  ON g.course_id = c.id
          GROUP BY c.course_name;
          ```
       - **RESULT:**
       
         course_name | students
         ---|:-:
         Planetary Zoology | 0
         Foreign Biosecurity | 1
         Alien Biosecurity | 1
         Planetary Business Management | 3
         Scavenging | 2
         Planetary Biosecurity | 1
         Leadership | 0
         Advanced Mathematics | 1
         Alien Finance | 0
         Dark Arts | 2
         
     - **The class taken by the largest number of students.**
       - **QUERY:**
         ```sql
         SELECT c.course_name AS course, COUNT(student_id) AS students
           FROM grades AS g
                JOIN courses AS c
                  ON c.id = g.course_id
          GROUP BY c.course_name
         HAVING COUNT(student_id) = 
                (SELECT MAX(students)
                   FROM 
                        (SELECT course_id, COUNT(student_id) AS students
                           FROM grades
                          GROUP BY course_id)
                        AS student_count);
         ```
       - **RESULT:**
       
         course | students
         ---|:-:
         Planetary Business Management | 3

1. **In your own words, explain the benefits of normalization. Include a real-world scenario where normalization is necessary.**
   - Through normalization we can increase the data integrity of our databases by removing duplication, and preventing redundant, neglected data from causing data inconsistancies.  A use for normalization would be in a scenario where quick "seek and update" operations are prioritized.  Such an instance could be an educational institution where once the data is created, we only have to update key values as the curriculum goes on, or pull up records that meet a given criteria.

2. **List and explain the different normal forms and how they relate to one another, with regard to your real-world scenario in the first question.**
   - `1NF` (First Normal Form)
     - First Normal Form confines our tables to have rows of data where no given column contains more than one value.  Using the scenario above, if a student has retaken a course and the entry was entered in the database as `54, 98` to represent each attempt at the course, this would not meet `1NF`.  This is due to the `grade` column containing multiple values.
     
   - `2NF` (Second Normal Form)
     - This form expands on `1NF` and requires that no attribute rely on a subset of the primary key.  Given our scenario, the following table would **NOT** be 2NF compliant as `grade` is dependent on `first_name` and `last_name`, but not on `age`.  Logically, we would need to know the `first_name` and `last_name` of the student in order to find the grade, but we don't care what their `age` is.
     
       **NOT 2NF**
       
       first_name | last_name | age | grade
       ---|---|:-:|:-:
       Colonel | Plum | 54 | A
       Billy | Bob | 23 | C
       Betty | Boolean | 31 | A
       
       **2NF COMPLIANT**
       
       first_name | last_name | age
       ---|---|:-:
       Colonel | Plum | 54
       Billy | Bob | 23
       Betty | Boolean | 31
       
       first_name | last_name | grade
       ---|---|:-:
       Colonel | Plum | A
       Billy | Bob | C
       Betty | Boolean | A

   - `3NF` (Third Normal Form)
     - For this form, we decouple our table even further requiring that no `functional dependencies` of `non-prime attributes` exist. This means that every attribute must relate to the `prime attribute`, or `Primary Key`.  The following tables show an example of a **non `3NF`**, and how to make it `3NF` complaint.
     
       **NOT 3NF**
       
       student_id | first_name | grade | course | instructor
       ---|---|:-:|---|---
       0013829 | Alicia | F | Molecular Biology | Burns
       0032847 | Alex | C | Geometry | Matthew
       0010300 | Tiffany | A | Marine Biology | Earnheart
       
       > This table does not comply with `3NF` since `instructor` depends on `course`, thus **`transitively`** it depends on `student_id`.  Since this relationship is transitive, it does not follow `3NF`.
       
       **3NF COMPLIANT**
       
       student_id | first_name | grade | course_id
       ---|---|:-:|:-:
       0013829 | Alicia | F | BIO-225
       0032847 | Alex | C | MAT-210
       0010300 | Tiffany | A | BIO-275
       
       course_id | course | instructor
       :-:|---|---
       BIO-225 | Molecular Biology | Burns
       MAT-210 | Geometry | Matthew
       BIO-275 | Marine Biology | Earnheart

3. **This [student_records](https://www.db-fiddle.com/f/kwVrsocvpqgfS1gNkAP51T/0) table contains students and their grades in different subjects. The schema is already in first normal form (1NF). Convert this schema to the third normal form (3NF) using the techniques you learned in this checkpoint.**

     **ORIGINAL**

     id | student_id | student_email | student_name | professor_id | professor_name | subject | grade
     :-:|:-:|---|---|:-:|---|---|:-:
     1 | 1 | john.b20@hogwarts.edu | John B | 2 | William C | Philosophy | A
     2 | 2 | sarah.s20@hogwarts.edu | Sarah S | 2 | William C | Philosophy | C
     3 | 3 | martha.l20@hogwarts.edu | Martha L | 1 | Natalie M | Economics |  A
     4 | 4 | james.g20@hogwarts.edu | James G | 3 | Mark W | Mathematics | B
     5 | 5 | stanley.p20@hogwarts.edu | Stanley P | 1 | Natalie M | Economics | B
     
     **3NF COMPLIANT**
     
     id | student_id |  professor_id | grade
     :-:|:-:|:-:|:-:
     1 | 1 | 2 | A
     2 | 2 | 2 | C
     3 | 3 | 1 | A
     4 | 4 | 3 | B
     5 | 5 | 1 | B

     student_id | student_email | student_name
     :-:|---|---
     1 | john.b20@hogwarts.edu | John B
     2 | sarah.s20@hogwarts.edu | Sarah S
     3 | martha.l20@hogwarts.edu | Martha L
     4 | james.g20@hogwarts.edu | James G |
     5 | stanley.p20@hogwarts.edu | Stanley P

     professor_id | professor_name | subject
     :-:|---|---
     1 | Natalie M | Economics
     2 | William C | Philosophy
     3 | Mark W | Mathematics

4. **In your own words, explain the potential disadvantages of normalizing the data above. What are its trade-offs? Submit your findings in the submission table and discuss them with your mentor in your next session.**
   - The tradeoff with normalization is that while we gain in update/write speed by reducing duplication, we loose performance speed on read operations.  This is because we have to use more `JOIN` statements to get the results we're looking for.  An example of this is with the table above, what could have been a simple select statement on the `student_records` table, will now require several joins in order to get the same information.  If, however, our main action is to write to the database, then the normalized version would be more appropriate.

5. **Looking at the tables you have normalized. If you need to denormalize to improve query performance or speed up reporting, how would you carry out denormalization for this database design? **
   - Depending on the most common lookups, I would include the columns necessary in the primary table being queiried.  For example, if the most common query ran is against the `student_record` table to get an instructors average class grade, in its current form, this would require joining the `student_records` with `professors` on `professor_id`, then group the results by `professor_id` and `subject`, and finally run an `AVG` on the `grade` column.  However, if we included the `professor_name` and `subject` in the `student_record`, we could use a simple `SELECT` statement with a `GROUP BY` to collapse the result, and make use of `AVG` to get the average grade based on the professor name, and subject.  This method does not require the use of a `JOIN`, so there is less time spent in table lookups.  This becomes more important as the database scales up in size.

6. **Explore the trade-offs between data normalization and denormalization in this scenario.**
   - Both have their strenths and weaknesses, and it becomes more important to strategize as the size of the database grows.  If we are using a `denormalized` database on a project with heavy read/reporting actions, we're not going to be very performant.  By contrast, using a `normalized` database in this case will provide the best performance for `read` actions, with the tradeoff of slower `write` actions.  It then becomes a matter of finding a balance, perhaps we don't need a `3NF` table and a `1NF` will give us the results we want.  The important thing, I believe, is knowing what options we have with the different _forms of normalization_, and what each provides in terms of tradeoff.

1. **Explain a subquery in your own words.**
     - A _subquery_ is a query within a query used to get a targeted dataset to use within the main query.  An example of such would be when querying a set of employees along with the average tenure of all employees.  We could use a subquery in the `SELECT` statement to return the `AVG(tenure)` to our query.  An example could look something like the following:
       ```sql
       SELECT e.name, e.hired_on, ( SELECT AGV(tenure)
                                    FROM employee_metrics ) AS avg_tenure
         FROM employees AS e
         JOIN employee_metrics as em
              ON e.id = em.employee_id;
       ```
2. **~~Where can you use a subquery within a `SELECT` statement?~~**
3. **When would you employ a subquery?**
   - Aggregate functions are a good example of when to use a Subquery.  `SUM`, `AVG`, and `COUNT` just to name a few.  Subquries are also used when we need to run a query on the result of another query, such as when using `EXISTS` and `IN`.
   
   ```sql
   SELECT s.first_name, s.last_name, 
     FROM students AS s
    WHERE s.id NOT IN (SELECT e.student_id
                         FROM enrollments AS e
                        WHERE e.course_id = 2132);
   ```
   
   The above query is an example of when to use a Subquery.  Here, we are looking for the students where are not currently encrolled in a given course--in this case, a course with a `course_id` of `2132`.

4. **Explain a row constructor in your own words.**
   - A _row constructor_ builds a new row using values provided for its fields.  In the checkpoint a `row_constructor` is mentioned, but no examples of use are given so the context is lost.  This feels like an after-thought add-in, so unfortunately I don't have an well thought-out explanation.
   
5. **What happens if a row in the subquery result provides a `NULL` value to the comparison?**
   - If the inner query returns a `NULL` value, then the outer query will also return a `NULL` value.
   
6. **What are the ways to use a subquery within a `WHERE` clause? If you can't remember them, do [these flashcards](https://quizlet.com/_3lcpwx) until you can.**
   - Some queries that we can use inside the `WHERE` clause are `IN`, `NOT IN`, `EXISTS`, and `NOT EXISTS`.  We are taking a set of data and comparing it to another set of data, such as "Find employess that are in table `x`, where table `x` is the result of another query to find employees who's vacation balance is more than two weeks."
   
7. **Using [this Adoption schema and data](https://www.db-fiddle.com/f/tpodLv3A43VL4gHqohqx2o/0), please write queries to retrieve the following information and include the results:**
   - **All volunteers. If the volunteer is fostering a dog, include each dog as well.**
   
       - **QUERY**
           ```sql
           SELECT v.first_name, v.last_name, (SELECT dogs.name
                                                FROM dogs
                                               WHERE dogs.id = v.foster_dog_id)
           FROM volunteers AS v;
           ```
           
       - **RESULT**
       
           first_name | last_name | dog_name
           |---|---|---|
           Albus | Dumbledore | null
           Rubeus | Hagrid | Munchkin
           Remus | Lupin | null
           Sirius | Black | null
           Marjorie | Dursley | Marmaduke
   - **The cat's name, adopter's name, and adopted date for each cat adopted within the past month to be displayed as part of the "Happy Tail" social media promotion which posts recent successful adoptions.***
   
        - **QUERY**
            ```sql
            SELECT a.first_name, c.name, ca.date
              FROM cat_adoptions AS ca
                   JOIN adopters AS a
                     ON a.id = ca.adopter_id
  
                   JOIN cats as c
                     ON c.id = ca.cat_id
             WHERE ca.date > CURRENT_DATE - INTERVAL '30 DAYS';
            ```

        - **RESULT**
        
           first_name | name | date
           |---|---|---|
           Arabella | Mushi | 2018-10-02T00:00:00.000Z
           Argus | Victoire | 2018-10-07T00:00:00.000Z
   - **Adopters who have not yet chosen a dog to adopt and generate all possible combinations of adopters and available dogs.**
   
       - **QUERY**
           ```sql
           SELECT a.first_name, a.last_name, d.name AS dog_name
             FROM adopters AS a
                  CROSS JOIN dogs AS d
            WHERE a.id NOT IN (SELECT adopter_id
                               FROM dog_adoptions)
              AND d.id NOT IN (SELECT dog_id
                               FROM dog_adoptions);
           ```
           
       - **RESULT**
       
           first_name | last_name | dog_name
           |---|---|---|
           Hermione | Granger | Boujee
           Arabella | Figg | Boujee
           Hermione | Granger | Munchkin
           Arabella | Figg | Munchkin
           Hermione | Granger | Marley
           Arabella | Figg | Marley
           Hermione | Granger | Lassie
           Arabella | Figg | Lassie
           Hermione | Granger | Marmaduke
           Arabella | Figg | Marmaduke

   - **Lists of all cats and all dogs who have not been adopted.**
       
       - **QUERY**
           ```sql
           SELECT c.id, c.name, c.gender, c.age, 'feline' AS species
             FROM cats AS c
                  LEFT JOIN cat_adoptions AS ca
                       ON c.id = ca.cat_id
            WHERE ca.adopter_id IS NULL
            UNION
           SELECT d.id, d.name, d.gender, d.age, 'canine' AS species
             FROM dogs AS d
                  LEFT JOIN dog_adoptions AS da
                       ON d.id = da.dog_id
            WHERE da.adopter_id IS NULL;
           ```
           
       - **RESULT**
       
           id | name | gender | age | species
           |:-:|---|:-:|:-:|---|
           2 | Seashell | F | 7 | feline
           5 | Nala | F | 1 | feline
           10002 | Munchkin | F | 0 | canine
           10004 | Marley | M | 0 | canine
           10001 | Boujee | F | 3 | canine
           10003 | Lassie | F | 7 | canine
           10006 | Marmaduke | M | 7 | canine
   - **The name of the person who adopted Rosco.**
   
       - **QUERY**
       
           ```sql
           SELECT a.first_name, a.last_name
             FROM adopters AS a
             JOIN dog_adoptions AS da
                  ON da.dog_id =
                     (SELECT id
                      FROM dogs
                      WHERE name = 'Rosco')
                  AND da.adopter_id = a.id;
           ```

       - **RESULT**
       
           first_name | last_name
           |---|---|
           Argus | Filch
8. **Using [this Library schema and data](https://www.db-fiddle.com/f/j4EGoWzHWDBVtiYzB9ygC4/0), write queries applying the following scenarios, and include the results:**
   - **To determine if the library should buy more copies of a given book, please provide the names and position, in order, of all of the patrons with a hold (request for a book with all copies checked out) on "Advanced Potion-Making".**
   
       - **QUERY**
       
           ```sql
           SELECT p.name, h.rank
             FROM patrons AS p
                  JOIN holds AS h
                    ON p.id = h.patron_id

                  JOIN books AS b
                    ON b.isbn = h.isbn
            WHERE b.isbn = '9136884926'
            ORDER BY h.rank;
           ```
       - **RESULT**

           name | rank
           |---|:-:|
           Terry Boot | 1
           Cedric Diggory | 2
   - **Make a list of all book titles and denote whether or not a copy of that book is checked out.**
   
       - **QUERY**
           ```sql
           SELECT b.title, COUNT (t.id)
             FROM transactions AS t
            RIGHT JOIN books b
	               ON b.isbn = t.isbn
                  AND t.checked_in_date IS NULL
            GROUP BY b.isbn;
           ```
       - **RESULT**
       
           title | count
           |---|:-:|
           Fantastic Beasts and Where to Find Them | 1
           Hogwarts: A History | 0
           Advanced Potion-Making | 1
   - **In an effort to learn which books take longer to read, the librarians would like you to create a list of average checked out time by book name in the past month.**
   
       - **QUERY**
           ```sql
           SELECT b.title, CAST(
                           AVG((t.checked_in_date - t.checked_out_date))
                           AS DEC(5,0))
             FROM books AS b
                  JOIN transactions AS t
                  ON b.isbn = t.isbn
                  WHERE (t.checked_out_date >= CURRENT_DATE - INTERVAL '1 MONTH') AND t.checked_in_date IS NOT NULL
            GROUP BY b.title 
            ORDER BY b.title ASC;
            ```
       - **RESULT**
        
           title | avg
           |---|:-:|
           Fantastic Beasts and Where to Find Them | 3
   - **In order to learn which items should be retired, make a list of all books that have not been checked out in the past 5 years.**

       - **QUERY**
           ```sql
           SELECT b.title, t.checked_out_date
             FROM books b
                  JOIN transactions t
                  ON b.isbn = t.isbn
            WHERE t.checked_out_date < CURRENT_DATE - INTERVAL '5 YEARS'
                  AND t.isbn NOT IN
                  (SELECT isbn FROM transactions
                   WHERE checked_in_date IS NULL)
            GROUP BY b.title, t.checked_out_date;
            ```
        - **RESULTS**
        
            title | checked_out_date
            |---|---|
            Hogwarts: A History | 2012-07-29T00:00:00.000Z
   - **List all of the library patrons. If they have one or more books checked out, correspond the books to the patrons.**

       - **QUERY**
           ```sql
           SELECT p.name, b.title
             FROM patrons AS p
                  JOIN transactions t
                    ON p.id = t.patron_id

                  LEFT JOIN books AS b
                    ON t.isbn = b.isbn
            ORDER BY p.id;
           ```
       - **RESULTS**
       
           name | title
           |---|---|
           Hermione Granger | Fantastic Beasts and Where to Find Them
           Hermione Granger | Hogwarts: A History
           Terry Boot | Advanced Potion-Making
           Terry Boot | Fantastic Beasts and Where to Find Them
           Padma Patil | Fantastic Beasts and Where to Find Them
           Cho Chang | Advanced Potion-Making
           Cedric Diggory | Fantastic Beasts and Where to Find Them
9. **Using [this Flight schema and data](https://www.db-fiddle.com/f/tyNDJ9M7QwZn6Y3SAxmK3M/0), write queries applying the following scenarios, and include the results:**
   - **To determine the most profitable airplanes, find all airplane models where each flight has had over 250 paying customers in the past month.**
   - **To determine the most profitable flights, find all destination-origin pairs where 90% or more of the seats have been sold in the past month.**
   - **The airline is looking to expand its presence in Asia and globally. Find the total revenue of any flight (not time restricted) arriving at or departing from Singapore (SIN).**
10. **Compare the subqueries you've written above. Compare them to the joins you wrote in Checkpoint 6. Which ones are more readable? Which were more logical to write?**

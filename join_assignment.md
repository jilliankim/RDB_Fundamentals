1. **How do you find related data that is held in two separate data tables?**
   - To find data that is held in multiple data tables, we would join the data between the tables based on a set of rules using the `JOIN` command.
2. **Explain, in your own words, the difference between a `CROSS JOIN`, `INNER JOIN`, `LEFT OUTER JOIN`, `RIGHT OUTER JOIN`, and `FULL OUTER JOIN`. Give a real world example for each.**
   - `CROSS JOIN`
     - This returns all possible permutations between the tables.  This could be seen both with, and without, the `CROSS JOIN` statement as demonstrated below.  The `CROSS JOIN` results in an `n` number of rows in the resulting table equal to the product of the tables used.
       - > x(y) = n  Where `x` is the number of rows in `Table A`, and `y` is the number of rows in `Table B`.
       - **Participants**
       
         | id | name | active |
         |---|---|---|
         | 1 | Colonel Mustard | True |
         | 2 | Miss Scarlet | False |
         | 3 | Professor Plum | True |
         
         **Items**
         
         | id | description | found |
         |---|---|---|
         | 19 | Pipe | False |
         | 22 | Wrench | True |
         | 32 | Candle Stick | False |
         
       - ```sql
         -- Implicit CROSS JOIN
         SELECT p.name, p.active, i.desc, i.found
         FROM participants AS p, items AS i
         ```
         **OR**
	 
         ```sql
         -- Explicit CROSS JOIN
         SELECT p.name, p.active, i.description, i.found
         FROM participants AS p
         CROSS JOIN items AS i
         ORDER BY p.name;
         ```
       **RESULT**
              
       | name | active | description | found |
       ---|:-:|---|:-:
       Colonel Mustard | 1 | Pipe | 0
       Colonel Mustard | 1 | Wrench | 1
       Colonel Mustard | 1 | Candle Stick | 0
       Miss Scarlet | 0 | Pipe | 0
       Miss Scarlet | 0 | Wrench | 1
       Miss Scarlet | 0 | Candle Stick | 0
       Professor Plum | 1 | Pipe | 0
       Professor Plum | 1 | Wrench | 1
       Professor Plum | 1 | Candle Stick | 0
       
   - `INNER JOIN`
     - Combines two or more tables based on shared columns using a boolean check.  The following example will return a table with the values from the rows in which the `employees` table contains an entry in the `department_id` column that also exists in the `id` column of the `departments` table.
       - **STAFF**
       
         | id | name | department_id | active
         |---|---|---|---|
         | 1 | Billy Bob | 12 | true |
         | 3 | Alex Boolean | 01 | false |
         | 7 | Jenn Ruby | 05 | false |
         | 9 | Mr. Null | 22 | true |
       
         **DEPARTMENTS**
       
         | id | name |
         |:-:|---|
         |01 | Executive |
         |10 | QA |
         |12 | Cust. Svc |
         |15 | Finance |
         |22 | I.T |
       
       - ```sql
         SELECT s.id, e.name, e.active, d.name
         FROM staff AS s
         JOIN departments AS d
         ON s.department_id = d.id;
         ```
         **RESULT**
       
         id | name | active | name
         ---|---|---|---
         3 | Alex Boolean | 0 | Executive
         1 | Billy Bob | 1 | Cust. Svc
         9 | Mr. Null | 1 | I.T.
       
   - `LEFT OUTER JOIN`
     - This type of join will first perform an `INNER JOIN`, as above, and **will also include** all rows on the **LEFT** table, using `NULL` as the value from the right table.  The example below uses our tables from the previous example.
       ```sql
       SELECT s.id, s.name, d.name
       FROM staff AS s
       LEFT JOIN departments AS d
       ON s.department_id = d.id;
       ```
       **RESULT**
       
       id | name | name
       |:-:|---|---|
       3 | Alex Boolean | Executive
       1 | Billy Bob | Cust. Svc
       9 | Mr. Null | I.T.
       7 | Jenn Ruby | null
       
   - `RIGHT OUTER JOIN`
     - This join is the opposite of a `LEFT OUTER JOIN`.  It performs the `INNER JOIN`, however instead of returning all rows in the `LEFT` table, all rows in the **`RIGHT`** table are returned.  Rows in which there are no matches, a `null` value will be used for the `LEFT` value.  Again, the following example will be using the `staff` and `departments` tables
       ```sql
       SELECT s.id, s.name, d.name
       FROM staff AS s
       RIGHT JOIN department AS d
       ON s.department_id = d.id;
       ```
       **RESULT**
       
       id | name | name
       |:-:|---|---|
       1 | Billy Bob | Cust. Svc
       3 | Alex Boolean | Executive
       9 | Mr. Null | I.T.
       null | null | QA
       null | null | Finance
       
   - `FULL OUTER JOIN`
     - This will result in a table containing all of the rows from all tables, after performing an `INNER JOIN`.  It is essentially the combination of `LEFT JOIN` and `RIGHT JOIN` from above.
       ```sql
       SELECT *
       FROM staff
       FULL JOIN departments
       ON staff.department_id = departments.id
       ORGANIZE BY staff.name;
       ```
       **RESULT**
       
       id | name | name
       |:-:|---|---|
       3 | Alex Boolean | Executive
       1 | Billy Bob | Cust. Svc
       7 | Jenn Ruby | (null)
       9 | Mr. Null | I.T.
       (null) | (null) | QA
       (null) | (null) | Finance
       
3. **Define primary key and foreign key. Give a real world example for each.**
   - **Primary Key (PK)**
     - Unique identifyer for each record (row) in a given database table.
     - In the examples used above, in each case the table's first column is `ID` which contains unique integers. This could serve as the table's `primary key`.
   - **Foreign Key (FK)**
     - The `PK` (Primary Key) of one or more tables included in another table to create a relation
     - In the examples above, the `staff` table contains a column for `department_id`, which contained an integer that matched an entry in the `department` table's `id` column. This could serve as a `foreign key`.
4. **Define aliasing.**
   - Using another name to reference a table, usually a single letter.  Examples of aliasing can be found throughout the SQL statements used in this assignment, such as the following:
     ```sql
     SELECT s.id, s.name, d.name
     FROM staff AS s
     LEFT JOIN departments AS d
     ON s.department_id = d.id;
     ```
     
   - In the above example, `s` is serving as an **alias** to the `staff` table, and `d` is serving as an **alias** to the `department` table.
   
5. **Change this query so that you are using aliasing:**

   **ORIGINAL**
   
   ```sql
   SELECT professor.name, compensation.salary, compensation.vacation_days FROM professor 
   JOIN compensation 
   ON professor.id = compensation.professor_id;
   ```
   **REFACTORED**
   
   ```sql
   SELECT p.name, c.salary, c.vacation_days
   FROM professor AS p
   JOIN compensation AS c
   ON p.id = c.professor_id;
   ```
6. **Why would you use a `NATURAL JOIN`? Give a real world example.**
   - A `NATURAL JOIN` would be used when we have shared columns in multiple tables, and each shared column is only returned once in the resulting table.  I honestly can't think of a reason to use `NATURAL JOIN`, since we cannot use a qualifier (i.e. `ON`) with it.  Perhaps in a scenario such as:
   
     **PRODUCTS**
     
     | id | item_name | warehouse_id | qty_stock |
     |:-:|---|:-:|:-:|
     | 1 | Rice Crispy Treats | 12 | 100 |
     | 3 | Generic Soda | 22 | 21 |
     | 7 | SQL for Dummies | 53 | 312 |
     | 9 | Pot Pie | 22 | 123 |
     
     **WAREHOUSES**
     
     | warehouse_id | city |
     |:-:|---|
     12 | Alexandria
     22 | Chicago
     53 | San Antonio
     25 | New York
     
     **QUERY**
     
     ```sql
     SELECT *
     FROM products
     NATURAL JOIN warehouses;
     ```
     
     **RESULT**
     
     warehouse_id | id | item_name | qty_stock | city
     |:-:|:-:|---|:-:|---|
     12 | 1 | Rice Crispy Treats | 100 | Alexandria
     22 | 3 | Generic Soda | 21 | Chicago
     22 | 9 | Pot Pie | 123 | Chicago
     53 | 7 | SQL for Dummies | 312 | San Antonio
     
   - The above sample lists information pertaining to which products are held at which warehouse.
      
7. **Using [this Employee schema and data](https://www.db-fiddle.com/f/sG1TKgR15GhH8cjbAwzjAm/0), write queries to find the following information:**
   - **All employees with their shifts if they have any. Also include any unscheduled shifts.**
     - ```sql
       SELECT employees.name, shifts.date, shifts.start_time, shifts.end_time
       FROM scheduled_shifts
            RIGHT JOIN employees
              ON scheduled_shifts.employee_id = employees.id
     
            JOIN shifts
              ON scheduled_shifts.shift_id = shifts.id
       ```
       
8. **Using [this Adoption schema and data](https://www.db-fiddle.com/f/tpodLv3A43VL4gHqohqx2o/0), please write queries to retrieve the following information and include the results:**
   - **All volunteers. If the volunteer is fostering a dog, include each dog as well.**
     
       ```sql
       SELECT v.first_name AS vfn, v.last_name AS vln, d.name
       FROM dogs AS d
            RIGHT JOIN volunteers AS v
              ON v.foster_dog_id = d.id;
       ```
       **RESULT**
       
       |Volunteer First Name|Volunteer Last Name|Dog Name|
       |---|---|---|
       |Rubeus|Hagrid|Munchkin|
       |Marjorie|Dursley|Marmaduke|
       |Sirius|Black|null|
       |Remus|Lupin|null|
       |Albus|Dumbledore|mull|
       
   - **The cat's name, adopter's name, and adopted date for each cat adopted within the past month to be displayed as part of the &quot;Happy Tail&quot; social media promotion which posts recent successful adoptions.**
     ```sql
     SELECT a.first_name, c.name, ca.date
     FROM cat_adoptions AS ca
          JOIN adopters AS a
            ON a.id = ca.adopter_id
       
          JOIN cats as c
            ON c.id = ca.cat_id
     WHERE ca.date > CURRENT_DATE - INTERVAL '30 DAYS';
     ```
     **RESULT**
     
     |first_name|name|date|
     |---|---|---|
     Arabella|Mushi|2018-07-06T00:00:00.000Z
     Argus|Victoire|2018-07-11T00:00:00.000Z
     
   - **Adopters who have not yet chosen a dog to adopt and generate all possible combinations of adopters and available dogs.**
     - ```sql
       SELECT * FROM adopters 
       CROSS JOIN dogs
       WHERE adopters.id 
       NOT IN (SELECT adopter_id FROM dog_adoptions)
       AND dogs.id NOT IN (SELECT dog_id FROM dog_adoptions)
       ```

   - **Lists of all cats and all dogs who have not been adopted.**
     - ```sql
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
        WHERE da.adopter_id IS NULL
       ```
       **RESULT**
	
       id | name | gender | age | species
       :-:|---|:-:|:-:|---|
       2 | Seashell | F | 7 | feline
       5 | Nala | F | 1 | feline
       10002 | Munchkin | F | 0 | canine
       10004 | Marley | M | 0 | canine
       10001 | Boujee | F | 3 | canine
       10003 | Lassie | F | 7 | canine
       10006 | Marmaduke | M | 7 | canine
       
   - **Volunteers who are available to foster. If they currently are fostering a dog, include the dog. Also include all dogs who are not currently in foster homes.**
     - ```sql
       SELECT *
         FROM volunteers AS v
              FULL JOIN dogs AS d
                ON v.foster_dog_id = d.id
        WHERE v.available_to_foster = true
              OR d.id IS NOT NULL
        ORDER BY v.first_name
       ```
       **RESULT**
	

       id | first_name | last_name | address | phone_number | available_to_foster | foster_dog_id | id | name | gender | age | weight | breed | intake_date | in_foster
       |:-:|---|---|---|---|:-:|:-:|:-:|---|---|:-:|:-:|---|---|---|
       5 | Marjorie | Dursley | 1990 Next Door to Fubster | 463-528-2253 | true | 10006 | 10006 | Marmaduke | M | 7 | 150 | great dane | 2016-03-22T00:00:00.000Z | null
       3 | Remus | Lupin | 12 Grimmauld Place | 627-283-3771 | true | null | null | null | null | null | null | null | null | null
       2 | Rubeus | Hagrid | 1 Edge of Forbidden Forest | 256-667-2378 | true | 10002 | 10002 | Munchkin | F | 0 | 8 | dachsund chihuahua | 2017-01-13T00:00:00.000Z | null
       4 | Sirius | Black | 12 Grimmauld Place | 878-666-4663 | true | null | null | null | null | null | null | null | null | null
       null | null | null | null | null | null | null | 10004 | Marley | M | 0 | 10 | labrador | 2017-05-04T00:00:00.000Z | null
       null | null | null | null | null | null | null | 10003 | Lassie | F | 7 | 17 | collie shepherd | 2016-07-05T00:00:00.000Z | null
       null | null | null | null | null | null | null | 10007 | Rosco | M | 5 | 180 | rottweiler | 2017-04-01T00:00:00.000Z | null
       null | null | null | null | null | null | null | 10001 | Boujee | F | 3 | 36 | labrador poodle | 
       
   - **The name of the person who adopted Rosco.**
     - ```sql
       SELECT a.first_name, a.last_name, da.adopter_id, da.dog_id
         FROM adopters AS a
         JOIN dog_adoptions AS da
              ON da.dog_id =
                 (SELECT id
                  FROM dogs
                  WHERE name = 'Rosco')
              AND da.adopter_id = a.id;
       ```
       **RESULT**
       
       first_name | last_name | adopter_id | dog_id
       |---|---|:-:|---|
       Argus | Filch | 3 | 10007

9. **Using [this Library schema and data](https://www.db-fiddle.com/f/j4EGoWzHWDBVtiYzB9ygC4/0), write queries applying the following scenarios:**
   - **To determine if the library should buy more copies of a given book, please provide the names and position, in order, of all of the patrons with a hold (request for a book with all copies checked out) on &quot;Advanced Potion-Making&quot;.**
     - ```sql
       SELECT p.name, h.rank
         FROM patrons AS p
              JOIN holds AS h
                ON p.id = h.patron_id
             
              JOIN books AS b
                ON b.isbn = h.isbn
        WHERE b.isbn = '9136884926'
        ORDER BY h.rank;
       ```
       **RESULT**
       
       name | rank
       |---|:-:|
       Terry Boot | 1
       Cedric Diggory | 2
   - **Make a list of all book titles and denote whether or not a copy of that book is checked out.**
     - ```sql
       SELECT b.title, COUNT (t.id)
       FROM transactions AS t
       RIGHT JOIN books b
	           ON b.isbn = t.isbn
             AND t.checked_in_date IS NULL
       GROUP BY b.isbn;
       ```
       **RESULT**
       
       title | count
       |---|:-:|
       Fantastic Beasts and Where to Find Them | 1
       Hogwarts: A History | 0
       Advanced Potion-Making | 1

   - **In an effort to learn which books take longer to read, the librarians would like you to create a list of average checked out time by book name in the past month.**
     - ```sql
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
        **RESULT**
        
       title | avg
       |---|:-:|
       Fantastic Beasts and Where to Find Them | 3
   - **In order to learn which items should be retired, make a list of all books that have not been checked out in the past 5 years.**
     - ```sql
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
        ***RESULTS***
        
        title | checked_out_date
        |---|---|
        Hogwarts: A History | 2012-07-29T00:00:00.000Z
   - **List all of the library patrons. If they have one or more books checked out, correspond the books to the patrons.**
     - ```sql
       SELECT p.name, b.title
         FROM patrons AS p
              JOIN transactions t
                ON p.id = t.patron_id
       
              LEFT JOIN books AS b
                ON t.isbn = b.isbn
        ORDER BY p.id;
       ```
       **RESULTS**
       
       name | title
       |---|---|
       Hermione Granger | Fantastic Beasts and Where to Find Them
       Hermione Granger | Hogwarts: A History
       Terry Boot | Advanced Potion-Making
       Terry Boot | Fantastic Beasts and Where to Find Them
       Padma Patil | Fantastic Beasts and Where to Find Them
       Cho Chang | Advanced Potion-Making
       Cedric Diggory | Fantastic Beasts and Where to Find Them

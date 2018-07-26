1. List 5 aggregate functions and explain, in your own words, what they are for. Include a real world example for each. If you can’t list these from memory, do these flashcards until you can!
   - `SUM`
     - Returns the sum of all numbers in a given column, such as adding all of number of widgets being ordered for the month of November.
       - ```sql
         SELECT id, prod_sku, SUM(qty)
         FROM orders
         WHERE EXTRACT(MONTH FROM order_date) = 11;
         ```
   - `AVG`
     - Returns the mathmatical average of all numbers in the column.
       - ```sql
         SELECT AVG(salary)
         FROM employees
         ```
   - `MIN`
     - Returns the lowest value in the provided series (column)
       - ```sql
         SELECT MIN(vac_days_used)
         FROM employees;
         ```
   - `MAX`
     - Returns the hightest value in the provided series (column)
       - ```sql
         SELECT course, MAX(test_score)
         FROM exams
         GROUP BY course;
         ```
   - `COUNT`
     - Returns the number of non-NULL values of a given expression
       - ```sql
         SELECT COUNT(salary)
         FROM employees
         WHERE salary > 50000;
         ```
       - ```sql
         SELECT department, COUNT(salary)
         FROM employees
         WHERE salary > 50000
         GROUP BY department;
         ```

2. Given this [donations](https://www.db-fiddle.com/f/9SzjowZ2NsbEZ4nYe6FQ3d/0) table, write queries and include the output for the following:
   - The total of all donations received in the past year.
     - ```sql
       SELECT CAST(SUM(amount) as DECIMAL(13,2))
       FROM donations;
       ```
       **RESULT**
       
       | sum |
       |-----|
       | 993.00 |
   - The total donations over time per donor (e.g. if Tanysha has donated 3 times with the amounts $25, $30, and $50, then the result would be | Tanysha | 105 |).
     - ```sql
       SELECT donor, CAST(SUM(amount) as DECIMAL(13,2))
       FROM donations
       GROUP BY donor
       ORDER BY donor;
       ```
       **RESULT**
       
       | donor | sum |
       |---|---|
       Arya | 60.00
       Bran | 25.00
       Brienne | 75.00
       Bronn | 20.00
       Daario | 10.00
       Daenerys | 173.00
       Gilly | 7.00
       Jon | 25.00
       Margaery | 120.00
       Melisandre | 45.00
       Missandei | 90.00
       Petyr | 70.00
       Samwell | 20.00
       Sansa | 33.00
       Theon | 20.00
       Tormund | 50.00
       Tyrion | 120.00
       Ygritte | 30.00
   - The average donation per donor.
     - ```sql
       SELECT donor, CAST(AVG(amount) as DECIMAL(13,2))
       FROM donations
       GROUP BY donor
       ORDER BY donor;
       ```
       **RESULT**
       
       | donor | avg |
       |---|---|
       Arya | 20.00
       Bran | 25.00
       Brienne | 75.00
       Bronn | 20.00
       Daario | 10.00
       Daenerys | 86.50
       Gilly | 7.00
       Jon | 25.00
       Margaery | 120.00
       Melisandre | 45.00
       Missandei | 22.50
       Petyr | 70.00
       Samwell | 20.00
       Sansa | 33.00
       Theon | 10.00
       Tormund | 50.00
       Tyrion | 40.00
       Ygritte | 30.00
   - The number of donations over $100.
     - ```sql
       SELECT COUNT(amount)
       FROM donations
       WHERE amount > 100
       ```
       **RESULT**
       
       | count |
       |---|
       | 2 |
   - The largest donation received in a single instance from a single donor.
     - ```sql
       SELECT donor, CAST(MAX(amount) as DECIMAL(13,2))
       FROM donations
       GROUP BY donor;
       ```
       **RESULT**
       
       | donor | max |
       |---|---|
       Samwell | 20.00
       Daario | 10.00
       Brienne | 75.00
       Tyrion | 60.00
       Petyr | 70.00
       Melisandre | 45.00
       Bran | 25.00
       Tormund | 50.00
       Ygritte | 30.00
       Gilly | 7.00
       Jon | 25.00
       Arya | 30.00
       Theon | 15.00
       Bronn | 20.00
       Margaery | 120.00
       Missandei | 30.00
       Sansa | 33.00
       Daenerys | 102.00
   - The smallest donation received.
     - ```sql
       SELECT MIN(amount)
       FROM donations;
       ```
       **RESULT**
       
       | min |
       |---|
       |5.0000|

3. How would you determine the display order of data returned by your SELECT statement?
   - We can use the `ORDER BY` statement to sort the data being returned by our `SELECT` statement, such as is used in question 2, statements 2 and 3.

4. What is a real world situation where you would use OFFSET?
   - Offet is used to advance by `x` number of matches. This could be useful if we want to segment the returned values, such as for sponsors.  We could search for all donors over `x` dollars, then pull the top 10 for recognition, and the next 5 for honorable mentions.
     - ```sql
       SELECT name, SUM(amount)
       FROM donors
       GROUP BY name
       ORDER BY amount DESC
       LIMIT 10;
       
       SELECT name, SUM(amount)
       FROM donors
       GROUP BY name
       ORDER BY amount DESC
       LIMIT 5
       OFFSET 10;
       ```

5. Why is it important to use ORDER BY when limiting your results?
   - To ensure the results are in the order we expect prior to limiting the return.  In the above examples, if we did not order the results prior to limiting the return, we could have ended up with any order of donors.  Since we only want the top donors, we order the results by the amount donated, in descending order.

6. What is the difference between HAVING and WHERE?
   - `HAVING` is used to filter groups, and is used **after** the `GROUP BY` statment.
   - `WHERE` is used to filter individual rows of data and is ran on each individaul row. This statement is used **before** the `GROUP BY` statement.

7. Correct the following SELECT statement:
   >  ```sql
   > SELECT id, SUM (amount)
   > FROM payment
   > HAVING SUM (amount) > 200;
   > ```
   **CORRECTED**
   ```sql
   SELECT id, SUM (amount)
   FROM payment
   GROUP BY id
   HAVING SUM (amount) > 200;
   ```
   
8. Follow the instructions for the scenarios below:

   - Given this [cats](https://www.db-fiddle.com/f/55ePhx9NLn7PzdGnebFaG7/0) table from the previous checkpoint, list all cats organized by intake date.
     - ```sql
       SELECT *
       FROM cats
       ORDER BY intake_date;
       ```
       **RESULT**
       
       | id | name | gender | age | intake_date | adoption_date |
       |---|---|---|---|---|---|
       1 | Mushi | M | 1 | 2016-01-09T00:00:00.000Z | 2016-03-22T00:00:00.000Z
       2 | Seashell | F | 7 | 2016-01-09T00:00:00.000Z | null
       3 | Azul | M | 3 | 2016-01-11T00:00:00.000Z | 2016-04-17T00:00:00.000Z
       4 | Victoire | M | 7 | 2016-01-11T00:00:00.000Z | 2016-09-01T00:00:00.000Z
       5 | Nala | F | 1 | 2016-01-12T00:00:00.000Z | null
       
   - Given this [adoptions](https://www.db-fiddle.com/f/drB8ewWskNNa2EZjkRgzQk/0) table, determine the 5 most recent adoptions to be featured for a social media promotion called "Happy Tails" which lists recent successful adoptions.
     - ```sql
       SELECT *
       FROM adoptions
       ORDER BY date DESC
       LIMIT 5;
       ```
       **RESULT**
       
       |id|adopter|cat|date|fee|
       |---|---|---|---|---|
       10093 | Hermione Granger | Crookshanks | 1993-08-31T00:00:00.000Z | 10.0000
       10054 | Arabella Figg | Mr. Tibbles | 1990-02-18T00:00:00.000Z | 30.0000
       10055 | Arabella Figg | Mr. Paws | 1990-02-18T00:00:00.000Z | 30.0000
       10040 | Arabella Figg | Snowy | 1989-11-29T00:00:00.000Z | 35.0000
       10037 | Arabella Figg | Tufty | 1988-05-03T00:00:00.000Z | 20.0000
       
   - There is a potential adopter looking for an adult female cat. In the most efficient way possible, list all female cats 2 or more years old from the cats table.
     - ```sql
       SELECT id, name, age
       FROM cats
       WHERE age >= 2;
       ```
       **RESULT**
       
       |id|name|age|
       |---|---|---|
       2 | Seashell | 7
       3 | Azul | 3
       4 | Victoire | 7
       
   - From the donations table (from problem #2), find the top 5 donors with the highest cumulative donation amounts to be honored as “Platinum Donors”.
     - ```sql
       SELECT donor, SUM(amount) AS total
       FROM donations
       GROUP BY donor
       ORDER BY total DESC
       LIMIT 5;
       ```
       **RESULT**
       
       |donor|total|
       |---|---|
       Daenerys | 173.0000
       Margaery | 120.0000
       Tyrion | 120.0000
       Missandei | 90.0000
       Brienne | 75.0000
       
   - From the donations table (from problem #2), find donors 6-15 with the next highest cumulative donation amounts to be honored as “Gold Donors”.
     - ```sql
       SELECT donor, SUM(amount) AS total
       FROM donations
       GROUP BY donor
       ORDER BY total DESC
       LIMIT 10
       OFFSET 5;
       ```
       **RESULT**
       
       |donor|total|
       |---|---|
       Petyr | 70.0000
       Arya | 60.0000
       Tormund | 50.0000
       Melisandre | 45.0000
       Sansa | 33.0000
       Ygritte | 30.0000
       Jon | 25.0000
       Bran | 25.0000
       Samwell | 20.0000
       Theon | 20.0000
       

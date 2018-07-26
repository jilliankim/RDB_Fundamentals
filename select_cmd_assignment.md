1. Write out a generic `SELECT` statement.
   - ```sql
     SELECT column(s) FROM table WHERE conditional(s) AND/OR/NOT conditional(s);
     ```
   - ```sql
     SELECT name FROM candidates WHERE age > 18;
     ```
   - ```sql
     SELECT name FROM party_rsvp WHERE invited=true AND thank_you_sent=false;
     ```
2. Create a fun way to remember the order of operations in a `SELECT` statement, such as a mnemonic.
   - I couldn't find any explicit *"Order of Operations for `SELECT` statements"* but my general thought is:
     > `SELECT` this/these column(s)
     > `FROM` this table
     > `WHERE` these statements are true
   - In the event of using conditionals, the use of parentheses may be needed in order to ensure the proper flow of comparison
     - ```sql
       SELECT sku, price
       FROM products
       WHERE price < 20.00 OR sale_price < 20.00 AND in_stock=true;
       ```
       is different from
       ```sql
       SELECT sku, price
       FROM products
       WHERE (price < 20.00 OR sale_price < 20.00) AND in_stock=true;
       ```
3. Given this [`dogs`](https://www.db-fiddle.com/f/kvD6xZQ14vRbpPe5muA92L/0) table, write queries to select the following pieces of data:
   - Display the name, gender, and age of all dogs that are part Labrador.
     - ```sql
       SELECT name, gender, age
       FROM dogs
       WHERE breed LIKE '%labrador%';
       ```
       **RESULT:**
       
       name | gender | age
       -----|--------|----
       Boujee | F | 3
       Marley | M | 0
       
   - Display the ids of all dogs that are under 1 year old.
     - ```sql
       SELECT id
       FROM dogs
       WHERE age < 1;
       ```
       **RESULT**
       
       | id |
       |----|
       |10002|
       |10004|
       
   - Display the name and age of all dogs that are female and over 35lbs.
     - ```sql
       SELECT name, age
       FROM dogs
       WHERE gender='F'
       AND weight > 35;
       ```
       **RESULT**
       
       name | age
       ---|---
       Boujee | 3
       
   - Display all of the information about all dogs that are not Shepherd mixes.
     - ```sql
       SELECT *
       FROM dogs
       WHERE breed NOT LIKE '%Shepherd%';
       ```
       **RESULT**
       
       |id|name|gender|age|weight|breed|intake_date|in_foster|
       |---|---|---|---|---|---|---|---|
       10001 | Boujee | F | 3 | 36 | labrador poodle | 2017-06-22T00:00:00.000Z | null
       10002 | Munchkin | F | 0 | 8 | dachsund chihuahua | 2017-01-13T00:00:00.000Z | 2017-01-31T00:00:00.000Z
       10004 | Marley | M | 0 | 10 | labrador | 2017-05-04T00:00:00.000Z | 2016-06-20T00:00:00.000Z
       10006 | Marmaduke | M | 7 | 150 | great dane | 2016-03-22T00:00:00.000Z | 2016-05-15T00:00:00.000Z
       10007 | Rosco | M | 5 | 180 | rottweiler | 2017-04-01T00:00:00.000Z | null
       
   - Display the id, age, weight, and breed of all dogs that are either over 60lbs or Great Danes.
     - ```sql
       SELECT id, age, weight, breed
       FROM dogs
       WHERE weight > 60 OR breed LIKE '%great dane%';
       ```
       **RESULT**
       
       id | age | weight | breed
       ---|---|---|---
       10006 | 7 | 150 | great dane
       100078 | 5 | 180 | rottweiler
       
   - >Intake teams typically guess the breed of shelter dogs, so the `breed` column may have multiple words (for example, "Labrador Collie mix").
4. Given this [`cats`](https://www.db-fiddle.com/f/55ePhx9NLn7PzdGnebFaG7/0) table, what records are returned from these queries?
   - `SELECT name, adoption_date FROM cats;`
     - name | adoption_date
       ---|---
       Mushi | 2016-03-22T00:00:00.000Z
       Seashell | null
       Azul | 2016-04-17T00:00:00.000Z
       Victoire | 2016-09-01T00:00:00.000Z
       Nala | null
       
   - `SELECT name, age FROM cats;`
     - name | age
       ---|---
       Mushi | 1
       Seashell | 7
       Azul | 3
       Victoire | 7
       Nala | 1
       
5. From the `cats` table, write queries to select the following pieces of data.
   - Display all the information about all of the available cats.
     - ```sql
       SELECT *
       FROM cats;
       ```
       **RESULT**
       
       id | name | gender | age | intake_date | adoption_date
       ---|---|---|---|---|---
       1 | Mushi | M | 1 | 2016-01-09T00:00:00.000Z | 2016-03-22T00:00:00.000Z
       2 | Seashell | F | 7 | 2016-01-09T00:00:00.000Z | null
       3 | Azul | M | 3 | 2016-01-11T00:00:00.000Z | 2016-04-17T00:00:00.000Z
       4 | Victoire | M | 7 | 2016-01-11T00:00:00.000Z | 2016-09-01T00:00:00.000Z
       5 | Nala | F | 1 | 2016-01-12T00:00:00.000Z | null
       
   - Display the name and sex of all cats who are 7 years old.
     - ```sql
       SELECT name, gender
       FROM cats
       WHERE age = 7;
       ```
       **RESULT**
       
       name | gender
       ---|---
       Seashell | F
       Victoire | M
       
   - Find all of the names of the cats, so you don’t choose duplicate names for new cats.
     - ```sql
       SELECT name
       FROM cats;
       ```
       **RESULT**
       
       | name |
       | ---- |
       | Mushi |
       | Seashell |
       | Azul |
       | Victoire |
       | Nala |
       
6. List each comparison operator and explain, in your own words, when you would use it. Include a real world example for each.
   - Less Than: `<`
     - This operator would be used when we want to ensure the value retrieved is **below**, or less than, a desired value
       - ```sql
         SELECT realtor, address
         FROM houses
         WHERE asking_price < 80000;
         ```
   - Greater Than: `>`
     - This operator would be used when we want to ensure the value retrieved is **above**, or greater than, a desired value
       - ```sql
         SELECT name, address
         FROM customers
         WHERE average_purchase > 100;
         ```
   - Less Than, Or Equal To: `<=`
     - This operator matches values that are less than, or equal to, the provided value
       - ```sql
         SELECT hotel, price, phn_number
         FROM vacation_spots
         WHERE (per_night * 4) <= 1000;
         ```
   - Greater Than, Or Equal To: `>=`
     - The `>=` returns entries that are greater than, or equal to, the provided value
       - ```sql
         SELECT name, grade
         FROM tests
         WHERE grade >= 70;
         ```
         
   - Exact Same: `=`
     - This operator would be used when we want to ensure the values retrieved are an exact match to the value provided
       - ```sql
         SELECT *
         FROM employees
         WHERE is_teacher=true;
         ```
   - Does Not Equal / Not The Same: `!=`, `<>`
     - This operator returns records that do not match the provided value
       - ```sql
         SELECT name, gender
         FROM cats
         WHERE age != 0;
         ```
   - Resembles String: `LIKE`
     - This operator is helpful if you know part of a string that you're looking for.  It uses the wildcard `%` to denote where in the string the match may occur.
       - ```sql
         SELECT id, name
         FROM employees
         WHERE last_name LIKE '%davis`;
         ```
       - ```sql
         SELECT id, name
         FROM employees
         WHERE first_name LIKE 'reb%';
         ```
       - ```sql
         SELECT id, name, adoption_date
         FROM dogs
         WHERE breed LIKE '%husky%';
         ```
   - Additional Conditional Required: `AND`
     - Requires all conditional statments to resolve to `true` in order to match
       - ```sql
         SELECT company, job_title
         FROM job_postings
         WHERE city LIKE '%chicago%'
         AND years_experience < 2;
         ```
   - Additional Conditional Optional: `OR`
     - Requires at least one conditional to be `true` in order to match
       - ```sql
         SELECT company, job_title
         FROM job_postings
         WHERE city LIKE '%chicago%'
         OR relocation_offered=true;
         ```
   - Additional Conditional Exclusion: `NOT`
     - Any records that match this conditional will not be included in the returned matches
       - ```sql
         SELECT company, job_title
         FROM job_postings
         WHERE city LIKE '%chicago%'
         AND job_title NOT LIKE '%senior%';
         ```
   
7. From the `cats` table, what data is returned from these queries?
   - `SELECT name FROM cats WHERE gender = ‘F’;`
     - | name |
       | --- |
       | Seashell |
       | Nala |
       
   - `SELECT name FROM cats WHERE age <> 3;`
     - | name |
       | --- |
       | Mushi |
       | Seashell |
       | Victoire |
       | Nala |
       
   - `SELECT id FROM cats WHERE name != ‘Mushi’ AND gender = ‘M’;`
     - | id |
       |----|
       | 3 |
       | 4 |

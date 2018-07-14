1. What data types do each of these values represent?
   1. "A Clockwork Orange"
      > `string`
   2. 42
      > `integer`
   3. 09/02/1945
      > `date`
   4. 98.7
      > `float`
   5. $15.99
      > `float`
2. Explain in your own words when a database might be used. Explain when a text file might be used.
   >This question could be interpreted in many ways.  The most thought provoking being that the text file **IS** a database, although an inefficient one.  In the text file, we'd store data as it relates to our application, in the context of the domain in which we are working.  Let's say we used a text file as the database for an address book.  We'd have to read in that text file, parse the information, and store it into memory.  Any time we looked up a name, we'd have to search the file.  Need to find all contacts in `Chicago`?  You'll need to search the file for each occurance of that city, and cross reference the name.  It gets highly inefficient very fast.  This is where **Database Management Systems (DMBS)** come into play.  We are able to assign indexes to specific table entries, which makes searching much simpler.  Instead of searching the entire file for users who's address is in Chicago, we could **Query** for **Users** who's **City** is equal to `Chicago`.  This is much more efficient.
3. Describe one difference between SQL and other programming languages.
   >One difference is that SQL is a declarative language, while most other languages are procedural.  This means that in SQL we state what we want to happen, or we **declare** the action.  In other languages, we have to state the **procedure** required to reach a given result, such as creating a function (procedure) to multiply 3 and 4.
4. In your own words, explain how the pieces of a database system fit together at a high level.
   >The table represents a "something", that is described by various `columns` of properties.  Each `row` of a `table` represents a new entry, and the `values` in that row, for each column, describe that entry.
5. Explain the meaning of table, row, column, and value.
   >- A **table** is a representation of something to be stored, an `object`
   >   - A `Users` table may store information about a user including username, password hash, last login, etc
   >- A **row** represents one item.
   >   - In the above `Users` table, each row is a new user
   >- A **column** represents data to be stored about the `object` being stored in the `table`
   >   - In our `Users` table, we may want to store the User's name, username, password hash, join date, etc. Each of these is a column in the table
   >- A **value** is the data to be stored in the `column` and `row` in which the object is saved
   >   - The `Users` table contains an entry for "David".  The value in the columns represent this User's name, username, etc.
6. List 3 data types that can be used in a table.
   >- String
   >- Integer
   >- Boolean
7. Given this [`payments`](https://www.db-fiddle.com/f/5gVGFmB8Aq66SejCFEbfdd/0) table, provide an English description of the following queries and include their results:
   - ```SQL
     SELECT date, amount
     FROM payments;
     ```
     >This command will select all data, and amount entries from the payments table and return a table consisting of only the date and amount columns.



   - ```SQL
     SELECT amount
     FROM payments
     WHERE amount > 500;
     ```
     >This command will `select` all ammounts in the `payments` table that have an `amount` over 500

   - ```SQL
     SELECT *
     FROM payments
     WHERE payee = 'Mega Foods';
     ```
     >This command will `select` **all** columns from the `payments` table where the `payee` is 'Mega Foods'

8. Given this ['users'](https://www.db-fiddle.com/f/iQAEYktwysXqcLQHv2dwbc/0) table, write SQL queries using the following criteria and include the output:
   * The email and sign-up date for the user named DeAndre Data.
     - ```SQL
       SELECT email, signup
       FROM users
       WHERE name = 'DeAndre Data';
       ```

       email | signup
       ------|-------
       datad@comcast.net | 2008-01-20T00:00:00.000Z
     
   * The user ID for the user with email 'aleesia.algorithm@uw.edu'.
     - ```SQL
       SELECT userid
       FROM users
       WHERE email = 'aleesia.algorithm@uw.edu';
       ```

       userid |
       -------|
       1 |
       
   * All the columns for the user ID equal to 4.
     - ```SQL
       SELECT *
       FROM users
       WHERE userid = 4;
       ```

       userid | name | email | signup
       -------|------|-------|-------
       1 | Brandy Boolean | bboolean@nasa.gov | 1999-10-15T00:00:00.000Z

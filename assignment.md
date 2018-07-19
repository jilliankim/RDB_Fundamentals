1. List the commands for adding, updating, and deleting data
   - > To add data to an existing table, we would use the `INSERT INTO` command, followed by the table name, columns the values correspond to (optional), followed by the command `VALUES`, and a series of values contained within parenthesis and comma separated.
     - ```sql
       INSERT INTO myTable (id, name, age) VALUES (1, 'Michael', 35);
       ```
   - > To update data, the `UPDATE` and `SET` commands are used. It is important to note here that we need to not only specify the table that we are updating, but the column to update, as well as which records we want to update. The command below will look in the table named `myTable`, and look for an entry with the `id` of `1`, and change the `id` to `2` as well as the `name` to `ANGELA`.
     - ```sql
       UPDATE myTable SET id=2, name='ANGELA' WHERE id=1;
       ```
     
   - > To delete data, the command to use is `DELETE FROM`.  We also need to qualify this with a selector so we can ensure we are only deleting the record we intend to remove. This action is irreversable.  The following command will look in the table `myTable` for records with the name 'ANGELA', and delete it.
     - ```sql
       DELETE FROM myTable WHERE name='ANGELA';
       ```
     
2. Explain the structure for each type of command.
   >- ADD data to the table using the following format
   >   - **INSERT INTO** *table* (*columns*) **VALUES** (*values*);
   >   - Listing the columns is optional with this comand. If the columns are omitted Postgres assumes the provided `VALUES` will be in the order in which the columns of the table are arranged.
   >- UPDATE data in the table using the following format
   >   - **UPDATE** *table* **SET** *column*=*value* **WHERE** *column*=*value*;
   >   - With this command, we are telling the database to **update** the given table, and to **SET** the values of the provided columns to the provided value only on those records that match our **WHERE** statement.
   >- DELETE data to the table using the following format
   >   - **DELETE FROM** *table* **WHERE** *column*=*value*;
   >   - Here, we tell the database to **DELETE FROM** the provided table all records that match the **WHERE** statement.

3. What are some the data types that can be used in tables? Give a real world example of each.
   >- Integer (int): Any whole number within a given range.  The range will vary based on the Management System used.
   >   - 33, -23, 12,090, 300
   >- varchar: A series of characters encased in a single quote on both ends
   >   - 'Hello', 'World', '!'
   >- decimal(p,s): Stores a decimal number with a maximum of `p` digits in the number (referred to as the `precision`), and `s` digits after the decimal (referred to as `scale`).
   >   - `decimal(5,2)` -- This will have a maximum of **5** digits, and no more than **2** after the decimal point.
   >     - 123.01, 12.99, 900.00
   >   - `decimal(10, 4)` -- This will have a maximum of **10** digits in the number, with no more than **4** digits after the decimal point.
   >     - 12.0001, 103244.301, 123.01

4. Think through how to create a new table to hold a list of people invited to a wedding. This table needs to have first and last name, whether they sent in their RSVP, the number of guests they are bringing, and the number of meals (1 for adults and 1/2 for children).
   - Which data type would you use to store each of the following pieces of information?
     - First and last name.
       - >varchar
     - Whether they sent in their RSVP.
       - >boolean
     - Number of guests.
       - >int
     - Number of meals.
       - >decimal(2,1)
   - Write a command that makes the table to track the wedding.
     - ```sql
       CREATE TABLE wedding_rsvp (
         id integer,
         firstname varchar(15),
         lastname varchar(15),
         rsvp boolean,
         guests int,
         meals decimal(2,1)
       );
       ```
   - Using the table we just created, write a command that adds a column to track whether they were sent a thank you card.
     - ```sql
       ALTER TABLE wedding_rsvp ADD COLUMN thank_you boolean;
       ```
   - You have decided to move the data about the meals to another table, so write a command to remove the column storing the number meals from the wedding table.
     - ```sql
       ALTER TABLE wedding_rsvp DROP COLUMN thank_you;
       ```
   - The guests are going to need a place to sit at the reception, so write a command that adds a column for table number.
     - ```sql
       ALTER TABLE wedding_rsvp ADD COLUMN table_number integer;
       ```
   - The wedding is over and we do not need to keep this information, so write a command that deletes the wedding table from the database.
     - ```sql
       DROP TABLE wedding_rsvp
       ```

5. Write a command to make a new table to hold the books in a library with the columns ISBN, title, author, genre, publishing date, number of copies, and available copies.
   - ```sql
     CREATE TABLE books (
       isbn int,
       title varchar(30),
       author varchar(30),
       genre varchar(15),
       publishing_date date, 
       copies int,
       available_copies int
     );
     ```
   - Find three books and add their information to the table.
     - ```sql
       INSERT INTO books VALUES
       (0984782869, 'Cracking the Coding Interview', 'Gayle Laakmann McDowell', 'Programming', '2015-07-15', 4, 0),
       (1848000693, 'The Algorithm Design Manual', 'Steven S Skiena', 'Programming', '2011-04-11', 10, 2),
       (1788626850, 'Node.js Web Development', 'David Herron ', 'Programming', '2018-05-30', 8, 3);
       ```
   - Someone has just checked out one of the books. Change the available copies column to 1 fewer.
     - ```sql
       UPDATE books SET available_copies=2 WHERE isbn=1788626850;
       ```
       or
     - ```sql
       UPDATE books SET available_copies=2 WHERE author='David Herron' AND title='Node.js Web Development';
       ```
   - Now one of the books has been added to the banned books list. Remove it from the table.
     - ```sql
       DELETE FROM books WHERE isbn=0984782869;
       ```

6. Write a command to make a new table to hold spacecrafts. Information should include id, name, year launched, country of origin, a brief description of the mission, orbiting body, if it is currently operating, and approximate miles from Earth. In addition to the table creation, provide commands that perform the following operations:
   - ```sql
     CREATE TABLE spacecrafts (
       id integer,
       name varchar(30),
       launched date,
       origin_country varchar(30),
       mission_desc varchar(max),
       orbiting_body varchar(30),
       is_orbiting boolean,
       miles_from_earth integer
     );
     ```
   - Add 3 non-Earth-orbiting satellites to the table.
     - ```sql
       INSERT INTO spacecrafts
       (1, 'sat_032_alpha', '2003-04-08', 'USA', 'Monitor the atmopsheric changes and chemistry of Mars.', 'Mars', true, '8000'),
       (2, 'sat_133_theta', '2014-01-22', 'Spain', 'Relay message of welcome to E.T.', 'unknown', 'true', '1000000'),
       (3, 'roy_001_gamma', '2018-02-07', 'USA', 'Needed a test payload, why not a Tesla?', 'sun', true, 'unknown');
       ```
   - Remove one of the satellites from the table since it has just been crashed into the planet.
     - ```sql
       DELETE FROM spacecrafts WHERE name='sat_032_alpha' AND launched='2003-04-08';
       ```
   - Edit another satellite because it is no longer operating and change the value to reflect that.
     - ```sql
       UPDATE spacecrafts SET is_orbiting=false WHERE id=2;
       ```

7. Write a command to make a new table to hold the emails in your inbox. This table should include an id, the subject line, the sender, any additional recipients, the body of the email, the timestamp, whether or not it’s been read, and the id of the email chain it’s in. Also provide commands that perform the following operations:
   - ```sql
     CREATE TABLE emails (
       id integer,
       subject varchar(30),
       sender varchar(30),
       other_recipients varchar(max),
       email_body varchar(max),
       email_timestamp timestamp,
       read boolean,
       chain_id integer
     );
     ```
   - Add 3 new emails to the inbox.
     - ```sql
       INSERT INTO emails VALUES
       (1, 'Test Msg', 'Michael Davis', 'Billy Bob, Colonel Mustard', 'This is just a test, thanks!', '2018-07-14 12:10:00:000Z', false, 1),
       (12, 'RE: Test Msg', 'Billy Bob', 'Michael Davis, Colonel Mustard', 'I did not do it!', '2018-07-14 12:24:41:000Z', true, 2),
       (21, 'RE: Test Msg', 'Colonel Mustard', 'Billy Bob, Michael Davis', 'Yes you did!  In the study, with the candle stick!', '2018-07-14 12:40:10:000Z', true, 3);
       ```
   - You’ve just deleted one of the emails, so write a command to remove the row from the inbox table.
     - ```sql
       DELETE FROM emails WHERE id=21;
       ```
   - You started reading an email but just heard a crash in another room. Mark the email as unread before investigating, so you can come back to it later.
     - ```sql
       UPDATE emails SET read=false WHERE id=12;
       ```

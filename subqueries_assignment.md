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
4. **Explain a row constructor in your own words.**
5. **What happens if a row in the subquery result provides a `NULL` value to the comparison?**
6. **What are the ways to use a subquery within a `WHERE` clause? If you can't remember them, do these flashcards until you can.**
7. **Using this Adoption schema and data, please write queries to retrieve the following information and include the results:**
   - **All volunteers. If the volunteer is fostering a dog, include each dog as well.**
   - **The cat's name, adopter's name, and adopted date for each cat adopted within the past month to be displayed as part of the "Happy Tail" social media promotion which posts recent successful adoptions.***
   - **Adopters who have not yet chosen a dog to adopt and generate all possible combinations of adopters and available dogs.**
   - **Lists of all cats and all dogs who have not been adopted.**
   - **The name of the person who adopted Rosco.**
8. **Using this Library schema and data, write queries applying the following scenarios, and include the results:**
   - **To determine if the library should buy more copies of a given book, please provide the names and position, in order, of all of the patrons with a hold (request for a book with all copies checked out) on "Advanced Potion-Making".**
   - **Make a list of all book titles and denote whether or not a copy of that book is checked out.**
   - **In an effort to learn which books take longer to read, the librarians would like you to create a list of average checked out time by book name in the past month.**
   - **In order to learn which items should be retired, make a list of all books that have not been checked out in the past 5 years.**
   - **List all of the library patrons. If they have one or more books checked out, correspond the books to the patrons.**
9. **Using this Flight schema and data, write queries applying the following scenarios, and include the results:**
   - **To determine the most profitable airplanes, find all airplane models where each flight has had over 250 paying customers in the past month.**
   - **To determine the most profitable flights, find all destination-origin pairs where 90% or more of the seats have been sold in the past month.**
   - **The airline is looking to expand its presence in Asia and globally. Find the total revenue of any flight (not time restricted) arriving at or departing from Singapore (SIN).**
10. **Compare the subqueries you've written above. Compare them to the joins you wrote in Checkpoint 6. Which ones are more readable? Which were more logical to write?**

1. 

  + The file executes the SQL statements when loaded. 
  + NOTICE:  table "films" does not exist, skipping  
    DROP TABLE # drops table if it already exists
    CREATE TABLE # creates table
    INSERT 0 1 # inserts a record
    INSERT 0 1 # inserts a record
    INSERT 0 1 # inserts a record

  + The first line in the "file_to_import.sql" drops the table if a table named files already exits. 

2. Write a SQL statement that returns all rows in the films table.

      + SELECT * FROM films;

3. Write a SQL statement that returns all rows in the films table with a title shorter than 12 letters.

      + SELECT * FROM films WHERE length(title) < 12;

4. Write the SQL statements needed to add two additional columns to the films table: director, which will hold a director's full name, and duration, which will hold the length of the film in minutes.

  + ALTER TABLE films
    ADD COLUMN director varchar(255), 
    ADD COLUMN duration integer;

5. UPDATE films SET director = 'John McTiernan', duration = 132 WHERE title = 'Die Hard';
   UPDATE films SET director = 'Michael Curtiz', duration = 102 WHERE title = 'Casablanca';
   UPDATE films SET director = 'Francis Ford Coppola', duration = 113 WHERE title = 'The Conversation';

6. INSERT INTO films (title, "year", genre, director, duration)
    VALUES (1984, 1956, 'scifi', 'Michael Anderson', 90),
       ('Tinker Tailor Soldier Spy', 2011, 'espionage', 'Tomas Alfredson', 127),
       ('The Birdcage', 1996, 'comedy', 'Mike Nichols', 118);

7. SELECT title, extract("year" from current_date) - year AS age FROM films
   ORDER BY age ASC;

8. SELECT title, duration FROM films
   WHERE duration > 120
   ORDER BY duration DESC;

9. SELECT title FROM films
   ORDER BY duration DESC
   LIMIT 1;
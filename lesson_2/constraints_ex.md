2. ALTER TABLE films ALTER COLUMN title SET NOT NULL;
ALTER TABLE films ALTER COLUMN year SET NOT NULL;
ALTER TABLE films ALTER COLUMN genre SET NOT NULL;
ALTER TABLE films ALTER COLUMN director SET NOT NULL;
ALTER TABLE films ALTER COLUMN duration SET NOT NULL;

3. not null is included in the columns nullable column

4. ALTER TABLE films ADD CONSTRAINT title UNIQUE (title);

5. The constraint appears as an index

6. ALTER TABLE films DROP CONSTRAINT title;

7. ALTER TABLE films ADD CONSTRAINT title_length CHECK (length(title) >= 1);

8. INSERT INTO films VALUES ('', 1901, 'action', 'JJ Abrams', 126);

    + This displays an error that the new row of values violates the "title_length" constraint and shows the failing row contents. 

9. This added constraint appears in the "Check constraints". 

10. ALTER TABLE films DROP CONSTRAINT title_length;

11. ALTER TABLE films ADD CONSTRAINT year_range CHECK (year BETWEEN 1900 AND 2100);

12. This constraint appears in the "Check constraints". 

13. ALTER TABLE films ADD CONSTRAINT director_name
    CHECK (length(director) >= 3 AND position(' ' in director) > 0);

14. This contraint appears in the "Check constraints".

15. UPDATE films SET director = 'Johnny' WHERE title = 'Die Hard';

    + It shows an error that says the edit you're trying to make violates the check constraint "director_name" and displays the contents of the row you're trying to add. 

16. Datatype, not null constraint, check constraint

17. You can define a default value but it'll throw an error when adding a value that won't pass the constraint. 

  CREATE TABLE candy (name text, size integer DEFAULT 0);
  ALTER TABLE candy ADD CONSTRAINT candy_size CHECK (size between 1 and 10);
  INSERT INTO candy (name) VALUES ('jolly rancher');

18. \d films;
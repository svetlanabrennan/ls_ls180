# DML / DDL / DCL Exercises

1. The other 2 sublanguages are:

  + Data Definition Language handles the structure of the database - describes how the data is structured. 
    - Example: CREATE TABLE, ALTER TABLE

  + Data Manipulation Language provides data manipulation in the database - it is used to create, read, update, delete the actual data in the database
    - Examples: SELECT, INSERT INTO

2. SELECT column_name FROM my_table;

  + It's using DML because it's using SELECT

3. DDL or DML?

  ```sql
    CREATE TABLE things (
      id serial PRIMARY KEY,
      item text NOT NULL UNIQUE,
      material text NOT NULL
    );
  ```

  + DDL because it's creating a table and structuring it

4. DDL or DML?
  ```sql
    ALTER TABLE things
    DROP CONSTRAINT things_item_key;
  ```

  + DDL because it's altering the constraint in the table

5. DDL or DML?

  ```sql
    INSERT INTO things VALUES (3, 'Scissors', 'Metal');
  ```

  + DML because it's adding data to the table

6. DDL or DML?

  ```sql
    UPDATE things
    SET material = 'plastic'
    WHERE item = 'Cup';
  ```

  + DML because it's updating the data

7. DDL or DML: `\d things`

  + None. This is psql meta command. 

8. DDl or DML?

  `DELETE FROM things WHERE item = 'Cup';`

  + DML because it's changing the data

9. DDL or DML?

  `DROP DATABASE xyzzy;`

  + DDL because it's completly deleting the database. It's DDL because it affects the structure of the database as well as deleting it. 

10. DDL or DML?

  `CREATE SEQUENCE part_number_sequence;`

  + This is DDL since it's setting up a sequence object to the database. 
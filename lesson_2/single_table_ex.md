1. 
  CREATE TABLE people (
    name varchar(255), 
    age integer, 
    occupation varchar(255);
  )

2. 
  INSERT INTO people (name, age, occupation) VALUES ('Abby', 34, 'biologist');
  INSERT INTO people (name, age) VALUES ('Mu''nisah', 26);
  INSERT INTO people (name, age, occupation) VALUES ('Mirabell', 40, 'contractor');

3. SELECT * FROM people WHERE name = 'Mu''nisah';
   SELECT * FROM people WHERE age = 26;
   SELECT * FROM people WHERE occupation IS NULL;

4. 
  CREATE TABLE birds (
    name varchar(255),
    length decimal(4, 1), 
    wingspan decimal(4, 1), 
    family text, 
    extinct boolean
  );

5. INSERT INTO birds
   VALUES ('Spotted Towhee', 21.6, 26.7, Emberizidae, false)
   ('American Robin', 25.5, 36.0, 'Turdidae', false);
   ('Greater Koa Finch', 19.0, 24.0, 'Fringillidae', true);
   ('Carolina Parakeet', 33.0, 55.8, 'Psittacidae', true);
   ('Common Kestrel', 35.5, 73.5, 'Falconidae', false);

6. SELECT name, family FROM birds
   WHERE extinct = False
   ORDER BY length DESC;

7. SELECT min(wingspan), max(wingspan), round(avg(wingspan), 1) FROM birds;

8. CREATE TABLE menu_items (
  item text, 
  prep_time integer, 
  ingrident_cost decimal(4, 2), 
  sales integer,
  menu_price decimal(4, 2)
)

9. INSERT INTO menu_items VALUES ('omelette', 10, 1.50, 182, 7.99);
   INSERT INTO menu_items VALUES ('tacos', 5, 2.00, 254, 8.99);
   INSERT INTO menu_items VALUES ('oatmeal', 1, 0.50, 79, 5.99);

10. SELECT item, menu_price - ingrident_cost AS profit FROM menu_items
    ORDER BY profit DESC LIMIT 1;

11. SELECT item, menu_price, ingrident_cost, 

    (prep_time / 60) * 13 AS labor,
    menu_price - ingrident_cost - (prep_time / 60) * 13 AS labor AS profit 
  FROM menu_items
  ORDER BY profit DESC;


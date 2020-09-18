1. INSERT INTO calls("when", duration, contact_id) VALUES('2016-01-18 14:47:00', 632, 6);

2. SELECT calls.when, calls.duration, contacts.first_name 
   FROM calls
   INNER JOIN contacts
   ON calls.contact_id = contacts.id
   WHERE (contacts.first_name || ' ' || contacts.last_name) != 'William Swift';

3. 
      INSERT INTO contacts(first_name, last_name, number) 
      VALUES('Merve', 'Elk', '6343511126'),
      ('Sawa', 'Fyodorov', '6125594874');

      INSERT INTO calls("when", duration, contact_id)
      VALUES ('2016-01-17 11:52:00', 175, 26),
      ('2016-01-18 21:22:00', 79, 27);

4. ALTER TABLE contacts ADD CONSTRAINT number_unique UNIQUE(number);

5. INSERT INTO contacts(first_name, last_name, number)
  VALUES('Test', 'One', '7204890809');

6. "when" needs to be quoted because there's reserved keyword for it and you can't use a column with the same name unless you use quotes
3. SELECT state, COUNT(id) FROM people
   GROUP BY state
   ORDER BY count DESC
   LIMIT 10;

4. 

  + option 1:
  SELECT SUBSTR(email, strpos(email, '@') + 1) as domain, COUNT(id) FROM people
   GROUP BY domain
   ORDER BY count DESC;
   

  + option 2:
   SELECT split_part(email, '@', 2) AS domain, COUNT(id) FROM people
   GROUP BY domain
   ORDER BY count DESC;

5. DELETE FROM people WHERE id = 3399;

6. DELETE FROM people WHERE state = 'CA';

7. UPDATE people SET given_name = UPPER(given_name) WHERE email LIKE '%teleworm.us';

8. DELETE FROM people;
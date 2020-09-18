1. CREATE TABLE devices (
  id serial PRIMARY KEY, 
  name text NOT NULL, 
  created_at timestamp DEFAULT CURRENT_TIMESTAMP
);

    CREATE TABLE parts (
      id serial PRIMARY KEY, 
      part_number integer UNIQUE NOT NULL, 
      device_id integer REFERENCES devices(id)
    );

2. INSERT INTO devices (name) 
    VALUES('Accelerometer'),
    ('Gyroscope');

    INSERT INTO parts(part_number, device_id)
    VALUES (5, 1), 
    (20, 1),
    (35, 1),
    (195, 2),
    (500, 2),
    (145, 2),
    (166, 2), 
    (37, 2);

    INSERT INTO parts(part_number) 
    VALUES (200),
    (8),
    (55);

3. SELECT devices.name, parts.part_number FROM devices
   INNER JOIN parts ON parts.device_id = devices.id;

4. SELECT * FROM parts WHERE part_number::text LIKE '3%';

5. SELECT devices.name AS name, count(parts.device_id) FROM devices 
  LEFT OUTER JOIN parts ON devices.id = parts.device_id
  GROUP BY name;

6. SELECT devices.name AS name, count(parts.device_id) AS count FROM devices 
  LEFT OUTER JOIN parts ON devices.id = parts.device_id
  GROUP BY name
  ORDER BY devices.name DESC;

7. SELECT part_number, device_id FROM parts WHERE device_id IS NOT NULL;
   SELECT part_number, device_id FROM parts WHERE device_id IS NULL;

8. INSERT INTO devices (name) VALUES ('Magnetometer');
   INSERT INTO parts (part_number, device_id) VALUES (42, 3);

   SELECT name AS oldest_device FROM devices ORDER BY created_at ASC LIMIT 1;

9. UPDATE parts SET device_id = 1 WHERE part_number = 166 OR part_number = 37;

    + Further explorations: UPDATE parts SET device_id = 2 WHERE part_number = (SELECT MIN(part_number) FROM parts);
  
10. DELETE FROM parts where device_id = 1;
    DELETE FROM devices WHERE name = 'Accelerometer';

    + Further explorations: You would have to add ON DELETE CASCADE to the foreign key in parts table and then use this DELETE FROM devices WHERE name = 'Accelerometer'; to delete all related data in one command. 
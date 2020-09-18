1. 

CREATE TABLE stars (
  id serial PRIMARY KEY,
  name varchar(25) UNIQUE NOT NULL, 
  distance integer NOT NULL CHECK (distance > 0), 
  spectral_type char(1),
  companions integer NOT NULL CHECK (companions >= 0)
);

CREATE TABLE planets (
  id serial PRIMARY KEY, 
  designation char(1),
  mass integer
);

2. ALTER TABLE planets ADD COLUMN star_id integer NOT NULL REFERENCES stars (id);

3. ALTER TABLE stars ALTER COLUMN name TYPE varchar(50);

4. ALTER TABLE stars ALTER COLUMN distance TYPE numeric;

  + Further exploration: it causes no errors because it doesn't violate any of the constraints. 

5. ALTER TABLE stars ADD CHECK (spectral_type IN ('O', 'B', 'A', 'F', 'G', 'K', 'M')), 
   ALTER COLUMN spectral_type SET NOT NULL;

    + Further exploration: If you add data into the column that's missing the spectral_type and then try to add the NOT NULL constraint, then it'll throw an error saying that column contains null values. If the spectral_type contains any values that are not in the CHECK constrainst you want to add, you'll get an error saying that this constraint is violated by some row. 

      - To fix this, you have to either remove the data throwing the errors or update the rows throwing the errors to contain the correct information. 

6. CREATE TYPE spectral_type_enum AS ENUM ('O', 'B', 'A', 'F', 'G', 'K', 'M');
  ALTER TABLE stars ALTER COLUMN spectral_type TYPE spectral_type_enum USING spectral_type::spectral_type_enum;

7. ALTER TABLE planets ALTER COLUMN mass TYPE numeric, 
   ALTER COLUMN mass SET NOT NULL, 
   ADD CHECK (mass > 0.0), 
   ALTER COLUMN designation SET NOT NULL;

8. ALTER TABLE planets ADD COLUMN semi_major_axis numeric NOT NULL;

  + Further explorations: it will prevent you from adding the column because the exists rows will have "null" for that column which violates the new column's constraint. 

    - You will need to add the column without the NOT NULL constraint. Then update the existing rows to include values for the new column. And then add the NOT NULL constraint to the new column. 

9. CREATE TABLE moons (
  id serial PRIMARY KEY, 
  designation integer NOT NULL CHECK (designation > 0), 
  semi_major_axis numeric CHECK (semi_major_axis > 0.0), 
  mass numeric CHECK (mass > 0.0), 
  planet_id integer NOT NULL REFERENCES planets(id)
);

10. dropdb extrasolar;
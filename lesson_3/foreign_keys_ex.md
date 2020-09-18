2. ALTER TABLE orders ADD CONSTRAINT orders_product_id_fkey FOREIGN KEY(product_id) REFERENCES products(id);

3. INSERT INTO products(name) 
    VALUES ('small bolt'), 
    ('large bolt');

    INSERT INTO orders(product_id, quantity)
    VALUES (1, 10),
    (1, 25),
    (2, 15);

4. SELECT quantity, name 
   FROM orders
   INNER JOIN products
   ON products.id = orders.product_id;

5. INSERT INTO orders (quantity) VALUES (5);

6. ALTER TABLE orders ALTER COLUMN product_id SET NOT NULL;

7. DELETE FROM orders WHERE id = 5;

8. CREATE TABLE reviews (
  id serial PRIMARY KEY, 
  body text NOT NULL,
  product_id integer REFERENCES products(id)
);

9. INSERT INTO reviews (product_id, body) VALUES (1, 'a little small');
  INSERT INTO reviews (product_id, body) VALUES (1, 'very round!');
  INSERT INTO reviews (product_id, body) VALUES (2, 'could have been smaller');

10. False. 
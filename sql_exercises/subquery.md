1. 

  CREATE TABLE bidders (
    id serial PRIMARY KEY, 
    name text NOT NULL
  );

  CREATE TABLE items (
    id serial PRIMARY KEY, 
    name text NOT NULL, 
    initial_price decimal(6, 2) NOT NULL CHECK (initial_price BETWEEN 0.01 AND 1000.00),
    sales_price decimal(6, 2) CHECK (initial_price BETWEEN 0.01 AND 1000.00)
  );

  CREATE TABLE bids (
    id serial PRIMARY KEY, 
    bidder_id integer NOT NULL REFERENCES bidders(id) ON DELETE CASCADE,
    item_id integer NOT NULL REFERENCES items(id) ON DELETE CASCADE, 
    amount decimal(6, 2) NOT NULL CHECK (amount BETWEEN 0.01 AND 1000.00)
  );

  CREATE INDEX ON bids (bidder_id, item_id);

  \copy bidders FROM 'bidders.csv' WITH HEADER CSV;
  \copy items FROM 'items.csv' WITH HEADER CSV;
  \copy bids FROM 'bids.csv' WITH HEADER CSV;

2. 

  SELECT name AS "Bid On Items" FROM items 
  WHERE items.id IN (SELECT DISTINCT item_id FROM bids);

3. 

    SELECT name AS "Not Bid On" FROM items 
    WHERE items.id NOT IN (SELECT DISTINCT item_id FROM bids);

4. 

  SELECT name FROM bidders 
  WHERE EXISTS (SELECT 1 FROM bids WHERE bidders.id = bids.bidder_id);

  + Further explorations
  
    SELECT DISTINCT bidders.name, bidders.id FROM bidders
    INNER JOIN bids ON bidders.id = bids.bidder_id
    ORDER BY bidders.id;

5. 

  SELECT MAX(result.count) FROM (SELECT COUNT(bidder_id) FROM bids GROUP BY bidder_id) AS result;

6. 
  
      SELECT name, (SELECT COUNT(item_id) FROM bids WHERE item_id = items.id)
      FROM items;

      + Further exploration:

        SELECT name, COUNT(item_id) FROM items
        LEFT OUTER JOIN bids ON item_id = items.id
        GROUP BY name;
7. 

     SELECT id FROM items WHERE ROW('Painting', 100.00, 250.00) = ROW(name, initial_price, sales_price);

8. 

    EXPLAIN SELECT name FROM bidders
    WHERE EXISTS (SELECT 1 FROM bids WHERE bids.bidder_id = bidders.id);

    + Output:

                                      QUERY PLAN                                
        --------------------------------------------------------------------------
        Hash Join  (cost=33.38..66.47 rows=635 width=32)
          Hash Cond: (bidders.id = bids.bidder_id)
          ->  Seq Scan on bidders  (cost=0.00..22.70 rows=1270 width=36)
          ->  Hash  (cost=30.88..30.88 rows=200 width=4)
                ->  HashAggregate  (cost=28.88..30.88 rows=200 width=4)
                      Group Key: bids.bidder_id
                      ->  Seq Scan on bids  (cost=0.00..25.10 rows=1510 width=4)
        (7 rows)

        - Total cost is 66.47

      
     EXPLAIN ANALYZE SELECT name FROM bidders
    WHERE EXISTS (SELECT 1 FROM bids WHERE bids.bidder_id = bidders.id);

    + Output

                                                                QUERY PLAN                                                      
    ---------------------------------------------------------------------------------------------------------------------
    Hash Join  (cost=33.38..66.47 rows=635 width=32) (actual time=0.033..0.036 rows=6 loops=1)
      Hash Cond: (bidders.id = bids.bidder_id)
      ->  Seq Scan on bidders  (cost=0.00..22.70 rows=1270 width=36) (actual time=0.006..0.007 rows=7 loops=1)
      ->  Hash  (cost=30.88..30.88 rows=200 width=4) (actual time=0.018..0.018 rows=6 loops=1)
            Buckets: 1024  Batches: 1  Memory Usage: 9kB
            ->  HashAggregate  (cost=28.88..30.88 rows=200 width=4) (actual time=0.014..0.016 rows=6 loops=1)
                  Group Key: bids.bidder_id
                  ->  Seq Scan on bids  (cost=0.00..25.10 rows=1510 width=4) (actual time=0.003..0.006 rows=26 loops=1)
    Planning Time: 0.089 ms
    Execution Time: 0.063 ms

    (10 rows)

    - Total cost is 66.47

9. 

    EXPLAIN ANALYZE SELECT MAX(bid_counts.count) FROM
    (SELECT COUNT(bidder_id) FROM bids GROUP BY bidder_id) AS bid_counts;

      + Output:

                                                          QUERY PLAN                                                   
          ---------------------------------------------------------------------------------------------------------------
          Aggregate  (cost=37.15..37.16 rows=1 width=8) (actual time=0.036..0.037 rows=1 loops=1)
            ->  HashAggregate  (cost=32.65..34.65 rows=200 width=12) (actual time=0.030..0.033 rows=6 loops=1)
                  Group Key: bids.bidder_id
                  ->  Seq Scan on bids  (cost=0.00..25.10 rows=1510 width=4) (actual time=0.009..0.013 rows=26 loops=1)
          Planning Time: 0.088 ms
          Execution Time: 0.094 ms
          (6 rows)


    EXPLAIN ANALYZE SELECT COUNT(bidder_id) AS max_bid FROM bids
    GROUP BY bidder_id
    ORDER BY max_bid DESC
    LIMIT 1;


    + Output:

                                                           QUERY PLAN                                                      
        ---------------------------------------------------------------------------------------------------------------------
        Limit  (cost=35.65..35.65 rows=1 width=12) (actual time=0.034..0.034 rows=1 loops=1)
          ->  Sort  (cost=35.65..36.15 rows=200 width=12) (actual time=0.033..0.033 rows=1 loops=1)
                Sort Key: (count(bidder_id)) DESC
                Sort Method: top-N heapsort  Memory: 25kB
                ->  HashAggregate  (cost=32.65..34.65 rows=200 width=12) (actual time=0.018..0.019 rows=6 loops=1)
                      Group Key: bidder_id
                      ->  Seq Scan on bids  (cost=0.00..25.10 rows=1510 width=4) (actual time=0.004..0.007 rows=26 loops=1)
        Planning Time: 0.070 ms
        Execution Time: 0.054 ms
        (9 rows)

    + The second statement is more efficient because the execution time is much faster. 


  + Further explorations

    EXPLAIN ANALYZE SELECT name,
    (SELECT COUNT(item_id) FROM bids WHERE item_id = items.id)
    FROM items;

    + Output:

                                                       QUERY PLAN                                                  
        -------------------------------------------------------------------------------------------------------------
        Seq Scan on items  (cost=0.00..25455.20 rows=880 width=40) (actual time=0.021..0.052 rows=6 loops=1)
          SubPlan 1
            ->  Aggregate  (cost=28.89..28.91 rows=1 width=8) (actual time=0.006..0.006 rows=1 loops=6)
                  ->  Seq Scan on bids  (cost=0.00..28.88 rows=8 width=4) (actual time=0.002..0.004 rows=4 loops=6)
                        Filter: (item_id = items.id)
                        Rows Removed by Filter: 22
        Planning Time: 0.085 ms
        Execution Time: 0.084 ms
        (8 rows)


    EXPLAIN ANALYZE SELECT name, COUNT(item_id) FROM items
    LEFT OUTER JOIN bids ON item_id = items.id
    GROUP BY name;

    + Output:

                                                             QUERY PLAN                                                      
          ---------------------------------------------------------------------------------------------------------------------
          HashAggregate  (cost=66.44..68.44 rows=200 width=40) (actual time=0.064..0.067 rows=6 loops=1)
            Group Key: items.name
            ->  Hash Right Join  (cost=29.80..58.89 rows=1510 width=36) (actual time=0.037..0.052 rows=27 loops=1)
                  Hash Cond: (bids.item_id = items.id)
                  ->  Seq Scan on bids  (cost=0.00..25.10 rows=1510 width=4) (actual time=0.003..0.006 rows=26 loops=1)
                  ->  Hash  (cost=18.80..18.80 rows=880 width=36) (actual time=0.019..0.019 rows=6 loops=1)
                        Buckets: 1024  Batches: 1  Memory Usage: 9kB
                        ->  Seq Scan on items  (cost=0.00..18.80 rows=880 width=36) (actual time=0.012..0.014 rows=6 loops=1)
          Planning Time: 0.132 ms
          Execution Time: 0.145 ms
          (10 rows)

      + The first statement with the subquery is much faster the the join table query. 
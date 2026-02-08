# PostgreSQL Practice Questions - 20 Comprehensive SQL Challenges

This document contains 20 PostgreSQL practice questions using the **DVD Rental Sample Database**, which is available on most PostgreSQL online practice platforms like:
- **PostgreSQL Tutorial** (https://www.postgresqltutorial.com/postgresql-getting-started/postgresql-sample-database/)
- **SQL Fiddle** (http://sqlfiddle.com/)
- **DB Fiddle** (https://www.db-fiddle.com/)

The DVD Rental database simulates a movie rental store with customers, films, rentals, payments, and inventory.

---

## Database Schema Overview

The DVD Rental database contains the following main tables:

- **actor** - Actor information
- **film** - Film information (title, description, release year, rental rate, etc.)
- **film_actor** - Many-to-many relationship between films and actors
- **category** - Film categories (e.g., Action, Comedy, Drama)
- **film_category** - Many-to-many relationship between films and categories
- **customer** - Customer information
- **rental** - Rental transactions
- **payment** - Payment transactions
- **inventory** - Available film copies
- **store** - Store information
- **staff** - Staff/employee information
- **address**, **city**, **country** - Location information

---

## Question 1: Basic SELECT and WHERE

**Topic:** Basic Queries, WHERE Clause

**Question:** Retrieve all films with a rental rate greater than $2.99 and a rating of 'PG' or 'PG-13'. Display the title, rental_rate, and rating, ordered by rental_rate in descending order.

**Solution:**
```sql
SELECT 
    title,
    rental_rate,
    rating
FROM film
WHERE 
    rental_rate > 2.99
    AND rating IN ('PG', 'PG-13')
ORDER BY rental_rate DESC;
```

**Explanation:** This demonstrates basic filtering with multiple conditions using WHERE, IN operator, and sorting with ORDER BY.

**Sample Output:**
```
title                  | rental_rate | rating
-----------------------+-------------+--------
CHITTY LOCK           | 4.99        | PG
MARRIED GO            | 4.99        | PG-13
HEAVENLY GUN          | 4.99        | PG
...
```

---

## Question 2: JOINS - Inner Join

**Topic:** INNER JOIN, Multiple Tables

**Question:** Find all films in the 'Action' category. Display the film title, category name, and release year. Sort by title.

**Solution:**
```sql
SELECT 
    f.title,
    c.name AS category_name,
    f.release_year
FROM film f
INNER JOIN film_category fc ON f.film_id = fc.film_id
INNER JOIN category c ON fc.category_id = c.category_id
WHERE c.name = 'Action'
ORDER BY f.title;
```

**Explanation:** This uses INNER JOIN to combine three tables. The film_category table serves as a bridge between film and category (many-to-many relationship).

**Sample Output:**
```
title                  | category_name | release_year
-----------------------+---------------+-------------
ACADEMY DINOSAUR       | Action        | 2006
BULL SHAWSHANK         | Action        | 2006
CLOCKWORK PARADISE     | Action        | 2006
...
```

---

## Question 3: Aggregate Functions - COUNT, GROUP BY

**Topic:** Aggregate Functions, GROUP BY

**Question:** Count how many films are in each category. Display the category name and film count, ordered by count in descending order.

**Solution:**
```sql
SELECT 
    c.name AS category_name,
    COUNT(fc.film_id) AS film_count
FROM category c
LEFT JOIN film_category fc ON c.category_id = fc.category_id
GROUP BY c.category_id, c.name
ORDER BY film_count DESC;
```

**Explanation:** GROUP BY groups records by category, and COUNT calculates the number of films in each category. LEFT JOIN ensures categories with no films are included.

**Sample Output:**
```
category_name | film_count
--------------+-----------
Sports        | 74
Foreign       | 73
Family        | 69
Documentary   | 68
...
```

---

## Question 4: Subqueries - IN Clause

**Topic:** Subqueries, IN Operator

**Question:** Find all customers who have rented films with 'DINOSAUR' in the title. Display customer first name, last name, and email.

**Solution:**
```sql
SELECT DISTINCT
    c.first_name,
    c.last_name,
    c.email
FROM customer c
WHERE c.customer_id IN (
    SELECT r.customer_id
    FROM rental r
    INNER JOIN inventory i ON r.inventory_id = i.inventory_id
    INNER JOIN film f ON i.film_id = f.film_id
    WHERE f.title LIKE '%DINOSAUR%'
)
ORDER BY c.last_name, c.first_name;
```

**Alternative using JOIN:**
```sql
SELECT DISTINCT
    c.first_name,
    c.last_name,
    c.email
FROM customer c
INNER JOIN rental r ON c.customer_id = r.customer_id
INNER JOIN inventory i ON r.inventory_id = i.inventory_id
INNER JOIN film f ON i.film_id = f.film_id
WHERE f.title LIKE '%DINOSAUR%'
ORDER BY c.last_name, c.first_name;
```

**Explanation:** The subquery finds customer IDs who rented films with 'DINOSAUR' in the title. The main query retrieves customer details. DISTINCT removes duplicates.

**Sample Output:**
```
first_name | last_name | email
-----------+-----------+------------------------
MARY       | SMITH     | mary.smith@example.com
PATRICIA   | JOHNSON   | patricia.johnson@...
LINDA      | WILLIAMS  | linda.williams@...
...
```

---

## Question 5: Aggregate Functions - SUM, AVG, MAX, MIN

**Topic:** Multiple Aggregate Functions

**Question:** Calculate payment statistics: total payments, average payment, maximum payment, and minimum payment for each customer. Show only customers with total payments over $100.

**Solution:**
```sql
SELECT 
    c.customer_id,
    c.first_name || ' ' || c.last_name AS customer_name,
    COUNT(p.payment_id) AS payment_count,
    SUM(p.amount) AS total_paid,
    ROUND(AVG(p.amount), 2) AS avg_payment,
    MAX(p.amount) AS max_payment,
    MIN(p.amount) AS min_payment
FROM customer c
INNER JOIN payment p ON c.customer_id = p.customer_id
GROUP BY c.customer_id, c.first_name, c.last_name
HAVING SUM(p.amount) > 100
ORDER BY total_paid DESC;
```

**Explanation:** This demonstrates multiple aggregate functions (COUNT, SUM, AVG, MAX, MIN), string concatenation (||), ROUND function, HAVING clause for filtering grouped results, and ROUND for decimal formatting.

**Sample Output:**
```
customer_id | customer_name    | payment_count | total_paid | avg_payment | max_payment | min_payment
------------+------------------+---------------+------------+-------------+-------------+------------
526         | KARL SEAL        | 45            | 221.55     | 4.92        | 10.99       | 0.99
148         | ELEANOR HUNT     | 46            | 216.54     | 4.71        | 10.99       | 0.99
...
```

---

## Question 6: LEFT JOIN and NULL Handling

**Topic:** LEFT JOIN, NULL Values, COALESCE

**Question:** List all films and the number of times each has been rented. Include films that have never been rented. Sort by rental count descending.

**Solution:**
```sql
SELECT 
    f.film_id,
    f.title,
    COALESCE(COUNT(r.rental_id), 0) AS rental_count
FROM film f
LEFT JOIN inventory i ON f.film_id = i.film_id
LEFT JOIN rental r ON i.inventory_id = r.inventory_id
GROUP BY f.film_id, f.title
ORDER BY rental_count DESC, f.title;
```

**Explanation:** LEFT JOIN ensures all films are included even if never rented. COALESCE returns 0 if the count is NULL. The rental count is calculated by grouping.

**Sample Output:**
```
film_id | title                | rental_count
--------+----------------------+-------------
103     | BUCKET BROTHERHOOD   | 34
738     | ROCKETEER MOTHER     | 33
382     | GRIT CLOCKWORK       | 32
42      | ARTIST COLDBLOODED   | 0
...
```

---

## Question 7: Date Functions and Filtering

**Topic:** Date Functions, DATE_TRUNC, EXTRACT

**Question:** Find all rentals made in June 2005. Display customer name, rental date, and return date. Calculate the rental duration in days.

**Solution:**
```sql
SELECT 
    c.first_name || ' ' || c.last_name AS customer_name,
    r.rental_date,
    r.return_date,
    DATE_PART('day', r.return_date - r.rental_date) AS rental_duration_days,
    f.title AS film_title
FROM rental r
INNER JOIN customer c ON r.customer_id = c.customer_id
INNER JOIN inventory i ON r.inventory_id = i.inventory_id
INNER JOIN film f ON i.film_id = f.film_id
WHERE 
    EXTRACT(YEAR FROM r.rental_date) = 2005
    AND EXTRACT(MONTH FROM r.rental_date) = 6
ORDER BY r.rental_date;
```

**Alternative using DATE_TRUNC:**
```sql
SELECT 
    c.first_name || ' ' || c.last_name AS customer_name,
    r.rental_date,
    r.return_date,
    DATE_PART('day', r.return_date - r.rental_date) AS rental_duration_days,
    f.title AS film_title
FROM rental r
INNER JOIN customer c ON r.customer_id = c.customer_id
INNER JOIN inventory i ON r.inventory_id = i.inventory_id
INNER JOIN film f ON i.film_id = f.film_id
WHERE DATE_TRUNC('month', r.rental_date) = '2005-06-01'
ORDER BY r.rental_date;
```

**Explanation:** EXTRACT gets specific parts of a date (year, month). DATE_PART calculates the difference between dates in days. DATE_TRUNC truncates date to specified precision.

**Sample Output:**
```
customer_name    | rental_date              | return_date              | rental_duration_days | film_title
-----------------+--------------------------+--------------------------+---------------------+------------------
MARY SMITH       | 2005-06-14 22:53:33      | 2005-06-19 21:00:33      | 4                   | ACADEMY DINOSAUR
PATRICIA JOHNSON | 2005-06-14 23:16:26      | 2005-06-18 00:41:26      | 3                   | ACE GOLDFINGER
...
```

---

## Question 8: Window Functions - ROW_NUMBER, RANK

**Topic:** Window Functions, PARTITION BY

**Question:** Rank films by rental rate within each rating category. Show title, rating, rental_rate, and rank.

**Solution:**
```sql
SELECT 
    title,
    rating,
    rental_rate,
    ROW_NUMBER() OVER (PARTITION BY rating ORDER BY rental_rate DESC) AS row_num,
    RANK() OVER (PARTITION BY rating ORDER BY rental_rate DESC) AS rank,
    DENSE_RANK() OVER (PARTITION BY rating ORDER BY rental_rate DESC) AS dense_rank
FROM film
ORDER BY rating, rental_rate DESC;
```

**Explanation:** Window functions allow calculations across rows related to the current row. PARTITION BY divides rows into groups. ROW_NUMBER assigns unique numbers, RANK allows gaps for ties, DENSE_RANK has no gaps.

**Sample Output:**
```
title              | rating | rental_rate | row_num | rank | dense_rank
-------------------+--------+-------------+---------+------+-----------
ALONE TRIP         | G      | 4.99        | 1       | 1    | 1
BEAST HUNCHBACK    | G      | 4.99        | 2       | 1    | 1
CHANCE RESURRECTION| G      | 4.99        | 3       | 1    | 1
CYCLONE FAMILY     | G      | 2.99        | 4       | 4    | 2
...
```

---

## Question 9: Common Table Expressions (CTE)

**Topic:** WITH Clause, CTE

**Question:** Using a CTE, find the top 5 customers by total rental spending, then list all their rentals.

**Solution:**
```sql
WITH top_customers AS (
    SELECT 
        c.customer_id,
        c.first_name || ' ' || c.last_name AS customer_name,
        SUM(p.amount) AS total_spent
    FROM customer c
    INNER JOIN payment p ON c.customer_id = p.customer_id
    GROUP BY c.customer_id, c.first_name, c.last_name
    ORDER BY total_spent DESC
    LIMIT 5
)
SELECT 
    tc.customer_name,
    tc.total_spent,
    f.title,
    r.rental_date,
    p.amount
FROM top_customers tc
INNER JOIN rental r ON tc.customer_id = r.customer_id
INNER JOIN payment p ON r.rental_id = p.rental_id
INNER JOIN inventory i ON r.inventory_id = i.inventory_id
INNER JOIN film f ON i.film_id = f.film_id
ORDER BY tc.total_spent DESC, r.rental_date;
```

**Explanation:** CTE (WITH clause) creates a temporary named result set that can be referenced in the main query. This improves readability and allows for complex queries to be broken down.

**Sample Output:**
```
customer_name | total_spent | title              | rental_date              | amount
--------------+-------------+--------------------+--------------------------+--------
KARL SEAL     | 221.55      | ACADEMY DINOSAUR   | 2005-05-25 11:30:37      | 2.99
KARL SEAL     | 221.55      | ACE GOLDFINGER     | 2005-05-28 10:35:23      | 4.99
ELEANOR HUNT  | 216.54      | ADAPTATION HOLES   | 2005-05-26 07:30:37      | 2.99
...
```

---

## Question 10: CASE Statements

**Topic:** CASE WHEN, Conditional Logic

**Question:** Categorize films by rental rate into 'Budget' (< $1), 'Standard' ($1-$2.99), 'Premium' ($3-$3.99), and 'Luxury' (>= $4). Count films in each category.

**Solution:**
```sql
SELECT 
    CASE 
        WHEN rental_rate < 1.00 THEN 'Budget'
        WHEN rental_rate >= 1.00 AND rental_rate < 3.00 THEN 'Standard'
        WHEN rental_rate >= 3.00 AND rental_rate < 4.00 THEN 'Premium'
        ELSE 'Luxury'
    END AS price_category,
    COUNT(*) AS film_count,
    ROUND(AVG(rental_rate), 2) AS avg_rental_rate,
    MIN(rental_rate) AS min_rate,
    MAX(rental_rate) AS max_rate
FROM film
GROUP BY price_category
ORDER BY 
    CASE price_category
        WHEN 'Budget' THEN 1
        WHEN 'Standard' THEN 2
        WHEN 'Premium' THEN 3
        WHEN 'Luxury' THEN 4
    END;
```

**Explanation:** CASE statements provide conditional logic similar to if/else. Can be used in SELECT, WHERE, and ORDER BY clauses.

**Sample Output:**
```
price_category | film_count | avg_rental_rate | min_rate | max_rate
---------------+------------+-----------------+----------+---------
Budget         | 0          | NULL            | NULL     | NULL
Standard       | 658        | 2.98            | 0.99     | 2.99
Premium        | 0          | NULL            | NULL     | NULL
Luxury         | 342        | 4.99            | 4.99     | 4.99
```

---

## Question 11: String Functions

**Topic:** String Manipulation, CONCAT, UPPER, LOWER, LENGTH, SUBSTRING

**Question:** Display customer information with formatted names and email validation. Show first name in uppercase, last name in lowercase, full name, email length, and email domain.

**Solution:**
```sql
SELECT 
    customer_id,
    UPPER(first_name) AS first_name_upper,
    LOWER(last_name) AS last_name_lower,
    CONCAT(first_name, ' ', last_name) AS full_name,
    email,
    LENGTH(email) AS email_length,
    SUBSTRING(email FROM POSITION('@' IN email) + 1) AS email_domain,
    SPLIT_PART(email, '@', 2) AS domain_alternative,
    LEFT(first_name, 1) || LEFT(last_name, 1) AS initials,
    REVERSE(last_name) AS reversed_last_name
FROM customer
WHERE email LIKE '%@sakilacustomer.org'
ORDER BY customer_id
LIMIT 10;
```

**Explanation:** PostgreSQL provides rich string functions: UPPER/LOWER for case conversion, CONCAT for concatenation, LENGTH for string length, SUBSTRING and SPLIT_PART for extraction, LEFT for prefix, REVERSE for reversing strings.

**Sample Output:**
```
customer_id | first_name_upper | last_name_lower | full_name      | email                    | email_length | email_domain
------------+------------------+-----------------+----------------+--------------------------+--------------+-------------------
1           | MARY             | smith           | MARY SMITH     | mary.smith@sakilacust... | 35           | sakilacustomer.org
2           | PATRICIA         | johnson         | PATRICIA JOHN..| patricia.johnson@saki... | 39           | sakilacustomer.org
...
```

---

## Question 12: Self Join

**Topic:** Self Join

**Question:** Find pairs of films that have the same rental rate and length. Display both film titles and their common attributes.

**Solution:**
```sql
SELECT DISTINCT
    f1.title AS film1,
    f2.title AS film2,
    f1.rental_rate,
    f1.length AS duration_minutes,
    f1.rating
FROM film f1
INNER JOIN film f2 ON 
    f1.rental_rate = f2.rental_rate 
    AND f1.length = f2.length
    AND f1.rating = f2.rating
    AND f1.film_id < f2.film_id  -- Avoid duplicates and self-matching
ORDER BY f1.rental_rate DESC, f1.length
LIMIT 20;
```

**Explanation:** Self join joins a table to itself. The condition `f1.film_id < f2.film_id` prevents duplicate pairs (A,B) and (B,A) and self-pairing (A,A).

**Sample Output:**
```
film1                | film2               | rental_rate | duration_minutes | rating
---------------------+---------------------+-------------+-----------------+--------
ALABAMA DEVIL        | BEAST HUNCHBACK     | 4.99        | 114             | PG
ALABAMA DEVIL        | BEAR GRACELAND      | 4.99        | 114             | PG
AFRICAN EGG          | CATCH AMISTAD       | 4.99        | 130             | G
...
```

---

## Question 13: UNION and UNION ALL

**Topic:** UNION, UNION ALL, Set Operations

**Question:** Create a report showing all film titles from 'Action' category and all film titles from 'Comedy' category with category labels.

**Solution:**
```sql
-- With UNION (removes duplicates)
SELECT 
    f.title,
    'Action' AS category,
    f.release_year
FROM film f
INNER JOIN film_category fc ON f.film_id = fc.film_id
INNER JOIN category c ON fc.category_id = c.category_id
WHERE c.name = 'Action'

UNION

SELECT 
    f.title,
    'Comedy' AS category,
    f.release_year
FROM film f
INNER JOIN film_category fc ON f.film_id = fc.film_id
INNER JOIN category c ON fc.category_id = c.category_id
WHERE c.name = 'Comedy'

ORDER BY title;
```

**With UNION ALL (keeps duplicates):**
```sql
SELECT 
    f.title,
    c.name AS category,
    f.release_year
FROM film f
INNER JOIN film_category fc ON f.film_id = fc.film_id
INNER JOIN category c ON fc.category_id = c.category_id
WHERE c.name IN ('Action', 'Comedy')
ORDER BY title, category;
```

**Explanation:** UNION combines results from multiple queries, removing duplicates. UNION ALL keeps all records including duplicates. Both queries must have the same number of columns with compatible types.

**Sample Output:**
```
title                | category | release_year
---------------------+----------+-------------
ACADEMY DINOSAUR     | Action   | 2006
ACE GOLDFINGER       | Comedy   | 2006
ADAPTATION HOLES     | Action   | 2006
...
```

---

## Question 14: EXISTS and NOT EXISTS

**Topic:** EXISTS, Correlated Subqueries

**Question:** Find customers who have rented films but never rented any film from the 'Horror' category.

**Solution:**
```sql
SELECT 
    c.customer_id,
    c.first_name || ' ' || c.last_name AS customer_name,
    c.email
FROM customer c
WHERE EXISTS (
    -- Customer has rentals
    SELECT 1 
    FROM rental r 
    WHERE r.customer_id = c.customer_id
)
AND NOT EXISTS (
    -- But no Horror rentals
    SELECT 1
    FROM rental r
    INNER JOIN inventory i ON r.inventory_id = i.inventory_id
    INNER JOIN film f ON i.film_id = f.film_id
    INNER JOIN film_category fc ON f.film_id = fc.film_id
    INNER JOIN category cat ON fc.category_id = cat.category_id
    WHERE r.customer_id = c.customer_id
    AND cat.name = 'Horror'
)
ORDER BY customer_name
LIMIT 10;
```

**Explanation:** EXISTS checks if a subquery returns any rows. NOT EXISTS checks if a subquery returns no rows. These are correlated subqueries that reference the outer query.

**Sample Output:**
```
customer_id | customer_name      | email
------------+--------------------+---------------------------
3           | LINDA WILLIAMS     | linda.williams@sakilacu...
7           | MARIA MILLER       | maria.miller@sakilacust...
...
```

---

## Question 15: Advanced Aggregation - GROUPING SETS, ROLLUP, CUBE

**Topic:** GROUPING SETS, ROLLUP, CUBE

**Question:** Generate a multi-level summary of payments by customer and payment date with subtotals and grand totals.

**Solution:**
```sql
-- Using ROLLUP for hierarchical totals
SELECT 
    DATE(payment_date) AS payment_day,
    customer_id,
    COUNT(*) AS payment_count,
    SUM(amount) AS total_amount,
    ROUND(AVG(amount), 2) AS avg_amount
FROM payment
WHERE payment_date >= '2007-02-01' AND payment_date < '2007-03-01'
GROUP BY ROLLUP(DATE(payment_date), customer_id)
ORDER BY payment_day NULLS LAST, customer_id NULLS LAST
LIMIT 20;
```

**Using CUBE for all combinations:**
```sql
SELECT 
    DATE(payment_date) AS payment_day,
    EXTRACT(HOUR FROM payment_date) AS hour_of_day,
    COUNT(*) AS payment_count,
    SUM(amount) AS total_amount
FROM payment
WHERE payment_date >= '2007-02-14' AND payment_date < '2007-02-15'
GROUP BY CUBE(DATE(payment_date), EXTRACT(HOUR FROM payment_date))
ORDER BY payment_day NULLS LAST, hour_of_day NULLS LAST;
```

**Using GROUPING SETS for specific combinations:**
```sql
SELECT 
    c.name AS category,
    f.rating,
    COUNT(*) AS film_count,
    ROUND(AVG(f.rental_rate), 2) AS avg_rental_rate
FROM film f
INNER JOIN film_category fc ON f.film_id = fc.film_id
INNER JOIN category c ON fc.category_id = c.category_id
GROUP BY GROUPING SETS (
    (c.name, f.rating),  -- By category and rating
    (c.name),            -- By category only
    (f.rating),          -- By rating only
    ()                   -- Grand total
)
ORDER BY category NULLS LAST, rating NULLS LAST;
```

**Explanation:** ROLLUP creates hierarchical subtotals from right to left. CUBE creates subtotals for all combinations. GROUPING SETS allows specifying exact grouping combinations. NULL values represent subtotal/total rows.

**Sample Output (ROLLUP):**
```
payment_day | customer_id | payment_count | total_amount | avg_amount
------------+-------------+---------------+--------------+-----------
2007-02-14  | 1           | 2             | 5.98         | 2.99
2007-02-14  | 2           | 1             | 4.99         | 4.99
2007-02-14  | NULL        | 3             | 10.97        | 3.66      -- Subtotal for day
NULL        | NULL        | 573           | 2809.27      | 4.91      -- Grand total
...
```

---

## Question 16: Recursive CTE

**Topic:** Recursive Common Table Expressions

**Question:** Create a number series from 1 to 10 and use it to show rental statistics for each day of the first 10 days of rentals.

**Solution:**
```sql
-- Generate number series
WITH RECURSIVE number_series AS (
    SELECT 1 AS n
    UNION ALL
    SELECT n + 1
    FROM number_series
    WHERE n < 10
),
first_rental_date AS (
    SELECT MIN(DATE(rental_date)) AS start_date
    FROM rental
)
SELECT 
    ns.n AS day_number,
    frd.start_date + (ns.n - 1) AS rental_date,
    COUNT(r.rental_id) AS rental_count,
    COALESCE(SUM(p.amount), 0) AS daily_revenue
FROM number_series ns
CROSS JOIN first_rental_date frd
LEFT JOIN rental r ON DATE(r.rental_date) = frd.start_date + (ns.n - 1)
LEFT JOIN payment p ON r.rental_id = p.rental_id
GROUP BY ns.n, frd.start_date
ORDER BY ns.n;
```

**Explanation:** Recursive CTEs call themselves to generate sequences or traverse hierarchies. The structure has a base case (SELECT 1) and recursive case (SELECT n + 1) with a termination condition (WHERE n < 10).

**Sample Output:**
```
day_number | rental_date | rental_count | daily_revenue
-----------+-------------+--------------+--------------
1          | 2005-05-24  | 8            | 35.92
2          | 2005-05-25  | 137          | 634.63
3          | 2005-05-26  | 174          | 800.26
...
```

---

## Question 17: Window Functions - LAG, LEAD, FIRST_VALUE, LAST_VALUE

**Topic:** Advanced Window Functions

**Question:** For each customer's payments, show the current payment, previous payment, next payment, and the difference from the previous payment.

**Solution:**
```sql
SELECT 
    customer_id,
    payment_id,
    payment_date,
    amount,
    LAG(amount) OVER (PARTITION BY customer_id ORDER BY payment_date) AS previous_payment,
    LEAD(amount) OVER (PARTITION BY customer_id ORDER BY payment_date) AS next_payment,
    amount - LAG(amount) OVER (PARTITION BY customer_id ORDER BY payment_date) AS diff_from_previous,
    FIRST_VALUE(amount) OVER (PARTITION BY customer_id ORDER BY payment_date) AS first_payment,
    LAST_VALUE(amount) OVER (
        PARTITION BY customer_id 
        ORDER BY payment_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) AS last_payment,
    SUM(amount) OVER (
        PARTITION BY customer_id 
        ORDER BY payment_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_total
FROM payment
WHERE customer_id IN (1, 2)
ORDER BY customer_id, payment_date
LIMIT 20;
```

**Explanation:** LAG accesses previous row, LEAD accesses next row. FIRST_VALUE gets first value in window, LAST_VALUE gets last value. The ROWS clause defines the window frame.

**Sample Output:**
```
customer_id | payment_id | payment_date         | amount | previous_payment | next_payment | diff_from_previous | first_payment | last_payment | running_total
------------+------------+----------------------+--------+------------------+--------------+-------------------+--------------+-------------+--------------
1           | 17503      | 2007-02-15 00:54:12  | 2.99   | NULL             | 0.99         | NULL              | 2.99         | 8.99        | 2.99
1           | 17504      | 2007-02-15 16:31:19  | 0.99   | 2.99             | 5.99         | -2.00             | 2.99         | 8.99        | 3.98
...
```

---

## Question 18: Pivoting Data with CASE and Aggregation

**Topic:** Pivot Tables, Conditional Aggregation

**Question:** Create a pivot table showing the count of films by rating (rows) and category (columns) for top 5 categories.

**Solution:**
```sql
SELECT 
    f.rating,
    COUNT(CASE WHEN c.name = 'Sports' THEN 1 END) AS sports,
    COUNT(CASE WHEN c.name = 'Foreign' THEN 1 END) AS foreign,
    COUNT(CASE WHEN c.name = 'Family' THEN 1 END) AS family,
    COUNT(CASE WHEN c.name = 'Documentary' THEN 1 END) AS documentary,
    COUNT(CASE WHEN c.name = 'Animation' THEN 1 END) AS animation,
    COUNT(*) AS total
FROM film f
INNER JOIN film_category fc ON f.film_id = fc.film_id
INNER JOIN category c ON fc.category_id = c.category_id
WHERE c.name IN ('Sports', 'Foreign', 'Family', 'Documentary', 'Animation')
GROUP BY f.rating
ORDER BY f.rating;
```

**Using FILTER (PostgreSQL 9.4+):**
```sql
SELECT 
    f.rating,
    COUNT(*) FILTER (WHERE c.name = 'Sports') AS sports,
    COUNT(*) FILTER (WHERE c.name = 'Foreign') AS foreign,
    COUNT(*) FILTER (WHERE c.name = 'Family') AS family,
    COUNT(*) FILTER (WHERE c.name = 'Documentary') AS documentary,
    COUNT(*) FILTER (WHERE c.name = 'Animation') AS animation,
    COUNT(*) AS total
FROM film f
INNER JOIN film_category fc ON f.film_id = fc.film_id
INNER JOIN category c ON fc.category_id = c.category_id
WHERE c.name IN ('Sports', 'Foreign', 'Family', 'Documentary', 'Animation')
GROUP BY f.rating
ORDER BY f.rating;
```

**Explanation:** Pivot tables transform rows into columns. CASE with COUNT creates conditional counts. FILTER clause is a cleaner PostgreSQL-specific alternative.

**Sample Output:**
```
rating | sports | foreign | family | documentary | animation | total
-------+--------+---------+--------+-------------+-----------+------
G      | 13     | 12      | 14     | 15          | 13        | 67
PG     | 17     | 15      | 18     | 14          | 15        | 79
PG-13  | 16     | 18      | 13     | 12          | 17        | 76
R      | 14     | 13      | 12     | 15          | 13        | 67
NC-17  | 14     | 15      | 12     | 12          | 13        | 66
```

---

## Question 19: Complex Join with Multiple Conditions

**Topic:** Complex Joins, Multiple Conditions

**Question:** Find all actors who have appeared in films in both 'Action' and 'Comedy' categories. Show actor name and count of films in each category.

**Solution:**
```sql
WITH actor_categories AS (
    SELECT 
        a.actor_id,
        a.first_name || ' ' || a.last_name AS actor_name,
        c.name AS category,
        COUNT(DISTINCT f.film_id) AS film_count
    FROM actor a
    INNER JOIN film_actor fa ON a.actor_id = fa.actor_id
    INNER JOIN film f ON fa.film_id = f.film_id
    INNER JOIN film_category fc ON f.film_id = fc.film_id
    INNER JOIN category c ON fc.category_id = c.category_id
    WHERE c.name IN ('Action', 'Comedy')
    GROUP BY a.actor_id, a.first_name, a.last_name, c.name
)
SELECT 
    action.actor_name,
    action.film_count AS action_films,
    comedy.film_count AS comedy_films,
    action.film_count + comedy.film_count AS total_films
FROM 
    (SELECT * FROM actor_categories WHERE category = 'Action') action
INNER JOIN 
    (SELECT * FROM actor_categories WHERE category = 'Comedy') comedy
ON action.actor_id = comedy.actor_id
ORDER BY total_films DESC, action.actor_name
LIMIT 10;
```

**Alternative using HAVING:**
```sql
SELECT 
    a.actor_id,
    a.first_name || ' ' || a.last_name AS actor_name,
    COUNT(DISTINCT CASE WHEN c.name = 'Action' THEN f.film_id END) AS action_films,
    COUNT(DISTINCT CASE WHEN c.name = 'Comedy' THEN f.film_id END) AS comedy_films,
    COUNT(DISTINCT f.film_id) AS total_films
FROM actor a
INNER JOIN film_actor fa ON a.actor_id = fa.actor_id
INNER JOIN film f ON fa.film_id = f.film_id
INNER JOIN film_category fc ON f.film_id = fc.film_id
INNER JOIN category c ON fc.category_id = c.category_id
WHERE c.name IN ('Action', 'Comedy')
GROUP BY a.actor_id, a.first_name, a.last_name
HAVING 
    COUNT(DISTINCT CASE WHEN c.name = 'Action' THEN f.film_id END) > 0
    AND COUNT(DISTINCT CASE WHEN c.name = 'Comedy' THEN f.film_id END) > 0
ORDER BY total_films DESC, actor_name
LIMIT 10;
```

**Explanation:** This demonstrates complex filtering where actors must appear in both categories. The CTE approach separates the logic, while the HAVING approach is more concise.

**Sample Output:**
```
actor_name          | action_films | comedy_films | total_films
--------------------+--------------+--------------+------------
SUSAN DAVIS         | 3            | 2            | 5
KIRSTEN PALTROW     | 2            | 3            | 5
BOB FAWCETT         | 2            | 2            | 4
...
```

---

## Question 20: Performance Optimization - Indexes and EXPLAIN

**Topic:** Query Optimization, EXPLAIN, Indexes

**Question:** Analyze and optimize a query to find the total revenue for each film. Show the query execution plan before and after optimization.

**Unoptimized Query:**
```sql
-- Without proper indexing
EXPLAIN ANALYZE
SELECT 
    f.title,
    COUNT(r.rental_id) AS rental_count,
    SUM(p.amount) AS total_revenue,
    ROUND(AVG(p.amount), 2) AS avg_rental_price
FROM film f
LEFT JOIN inventory i ON f.film_id = i.film_id
LEFT JOIN rental r ON i.inventory_id = r.inventory_id
LEFT JOIN payment p ON r.rental_id = p.rental_id
GROUP BY f.film_id, f.title
ORDER BY total_revenue DESC NULLS LAST
LIMIT 10;
```

**Analysis Query:**
```sql
-- Check existing indexes
SELECT 
    tablename,
    indexname,
    indexdef
FROM pg_indexes
WHERE schemaname = 'public'
AND tablename IN ('film', 'inventory', 'rental', 'payment')
ORDER BY tablename, indexname;
```

**Optimization - Create Indexes:**
```sql
-- Create indexes if they don't exist
CREATE INDEX IF NOT EXISTS idx_inventory_film_id ON inventory(film_id);
CREATE INDEX IF NOT EXISTS idx_rental_inventory_id ON rental(inventory_id);
CREATE INDEX IF NOT EXISTS idx_payment_rental_id ON payment(rental_id);
CREATE INDEX IF NOT EXISTS idx_rental_date ON rental(rental_date);
CREATE INDEX IF NOT EXISTS idx_payment_date ON payment(payment_date);
```

**Optimized Query with Materialized View:**
```sql
-- Create materialized view for faster repeated queries
CREATE MATERIALIZED VIEW IF NOT EXISTS film_revenue_summary AS
SELECT 
    f.film_id,
    f.title,
    f.rating,
    f.rental_rate,
    COUNT(r.rental_id) AS rental_count,
    COALESCE(SUM(p.amount), 0) AS total_revenue,
    ROUND(COALESCE(AVG(p.amount), 0), 2) AS avg_rental_price
FROM film f
LEFT JOIN inventory i ON f.film_id = i.film_id
LEFT JOIN rental r ON i.inventory_id = r.inventory_id
LEFT JOIN payment p ON r.rental_id = p.rental_id
GROUP BY f.film_id, f.title, f.rating, f.rental_rate;

-- Create index on materialized view
CREATE INDEX idx_film_revenue_total ON film_revenue_summary(total_revenue DESC);

-- Refresh materialized view (run periodically)
REFRESH MATERIALIZED VIEW film_revenue_summary;

-- Query the materialized view (much faster)
SELECT *
FROM film_revenue_summary
ORDER BY total_revenue DESC
LIMIT 10;
```

**Query with Query Hints:**
```sql
-- Use EXPLAIN to see query plan
EXPLAIN (ANALYZE, BUFFERS, VERBOSE)
SELECT 
    f.title,
    COUNT(r.rental_id) AS rental_count,
    SUM(p.amount) AS total_revenue
FROM film f
INNER JOIN inventory i ON f.film_id = i.film_id
INNER JOIN rental r ON i.inventory_id = r.inventory_id
INNER JOIN payment p ON r.rental_id = p.rental_id
WHERE r.rental_date >= '2005-06-01'
GROUP BY f.film_id, f.title
HAVING COUNT(r.rental_id) > 5
ORDER BY total_revenue DESC
LIMIT 10;
```

**Explanation:** 
- EXPLAIN ANALYZE shows actual query execution plan and timing
- Indexes speed up JOIN operations and WHERE clauses
- Materialized views pre-compute expensive queries
- ANALYZE, BUFFERS, VERBOSE provide detailed execution statistics
- Proper indexing can reduce query time from seconds to milliseconds

**Sample EXPLAIN Output:**
```
QUERY PLAN
---------------------------------------------------------------------------
Limit  (cost=XXX..XXX rows=10 width=XX) (actual time=XX..XX rows=10 loops=1)
  ->  Sort  (cost=XXX..XXX rows=XXX width=XX) (actual time=XX..XX rows=10 loops=1)
        Sort Key: (sum(p.amount)) DESC
        ->  HashAggregate  (cost=XXX..XXX rows=XXX width=XX) (actual time=XX..XX rows=XXX loops=1)
              ->  Hash Join  (cost=XXX..XXX rows=XXX width=XX) (actual time=XX..XX rows=XXX loops=1)
                    ...
Planning Time: XX ms
Execution Time: XX ms
```

**Sample Output:**
```
title                | rental_count | total_revenue | avg_rental_price
---------------------+--------------+--------------+-----------------
TELEGRAPH VOYAGE     | 39           | 231.73       | 5.94
WIFE TURN           | 39           | 223.69       | 5.74
ZORRO ARK           | 38           | 214.69       | 5.65
...
```

---

## Summary

These 20 questions cover:

1. **Basic SELECT** - Filtering, sorting
2. **JOINS** - INNER JOIN, multiple tables
3. **Aggregation** - COUNT, GROUP BY
4. **Subqueries** - IN clause
5. **Multiple Aggregates** - SUM, AVG, MAX, MIN, HAVING
6. **LEFT JOIN** - NULL handling, COALESCE
7. **Date Functions** - EXTRACT, DATE_PART, DATE_TRUNC
8. **Window Functions** - ROW_NUMBER, RANK, DENSE_RANK
9. **CTE** - WITH clause
10. **CASE** - Conditional logic
11. **String Functions** - UPPER, LOWER, CONCAT, SUBSTRING
12. **Self Join** - Joining table to itself
13. **UNION** - Set operations
14. **EXISTS** - Correlated subqueries
15. **ROLLUP/CUBE** - Advanced grouping
16. **Recursive CTE** - Hierarchical queries
17. **Advanced Windows** - LAG, LEAD, FIRST_VALUE, LAST_VALUE
18. **Pivoting** - Conditional aggregation
19. **Complex Joins** - Multiple conditions
20. **Optimization** - EXPLAIN, indexes, materialized views

## Practice Platforms

You can practice these queries on:
- **PostgreSQL Tutorial**: https://www.postgresqltutorial.com/postgresql-sample-database/
- **DB Fiddle**: https://www.db-fiddle.com/
- **SQL Fiddle**: http://sqlfiddle.com/
- **PgExercises**: https://pgexercises.com/

Happy practicing! 🎯
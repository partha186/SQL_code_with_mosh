# SQL_code_with_mosh

use sql_store;

select 1,2;
select * from customers
where customer_id=1
order by first_name; 

select first_name,last_name ,points,
(points+10)*10 as 'discount factor' 
from customers;

select distinct state from customers;

-- return all products
-- name
-- unit price
-- new price (unit_price*1.1)
-- >
-- <=
-- =
-- !=
-- <>
select name,unit_price,unit_price*1.1 as new_price from products;

select * from customers where points>3000 and city is not null;
select * from customers where birth_date>'1990-01-01';

-- --get orders placed this year

select * from orders where order_date>='1990-01-01';

select * from customers where birth_date>'1990-01-01' and points>1000;
select * from customers where birth_date>'1990-01-01' 
or(points >1000 and state='VA');

-- Important negation:::::::
select * from customers where not (birth_date<='1990-01-01' and (points <1000 ));

-- from oredr_items table get the 
-- order #6
-- where the total price is greater 30

select * from order_items where order_id=6 and unit_price*quantity >30;

-- IN operatror
select * from customers where state='VA' or state='FL' or state='GA';
select * from customers where state in('VA','FL','GA');

-- return products with qty equal to 49,38,72;
select * from products where quantity_in_stock in (49,38,72);

-- Between;;;
select * from customers where points>=1000 and points<=3000;
select * from customers where points between 1000 and 3000;

-- return cust born in 1/1/1990 and 1/1/2000
select * from customers where birth_date between '1990-1-1' and '2000-1-1';

-- like operator:::::
select * from customers where last_name like 'b%';
select * from customers where last_name like 'b____y';
-- % any number of characters
-- _ represents a single character


-- get customers whose address contain trail or avenue phone num end with 9;
select * from customers where (address like '%trail%' or address like '%avenue%') 
and phone like '%9%';

-- regex
-- REGEXP Operator:

-- Regular Expression operator

-- ^[carrot sign] -> ^field => must start with field

-- field$ -> must end with field

-- field|mac -> have field or mac [pipe]

-- ^field|mac|rose -> start with field or have mac or have rose

-- [gim]e -> any of characters in brackets can come before e -> ge or ie or me in reality

-- e[fmq] -> ef or em or eq

-- [a-z]e -> range of ae or be or ... ze.

-- ^ beginning

-- $ end

-- | logical or

-- [abcd] a or b or c or d

-- [a-f] range

-- PATTERN	 WHAT THE PATTERN MATCHES
-- -------  ------------------------
-- * ----> Zero or more instances of string preceding it
-- + ---->	One or more instances of strings preceding it
-- . ---->	Any single character
-- | ---->	Logical OR
-- ? ---->	Match zero or one instances of the strings preceding it.
-- ^ ---->	caret(^) matches Beginning of string
-- $ ---->	End of string
-- [abc]  ----> Any character listed between the square brackets
-- [^abc] ----> Any character not listed between the square brackets
-- [A-Z]  ----> match any upper case letter.
-- [a-z]  ----> match any lower case letter
-- [0-9]  ----> match any digit from 0 through to 9.
-- [[:<:]]----> matches the beginning of words.
-- [[:>:]]----> matches the end of words.
-- [:class:]---->matches a character class i.e. [:alpha:] to match letters, [:space:] to match white space, [:punct:] is match		      punctuations and [:upper:] for upper class letters.

-- p1|p2|p3----> Alternation; matches any of the patterns p1, p2, or p3
-- {n} ----> n instances of preceding element
-- {m,n} ----> m through n instances of preceding element

select * from customers where last_name like '%field%';
SELECT * FROM CUSTOMERS WHERE LAST_NAME REGEXP 'FIELD';
SELECT * FROM CUSTOMERS WHERE LAST_NAME REGEXP '^FIELD';-- BEGIBNNING OF STRING
SELECT * FROM CUSTOMERS WHERE LAST_NAME REGEXP 'FIELD$';-- END OF THE SRTRING
SELECT * FROM CUSTOMERS WHERE LAST_NAME REGEXP '^FIELD|MAC|ROSE';-- any subpattern match OF THE STRING
SELECT * FROM CUSTOMERS WHERE LAST_NAME REGEXP '[GI]E';-- GE,IE,
SELECT * FROM CUSTOMERS WHERE LAST_NAME REGEXP '[A-H]E';-- GE,IE,

-- GET THE CUSTOMERS WHOSE 
-- FIRST NAME ARE ELKA OR AMBUR
-- LASTNAME END WITH EY OR ON
-- LAST NAME END WITH MY OR CONTAINS SE
-- LASTNAME CONTAIN b FOLLOWED BY R OR U;

SELECT * FROM CUSTOMERS WHERE FIRST_NAME REGEXP  'ELKA|AMBUR';
SELECT * FROM CUSTOMERS WHERE LAST_NAME REGEXP  'EY$|ON$';
SELECT * FROM CUSTOMERS WHERE LAST_NAME REGEXP  '^MY$|SE';
SELECT * FROM CUSTOMERS WHERE LAST_NAME REGEXP  'B[RU]';-- br|bu

-- IS NULL Operator:
-- Null represents absence of value
SELECT * FROM CUSTOMERS WHERE phone IS NULL;
SELECT * FROM CUSTOMERS WHERE phone IS NOT NULL;
-- GET THE ORDERS THAT ARE NOT SHIPPED::
SELECT * FROM SQL_STORE.orders WHERE shipped_date IS NULL;

-- ORDER BY CLAUSE;;;
-- Default done via PRIMARY KEY Column-- Used for sorting as per condition-- ORDER BY first_name
-- Default ASC-- DESC can be done-- Multiple columns can be done. first_column sort, then each sorted result group gets sorted using second_column
-- ORDER BY state DESC, first_name-- We can sort data in MYSQL by any column regardless they are being picked or not for display.
-- Alias sorting allowed in MySQL.-- Sorting columns using their position [ORDER BY 1, 2] should be avoided.

SELECT * FROM CUSTOMERS ORDER BY FIRST_NAME DESC;
SELECT * FROM CUSTOMERS ORDER BY STATE DESC ,FIRST_NAME ASC;
SELECT FIRST_NAME,LAST_NAME ,10 AS POINTS FROM CUSTOMERS ORDER BY POINTS,FIRST_NAME ASC;
SELECT FIRST_NAME,LAST_NAME ,10 AS POINTS FROM CUSTOMERS ORDER BY 1,2;

SELECT *,quantity*unit_price AS TOTAL_PRICE FROM order_items 
WHERE order_id = 2 ORDER BY quantity*unit_price DESC;

-- LIMIT Clause:
-- LIMIT 6, 3-- 6 is offset here.-- 3 is the limit of records after 6 offset.
-- Should always come at the end in terms of order.
SELECT * FROM CUSTOMERS LIMIT 5;
SELECT  *  FROM customers LIMIT 300 ; 
SELECT * FROM CUSTOMERS LIMIT 6,3;  -- offset, num to show; show 7.8.9 
-- GET THE TOP THREE LOYAL CUSTOMERS;
SELECT * FROM CUSTOMERS ORDER BY POINTS DESC LIMIT 3; -- LIMIT order is always at the end
SELECT * FROM order_items WHERE order_id = 2 
ORDER BY quantity, unit_price DESC LIMIT 6,3;

-- FROM MULTIPLE TABLE:
-- It's better practice to give aliases to the tables involved.
-- It's not required to prefix alias to the unambiguous columns in SELECT clause. 
-- Example: oi.order_id can simply be order_id.
-- INNER keyword is optional as that's the default behaviour for JOIN.
SELECT * FROM ORDERS JOIN CUSTOMERS ON ORDERS.CUSTOMER_ID =CUSTOMERS.CUSTOMER_ID; -- inner join

SELECT FIRST_NAME,ORDERS.CUSTOMER_ID,LAST_NAME FROM ORDERS 
JOIN CUSTOMERS ON ORDERS.CUSTOMER_ID =CUSTOMERS.CUSTOMER_ID;

SELECT FIRST_NAME,O.CUSTOMER_ID,LAST_NAME FROM ORDERS O
JOIN CUSTOMERS C ON O.CUSTOMER_ID =C.CUSTOMER_ID;  -- use alias to simplify

-- Use to join two tables to extract data residing in the two table but are linked somehow
SELECT oi.order_id,p.product_id,p.name,oi.quantity,oi.unit_price
FROM order_items oi
INNER JOIN products p ON oi.product_id = p.product_id;

-- JOINING ACROSS DATABASES:

-- You simply prefix the name of the database before 
-- the table names to join tables residing in different databases.
-- If the current database in use is "A" then no need to prefix table residing in "A" with "A".
SELECT oi.order_id,p.product_id,p.name,oi.quantity,oi.unit_price
FROM order_items oi
INNER JOIN sql_inventory.products p ON oi.product_id = p.product_id; -- Cross-database search

-- SELF JOIN:Exactly the same as INNER JOIN. 
-- Just requires different aliases for the same table and prefix becomes necessary 
-- due to column ambiguity.
SELECT o.order_id,o.order_date,c.first_name,c.last_name,os.name AS status
FROM orders o JOIN customers c ON o.customer_id = c.customer_id
JOIN order_statuses os ON o.status = os.order_status_id; -- self join

-- Self Join
USE SQL_HR;
select * from employees; 
SELECT 
	e.employee_id,
    e.first_name,
    m.first_name as manager
    FROM EMPLOYEES E
left JOIN EMPLOYEES M ON E.REPORTS_TO=M.EMPLOYEE_ID; -- self join

-- Using clause;;;;
-- USING keyword works only when the column name across different tables are same.
-- Equivalent to ON cond_1
-- Can be used for single and multiple columns.
use sql_store;
select o.order_id,c.first_name,sh.name as shipper from orders o join customers c
using (customer_id) join shippers sh using(shipper_id);

-- AND
SELECT *
FROM order_items oi
JOIN order_item_notes oin 
on oi.order_id=oin.order_id and oi.product_id=oin.product_id;
-- or
SELECT *
FROM order_items oi
JOIN order_item_notes oin USING (order_id, product_id);

-- AND
use sql_invoicing;
SELECT
    p.date,
    c.name AS client,
    p.amount,
    pm.name AS payment_method
FROM
    payments p
    JOIN clients c
      USING (client_id)
    JOIN payment_methods pm
      ON p.payment_method = pm.payment_method_id;
      
-- cross join;;;;
USE sql_store;

-- Do a cross join between sippers and products
--   using the implicit syntax
SELECT
    s.name AS shipper,
    p.name AS product
FROM
    shippers s, products p
ORDER BY shipper;

--   and then using the explicit syntax
SELECT
    s.name AS shipper,
    p.name AS product
FROM
    shippers s CROSS JOIN products p
ORDER BY shipper;

-- Unions----
USE sql_store;
-- Combine multiple rows rather than columns.
-- Combine multiple query results.
-- These queries can be from the same table or multiple table

SELECT order_id, order_date, 'Active' AS status
FROM orders o
WHERE o.order_date >= '2019-01-01'
UNION
SELECT order_id, order_date, 'Archived' AS status
FROM orders o
WHERE o.order_date <= '2019-01-01';

SELECT
    customer_id,
    first_name,
    points,
    'Bronze' AS type
FROM
    customers
WHERE
    points < 2000
UNION
SELECT
    customer_id,
    first_name,
    points,
    'Silver' AS type
FROM
    customers
WHERE
    points BETWEEN 2000 AND 3000
UNION
SELECT
    customer_id,
    first_name,
    points,
    'Gold' AS type
FROM
    customers
WHERE
    points > 3000
ORDER BY
    first_name;
    
-- INSERT INTO------
-- INSERT INTO table_name,
-- VALUES(DEFAULT, 'value_1', 'value_2', NULL, '2002-01-01');
-- OR
-- INSERT INTO table_name (col_val_1, col_val_2, col_date)
-- VALUES (value_1, value_2, '2002-01-01');

-- String and Date value should be within ''.
-- Numbers can be without ''.
-- DEFAULT keyword is used for autoincrement column and NULL allowed column.
-- NULL keyword is for null allowed column.
USE SQL_STORE;
insert into customers values(DEFAULT,'JOHN','SMITH','1990-01-01');

-- Inserting multiple values is allowed in one go.
-- Inserting hierarchical values: This is inserting the values in one table and then use that new record to insert value in other table.
-- LAST_INSERT_ID() is the function that gives back the last insert ID.
USE SQL_STORE;
SELECT * FROM ORDERS;
INSERT INTO orders (customer_id, order_date, status)
VALUES (1, '2021-01-01', 1);

insert into shippers(name)
values('Shipper1'),
('Shipper2'),
('Shipper3');
-- Insert three rows in the products table
INSERT INTO products (name, quantity_in_stock, unit_price)
VALUES ('Product1', 10, 1.95),
       ('Product2', 11, 1.95),
       ('Product3', 12, 1.95);
-- insert tables using hierarchial rows.......  
-- select last_insert_id();       
INSERT INTO order_items
VALUES (LAST_INSERT_ID(), 1, 1, 2.95),
    (LAST_INSERT_ID(), 2, 1, 2.95);

-- copying table from _copy_of_a_table.sql

use sql_store;
CREATE TABLE orders_archived AS SELECT * FROM orders;
-- Copies the table but not the column attributes like Primary Key marking, AI marking, etc.
-- It contains the subquery of SELECT...

TRUNCATE orders_archived;
-- deletes all the records within the table.
--
insert into orders_archived
select * from orders where order_date<'2019-01-01';

-- We can use subquery inside INSERT statement
USE sql_invoicing;
CREATE TABLE invoices_archived AS
    SELECT i.invoice_id,i.number,
	c.name AS client,
	i.invoice_total,
	i.payment_total,
	i.invoice_date, 
	i.due_date,
	i.payment_date
    FROM invoices i JOIN clients c ON i.client_id = c.client_id
    WHERE i.payment_date IS NOT NULL;
-- or
CREATE TABLE invoices_archived AS
    SELECT i.invoice_id, i.number,
	c.name AS client,
	i.invoice_total,
	i.payment_total,
	i.invoice_date,
	i.due_date,
	i.payment_date
    FROM invoices i join clients c USING (client_id)
    WHERE i.payment_date IS NOT NULL;


--  Updating Data-- Updating Single Row

-- UPDATE table_name
-- SET col_name_1 = value_1/expression, col_name_2 = value_2/expression
-- WHERE cond_1 (this condition is necessary);

-- OR
-- ----------------------------------
UPDATE invoices
SET
	payment_total = invoice_total * 0.5,
	payment_date = due_date
WHERE invoice_id = 3; 
-- -----------------------------------
UPDATE invoices
SET
	payment_total = default,
	payment_date = null
-- WHERE invoice_id = 1;-- 
WHERE client_id IN (3, 5);

-- Updating Multiple Row
-- * Leave the WHERE Clause
-- Updating Subqueries in Updates
use sql_store;
UPDATE orders
SET comments = 'GOLD CUSTOMERS'
WHERE customer_id IN (SELECT customer_id
						FROM customers
						WHERE points > 3000);

-- SUBQUERIES
-- A subquery is a select query within another query.
-- Question: Find (SQL MOSH Databases) the products where the unit price is greater than the product Lettuce with id = 3
use sql_invoicing;
select client_id from clients where name = 'MyWorks';
UPDATE invoices
SET
	payment_total = invoice_total * 0.5,
	payment_date = due_date
    where client_id in (select client_id from clients where state in ('CA','NY'));

-- njkhnkjnkj

SELECT *
FROM products
WHERE unit_price > (SELECT unit_price
                    FROM products
                    WHERE product_id = 3);
                    
-- ::::::::::::::Aggreagte functions::::::::::::::::
-- Aggregate Functions:

-- MAX()
-- MIN()
-- AVG()
-- SUM()
-- COUNT()
use sql_invoicing;
SELECT
	MAX(invoice_total) AS highest,
	MIN(invoice_total) AS lowest, 
	AVG(invoice_total) AS average,
	SUM(invoice_total * 1.1) AS total,
	COUNT(DISTINCT invoice_total) AS number_of_invoices,
	COUNT(*) AS total_records
FROM invoices
WHERE invoice_date > '2019-07-01';

-- Work on Non-NULL values
-- Work on Date Values and String values as well.
-- Work on Numeric values
-- Work with WHERE clause
-- By Default duplicate values are included. Using DISTINCT can give DISTINCT values.

SELECT
    'First half of 2019' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total -payment_total ) AS what_we_expect
FROM
    invoices
WHERE 
    invoice_date BETWEEN '2019-01-01' AND '2019-06-30'
UNION
SELECT
    'Second half of 2019' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total -payment_total ) AS what_we_expect
FROM
    invoices
WHERE 
    invoice_date BETWEEN '2019-07-01' AND '2020-12-31'
UNION
	SELECT
    'Total' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total -payment_total ) AS what_we_expect
FROM
    invoices
WHERE 
    invoice_date BETWEEN '2019-01-01' AND '2020-12-31';
    
--     
-- GROUP BY Clause:
-- Group BY include all the SELECT columns except the aggregate one.

SELECT
    client_id,
    SUM(invoice_total) AS total_sales
FROM invoices
where invoice_date>='2019-07-01'
GROUP BY client_id
ORDER BY total_sales DESC;

select 
state ,
city ,
sum(invoice_total) as total_sales 
from invoices 
join clients using(client_id)
group by state ,city
order by total_sales desc;

-- by default the result is sorted by column_name in GROUP BY clause.
-- GROUP BY is always after FROM and WHERE Clause and before ORDER BY clause.

SELECT date,
    pm.name AS payment_method,
    SUM(amount) AS total_payments
FROM payments p
        JOIN payment_methods pm on p.payment_method = pm.payment_method_id
GROUP BY date, payment_method
ORDER BY date;

-- HAVING CLAUSE:
-- Filter data after the rows have been grouped.

SELECT client_id,
    SUM(invoice_total) AS total_sales,
    count(*) as number_of_invoices
FROM invoices
GROUP BY client_id
HAVING total_sales > 800 and number_of_invoices>5;

-- Get the customers
--     located in Virginia
--     who have spent more than $100

use sql_store;

select * from customers c LEFT JOIN orders o USING(customer_id)
    LEFT JOIN order_items oi USING(order_id)
where state='VA';

SELECT
    c.customer_id AS id,
    c.first_name AS first_name,
    c.last_name AS last_name,
    SUM(oi.quantity * oi.unit_price) AS total_sales
FROM
    customers c LEFT JOIN orders o USING(customer_id)
    LEFT JOIN order_items oi USING(order_id)
WHERE
    c.state = 'VA'
GROUP BY 
    id, first_name, last_name
HAVING
    total_sales > 100;
    
-- Note: If we use WITH ROLLUP clause, the fields used in
-- GROUP BY must the real fields, not alias.
use sql_invoicing;

select state,city,sum(invoice_total) as total_sales 
from invoices i join clients c using(client_id)
group by state, city with rollup; 
-- rollup
SELECT
    pm.name AS payment_method,
    SUM(p.amount) AS total
FROM
    payments p JOIN payment_methods pm
        ON pm.payment_method_id = p.payment_method
GROUP BY
    pm.name WITH ROLLUP;
    

-- Subqueries:
-- Find products that are more 
-- expensive than Lettuce (id = 3)
use sql_store;
SELECT *
FROM products
WHERE unit_price > (
	SELECT unit_price
    FROM products
    WHERE product_id = 3
);
-- Get the customer located in Virginia and who have spent more than $100.

-- In sql_hr database:
-- Find employees who earn more than average
use sql_hr;
SELECT *
FROM employees
WHERE salary > (
	SELECT AVG(salary)
    FROM employees
);

select * from sql_store.order_items;

use sql_store;
-- find products that have never been ordered
select * from products where product_id not in (select distinct product_id
from order_items);

-- Find client without invoices;
use sql_invoicing;
select * from clients where client_id not in (select distinct client_id from invoices);

-- subqueries and joins::
-- Find clients without invoices
SELECT *
FROM clients
WHERE client_id NOT IN(
	SELECT DISTINCT client_id
    FROM invoices
);

-- Rewrite the same code by using JOIN
SELECT *
FROM clients
LEFT JOIN invoices USING (client_id)
WHERE invoice_id IS NULL;

-- Find customers who have ordered lettuce (id=3)
-- Select customer_id, first_name, last_name
USE sql_store;

-- Solution 1 by using subquery
SELECT 
	customer_id, 
    first_name, 
    last_name
FROM customers
WHERE customer_id IN (
		SELECT DISTINCT customer_id
		FROM orders
		WHERE order_id IN ( 
			SELECT order_id 
			FROM order_items
			WHERE product_id = 3)
);

-- Solution 2 by using subquery
SELECT 
	customer_id, 
    first_name, 
    last_name
FROM customers
WHERE customer_id IN (
		SELECT o.customer_id
		FROM order_items oi
		JOIN orders o USING (order_id)
		WHERE product_id = 3
);

-- Solution by using join
SELECT DISTINCT
	customer_id, 
    first_name, 
    last_name
FROM customers c
JOIN orders o USING (customer_id)
JOIN order_items oi USING (order_id)
WHERE oi.product_id = 3;

-- Select invoices larger than all invoices of
-- client 3

USE sql_invoicing;

SELECT *
FROM invoices
WHERE invoice_total > (
	SELECT MAX(invoice_total)
	FROM invoices
	WHERE client_id = 3
);
    
SELECT *
FROM invoices 
WHERE invoice_total > ALL (  -- ALL keyword 
	SELECT invoice_total
    FROM invoices
    WHERE client_id = 3
);

-- Select products that were sold by
-- the unit (quantity = 1)

USE sql_invoicing;

SELECT *
FROM invoices
WHERE invoice_total > ALL (
	SELECT invoice_total
    FROM invoices
    WHERE client_id = 3
);

-- Select clients with at least two invoices

SELECT *
FROM clients
WHERE client_id = ANY (
	SELECT client_id
    FROM invoices
    GROUP BY client_id
    HAVING COUNT(*) >= 2
);

/* 
NOTE: the difference between ANY and ALL keyword:
ANY means that the condition will be satisfied if the 
operation is true for any of the values in the range. 
ALL means that the condition will be satisfied only if 
the operation is true for all values in the range. 
*/

-- Select employees whose salary is 
-- above the average in their office

USE sql_hr;

-- Sudo code would be:
-- for each employee
-- 	calculate the avg salary for employee.office
-- 	return the employee if salary > avg

SELECT *
FROM employees e
WHERE salary > (
	SELECT AVG(salary)
    FROM employees
    WHERE office_id = e.office_id
);

-- Get invoices that are larger than the
-- client's average invoice amount

USE sql_invoicing;

SELECT *
FROM invoices i
WHERE invoice_total > (
	SELECT AVG(invoice_total)
    FROM invoices
    WHERE client_id = i.client_id
);

-- Select clients that have an invoice

SELECT *
FROM clients
WHERE client_id IN (
	SELECT DISTINCT client_id
    	FROM invoices
);

SELECT *
FROM clients c
WHERE EXISTS (
	SELECT client_id
    	FROM invoices
    	WHERE client_id = c.client_id
);

-- Find the products that have never been ordered

USE sql_store;

SELECT *
FROM products p
WHERE NOT EXISTS (
	SELECT product_id
    	FROM order_items
    	WHERE product_id = p.product_id
);

-- when a subquery returns a lot of data, it
-- is more efficient to use EXISTS operator instead.

USE sql_invoicing;

SELECT
    invoice_id,
    invoice_total,
    (SELECT AVG(invoice_total)
	FROM invoices) AS invoice_average,
	invoice_total - (SELECT invoice_average) AS difference
FROM invoices;

-- show all clients with thir total_sales for issued invoices

SELECT 
    client_id,
    name,
    (SELECT SUM(invoice_total)
	FROM invoices
        WHERE client_id = c.client_id) AS total_sales,
    (SELECT AVG(invoice_total) 
	FROM invoices) AS average, 
    (SELECT total_sales - average) AS difference
FROM clients c;

-- Numeric Functions::

SELECT ROUND(5.7345, 2);
SELECT CEILING(5.7);
SELECT FLOOR(5.7);
SELECT ABS(-5.2);
SELECT RAND();

SELECT LENGTH('sky');
SELECT UPPER('sky');
SELECT LOWER('sky');
SELECT LTRIM('    sky');
SELECT RTRIM('sky    ');
SELECT TRIM('sky'   );
SELECT LEFT('Kindergarten', 4);
SELECT RIGHT('Kindergarten', 6);
SELECT SUBSTRING('Kindergarten', 3, 5);
SELECT SUBSTRING('Kindergarten', 3);
SELECT LOCATE('N', 'Kindergarten');
SELECT LOCATE('q', 'Kindergarten');
SELECT LOCATE('garten', 'Kindergarten');
SELECT REPLACE('Kindergarten', 'garten', 'garden');
SELECT CONCAT('first', 'last');

USE sql_store;
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM customers;

SELECT NOW(), CURDATE(), CURTIME();
SELECT YEAR(NOW());
SELECT MONTH(NOW());
SELECT DAY(NOW());
SELECT HOUR(NOW());
SELECT MINUTE(NOW());
SELECT DAYNAME(NOW());
SELECT MONTHNAME(NOW());
SELECT EXTRACT(DAY FROM NOW());
SELECT EXTRACT(YEAR FROM NOW());

SELECT * 
FROM orders
WHERE order_date >= '2019-01-01';

SELECT * 
FROM orders
WHERE YEAR(order_date) = YEAR(NOW());

-- Calculating dates and times
SELECT DATE_FORMAT(NOW(), '%M %d %Y');
SELECT TIME_FORMAT(NOW(), '%H:%i %p');

SELECT DATE_ADD(NOW(), INTERVAL 1 DAY);
SELECT DATE_ADD(NOW(), INTERVAL -1 DAY);

SELECT DATE_ADD(NOW(), INTERVAL 1 YEAR);
SELECT DATE_SUB(NOW(), INTERVAL 1 YEAR); 

SELECT DATEDIFF('2019-01-05 09:00', '2019-01-01 17:00');
SELECT TIME_TO_SEC('09:00') - TIME_TO_SEC('09:02');


-- IFNULL and COALESCE functions
USE sql_store;
SELECT 
	order_id,
    IFNULL(shipper_id,'Not assigned') AS shipper
FROM orders;

SELECT 
	order_id,
    COALESCE(shipper_id, comments, 'Not assigned') AS shipper
FROM orders;

SELECT 
	CONCAT(first_name, ' ', last_name) AS customer,
    IFNULL(phone, 'Unknown') AS phone
FROM customers;

-- IF function
USE sql_store;
SELECT
	order_id, 
    order_date,
    IF(YEAR(order_date) = YEAR(NOW()), 
    'Active', 
    'Archived') AS category
FROM orders;

-- Create table having orders, frequency, nsame, product_id

SELECT 
	product_id,
    name,
    COUNT(*) AS orders,
    IF(COUNT(*) > 1, 
    'Many times', 'Once') AS frequency
FROM products
JOIN order_items USING (product_id)
GROUP BY product_id, name;

-- :::::: CASE statement::::
USE sql_store;
SELECT 
	order_id,
    CASE
		WHEN YEAR(order_date) = YEAR(NOW()) THEN 'Active'
        WHEN YEAR(order_date) = YEAR(NOW()) - 1 THEN 'Last Year'
        WHEN YEAR(order_date) < YEAR(NOW()) - 1 THEN 'Archived'
        ELSE 'Future'
	END AS category
FROM orders;

SELECT
	CONCAT(first_name, ' ', last_name) AS customer,
    points,
    CASE
		WHEN points > 3000 THEN 'Gold'
        WHEN points BETWEEN 2000 AND 3000 THEN 'Silver'
        WHEN points < 2000 THEN 'Bronze'
	END AS category
FROM customers
ORDER BY points DESC;


-- :::::::::::: VIEWS in procsql::::::::::::
USE sql_invoicing;

-- create a view
CREATE OR REPLACE VIEW sales_by_client AS
SELECT 
	c.client_id,
    c.name,
    SUM(invoice_total) AS total_sales
FROM clients c
JOIN invoices i USING (client_id)
GROUP BY client_id, name;

-- use a view in SQL syntax
SELECT * 
FROM sales_by_client
ORDER BY total_sales DESC;

SELECT * 
FROM sales_by_client
WHERE total_sales > 500;

SELECT * 
FROM sales_by_client
JOIN clients USING (client_id);

-- Create a view to see the balance
-- for each client.
--
-- clients_balance
-- client_id
-- name
-- balance

CREATE VIEW clients_balance AS
SELECT 
	c.client_id,
	c.name,
    SUM(invoice_total - payment_total) AS balance
FROM clients c
JOIN invoices i USING (client_id)
GROUP BY client_id, name;

SELECT * FROM clients_balance;


DROP VIEW sales_by_client;

/* Common ways to ALTER or DROP views are:
1. DROP view
2. CREATE OR REPLACE VIEW
3. Go to the edit mode of the view and add a change
It is a common practice to save views in separate
sql script files, so those are at desposal to everyone 
engaged on the project.*/

DROP VIEW sales_by_client;
/* Common ways to ALTER or DROP views are:
1. DROP view
2. CREATE OR REPLACE VIEW
3. Go to the edit mode of the view and add a change
It is a common practice to save views in separate
sql script files, so those are at desposal to everyone 
engaged on the project.*/

/* If the view doesn't have the following words:
-- DISTINCT
-- Aggregate Functions (MIN, MAX, SUM)
-- GROUP BY / HAVING
-- UNION
we consider it as an updateable view */

CREATE OR REPLACE VIEW invoices_with_balance AS
SELECT
	invoice_id,
    number,
    client_id,
    invoice_total,
    payment_total,
    invoice_total - payment_total AS balance,
    invoice_date,
    due_date,
    payment_date
FROM invoices
WHERE (invoice_total - payment_total) > 0;

DELETE FROM invoices_with_balance
WHERE invoice_id = 1;

UPDATE invoices_with_balance
SET due_date = DATE_ADD(due_date, INTERVAL 2 DAY)
WHERE invoice_id = 2;

/* If the view doesn't have the following words:
-- DISTINCT
-- Aggregate Functions (MIN, MAX, SUM)
-- GROUP BY / HAVING
-- UNION
we consider it as an updateable view */

CREATE OR REPLACE VIEW invoices_with_balance AS
SELECT
	invoice_id,
    number,
    client_id,
    invoice_total,
    payment_total,
    invoice_total - payment_total AS balance,
    invoice_date,
    due_date,
    payment_date
FROM invoices
WHERE (invoice_total - payment_total) > 0
WITH CHECK OPTION;
-- this option doesn't allow the view to be updated in case the update change 
-- would exclude the row afterwards;

DELETE FROM invoices_with_balance
WHERE invoice_id = 1;

UPDATE invoices_with_balance
SET due_date = DATE_ADD(due_date, INTERVAL 2 DAY)
WHERE invoice_id = 2;

-- check the check option
UPDATE invoices_with_balance
SET payment_total = invoice_total
WHERE invoice_id = 3;
-- 15:32:51	UPDATE invoices_with_balance SET payment_total = invoice_total WHERE invoice_id = 3	Error Code: 1369. CHECK OPTION failed 'sql_invoicing.invoices_with_balance'	0.000 sec


-- ++++++++++++++++++++++++++========Procedures==========++++++++++++++++++++++++++++
-- Create stored procedure----------
DELIMITER $$
CREATE PROCEDURE get_clients()
BEGIN
	SELECT * FROM clients;
END $$

DELIMITER ;

CALL get_clients();

-- Create a stored procedure called
-- get_invoices_with_balance
-- to return all the invoices with a balance > 0

DELIMITER $$
CREATE PROCEDURE get_invoices_with_balance()
BEGIN
	SELECT * FROM invoices 
	WHERE invoice_total - payment_total > 0;
END $$
DELIMITER ;

CALL get_invoices_with_balance();.

-- use the created view within a stored procedure
DELIMITER $$
CREATE PROCEDURE get_invoices_with_balance_view()
BEGIN
	SELECT * FROM invoices_with_balance
	WHERE balance > 0;
END $$
DELIMITER ;

CALL get_invoices_with_balance_view();

-- MySQL Workbench deals with the delimiter change;
CREATE DEFINER=`root`@`localhost` PROCEDURE `get_payments`()
BEGIN
	SELECT * FROM payments;
END

DROP PROCEDURE IF EXISTS get_clients;

DELIMITER $$
CREATE PROCEDURE get_clients()
BEGIN
	SELECT * FROM clients;
END $$
DELIMITER ;

DROP PROCEDURE IF EXISTS get_clients_by_state;

DELIMITER $$
CREATE PROCEDURE get_clients_by_state
(
	state CHAR(2)
)
BEGIN
	/* IF state IS NULL THEN
		SELECT * FROM clients;
	ELSE 
		SELECT * FROM clients c
		WHERE c.state = state;
		-- SET state = 'CA';
	END IF; */
    -- cleaner code:
    SELECT * FROM clients c
    WHERE c.state = IFNULL(state, c.state);
    
END $$
DELIMITER ;

-- Write a stored procedure to return invoices
-- for a given client
-- 
-- get_invoices_by_client

DROP PROCEDURE IF EXISTS get_invoices_by_client;

DELIMITER $$
CREATE PROCEDURE get_invoices_by_client
(
	client_id INT
)
BEGIN
	SELECT * FROM invoices i
    WHERE i.client_id = client_id;
END $$
DELIMITER ;

-- Write a stored procedure called get_payments
-- with two parameters
--
-- client_id => INT
-- payment_method_id => TINYINT

DROP PROCEDURE IF EXISTS get_payments;

DELIMITER $$
CREATE PROCEDURE get_payments
(
	client_id INT,
    payment_method_id TINYINT
)
BEGIN
	SELECT * FROM payments p
    WHERE p.client_id = IFNULL(client_id, p.client_id) AND
		p.payment_method = IFNULL(payment_method_id, p.payment_method);
END $$
DELIMITER ;

DROP PROCEDURE IF EXISTS make_payment;

DELIMITER $$

CREATE PROCEDURE make_payment
(
	invoice_id INT,
    payment_amount DECIMAL(9, 2),
    payment_date DATE
)
BEGIN
	IF payment_amount <= 0 THEN
		SIGNAL SQLSTATE '22003' SET MESSAGE_TEXT = 'Invalid payment amount';
	END IF;
	
	UPDATE invoices i
    SET
		i.payment_total = payment_amount,
        i.payment_date = payment_date
	WHERE i.invoice_id = invoice_id;

END $$
DELIMITER ;

DROP PROCEDURE IF EXISTS get_upaid_invoices_for_client;

DELIMITER $$

CREATE PROCEDURE get_upaid_invoices_for_client
(
	client_id INT,
    OUT invoices_count INT,
    OUT invoices_total DECIMAL(9, 2)
    -- by default these parameters are input parameters; when we
    -- add OUT it represents output parameters;
)
BEGIN
	SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices i
    WHERE i.client_id = client_id
		AND payment_total = 0;
END $$
DELIMITER ;

-- > USER or session variables
/* MySQL provides a SET and SELECT statement to declare and initialize 
a variable. The user-defined variable name starts with @ symbol. The 
user-defined variables are not case-sensitive such as @name and @NAME; 
both are the same. A user-defined variable declares by one person cannot
 visible to another person.*/
 
SET @invoices_count = 0

-- > LOCAL variable
/* The scope of the local variable is in a stored program block in 
which it is declared. MySQL uses the DECLARE keyword to specify the 
local variable. The DECLARE statement also combines a DEFAULT clause 
to provide a default value to a variable.*/

DROP PROCEDURE IF EXISTS get_risk_factor;

DELIMITER $$

CREATE PROCEDURE get_risk_factor()
BEGIN
	DECLARE risk_factor DECIMAL(9, 2) DEFAULT 0;
    DECLARE invoices_total DECIMAL(9, 2);
    DECLARE invoices_count INT;
    
    SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices;
    
    SET risk_factor = invoices_total /  invoices_count * 5;
    
    SELECT risk_factor;
    
-- risk_factor = invoices_total / invoices_count * 5
END $$
DELIMITER ;

-- > USER-DEFINED FUNCTIONS
DROP FUNCTION IF EXISTS get_risk_factor_for_client;
DELIMITER $$

CREATE FUNCTION get_risk_factor_for_client 
(
	client_id INT
)
RETURNS INTEGER
-- Atributes:
-- DETERMINISTIC -- if you give this function the same input parameters, it always returns the same value;
-- MODIFIES SQL DATA
READS SQL DATA
BEGIN
	DECLARE risk_factor DECIMAL(9, 2) DEFAULT 0;
    DECLARE invoices_total DECIMAL(9, 2);
    DECLARE invoices_count INT;
    
    SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices i
    WHERE i.client_id = client_id;
    
    SET risk_factor = invoices_total /  invoices_count * 5;
    
	RETURN IFNULL(risk_factor, 0);
END $$
DELIMITER ;

SELECT 
	client_id,
    name,
    get_risk_factor_for_client(client_id) AS risk_factor
FROM clients; 


-- TRIGGER 
-- A block of SQL code that automatically gets executed
-- before or after an INSERT, UPDATE or DELETE statement.

DELIMITER $$

DROP TRIGGER IF EXISTS payments_after_insert;

CREATE TRIGGER payments_after_insert
	AFTER INSERT ON payments
    FOR EACH ROW
BEGIN
	UPDATE invoices 
    SET payment_total = payment_total + NEW.amount
    WHERE invoice_id = NEW.invoice_id;
    
    INSERT INTO payments_audit
    VALUES (NEW.client_id, NEW.date, NEW.amount, 'Insert', NOW());
END $$

DELIMITER ;

INSERT INTO payments
VALUES (DEFAULT, 5, 3, '2019-01-01', 10, 1);

-- Create a trigger that gets fired when we
-- delete a payment.
DELIMITER $$

DROP TRIGGER IF EXISTS payments_after_delete;

CREATE TRIGGER payments_after_delete
	AFTER DELETE ON payments
    FOR EACH ROW
BEGIN
	UPDATE invoices 
    SET payment_total = payment_total - OLD.amount
    WHERE invoice_id = OLD.invoice_id;
    
	INSERT INTO payments_audit
    VALUES (OLD.client_id, OLD.date, OLD.amount, 'Delete', NOW());
END $$

DELIMITER ;

DELETE FROM payments
WHERE payment_id = 9;

SHOW TRIGGERS LIKE 'payments%';
-- Naming convention for triggers:
-- table_before or after_insert or update or delete

USE sql_invoicing;

CREATE TABLE payments_audit
(
	client_id 	INT				NOT NULL,
    date		DATE			NOT NULL,
    amount		DECIMAL(9, 2)	NOT NULL,
    action_type	VARCHAR(50)		NOT NULL,
    action_date	DATETIME		NOT NULL
);

INSERT INTO payments
VALUES (DEFAULT, 5, 3, '2019-01-01', 10, 1);
DELETE FROM payments
WHERE payment_id = 10;


-- EVENTS
-- A task (or block of SQL code) that gets executed according to a schedule;
-- We use events to automate db maintenance tasks;

DELIMITER $$

DROP EVENT IF EXISTS yearly_delete_stale_audit_rows;

CREATE EVENT yearly_delete_tale_audit_rows
ON SCHEDULE 
	-- AT '2019-05-01' -> this is when an event is done only once;
    EVERY 1 YEAR STARTS '2019-01-01' ENDS '2029-01-01'
DO BEGIN
	DELETE FROM payments_audit
    WHERE action_date < NOW() - INTERVAL 1 YEAR;
    
    -- DATEADD (NOW(), INTERVAL -1 YEAR) or:
    -- DATESUB (NOW(), INTERVAL 1 YEAR)
END $$

DELIMITER ;

SHOW EVENTS LIKE 'yearly%';
ALTER EVENT yearly_delete_tale_audit_rows DISABLE; -- afterwards, we can set ENABLE status;

USE sql_store;

START TRANSACTION;

INSERT INTO orders (customer_id, order_date, status)
VALUES (1, '2019-01-01', 1);

INSERT INTO order_items
VALUES (LAST_INSERT_ID(), 1, 1, 1);

COMMIT;
-- ROLLBACK; this is an option to return to the previous stage;

SHOW VARIABLES LIKE 'autocommit'; -- set ON by default;

-- EVENTS
-- A task (or block of SQL code) that gets executed according to a schedule;
-- We use events to automate db maintenance tasks;

DELIMITER $$

DROP EVENT IF EXISTS yearly_delete_stale_audit_rows;

CREATE EVENT yearly_delete_tale_audit_rows
ON SCHEDULE 
	-- AT '2019-05-01' -> this is when an event is done only once;
    EVERY 1 YEAR STARTS '2019-01-01' ENDS '2029-01-01'
DO BEGIN
	DELETE FROM payments_audit
    WHERE action_date < NOW() - INTERVAL 1 YEAR;
    
    -- DATEADD (NOW(), INTERVAL -1 YEAR) or:
    -- DATESUB (NOW(), INTERVAL 1 YEAR)
END $$

DELIMITER ;

SHOW EVENTS LIKE 'yearly%';
ALTER EVENT yearly_delete_tale_audit_rows DISABLE; -- afterwards, we can set ENABLE status;

USE sql_store;

START TRANSACTION;

INSERT INTO orders (customer_id, order_date, status)
VALUES (1, '2019-01-01', 1);

INSERT INTO order_items
VALUES (LAST_INSERT_ID(), 1, 1, 1);

COMMIT;
-- ROLLBACK; this is an option to return to the previous stage;

SHOW VARIABLES LIKE 'autocommit'; -- set ON by default;

/* 
Database concurrency is the ability of a database to allow multiple 
users to affect multiple transactions. This is one of the main properties
that separates a database from other forms of data storage, like spreadsheets.

Concurrency problems:
1. lost updates
2. dirty reads
3. non-repeating reads
4. phantom reads
*/

SHOW VARIABLES LIKE 'transaction_isolation';
SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE; -- only for the current session;
SET GLOBAL TRANSACTION ISOLATION LEVEL SERIALIZABLE; -- set it globally;

-- ISOLATION LEVELS --
-- 1. READ UNCOMMITTED - the lowest isolation level with all concurrency problems;
-- 2. READ COMMITTED - here we don't have dirty reads, but do have unrepeatable or inconsistent reads.
-- 3. REPEATABLE READ - this is the DEFAULT isolation level in mysql, which solves most of the concurrency problems (not phantom reads);
-- 4. SERIALIZABLE - solves all concurrency problems, but has more locks, recourses which can hurt performances and reliability.

-- MySQL data types:

-- STRINGS
/*
CHAR(x)				fixed-length strings
VARCHAR(x)			variable-length strings -> 50 for short strings and 255 for medium-length strings
MEDIUMTEXT 			max 16MB
LONGTEXT 			max 4GB
TINYTEXT			max 255 bytes
TEXT				max 64KB
*/

-- NUMERIC
/*
INT 				whole number
TINYINT				1b 	[-128, 127]
UNSIGNED TINYINT 		[0, 255] -> particularly convenient for data that shouldn't be negative (ex. age)
SMALLINT			2b	[-32K, 32K]
MEDIUMINT 			3b	[-8M, 8M]
INT					4b	[-2B, 2B]
BIGINT 				8b	[-9Z, 9Z]

ZEROFILL:
INT(4) => 0001

DECIMAL(precision, scale) 	DECIMAL(9, 2) => 1234567.89
DEC
NUMERIC
FIXED
FLOAT		4b
DOUBLE 		8b
*/

-- BOOLEAN 
/*
BOOL
BOOLEAN
UPDATE posts
SET is_published = 1 		# or FALSE
*/

-- ENUM
/*
ENUM ('small', 'medium', 'large') => only these values are allowed to be entered;
SET(...) => set can store multiple values;
*/

-- DATE AND TIME
/*
DATE
TIME
DATETIME		8b
TIMESTAMP		4b (up to 2038) 	when a row was changed or updated;
YEAR
*/

-- BLOB => used to store large binary data such as images, videos, pdfs;
/*
TINYBLOB		255b
BLOB			65KB
MEDIUMBLOB 		16MB
LONGBLOB		4GB
*/

-- SPATIAL
-- JSON => lightwight format for storing and transferring data over the Internet.

-- Designing a database:
-- 1. understand the requirements
-- 2. build a conceptual model
-- 3. build a logical model
-- 4. build a physical model

-- CONCEPTUAL MODEL 	-> represents the entities and their relationships.
-- LOGICAL MODEL 		-> creating relationships between entities; more details than the conceptual model.
-- PHYSICAL MODEL		-> the implementation of logical model in mySQL.courses 


-- create a databse

CREATE DATABASE IF NOT EXISTS sql_store2;
DROP DATABASE IF EXISTS sql_store2;

-- create a table

CREATE DATABASE IF NOT EXISTS sql_store2;
USE sql_store2;
DROP TABLE IF EXISTS orders; -- because we cannot delete customers table first, since it is dependent on the relationship with orders table;
DROP TABLE IF EXISTS customers; -- or CREATE TABLE IF NOT EXISTS customers

CREATE TABLE customers
	(
		customer_id 	INT PRIMARY KEY AUTO_INCREMENT,
		first_name 		VARCHAR(50) CHARACTER SET latin1 NOT NULL,
        points			INT NOT NULL DEFAULT 0,
        email			VARCHAR(255) NOT NULL UNIQUE
	);
    
-- alter a table

ALTER TABLE customers
	ADD last_name 	VARCHAR(50) NOT NULL AFTER first_name,
    ADD city 		VARCHAR(50) NOT NULL,
    MODIFY COLUMN first_name VARCHAR(55) DEFAULT '',
    DROP points;
    
-- create relationships
-- DROP TABLE IF EXISTS orders; -- or CREATE TABLE IF NOT EXISTS customers
CREATE TABLE orders
	(
		order_id 		INT PRIMARY KEY,
		customer_id		INT NOT NULL,
        FOREIGN KEY fk_orders_customers (customer_id) -- fk_childTable_customersTable
			REFERENCES customers (customer_id)
            ON UPDATE CASCADE -- or SET NULL, NO ACTION
            ON DELETE NO ACTION
	);
    
-- create and drop relationships between table that already exists

ALTER TABLE orders
	ADD PRIMARY KEY (order_id),
    DROP PRIMARY KEY, -- when dropping primary key, we don't need to specify name of the columns
	DROP FOREIGN KEY fk_orders_customers,
    ADD FOREIGN KEY fk_orders_customers (customers_id)
		REFERENCES customers (customer_id)
        ON UPDATE CASCADE
        ON DELETE NO ACTION;

-- CHARACTER SETS AND COLLATIONS

SHOW CHARSET;
CHAR(10) -> 10 X 3 = 30 BITES X 1000000 = 30M

CREATE DATABASE db_name
	CHARACTER SET latin1;

ALTER DATABASE db_name
	CHARACTER SET latin1;

-- the same goes to tables

CREATE TABLE table1
CHARACTER SET latin1;

ALTER TABLE table1
CHARACTER SET latin1;

-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema school
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema school
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `school` DEFAULT CHARACTER SET utf8 ;
USE `school` ;

-- -----------------------------------------------------
-- Table `school`.`students`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `school`.`students` (
  `student_id` INT NOT NULL AUTO_INCREMENT,
  `first_name` VARCHAR(50) NOT NULL,
  `last_name` VARCHAR(50) NOT NULL,
  `email` VARCHAR(255) NOT NULL,
  `date_registered` DATETIME NOT NULL,
  PRIMARY KEY (`student_id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `school`.`instructors`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `school`.`instructors` (
  `instructor_id` SMALLINT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(50) NOT NULL,
  PRIMARY KEY (`instructor_id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `school`.`courses`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `school`.`courses` (
  `course_id` INT NOT NULL AUTO_INCREMENT,
  `instructor_id` SMALLINT NOT NULL,
  `title` VARCHAR(255) NOT NULL,
  `price` DECIMAL(5,2) NOT NULL,
  PRIMARY KEY (`course_id`, `instructor_id`),
  INDEX `fk_courses_instructors1_idx` (`instructor_id` ASC) VISIBLE,
  CONSTRAINT `fk_courses_instructors`
    FOREIGN KEY (`instructor_id`)
    REFERENCES `school`.`instructors` (`instructor_id`)
    ON DELETE NO ACTION
    ON UPDATE CASCADE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `school`.`enrollments`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `school`.`enrollments` (
  `student_id` INT NOT NULL,
  `course_id` INT NOT NULL,
  `date` DATETIME NOT NULL,
  `price` DECIMAL(5,2) NOT NULL,
  INDEX `fk_enrollments_students_idx` (`student_id` ASC) VISIBLE,
  INDEX `fk_enrollments_courses1_idx` (`course_id` ASC) VISIBLE,
  PRIMARY KEY (`student_id`, `course_id`),
  CONSTRAINT `fk_enrollments_students`
    FOREIGN KEY (`student_id`)
    REFERENCES `school`.`students` (`student_id`)
    ON DELETE NO ACTION
    ON UPDATE CASCADE,
  CONSTRAINT `fk_enrollments_courses`
    FOREIGN KEY (`course_id`)
    REFERENCES `school`.`courses` (`course_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `school`.`tags`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `school`.`tags` (
  `tag_id` TINYINT NOT NULL,
  `name` VARCHAR(50) NOT NULL,
  PRIMARY KEY (`tag_id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `school`.`course_tabs`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `school`.`course_tabs` (
  `course_id` INT NOT NULL,
  `tag_id` TINYINT NOT NULL,
  PRIMARY KEY (`course_id`, `tag_id`),
  INDEX `fk_course_tabs_tags1_idx` (`tag_id` ASC) VISIBLE,
  CONSTRAINT `fk_course_tabs_courses1`
    FOREIGN KEY (`course_id`)
    REFERENCES `school`.`courses` (`course_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_course_tabs_tags1`
    FOREIGN KEY (`tag_id`)
    REFERENCES `school`.`tags` (`tag_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;


EXPLAIN SELECT customer_id FROM customers WHERE state = 'CA';

CREATE INDEX idx_state ON customers (state);

-- Write a query to find customers with more than
-- 1000 points.

EXPLAIN SELECT customer_id
FROM customers
WHERE points > 1000;

CREATE INDEX idx_points ON customers (points);

SHOW INDEXES IN customers;
ANALYZE TABLE customers;

SHOW INDEXES IN orders;

/* 
The PRIMARY key index is also called a CLUSTERED index, which is
automatically created when a primary key is set. 
*/

-- PREFIX Indexes
CREATE INDEX idx_lastname ON customers (last_name(20));

SELECT 
	COUNT(DISTINCT LEFT(last_name, 1)),
	COUNT(DISTINCT LEFT(last_name, 5)),
	COUNT(DISTINCT LEFT(last_name, 10))
FROM customers;

-- FULL-TEXT Indexes
CREATE FULLTEXT INDEX idx_title_body ON posts (title, body);
 -- in natural mode
SELECT *
FROM posts
WHERE MATCH (title, body) AGAINST('react redux');
 -- in boolean mode
SELECT *, MATCH (title, body) AGAINST('react redux') -- this is relevancy factor (from 0 to 1)
FROM posts
WHERE MATCH (title, body) AGAINST('react -redux +form' IN BOOLEAN MODE);

SELECT *, MATCH (title, body) AGAINST('react redux') -- this is relevancy factor (from 0 to 1)
FROM posts
WHERE MATCH (title, body) AGAINST('"handling a form"' IN BOOLEAN MODE);

-- COMPOSITE Indexes
USE sql_store;
SHOW INDEXES IN customers;

CREATE INDEX idx_state_points ON customers (state, points); 
-- put the most frequently used columns first
-- put the column with higer cardinality first

EXPLAIN SELECT customer_id 
FROM customers
WHERE state = 'CA' 
	AND points > 1000;
    
DROP INDEX idx_state ON customers;
DROP INDEX idx_points ON customers;

EXPLAIN SELECT customer_id
FROM customers
WHERE state = 'CA' AND last_name LIKE 'A%';

SELECT 
	COUNT(DISTINCT state),
    COUNT(DISTINCT last_name)
FROM customers;

CREATE INDEX idx_lastname_state ON customers (last_name, state); 


-- When indexes are ignored

EXPLAIN SELECT customer_id FROM customers
WHERE state = 'CA' OR points > 1000;

-- to speed up this index, we use UNION and 
-- create an index on the customers' column points;
CREATE INDEX idx_points ON customers (points);
	EXPLAIN 
		SELECT customer_id FROM customers
		WHERE state = 'CA' 
		UNION 
		SELECT customer_id FROM customers
		WHERE points > 1000;
        
EXPLAIN SELECT customer_id FROM customers
WHERE points + 10 > 2010;

-- to speed up the index for this query, we isolate our columns like this:
EXPLAIN SELECT customer_id FROM customers
WHERE points > 2000;
    
    
-- Using indexes for sorting data

EXPLAIN SELECT customer_id FROM customers 
ORDER BY first_name;
/* 
When we sort by the column which is not included in our index,
we increase the complexity of the background processes of that index.
*/

-- Show MySQL server variables:
SHOW STATUS;
-- Show MySQL server variables associated to a query cost:
SHOW STATUS LIKE 'last_query_cost';

/* If we have an index on these columns a and b, 
here are the options for sorting which won't be 
expensive in terms of background search processes:
-- a
-- a, b
-- a DESC, b DESC
*/

EXPLAIN SELECT customer_id, state FROM customers
ORDER BY state;

/* TIP: When creating indexes, look at your WHERE, ORDER BY and SELECT
clauses and try to include those columns in indexes as well. It will secure
more optimized search for indexes' purposes. */

-- INDEX MAINTENANCE
-- Duplicate Indexes -> double indexes created on the same columns (A, B, C)
-- Redundant Indexes -> when we have indexes on (A, B) and only on column A, but not on (B, A).
-- Before creating new indexes, check the existing ones and make sure to drop duplicate, 
-- redundant as well as unused indexes;
    




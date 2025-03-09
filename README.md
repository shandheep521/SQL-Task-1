# SQL-Task-1



POINT 1: Retrieve all customers who have placed an order in the last 30 days.

SELECT id, name, email, address 
FROM customers 
WHERE id IN (
    SELECT customer_id 
    FROM orders 
    WHERE order_date >= SYSDATE - 30
);

POINT 2: Get the total amount of all orders placed by each customer.

SELECT SUM(TOTAL_AMOUNT) 
FROM orders;

POINT 3: Update the price of Product C to 45.00.
UPDATE products 
SET PRICE=45.00 WHERE NAME='Product C';

POINT 4: Add a new column discount to the products table.

ALTER TABLE products 
ADD discount NUMBER(10,2);

POINT 5: Retrieve the top 3 products with the highest price.

SELECT * FROM products 
ORDER BY price DESC 
FETCH FIRST 3 ROWS ONLY;

POINT 6: Get the names of customers who have ordered Product A.

SELECT name 
FROM customers 
WHERE id IN (
    SELECT customer_id 
    FROM orders 
    WHERE id IN (
        SELECT order_id 
        FROM order_items 
        WHERE product_id = (
            SELECT id FROM products WHERE name = 'Product A'
        )
    )
);

POINT 7: Join the orders and customers tables to retrieve the customers name and order date for each order. 

SELECT name AS customer_name, 
(SELECT order_date FROM orders WHERE orders.customer_id = customers.id) AS order_date
FROM customers
WHERE id IN (SELECT customer_id FROM orders);

POINT 8: Retrieve the orders with a total amount greater than 150.00.
SELECT*FROM 
orders WHERE TOTAL_AMOUNT>150.00;

POINT 9: Normalize the database by creating a separate table for order items and updating the orders table to reference the order_items table.

CREATE TABLE order_items (
    id NUMBER PRIMARY KEY,
    order_id NUMBER NOT NULL,
    product_id NUMBER NOT NULL,
    quantity NUMBER(10) NOT NULL,
    price_per_unit NUMBER(10,2) NOT NULL,
    
    CONSTRAINT fk_order FOREIGN KEY (order_id) REFERENCES orders(id),
    CONSTRAINT fk_product FOREIGN KEY (product_id) REFERENCES products(id)
);


POINT 10: Retrieve the average total of all orders.

SELECT AVG(order_total) AS average_order_total
FROM (
    SELECT order_id, SUM(quantity * price_per_unit) AS order_total
    FROM order_items
    GROUP BY order_id
);



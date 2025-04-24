# task-3
-- =============================
-- E-Commerce SQL Deliverables
-- =============================

-- 1. SELECT, WHERE, ORDER BY, GROUP BY

-- a. Select all products with price above $50
SELECT * FROM Products
WHERE price > 50
ORDER BY price DESC;

-- b. List users ordered by creation date
SELECT user_id, name, email, created_at
FROM Users
ORDER BY created_at DESC;

-- c. Total number of orders per user
SELECT user_id, COUNT(*) AS total_orders
FROM Orders
GROUP BY user_id;

-- d. Average product price
SELECT AVG(price) AS average_price FROM Products;

-- 2. JOINS

-- a. INNER JOIN: Get orders with user names
SELECT o.order_id, u.name, o.total_amount, o.status
FROM Orders o
INNER JOIN Users u ON o.user_id = u.user_id;

-- b. LEFT JOIN: List all users and their orders (if any)
SELECT u.name, o.order_id, o.status
FROM Users u
LEFT JOIN Orders o ON u.user_id = o.user_id;

-- c. RIGHT JOIN: List all orders and any user info (if available)
-- Note: RIGHT JOIN is similar to LEFT JOIN with table order reversed
SELECT u.name, o.order_id, o.status
FROM Orders o
RIGHT JOIN Users u ON o.user_id = u.user_id;

-- 3. Subqueries

-- a. List users who made orders over $100
SELECT name FROM Users
WHERE user_id IN (
    SELECT user_id FROM Orders WHERE total_amount > 100
);

-- b. Products that are more expensive than the average price
SELECT * FROM Products
WHERE price > (
    SELECT AVG(price) FROM Products
);

-- 4. Aggregate Functions (SUM, AVG)

-- a. Total revenue from all payments
SELECT SUM(amount) AS total_revenue FROM Payments;

-- b. Average order value
SELECT AVG(total_amount) AS avg_order_value FROM Orders;

-- 5. Views

-- a. Create view for product sales summary
CREATE VIEW ProductSales AS
SELECT p.product_id, p.name, SUM(oi.quantity) AS total_quantity_sold
FROM Products p
JOIN OrderItems oi ON p.product_id = oi.product_id
GROUP BY p.product_id, p.name;

-- b. Create view for user order summary
CREATE VIEW UserOrderSummary AS
SELECT u.user_id, u.name, COUNT(o.order_id) AS total_orders, SUM(o.total_amount) AS total_spent
FROM Users u
JOIN Orders o ON u.user_id = o.user_id
GROUP BY u.user_id, u.name;

-- 6. Indexing (Query Optimization)

-- a. Create index on Users.email
CREATE INDEX idx_users_email ON Users(email);

-- b. Create index on Orders.user_id
CREATE INDEX idx_orders_user_id ON Orders(user_id);

-- c. Create index on OrderItems.product_id
CREATE INDEX idx_orderitems_product_id ON OrderItems(product_id);
 

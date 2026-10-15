# SQL-Project
project includs railway reservation - 50 queries and banking senariors - 45 qeries  total-95 queries

-- 1. Accounts with balance greater than 20,000

CREATE TABLE customers ( customer_id INT PRIMARY KEY AUTO_INCREMENT, name VARCHAR(50), city VARCHAR(50) );


-- 1.Customers
CREATE TABLE accounts ( account_id INT PRIMARY KEY AUTO_INCREMENT, customer_id INT, account_type VARCHAR(20), balance DECIMAL(10,2)
, FOREIGN KEY (customer_id) REFERENCES customers(customer_id) );
-- 2.Accounts
INSERT INTO customers (name, city) VALUES ('Janani','Chennai'), ('Arun','Coimbatore'), ('Priya','Madurai'), ('Karthik','Salem'); 
INSERT INTO accounts (customer_id, account_type, balance) VALUES (1,'Savings',50000), (1,'Current',20000), (2,'Savings',30000), (3,'Savings',15000), (4,'Current',40000);

-- RAILWAY RESERVATION SCENARIO

-- 3. Trains
CREATE TABLE trains ( train_id INT PRIMARY KEY AUTO_INCREMENT,

train_name VARCHAR(50), source VARCHAR(50), destination VARCHAR(50) );

-- 4. Bookings
CREATE TABLE bookings ( booking_id INT PRIMARY KEY AUTO_INCREMENT, train_id INT, passenger_name VARCHAR(50), fare DECIMAL(10,2), 
status VARCHAR(20), FOREIGN KEY (train_id) REFERENCES trains(train_id) );

INSERT INTO trains (train_name, source, destination) VALUES ('Express1','Chennai','Madurai'), 
('Express2','Coimbatore','Salem'), ('Express3','Madurai','Chennai'); INSERT INTO bookings (train_id, 
passenger_name, fare, status) VALUES (1,'Janani',500,'Confirmed'), (1,'Arun',500,'Waiting'), 
(2,'Priya',300,'Confirmed'), (3,'Karthik',450,'Cancelled'), (2,'Meena',300,'Confirmed');

-- BANKING SCENARIO – QUESTIONS
-- Basic + WHERE + Operators (1–15)

-- 1. Accounts with balance greater than 20,000
SELECT *
FROM accounts
WHERE balance > 20000;

-- 2. Customers who live in Chennai
SELECT *
FROM customers
WHERE city = 'Chennai';

-- 3. Accounts with balance between 20,000 and 50,000
SELECT *
FROM accounts
WHERE balance BETWEEN 20000 AND 50000;

-- 4. Customers whose names start with 'J'
SELECT *
FROM customers
WHERE name LIKE 'J%';

-- 5. Accounts of type Savings or Current
SELECT *
FROM accounts
WHERE account_type IN ('Savings', 'Current');

-- 6. Accounts that are not Savings
SELECT *
FROM accounts
WHERE account_type <> 'Savings';

-- 7. Customers whose names contain the letter 'a'
SELECT *
FROM customers
WHERE name LIKE '%a%';

-- 8. Accounts with balance <= 30,000
SELECT *
FROM accounts
WHERE balance <= 30000;

-- 9. Customers who are not from Madurai
SELECT *
FROM customers
WHERE city <> 'Madurai';

-- 10. Accounts with balance outside 10,000 to 40,000
SELECT *
FROM accounts
WHERE balance NOT BETWEEN 10000 AND 40000;

-- 11. Customers whose names end with 'i'
SELECT *
FROM customers
WHERE name LIKE '%i';

-- 12. Accounts with balance equal to 50,000
SELECT *
FROM accounts
WHERE balance = 50000;

-- 13. Customers from Chennai or Salem
SELECT *
FROM customers
WHERE city IN ('Chennai', 'Salem');

-- 14. Accounts with balance greater than 10,000
--     and less than 40,000
SELECT *
FROM accounts
WHERE balance > 10000 AND balance < 40000;

-- 15. Accounts whose type is not Current
SELECT *
FROM accounts
WHERE account_type NOT IN ('Current');

-- B. ORDER BY (16–18)
-- 16. Accounts sorted by balance in descending order
SELECT *
FROM accounts
ORDER BY balance DESC;

-- 17. Customers sorted alphabetically by name
SELECT *
FROM customers
ORDER BY name ASC;

-- 18. Sort by account type, then balance descending
SELECT *
FROM accounts
ORDER BY account_type ASC, balance DESC;

-- C. Aggregate Functions (19–23)

-- 19. Total balance of all accounts
SELECT SUM(balance) AS total_balance
FROM accounts;

-- 20. Average account balance
SELECT AVG(balance) AS average_balance
FROM accounts;

-- 21. Maximum account balance
SELECT MAX(balance) AS maximum_balance
FROM accounts;

-- 22. Minimum account balance
SELECT MIN(balance) AS minimum_balance
FROM accounts;

-- 23. Total number of customers
SELECT COUNT(*) AS total_customers
FROM customers;

-- D. GROUP BY and HAVING (24–28)

-- 24. Total balance grouped by account type
SELECT account_type, SUM(balance) AS total_balance
FROM accounts
GROUP BY account_type;

-- 25. Average balance for each account type
SELECT account_type, AVG(balance) AS average_balance
FROM accounts
GROUP BY account_type;

-- 26. Account types with average balance > 20,000
SELECT account_type, AVG(balance) AS average_balance
FROM accounts
GROUP BY account_type
HAVING AVG(balance) > 20000;

-- 27. Number of accounts for each customer
SELECT customer_id, COUNT(*) AS account_count
FROM accounts
GROUP BY customer_id;

-- 28. Customers having more than one account
SELECT c.customer_id, c.name, COUNT(a.account_id) AS account_count
FROM customers c
JOIN accounts a ON c.customer_id = a.customer_id
GROUP BY c.customer_id, c.name
HAVING COUNT(a.account_id) > 1;

-- E. JOIN Queries (29–35)

-- 29. Customer names along with account balances
SELECT c.name, a.balance
FROM customers c
JOIN accounts a ON c.customer_id = a.customer_id;

-- 30. All customers and their accounts,
--     including customers without accounts
SELECT c.customer_id, c.name, a.account_id,
       a.account_type, a.balance
FROM customers c
LEFT JOIN accounts a ON c.customer_id = a.customer_id;

-- 31. All accounts and corresponding customer details
SELECT a.account_id, a.account_type, a.balance,
       c.customer_id, c.name, c.city
FROM accounts a
LEFT JOIN customers c ON a.customer_id = c.customer_id;

-- 32. Customer names and account types for balance > 20,000
SELECT c.name, a.account_type, a.balance
FROM customers c
JOIN accounts a ON c.customer_id = a.customer_id
WHERE a.balance > 20000;

-- 33. Customer names with their total balance
SELECT c.customer_id, c.name,
       COALESCE(SUM(a.balance), 0) AS total_balance
FROM customers c
LEFT JOIN accounts a ON c.customer_id = a.customer_id
GROUP BY c.customer_id, c.name;

-- 34. Customer names and balances sorted by balance
SELECT c.name, a.balance
FROM customers c
JOIN accounts a ON c.customer_id = a.customer_id
ORDER BY a.balance ASC;

-- 35. Number of accounts for each city
SELECT c.city, COUNT(a.account_id) AS account_count
FROM customers c
LEFT JOIN accounts a ON c.customer_id = a.customer_id
GROUP BY c.city;

-- F. Subqueries (36–40)


-- 36. Accounts with balance greater than average balance
SELECT *
FROM accounts
WHERE balance > (
    SELECT AVG(balance)
    FROM accounts
);

-- 37. Customers who have accounts
SELECT *
FROM customers
WHERE customer_id IN (
    SELECT customer_id
    FROM accounts
);

-- 38. Customers who do not have any accounts
SELECT *
FROM customers
WHERE customer_id NOT IN (
    SELECT customer_id
    FROM accounts
    WHERE customer_id IS NOT NULL
);

-- 39. Account(s) with the maximum balance
SELECT *
FROM accounts
WHERE balance = (
    SELECT MAX(balance)
    FROM accounts
);

-- 40. Customers whose total balance is greater than 40,000
SELECT c.customer_id, c.name,
       SUM(a.balance) AS total_balance
FROM customers c
JOIN accounts a ON c.customer_id = a.customer_id
GROUP BY c.customer_id, c.name
HAVING SUM(a.balance) > 40000;

-- Part 2: Railway Reservation (Questions 41–50)
-- A. Basic Queries and Operators (41–45)

-- 41. Bookings with fare greater than 400
SELECT *
FROM bookings
WHERE fare > 400;

-- 42. Bookings whose status is not Confirmed
SELECT *
FROM bookings
WHERE status <> 'Confirmed';

-- 43. Trains starting from Chennai
SELECT *
FROM trains
WHERE source = 'Chennai';

-- 44. Bookings with fare between 300 and 500
SELECT *
FROM bookings
WHERE fare BETWEEN 300 AND 500;

-- 45. Passengers whose names start with 'A'
SELECT *
FROM bookings
WHERE passenger_name LIKE 'A%';

-- B. JOIN, GROUP BY and ORDER BY (46–48)

-- 46. Train names along with passenger names
SELECT t.train_name, b.passenger_name
FROM trains t
JOIN bookings b ON t.train_id = b.train_id;

-- 47. Number of bookings for each train
SELECT t.train_id, t.train_name,
       COUNT(b.booking_id) AS booking_count
FROM trains t
LEFT JOIN bookings b ON t.train_id = b.train_id
GROUP BY t.train_id, t.train_name;

-- 48. Total fare collected for each train
SELECT t.train_id, t.train_name,
       COALESCE(
           SUM(CASE
               WHEN b.status = 'Confirmed' THEN b.fare
               ELSE 0
           END), 0
       ) AS total_fare_collected
FROM trains t
LEFT JOIN bookings b ON t.train_id = b.train_id
GROUP BY t.train_id, t.train_name;

-- C. Subqueries (49–50)

-- 49. Bookings with the highest fare
SELECT *
FROM bookings
WHERE fare = (
    SELECT MAX(fare)
    FROM bookings
);

-- 50. Trains having more than one booking
SELECT t.train_id, t.train_name,
       COUNT(b.booking_id) AS booking_count
FROM trains t
JOIN bookings b ON t.train_id = b.train_id
GROUP BY t.train_id, t.train_name
HAVING COUNT(b.booking_id) > 

-- Part 3: Employee Management (Questions 1–15)
-- A. GROUP BY and Aggregate Functions (1–7)

-- 1. Total employees in each department
SELECT department, COUNT(*) AS total_employees
FROM Employee
GROUP BY department;

-- 2. Average salary in each department
SELECT department, AVG(salary) AS average_salary
FROM Employee
GROUP BY department;

-- 3. Departments having more than one employee
SELECT department, COUNT(*) AS employee_count
FROM Employee
GROUP BY department
HAVING COUNT(*) > 1;

-- 4. Highest salary in each department
SELECT department, MAX(salary) AS highest_salary
FROM Employee
GROUP BY department;

-- 5. Lowest salary in each department
SELECT department, MIN(salary) AS lowest_salary
FROM Employee
GROUP BY department;

-- 6. Departments with average salary > 50,000
SELECT department, AVG(salary) AS average_salary
FROM Employee
GROUP BY department
HAVING AVG(salary) > 50000;

-- 7. Total salary expenditure per department
SELECT department, SUM(salary) AS total_salary
FROM Employee
GROUP BY department;

-- B. ORDER BY and Other Queries (8–15)

-- 8. Employees sorted by salary descending
SELECT *
FROM Employee
ORDER BY salary DESC;

-- 9. Sort by department, then salary descending
SELECT *
FROM Employee
ORDER BY department ASC, salary DESC;

-- 10. Cities having more than one employee
SELECT city, COUNT(*) AS employee_count
FROM Employee
GROUP BY city
HAVING COUNT(*) > 1;

-- 11. Total salary paid in each city
SELECT city, SUM(salary) AS total_salary
FROM Employee
GROUP BY city;

-- 12. Departments ordered by total salary, highest first
SELECT department, SUM(salary) AS total_salary
FROM Employee
GROUP BY department
ORDER BY total_salary DESC;

-- 13. Number of employees per department
--     whose salary is greater than 50,000
SELECT department, COUNT(*) AS employee_count
FROM Employee
WHERE salary > 50000
GROUP BY department;

-- 14. Difference between highest and lowest salary
--     in each department
SELECT department,
       MAX(salary) - MIN(salary) AS salary_difference
FROM Employee
GROUP BY department;

-- 15. Top 3 highest-paid employees
SELECT *
FROM Employee
ORDER BY salary DESC
LIMIT 3;

-- Part 4: Customers & Orders (Questions 16–30)

-- A. Customer Purchase and Order Aggregates (16–22)

-- 16. Total order amount for each customer
SELECT c.customer_id, c.customer_name,
       COALESCE(SUM(o.amount), 0) AS total_order_amount
FROM Customers c
LEFT JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;

-- 17. Customers who placed more than 3 orders
SELECT c.customer_id, c.customer_name,
       COUNT(o.order_id) AS order_count
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING COUNT(o.order_id) > 3;

-- 18. Average order amount for each customer
SELECT c.customer_id, c.customer_name,
       AVG(o.amount) AS average_order_amount
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;

-- 19. Highest order amount for each customer
SELECT c.customer_id, c.customer_name,
       MAX(o.amount) AS highest_order_amount
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;

-- 20. Customers sorted by total purchase amount
SELECT c.customer_id, c.customer_name,
       COALESCE(SUM(o.amount), 0) AS total_purchase
FROM Customers c
LEFT JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
ORDER BY total_purchase DESC;

-- 21. Customers whose total purchases exceed 10,000
SELECT c.customer_id, c.customer_name,
       SUM(o.amount) AS total_purchase
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING SUM(o.amount) > 10000;

-- 22. Customer names with total number of orders
SELECT c.customer_id, c.customer_name,
       COUNT(o.order_id) AS total_orders
FROM Customers c
LEFT JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;

-- B. Subqueries and Advanced Aggregation (23–30)

-- 23. Customer(s) who spent the highest total amount
SELECT c.customer_id, c.customer_name,
       SUM(o.amount) AS total_spent
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING SUM(o.amount) = (
    SELECT MAX(customer_total)
    FROM (
        SELECT SUM(amount) AS customer_total
        FROM Orders
        GROUP BY customer_id
    ) AS totals
);

-- 24. Customer(s) who placed the maximum number of orders
SELECT c.customer_id, c.customer_name,
       COUNT(o.order_id) AS total_orders
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING COUNT(o.order_id) = (
    SELECT MAX(order_count)
    FROM (
        SELECT COUNT(*) AS order_count
        FROM Orders
        GROUP BY customer_id
    ) AS order_totals
);

-- 25. Customers with average order amount > 2,000
SELECT c.customer_id, c.customer_name,
       AVG(o.amount) AS average_order_amount
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING AVG(o.amount) > 2000;

-- 26. Top 5 customers by total purchase amount
SELECT c.customer_id, c.customer_name,
       SUM(o.amount) AS total_purchase
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
ORDER BY total_purchase DESC
LIMIT 5;

-- 27. Minimum order amount for each customer
SELECT c.customer_id, c.customer_name,
       MIN(o.amount) AS minimum_order_amount
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;

-- 28. Customers whose total orders exceed 5,000
SELECT c.customer_id, c.customer_name,
       SUM(o.amount) AS total_purchase
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING SUM(o.amount) > 5000;

-- 29. Customer-wise total orders and total purchase amount
SELECT c.customer_id, c.customer_name,
       COUNT(o.order_id) AS total_orders,
       COALESCE(SUM(o.amount), 0) AS total_purchase
FROM Customers c
LEFT JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;

-- 30. Customers with more than 2 orders
--     and total spending > 8,000
SELECT c.customer_id, c.customer_name,
       COUNT(o.order_id) AS total_orders,
       SUM(o.amount) AS total_purchase
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING COUNT(o.order_id) > 2
   AND SUM(o.amount) > 8000;
   
-- Part 5: Students (Questions 31–45)   
-- A. GROUP BY, Aggregate Functions and HAVING (31–45)

-- 31. Average marks in each department
SELECT department, AVG(marks) AS average_marks
FROM Students
GROUP BY department;

-- 32. Departments with average marks above 75
SELECT department, AVG(marks) AS average_marks
FROM Students
GROUP BY department
HAVING AVG(marks) > 75;

-- 33. Highest mark in each department
SELECT department, MAX(marks) AS highest_marks
FROM Students
GROUP BY department;

-- 34. Total number of students in each department
SELECT department, COUNT(*) AS total_students
FROM Students
GROUP BY department;

-- 35. Departments having more than 5 students
SELECT department, COUNT(*) AS total_students
FROM Students
GROUP BY department
HAVING COUNT(*) > 5;

-- 36. Departments sorted by average marks descending
SELECT department, AVG(marks) AS average_marks
FROM Students
GROUP BY department
ORDER BY average_marks DESC;

-- 37. Top 3 departments based on average marks
SELECT department, AVG(marks) AS average_marks
FROM Students
GROUP BY department
ORDER BY average_marks DESC
LIMIT 3;

-- 38. Departments with average marks between 70 and 90
SELECT department, AVG(marks) AS average_marks
FROM Students
GROUP BY department
HAVING AVG(marks) BETWEEN 70 AND 90;

-- 39. Total marks scored by students in each department
SELECT department, SUM(marks) AS total_marks
FROM Students
GROUP BY department;

-- 40. Departments sorted by total number of students
SELECT department, COUNT(*) AS total_students
FROM Students
GROUP BY department
ORDER BY total_students DESC;

-- 41. Lowest mark in each department
SELECT department, MIN(marks) AS lowest_marks
FROM Students
GROUP BY department;

-- 42. Departments where the highest mark is greater than 90
SELECT department, MAX(marks) AS highest_marks
FROM Students
GROUP BY department
HAVING MAX(marks) > 90;

-- 43. Number of students scoring above 80 in each department
SELECT department, COUNT(*) AS students_above_80
FROM Students
WHERE marks > 80
GROUP BY department;

-- 44. Departments where more than 3 students scored above 75
SELECT department, COUNT(*) AS students_above_75
FROM Students
WHERE marks > 75
GROUP BY department
HAVING COUNT(*) > 3;

-- 45. Departments ordered by highest mark descending
SELECT department, MAX(marks) AS highest_marks
FROM Students
GROUP BY department
ORDER BY highest_marks DESC;



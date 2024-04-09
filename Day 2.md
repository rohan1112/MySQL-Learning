<!--
    --Create the table
CREATE TABLE salesperson (
SNUM INT,
SNAME VARCHAR(255),
CITY VARCHAR(255),
COMM float
);

-- Insert values into the table
INSERT INTO salesperson (SNUM, SNAME, CITY, COMM) VALUES
(1001, 'Peel', 'London', 0.12),
(1002, 'Serres', 'San Jose', 0.13),
(1004, 'Motika', 'London', 0.11),
(1007, 'Rifkin', 'Barcelona', 0.15),
(1003, 'AxelRod', 'New York', 0.10),
(1005, 'Fran', 'London', 0.26);

-- Create the table
CREATE TABLE Customers (
CNUM INT,
CNAME VARCHAR(255),
CITY VARCHAR(255),
RATING INT,
SNUM INT
);

-- Insert values into the table
INSERT INTO Customers (CNUM, CNAME, CITY, RATING, SNUM) VALUES
(2001, 'Hoffman', 'London', 100, 1001),
(2002, 'Giovanni', 'Rome', 200, 1003),
(2003, 'Liu', 'San Jose', 200, 1002),
(2004, 'Grass', 'Berlin', 300, 1002),
(2006, 'Clemens', 'London', 100, 1001),
(2008, 'Cisneros', 'San Jose', 300, 1007),
(2007, 'Pereira', 'Rome', 100, 1004);

CREATE TABLE Orders (
ONUM INT,
AMT DECIMAL(10, 2),
ODATE DATE,
CNUM INT,
SNUM INT
);

-- Insert values into the table
INSERT INTO Orders (ONUM, AMT, ODATE, CNUM, SNUM) VALUES
(3001, 18.69, '1996-10-03', 2008, 1007),
(3003, 767.19, '1996-10-03', 2001, 1001),
(3002, 1900.10, '1996-10-03', 2007, 1004),
(3005, 5160.45, '1996-10-03', 2003, 1002),
(3006, 1098.16, '1996-10-03', 2008, 1007),
(3009, 1713.23, '1996-10-04', 2002, 1003),
(3007, 75.75, '1996-10-04', 2002, 1003),
(3008, 4723.00, '1996-10-05', 2006, 1001),
(3010, 1309.95, '1996-10-06', 2004, 1002),
(3011, 9891.88, '1996-10-06', 2006, 1001); -->

## 1. List all the columns of the Salespeople table.

    SELECT * FROM salespeople;

## 2. List all customers with a rating of 100.

     SELECT * FROM customers WHERE rating=100;

## 3. Find all records in the Customer table with NULL values in the city column.

     SELECT * FROM customers WHERE city IS NULL;

## 4. Find the largest order taken by each salesperson ON each date.

     SELECT max(amt),odate,s.sname FROM orders o JOIN salespeople s ON o.snum=s.snum GROUP BY odate,sname;

    SELECT * FROM(SELECT odate,sname,onum,amt, row_number()over(partition by odate ORDER BY amt DESC) as rn FROM salespeople s JOIN orders o ON s.snum=o.snum) as t WHERE rn=1;

## 5. Arrange the Orders table by DESCending customer number.

    SELECT * FROM orders ORDER BY cnum DESC;

## 6. Find which salespeople currently have orders in the Orders table.

     SELECT DISTINCT(sname),s.snum FROM salespeople s JOIN orders o ON s.snum=o.snum;

## 7. List names of all customers matched with the salespeople serving them.

     SELECT sname as "Salesperson Name",c.cname as "Customer Name" FROM salespeople s JOIN customers c ON s.snum=c.snum;

## 8. Find the names and numbers of all salespeople who had more than one customer.

     SELECT c.snum,s.sname,COUNT(c.snum) as cnt FROM customers c JOIN salespeople s ON c.snum=s.snum GROUP BY snum,sname HAVING cnt>1;

    SELECT s.snum,s.sname FROM customers c, salespeople s WHERE c.snum=s.snum GROUP BY snum,sname HAVING COUNT(DISTINCT cnum)>1;

    SELECT s.sname,s.snum FROM salespeople s WHERE EXISTS (SELECT 1 FROM customers c WHERE s.snum=c.snum HAVING COUNT(*)>1);

## 9. Count the orders of each of the salespeople and output the results in descending order.

     SELECT snum,COUNT(onum) as cnt FROM orders GROUP BY snum ORDER BY cnt DESC ;

## 10. List the Customer table if and only if one or more of the customers in the Customer table are

located in San Jose.

     SELECT * FROM customers WHERE EXISTS (SELECT 1 FROM customers WHERE city="San Jose");

## 11. Match salespeople to customers according to what city they lived in.

     SELECT s.sname, c.cname FROM salespeople s JOIN customers c ON c.city=s.city;

## 12. Find the largest order taken by each salesperson.

    SELECT snum ,max(amt) FROM orders GROUP BY snum;

## 13. Find customers in San Jose who have a rating above 200.

     SELECT cname FROM customers WHERE city="San Jose" and rating>200;

## 14. List the names and commissions of all salespeople in London.

    SELECT sname,comm FROM salespeople WHERE city="London";

## 15. List all the orders of salesperson Motika FROM the Orders table.

    SELECT * FROM orders o JOIN salespeople s ON o.snum=s.snum WHERE s.sname="motika";

## 16. Find all customers with orders on October 3.

    SELECT c.cnum,c.cname,c.city, o.odate as "Order date" FROM customers c JOIN orders o ON c.cnum=o.cnum  WHERE odate="1996-10-03";

## 17. Give the sums of the amounts FROM the Orders table, grouped by date, eliminating all those dates WHERE the SUM was not at least 2000.00 above the MAX amount.

    SELECT sum(amt),odate,max(amt) FROM orders GROUP BY odate HAVING sum(amt)-max(amt)>2000;

## 18. SELECT all orders that had amounts that were greater than at least one of the orders from October 6.

     SELECT * FROM orders WHERE amt>(SELECT min(amt) FROM orders WHERE odate="1996-10-06") and odate<>"1996-10-06" ;

## 19. Write a query that uses the EXISTS operator to extract all salespeople who have customers with a rating of 300.

    SELECT s.snum,s.sname,c.cname,c.rating FROM salespeople s JOIN customers c ON s.snum=c.snum WHERE EXISTS(SELECT 1 FROM customers c2 WHERE c.rating>200);

## 20. Find all pairs of customers having the same rating.

    SELECT c1.cname, c2.cname FROM customers c1 ,customers c2 WHERE c1.cname<c2.cname and c1.rating=c2.rating;

## 21. Find all customers whose CNUM is 1000 above the SNUM of Serres.

    SELECT * FROM customers WHERE cnum>1000+(SELECT snum FROM salespeople WHERE sname="Serres");

## 22. Give the salespeople’s commissions as percentages instead of decimal numbers.

    SELECT sname,round((comm)*100,2) as "Percentage of Commission" FROM salespeople;

## 23. Find the largest order taken by each salesperson ON each date, eliminating those MAX orders which are less than $3000.00 in value.

    SELECT max(amt),odate,s.sname FROM orders o JOIN salespeople s ON o.snum=s.snum GROUP BY odate,sname HAVING max(amt)<3000;

    SELECT * FROM(SELECT odate,sname,onum,amt, row_number()over(partition by odate ORDER BY amt DESC) as rn FROM salespeople s JOIN orders o ON s.snum=o.snum) as t WHERE rn=1 WHERE amt<=3000;

## 24. List the largest orders for October 3, for each salesperson.

    SELECT snum,max(amt),odate FROM orders WHERE odate="1996-10-03" GROUP BY snum;

## 25. Find all customers located in cities WHERE Serres (SNUM 1002) has customers.

     select * from customers where city in(select c.city from customers c,salespeople s where s.snum=c.snum and c.snum=1002);

## 26. SELECT all customers with a rating above 200.00.

    SELECT * FROM customers WHERE rating>200;

## 27. COUNT the number of salespeople currently listing orders in the Orders table.

    SELECT COUNT(DISTINCT snum) FROM orders ;

## 28. Write a query that produces all customers serviced by salespeople with a commission above 12%. Output the customer’s name and the salesperson’s rate of commission.

    SELECT c.cname,s.comm FROM customers c JOIN salespeople s ON c.snum=s.snum WHERE s.comm\*100>12;

## 29. Find salespeople who have multiple customers.

    SELECT s.sname,s.snum FROM salespeople s WHERE EXISTS (SELECT 1 FROM customers c WHERE s.snum=c.snum HAVING COUNT(*)>1);

## 30. Find salespeople with customers located in their city.

    SELECT DISTINCT s.sname,s.city FROM salespeople s ,customers c WHERE s.snum=c.snum and s.city=c.city;

## 31. Find all salespeople whose name starts with ‘P’ and the fourth character is ‘l’.

    SELECT sname FROM salespeople WHERE sname like "p__l%";

## 32. Write a query that uses a subquery to obtain all orders for the customer named Cisneros. Assume you do not know his customer number.

    SELECT * FROM orders o JOIN customers c ON o.cnum=c.cnum WHERE o.cnum=(SELECT cnum FROM customers WHERE cname="Cisneros");

## 33. Find the largest orders for Serres and Rifkin.

    SELECT o.snum,max(amt) FROM orders o JOIN salespeople s ON o.snum=s.snum WHERE (sname="rifkin" or sname="serres") GROUP BY o.snum;

## 34. Extract the Salespeople table in the following order : SNUM, SNAME, COMMISSION, CITY.

    SELECT SNUM, SNAME, COMM as COMMISION, CITY FROM salespeople;

## 35. SELECT all customers whose names fall in between ‘A’ and ‘G’ alphabetical range.

    SELECT cname FROM customers WHERE left(cname,1) between 'A' and 'G';

## 36. SELECT all the possible combinations of customers that you can assign.

    --Not sure
    SELECT * FROM customers cross JOIN salespeople;

## 37. SELECT all orders that are greater than the average for October 4.

    SELECT amt,odate FROM orders WHERE amt>(SELECT avg(amt) FROM orders WHERE odate="1996-10-04" GROUP BY odate);

## 38. Write a SELECT command using a corelated subquery that SELECTs the names and numbers of all customers with ratings equal to the maximum for their city.

    SELECT c.cnum,c.cname,c.city FROM customers c WHERE (SELECT max(rating) FROM customers c2 WHERE c.city=c2.city)=c.rating;

## 39. Write a query that totals the orders for each day and places the results in DESCending order.

     SELECT sum(amt),odate FROM orders GROUP BY odate ORDER BY sum(amt) DESC ;

## 40. Write a SELECT command that produces the rating followed by the name of each customer in San Jose.

    SELECT rating , cname FROM customers WHERE city="san jose";

## 41. Find all orders with amounts smaller than any amount for a customer in San Jose.

    SELECT * FROM orders WHERE amt<any(SELECT max(amt) FROM orders o , customers c WHERE o.cnum=c.cnum and city="san jose");

    SELECT * FROM orders WHERE amt<any(SELECT amt FROM orders o , customers c WHERE o.cnum=c.cnum and city="san jose");

## 42. Find all orders with above average amounts for their customers.

    SELECT * FROM orders WHERE amt>(SELECT avg(amt) FROM orders);

## 43. Write a query that SELECTs the highest rating in each city.

    SELECT max(rating),city FROM customers GROUP BY city;

## 44. Write a query that calculates the amount of the salesperson’s commission on each ORDER BY a customer with a rating above 100.00.

    SELECT sname,cname,rating,comm,amt\*comm FROM orders o JOIN customers c ON o.cnum=c.cnum JOIN salespeople s ON o.snum=s.snum and rating>100;

## 45. COUNT the customers with ratings above San Jose’s average.

    SELECT COUNT(*),cname,rating FROM customers WHERE rating>(SELECT avg(rating) FROM customers WHERE city="san jose") group by cname,rating;

## 46. Write a query that produces all pairs of salespeople with themselves as well as duplicate rows with the order reversed.

    SELECT s1.sname,s2.sname FROM salespeople s1, salespeople s2 ;

## 47. Find all salespeople that are located in either Barcelona or London.

    SELECT * FROM salespeople WHERE city in("Barcelona","London");

## 48. Find all salespeople with only one customer.

    SELECT s.sname,s.snum FROM salespeople s WHERE EXISTS (SELECT 1 FROM customers c WHERE s.snum=c.snum HAVING COUNT(*)=1);

## 49. Write a query that JOINs the Customer table to itself to find all pairs of customers served by a single salesperson.

    SELECT c1.cname,c2.cname, c1.snum FROM customers c1,customers c2 WHERE c1.snum=c2.snum and c1.cname<c2.cname;

## 50. Write a query that will give you all orders for more than $1000.00

    SELECT * FROM orders WHERE amt>1000;

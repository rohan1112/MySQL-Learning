## 51. Write a query that lists each order number followed by the name of the customer who made that order.

    SELECT onum,cname FROM orders o,customers c WHERE o.cnum=c.cnum;

## 52. Write 2 queries that SELECT all salespeople (by name and number) who have customers in their cities who they do not service, one using a JOIN and one a corelated subquery. Which solution is more elegant?

    SELECT distinct s.sname,s.snum FROM salespeople s,customers c WHERE s.snum!=c.snum AND s.city=c.city;

    SELECT distinct s.sname,s.snum FROM salespeople s WHERE EXISTS (SELECT 1 FROM customers c WHERE s.snum!=c.snum AND s.city=c.city);

    SELECT snum,sname FROM salespeople s WHERE city=any(SELECT city FROM customers c WHERE s.snum!=c.snum);

## 53. Write a query that SELECTs all customers whose ratings are equal to or greater than ANY (in the SQL sense) of Serres’?

    SELECT * FROM customers WHERE rating>=any(SELECT rating FROM customers c 
    JOIN 
    salespeople s on c.snum=s.snum WHERE s.sname="serres");

## 54. Write 2 queries that will produce all orders taken on October 3 or October 4.

    SELECT * FROM orders WHERE odate in ("1996-10-03","1996-10-04");

    SELECT * FROM orders WHERE odate="1996-10-03" or odate="1996-10-04";

## 55. Write a query that produces all pairs of orders by a given customer. Name that customer and eliminate duplicates.

    SELECT o1.onum,o2.onum,c.cname FROM orders o1,orders o2 ,customers c 
    WHERE o1.cnum=o2.cnum AND o1.onum<o2.onum AND o1.cnum=c.cnum;

    SELECT first,second,t.cnum,c.cname FROM 
    (SELECT o1.onum as first,o2.onum second,o1.cnum FROM orders o1, orders o2 WHERE o1.cnum=o2.cnum 
    AND o1.onum<>o2.onum AND o1.onum>o2.onum) as t 
    JOIN customers c on c.cnum=t.cnum;

## 56. Find only those customers whose ratings are higher than every customer in Rome.

    SELECT * FROM customers WHERE rating>(SELECT MAX(rating) FROM customers WHERE city="rome");

## 57. Write a query on the Customers table whose output will exclude all customers with a rating <= 100.00, unless they are located in Rome.

    SELECT * FROM customers WHERE NOT (rating<=100 AND city<>"rome");

## 58. Find all rows FROM the Customers table for which the salesperson number is 1001.

    SELECT * FROM customers c , salespeople s WHERE c.snum=s.snum AND snum=1001;

## 59. Find the total amount in Orders for each salesperson for whom this total is greater than the amount of the largest order in the table.

    SELECT SUM(amt) as s FROM orders GROUP BY snum having s>any(SELECT MAX(amt) FROM orders);

## 60. Write a query that SELECTs all orders save those with zeroes or NULLs in the amount field.

## 61. Produce all combinations of salespeople and customer names such that the former precedes the latter alphabetically, and the latter has a rating of less than 200.

    SELECT *,s.snum FROM salespeople s cross JOIN customers c WHERE s.sname<c.cname AND c.rating<200;

## 62. List all Salespeople’s names and the Commission they have earned.

    SELECT sname,SUM(comm*amt) as commision FROM salespeople s JOIN orders o on s.snum=o.snum GROUP BY sname;

## 63. Write a query that produces the names and cities of all customers with the same rating as Hoffman. Write the query using Hoffman’s CNUM rather than his rating, so that it would still be usable if his rating changed.

    SELECT cname,city FROM customers WHERE rating=(SELECT rating FROM customers WHERE cnum=2001) AND cnum<>2001;

## 64. Find all salespeople for whom there are customers that follow them in alphabetical order.

    SELECT * FROM customers c JOIN salespeople s on s.snum=c.snum WHERE c.cname<s.sname;

## 65. Write a query that produces the names and ratings of all customers of all who have above average orders.

    SELECT cname,rating, AVG(amt) FROM customers c JOIN orders o on c.cnum=o.cnum 
    GROUP BY
    c.cname,rating having AVG(amt)>(SELECT AVG(amt) FROM orders);

## 66. Find the SUM of all purchases FROM the Orders table.

    SELECT SUM(amt) FROM orders;

## 67. Write a SELECT command that produces the order number, amount and date for all rows in the order table.

    SELECT onum,amt,odate FROM orders;

## 68. COUNT the number of nonNULL rating fields in the Customers table (including repeats).

    SELECT COUNT(rating) FROM customers;

## 69. Write a query that gives the names of both the salesperson and the customer for each order after the order number.

    SELECT onum,cname,sname FROM salespeople s JOIN customers c on s.snum=c.snum JOIN orders o on c.cnum=o.cnum;

## 70. List the commissions of all salespeople servicing customers in London.

    SELECT * FROM salespeople s JOIN customers c on s.snum=c.snum WHERE c.city="London";

## 71. Write a query using ANY or ALL that will find all salespeople who have no customers located in their city.

## 72. Write a query using the EXISTS operator that SELECTs all salespeople with customers located in their cities who are not assigned to them.

    SELECT * FROM salespeople WHERE EXISTS (SELECT * FROM salespeople s , customers c WHERE s.city=c.city AND s.snum<>c.snum);

## 73. Write a query that SELECTs all customers serviced by Peel or Motika. (Hint : The SNUM field relates the two tables to one another.)

    SELECT * FROM customers c , salespeople s WHERE s.snum=c.snum AND s.sname in ('peel','motika');

## 74. COUNT the number of salespeople registering orders for each day. (If a salesperson has more than one order on a given day, he or she should be COUNTed only once.)

    SELECT snum,odate,COUNT(distinct snum) FROM orders GROUP BY odate,snum;

## 75. Find all orders attributed to salespeople in London.

    SELECT * FROM orders o, salespeople s WHERE o.snum=s.snum AND s.city="London";

## 76. Find all orders by customers not located in the same cities as their salespeople.

    SELECT * FROM orders o JOIN salespeople s on o.snum=s.snum JOIN customers c on s.snum=c.snum WHERE c.city<>s.city ;

## 77. Find all salespeople who have customers with more than one current order.

    SELECT s.sname,COUNT(cnum) FROM orders o , salespeople s  WHERE s.snum=o.snum GROUP BY o.cnum,s.sname having COUNT(o.cnum)>1;

## 78. Write a query that extracts FROM the Customers table every customer assigned to a salesperson who currently has at least one other customer (besides the customer being selected) with orders in the Orders table.

## 79. Write a query that SELECTs all customers whose names begin with ‘C’.

     SELECT * FROM customers WHERE cname like "c%";

## 80. Write a query on the Customers table that will find the highest rating in each city. Put the output in this form : for the city (city) the highest rating is : (rating).

    SELECT city,MAX(rating) rating FROM customers GROUP BY city  ;

## 81. Write a query that will produce the SNUM values of all salespeople with orders currently in the Orders table (without any repeats).

    SELECT snum FROM orders GROUP BY snum;

## 82. Write a query that lists customers in descending order of rating. Output the rating field first, followed by the customer’s names and numbers.

    SELECT rating,cname,cnum FROM customers ORDER BY rating desc;

## 83. Find the average commission for salespeople in London.

    SELECT AVG(comm) FROM salespeople WHERE city="london";

## 84. Find all orders credited to the same salesperson who services Hoffman (CNUM 2001).

    SELECT * FROM orders WHERE snum=(SELECT snum FROM customers WHERE cname="Hoffman");

    SELECT * FROM orders WHERE snum=(SELECT snum FROM customers WHERE cname="Hoffman") AND cnum<>2001;

## 85. Find all salespeople whose commission is in between 0.10 and 0.12 (both inclusive).

    SELECT * FROM salespeople WHERE comm between 0.10 AND 0.12;

## 86. Write a query that will give you the names and cities of all salespeople in London with a commission above 0.10.

    SELECT sname,city FROM salespeople WHERE city="London" AND comm>0.10;

## 87. What will be the output FROM the following query?
### SELECT \* FROM ORDERS WHERE (amt < 1000 OR NOT (odate = 10/03/1996 AND cnum > 2003));

## 88. Write a query that SELECTs each customer’s smallest order.

    SELECT min(amt),cnum FROM orders GROUP BY cnum;

## 89. Write a query that SELECTs the first customer in alphabetical order whose name begins with G.

    SELECT * FROM customers WHERE cname>'g' ORDER BY cname ;

## 90. Write a query that COUNTs the number of different nonNULL city values in the Customers table.

    SELECT COUNT(distinct city) FROM customers;

## 91. Find the average amount FROM the Orders table.

    SELECT AVG(amt) FROM orders;

## 92. What would be the output FROM the following query?
### SELECT \* FROM ORDERS WHERE NOT ((odate = 10/03/96 OR snum > 1006) AND amt >=1500);

## 93. Find all customers who are not located in San Jose and whose rating is above 200.

    SELECT * FROM customers WHERE city<>"San Jose" AND rating > 200;

## 94. Give a simpler way to write this query :
### SELECT snum, sname, city, comm FROM salespeople WHERE (comm > + 0.12 OR comm < 0.14);

    SELECT * FROM salespeople ;

## 95. Evaluate the following query :
### SELECT \* FROM orders WHERE NOT ((odate = 10/03/96 AND snum > 1002) OR amt > 2000.00);

## 96. Which salespersons attend to customers not in the city they have been assigned to?

## 97. Which salespeople get commission greater than 0.11 are serving customers rated less than 250?

    SELECT * FROM salespeople s , customers c  WHERE c.snum=s.snum AND comm>0.11 AND rating<250;

    SELECT s.* FROM salespeople s , customers c  WHERE c.snum=s.snum AND comm>0.11 AND rating<250;

## 98. Which salespeople have been assigned to the same city but get different commission percentages?

## 99. Which salesperson has earned the most by way of commission?

    SELECT MAX(commision) FROM (SELECT SUM(comm*amt) as commision FROM salespeople s 
    JOIN 
    orders o on s.snum=o.snum GROUP BY o.snum) as t;

## 100.Does the customer who has placed the maximum number of orders have the maximum rating?

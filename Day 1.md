## 1. Display the name of all employees whose salary is BETWEEN 500 and 1300

    SELECT ename FROM emp WHERE salary BETWEEN 500 AND 1300;

## 2. Display the name of all employees who are in HR dept

    SELECT ename,deptname FROM emp JOIN dept ON emp.deptid=dept.deptid WHERE deptname="HR";

## 3. Display the name of all the dept and COUNT of all the employees in that dept.

    SELECT deptname,COUNT(eid) FROM emp RIGHT JOIN dept ON emp.deptid=dept.deptid GROUP BY deptname;

## 4. Display the name , deptname and salary of the employee whose salary is highest.

    SELECT ename,deptname,salary FROM emp LEFT JOIN dept ON emp.deptid=dept.deptid ORDER BY salary DESC LIMIT 1 ;

    SELECT ename,salary FROM emp WHERE salary=(SELECT MAX(salary) FROM emp);

## 5. Display the name , deptname and salary of the employee whose salary is lowest.

    SELECT ename,deptname,salary FROM emp LEFT JOIN dept ON emp.deptid=dept.deptid ORDER BY salary asc LIMIT 1 ;

    SELECT ename,salary FROM emp WHERE salary=(SELECT MIN(salary) FROM emp);

## 6. Display the name , deptname of employee whose salary is secONd highest.

    SELECT ename,deptname,salary FROM emp LEFT JOIN dept ON emp.deptid=dept.deptid ORDER BY salary DESC LIMIT 1 OFFSET 1 ;

## 7. display the name, deptname of top five earning employees.

    SELECT ename,deptname FROM emp LEFT JOIN dept ON emp.deptid=dept.deptid ORDER BY salary DESC LIMIT 5 ;

## 8. Display the deptname, name of top 5 earning employees FROM each dept.

    SELECT * FROM (
    SELECT deptname,ename,salary,
    row_number() over(partitiON by deptname ORDER BY salary DESC) rn
    FROM emp JOIN dept
    ON emp.deptid=dept.deptid) as t WHERE rn<=5;

    SELECT d.deptname, e.ename AS EmployeeName, e.salary AS Salary FROM emp e JOIN dept d ON e.deptid = d.deptid WHERE (SELECT COUNT(*) FROM emp WHERE deptid = e.deptid AND salary >= e.salary) <= 5 ORDER BY d.deptname, e.salary DESC;

## 9. Display the avg salary of each dept.

     SELECT deptname,AVG(salary) dept_avg_salary FROM emp RIGHT JOIN dept ON emp.deptid=dept.deptid GROUP BY deptname;

## 10.Display the min and MAX salary of each dept.

     SELECT deptname,MIN(salary) dept_min_salary ,MAX(salary) dept_MAX_salary FROM emp RIGHT JOIN dept ON emp.deptid=dept.deptid GROUP BY deptname;

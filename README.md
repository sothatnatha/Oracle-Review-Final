#Reviews Oracle Statement For Final Year3 S1 
--1
SELECT job_id,
DECODE(job_id, 'IT_PROG',salary*1.1,
                'ST-MAN ', salary*1.2,
                'AD_VP',salary*1.3,
                'ST_CLERK', salary*1.4,
                salary
        ) SALARY 
FROM employees;
--2
SELECT department_id,
CASE
    WHEN department_id=50 THEN salary+1000
    WHEN department_id=80 THEN salary+500
    WHEN department_id=90 THEN salary+200
    WHEN department_id=100 THEN salary+100
  ELSE salary
END "SALARY"
FROM employees;

--3
SELECT department_id, SUM(salary), COUNT(*)
FROM employees
GROUP BY department_id;

--4
SELECT department_id, job_id, SUM(salary), COUNT(*) 
FROM employees
HAVING department_id > 40 AND  SUM(salary) > 10000
GROUP BY department_id,job_id,salary
ORDER BY job_id DESC;

--5
SELECT department_id, department_name,location_id,State_province,country_ID
FROM departments 
NATURAL JOIN locations;

--6
SELECT department_id, department_name,location_id,State_province,country_ID
FROM departments JOIN locations
USING(location_id) JOIN countries USING(country_id);

--7
SELECT d.department_id, d.department_name, l.State_province, l.country_ID 
FROM departments d JOIN locations l
ON d.location_id = l.location_id;

--8
SELECT d.department_id, d.department_name, l.State_province, l.country_ID 
FROM departments d JOIN locations l
ON d.location_id = l.location_id
WHERE d.department_id > 100;

--9
SELECT e.last_name "mgr",e.last_name "emp"
FROM EMPLOYEES e JOIN EMPLOYEES m 
ON m.EMPLOYEE_ID = e.MANAGER_ID;

--10
SELECT e.last_name "mgr", e.last_name "emp", COUNT(e.last_name) "total_emp"
FROM EMPLOYEES e JOIN EMPLOYEES m 
ON m.EMPLOYEE_ID = e.MANAGER_ID
GROUP BY m.last_name, e.last_name;

--11
SELECT d.department_id, d.department_name, l.location_id
FROM departments d LEFT OUTER JOIN locations l
ON d.location_id = l.location_id;

--12
SELECT d.department_id, d.department_name, l.location_id, l.state_province,l.country_id
FROM departments d RIGHT OUTER JOIN locations l
ON d.location_id = l.location_id;

--13
SELECT d.department_id, d.department_name, l.location_id, l.state_province,l.country_id
FROM departments d FULL OUTER JOIN locations l
ON d.location_id = l.location_id;

--14
SELECT d.department_id, d.department_name, l.location_id, l.state_province,l.country_id
FROM departments d CROSS JOIN locations l;

--15
-- Join 3 tables
SELECT last_name, city, department_name
FROM employees
CROSS JOIN departments
CROSS JOIN locations;

--16
-- Join 3 tables
SELECT e.last_name, l.city, d.department_name
FROM employees e JOIN departments d
ON d.department_id = e.department_id JOIN locations l
ON d.location_id = l.location_id
WHERE l.city = 'Toronto';

--17
SELECT e.last_name, d.department_id, d.department_name
FROM employees e
CROSS JOIN departments d
WHERE e.last_name like '%an%';


--18
SELECT last_name, department_id, department_name
FROM employees
NATURAL JOIN departments
WHERE department_id in (90,100,101);

--19
SELECT department_id
From employees;
SELECT d.department_id, COUNT(e.last_name) 
FROM employees e
JOIN departments d
ON e.department_id = d.department_id
GROUP BY d.department_id;

--20
SELECT last_name, job_id, salary, hire_date
FROM employees
HAVING salary <= AVG(salary)
GROUP BY last_name, job_id, salary, hire_date;

--21
SELECT last_name, job_id, salary, hire_date
FROM employees
HAVING salary = MIN(salary)
GROUP BY last_name, job_id, salary, hire_date;

--22
SELECT last_name, job_id, salary, hire_date
FROM employees
WHERE job_id = (
    SELECT job_id
    FROM employees 
    WHERE employee_id = 141 ) 
AND salary = (
    SELECT salary
    FROM employees 
    WHERE employee_id = 100 ); -- 0 rows

--23
SELECT last_name, job_id, salary, hire_date
FROM employees
WHERE hire_date = (
    SELECT MIN(hire_date)
    FROM employees
);

--24
SELECT last_name, (sysdate-hire_date)/7 AS WEEKS
FROM  employees;

--25
SELECT sysdate, NEXT_DAY(sysdate, 'MONDAY'),  LAST_DAY(sysdate)
FROM DUAL;

--26
SELECT department_id
FROM departments
MINUS
SELECT department_id
FROM employees
WHERE job_id = 'AD_VP';

--27
SELECT employee_id, job_id
FROM employees
UNION ALL
SELECT employee_id, job_id
FROM job_history;

--28
SELECT employee_id, job_id
FROM employees
INTERSECT
SELECT employee_id, job_id
FROM job_history;

--29
SELECT employee_id, job_id
FROM employees
WHERE employee_id IN ( 
    SELECT employee_id
    FROM employees
    MINUS
    SELECT employee_id
    FROM job_history
); 

--30
SELECT department_id, NULL "location_id", hire_date 
FROM employees 
UNION
SELECT department_id, location_id, NULL
FROM departments;

--31
SELECT employee_id, department_id, salary,  NULL "location_id", hire_date 
FROM employees 
UNION
SELECT NULL, department_id, NULL, location_id, NULL
FROM departments;

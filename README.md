# Pewlett-Hackard-Analysis
## Challenge
For this Challenge, I two deliverables I had to query in SQL. The first deliverable the boss wanted was a data chart reflecting the Number of Retiring Employees by Title where I had to have six columns consisting of Employee number, first name, last name, Title, from_date, and Salary. The second deliverable was to find the mentorship eligibility of the employees which consisted of six columns of Employee number, first name, last name , Title, from_date, and to_date where, according to the challenge description, "to be eligible to participate in the mentorship program, employees will need to have a date of birth that falls between January 1, 1965 and December 31, 1965."

For the first deliverable I had to use the query of:

SELECT e.emp_no,
e.first_name,
e.last_name,
ti.title,
s.from_date,
s.salary
INTO Number_Retiring_Employees_By_Title
FROM employees as e
INNER JOIN titles as ti
ON (e.emp_no = ti.emp_no)
INNER JOIN salaries as s
ON (e.emp_no = s.emp_no)
WHERE (e.birth_date BETWEEN '1952-01-01' AND '1955-12-31')
AND (e.hire_date BETWEEN '1985-01-01' AND '1988-12-31');

I then had to partition the query to get rid of all of the duplicates in job titles and create a data chart coming back with only the most recent job titles for the employees. The partitioning of the code above looked as follows: 

SELECT emp_no,
first_name,
last_name,
title,
from_date,
salary
INTO Recent_Retiree_Titles
FROM
 (SELECT emp_no,
first_name,
last_name,
title,
from_date,
salary, ROW_NUMBER() OVER
 (PARTITION BY (emp_no)
 ORDER BY title DESC) rn
 FROM Number_Retiring_Employees_By_Title
 ) tmp WHERE rn = 1
ORDER BY emp_no;.

This code helped to shore up the duplicate employee names with different titles and provide only the employee names with the most recent titles they had. I then put the data chart queried into a csv file and uploaded it into the repository.
The next deliverable wanted me to find the eligible employees to mentor younger employees who were born in 1965. The first query looked like this: 

SELECT e.emp_no,
e.first_name,
e.last_name,
ti.title,
s.from_date,
s.to_date
INTO Mentorship_Eligibility
FROM employees as e
INNER JOIN titles as ti
ON (e.emp_no = ti.emp_no)
INNER JOIN salaries as s
ON (e.emp_no = s.emp_no)
WHERE (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31');.

It looks very similar to the first query, besides the one column of me deleting the salary and adding the to_date column for the employee data. I then partitioned the data getting rid of the duplicates of title which looked like:

SELECT emp_no,
first_name,
last_name,
title,
from_date,
to_date
INTO New_Mentorship_Eligibility
FROM
 (SELECT emp_no,
first_name,
last_name,
title,
from_date,
to_date, ROW_NUMBER() OVER
 (PARTITION BY (emp_no)
 ORDER BY title DESC) rn
 FROM Mentorship_Eligibility
 ) tmp WHERE rn = 1
ORDER BY emp_no;.

This query was then named New_Mentorship_Eligibility and converted into a csv file and exported into my Data folder.
My problems with this module were in the partitioning section of each deliverable as I kept coming up with errors on my SELECT portions, I had to delete the csv section ahead of the columns listed and it ended up working. 

The results of this challenge of both deliverables are two succinct 20,000 + row employee data charts where management can go in and see which employees who are retiring and who are eligible to mentor (and also born in 1965). The limitations to the analysis queried within SQL are limited to how many columns are displayed. If more information is needed, I would have to create more columns of data and then select them into the query which would then display them on a new data chart. The next steps I would recommend would be to add the departments of each employee so we could see how many employees belonged to each specific department and then create deliverables to each department showing each department the status of each of their respective employees.
![alt text](https://github.com/mhvarner/Pewlett-Hackard-Analysis/blob/master/EmployeeDB.png)

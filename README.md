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
I then had to partition the query to get rid of all of the duplicates in job titles and create a data chart coming back with only the most recent job titles for the employees.

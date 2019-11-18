# Pewlett-Hackard-Analysis

**Project Summary:**

-*Number of individuals retiring= **82761*** 

	-Assumption: retiring criteria is set to be
		-"Birth Date" between 1952-01-01 and 1955-12-31
		-"hiring Date" between 1985-01-01 and 1988-12-31
		-Detailed info can be found on retirement_info.csv

-*Number of individuals being hired= **499999*** 

	-Detailed info can be found on retirement_info.csv  
	
-*Number of individuals available for mentorship role= **1465*** 

	-Assumption: mentorship role criteria is set to be
		-"Birth Date" between 1965-01-01 and 1965-12-31
		-Detailed info can be found on mentor.csv

**Recommendation:**
-*It is recommended to further analyze mentorship eligibility by limiting individuals based on seniority. Also it is possible to identify younger staff based on seniority and priorotize mentorship. It is possible to find the cost based on individules salary and duration of mentorship.* 


https://github.com/hamedhi66/Pewlett-Hackard-Analysis/blob/master/Challenge.png

SELECT e.emp_no,
	e.first_name,
	e.last_name,
	t.title,
	t.from_date,
	s.salary
	
INTO title_retiring

FROM employees as e

RIGHT JOIN titles as t

ON (e.emp_no = t.emp_no)

RIGHT JOIN salaries as s

ON (e.emp_no = s.emp_no)

https://github.com/hamedhi66/Pewlett-Hackard-Analysis/blob/master/title_retirering.png



SELECT emp_no,
	first_name,
	last_name,
	title,
	from_date,
	salary

INTO title_recent

FROM(
	SELECT emp_no, first_name, last_name,
	title, from_date, salary,
    ROW_NUMBER() OVER 
(PARTITION BY (first_name, last_name) ORDER BY from_date DESC) rn
   FROM title_retiring
  ) tmp WHERE rn = 1;

https://github.com/hamedhi66/Pewlett-Hackard-Analysis/blob/master/title_recent.png



SELECT tr.emp_no, 
		tr.first_name, 
		tr.last_name, 
		tr.title, 
		tr.from_date,
		e.birth_date 

INTO retirement_birth

FROM title_recent as tr

left join employees as e

on tr.emp_no=e.emp_no

WHERE (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31');


https://github.com/hamedhi66/Pewlett-Hackard-Analysis/blob/master/retirement_birth.png


SELECT rb.emp_no,
	rb.first_name,
	rb.last_name,
	rb.title, 
	rb.from_date,
	de.to_date

INTO mentor

FROM retirement_birth as rb

LEFT JOIN dept_emp as de

ON rb.emp_no = de.emp_no

WHERE de.to_date = ('9999-01-01');

https://github.com/hamedhi66/Pewlett-Hackard-Analysis/blob/master/Mentor.png

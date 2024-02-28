# SQL-Challenge
Module 9 Asignment
-----------------------------
# Instructions
-----------------------------
This Challenge is divided into three parts: data modeling, data engineering, and data analysis.
-----------------------------
# Technology Used
------------------------------
* PostgreSQL
# Data Modeling
--------------------------------
Inspect the CSV files, and then sketch an Entity Relationship Diagram of the tables. 
To create the sketch, feel free to use a tool like QuickDBDLinks to an external site.
<img width="901" alt="Screenshot 2024-02-28 131415" src="https://github.com/Beyonka86/SQL-Challenge/assets/111611012/73fd6a24-adc0-4706-a285-f1e19d6a0d79">
# Data Engineering
-------------------------------------
1. Use the provided information to create a table schema for each of the six CSV files. Be sure to do the following:
Remember to specify the data types, primary keys, foreign keys, and other constraints.
For the primary keys, verify that the column is unique. Otherwise, create a composite keyLinks to an external site., which takes two primary keys to uniquely identify a row.
Be sure to create the tables in the correct order to handle the foreign keys.
2. Import each CSV file into its corresponding SQL table.
---------------------------------------
-- Create a new table
CREATE TABLE titles (
  title_id VARCHAR NOT NULL PRIMARY KEY,
  title VARCHAR NOT NULL
);

-- View the table data
SELECT * FROM titles

CREATE TABLE employees (
  	emp_no INT NOT NULL PRIMARY KEY,
	emp_title VARCHAR NOT NULL,
	birth_date DATE NOT NULL,
	first_name VARCHAR NOT NULL,
	last_name VARCHAR NOT NULL,
	sex VARCHAR NOT NULL,
	hire_date DATE NOT NULL,
	FOREIGN KEY(emp_title) REFERENCES titles (title_id)
);

-- View the table data
SELECT * FROM employees

-- Create a new table
CREATE TABLE departments (
  dept_no VARCHAR  NOT NULL PRIMARY KEY,
  dept_name VARCHAR NOT NULL
);

-- View the table data
SELECT * FROM departments

-- Create a new table
CREATE TABLE dept_emp (
 	emp_no INT NOT NULL,
	dept_no VARCHAR NOT NULL,
	PRIMARY KEY (emp_no, dept_no),
	FOREIGN KEY(emp_no) REFERENCES employees (emp_no),
	FOREIGN KEY(dept_no) REFERENCES departments (dept_no)
);

-- View the table data
SELECT * FROM dept_emp

-- Create a new table
CREATE TABLE dept_manager (
 	dept_no VARCHAR NOT NULL,
	emp_no INT NOT NULL,
	PRIMARY KEY (emp_no, dept_no),
	FOREIGN KEY(emp_no) REFERENCES employees (emp_no),
	FOREIGN KEY(dept_no) REFERENCES departments (dept_no)
);

-- View the table data
SELECT * FROM dept_manager

-- Create a new table
CREATE TABLE salaries (
	emp_no INT NOT NULL,
	salary INT NOT NULL,
	PRIMARY KEY (emp_no),
	FOREIGN KEY(emp_no) REFERENCES employees (emp_no)
);

-- View the table data
SELECT * FROM salaries

# Data Analysis
------------------------------------------------------------------------------
1. List the employee number, last name, first name, sex, and salary of each employee.

SELECT employees.emp_no, employees.last_name, employees.first_name, employees.sex, salaries.salary
FROM employees
JOIN salaries
ON employees.emp_no = salaries.emp_no;

2. List the first name, last name, and hire date for the employees who were hired in 1986.

SELECT first_name, last_name, hire_date
FROM employees
WHERE  hire_date BETWEEN '1/1/1986' AND '12/31/1986'
ORDER BY  hire_date;

3. List the manager of each department along with their department number, department name, employee number, last name, and first name.

SELECT departments.dept_no, departments.dept_name, dept_manager.emp_no, employees.last_name, employees.first_name
FROM departments
JOIN dept_manager
ON departments.dept_no = dept_manager.dept_no
JOIN employees
ON dept_manager.emp_no = employees.emp_no;

4. List the department number for each employee along with that employee’s employee number, last name, first name, and department name.

SELECT dept_emp.emp_no, employees.last_name, employees.first_name, departments.dept_name
FROM dept_emp
JOIN employees
ON dept_emp.emp_no = employees.emp_no
JOIN departments
ON dept_emp.dept_no = departments.dept_no;

5. List first name, last name, and sex of each employee whose first name is Hercules and whose last name begins with the letter B.

SELECT employees.first_name, employees.last_name, employees.sex
FROM employees
WHERE first_name = 'Hercules' AND last_name LIKE 'B%'

6. List each employee in the Sales department, including their employee number, last name, and first name.

SELECT departments.dept_name, employees.last_name, employees.first_name
FROM dept_emp
JOIN employees
ON dept_emp.emp_no = employees.emp_no
JOIN departments
ON dept_emp.dept_no = departments.dept_no
WHERE departments.dept_name = 'Sales';

7. List each employee in the Sales and Development departments, including their employee number, last name, first name, and department name.

SELECT dept_emp.emp_no, employees.last_name, employees.first_name, departments.dept_name
FROM dept_emp
JOIN employees
ON dept_emp.emp_no = employees.emp_no
JOIN departments
ON dept_emp.dept_no = departments.dept_no
WHERE departments.dept_name = 'Sales' 
OR departments.dept_name = 'Development';

8. List the frequency counts, in descending order, of all the employee last names (that is, how many employees share each last name).

SELECT last_name,
COUNT(last_name) AS "frequency"
FROM employees
GROUP BY last_name
ORDER BY
COUNT(last_name) DESC;

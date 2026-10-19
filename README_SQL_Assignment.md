# SQL Assignment: Beginner to Advanced

![SQL](https://img.shields.io/badge/SQL-Learning-blue?style=for-the-badge&logo=mysql&logoColor=white)
![Level](https://img.shields.io/badge/Level-Beginner%20to%20Advanced-success?style=for-the-badge)
![Practice](https://img.shields.io/badge/Focus-Questions%20%26%20Examples-orange?style=for-the-badge)

A beginner-friendly SQL assignment with short answers and **one simple
example for each question**. The questions are arranged from core
concepts to advanced database features.

> **Note:** SQL syntax varies slightly between database systems.
> Examples mainly use common SQL or MySQL-style syntax. A few questions
> in the original assignment refer to a query whose text was not
> included; for those, a representative example is provided instead.

## Table of Contents

1.  [SQL Fundamentals](#1-sql-fundamentals)
2.  [Creating Tables and Keys](#2-creating-tables-and-keys)
3.  [Filtering, Sorting, and Limiting
    Results](#3-filtering-sorting-and-limiting-results)
4.  [Changing and Removing Data](#4-changing-and-removing-data)
5.  [Aggregate Functions](#5-aggregate-functions)
6.  [GROUP BY and HAVING](#6-group-by-and-having)
7.  [Table Relationships and JOINs](#7-table-relationships-and-joins)
8.  [Subqueries and ALL/ANY](#8-subqueries-and-allany)
9.  [String Functions](#9-string-functions)
10. [Date and Time Functions](#10-date-and-time-functions)
11. [CASE Statements and Logical
    Operators](#11-case-statements-and-logical-operators)
12. [Views and Indexes](#12-views-and-indexes)
13. [Stored Procedures and Triggers](#13-stored-procedures-and-triggers)
14. [Query Optimization](#14-query-optimization)
15. [Window Functions](#15-window-functions)

------------------------------------------------------------------------

## 1. SQL Fundamentals

### 1. What is SQL and why is it important?

SQL (Structured Query Language) is used to create, read, update, and
manage data in relational databases.

**Example**

``` sql
SELECT * FROM Movies;
```

### 2. What is the difference between `WHERE` and `GROUP BY`?

`WHERE` filters individual rows; `GROUP BY` combines rows with the same
value into groups.

**Example**

``` sql
SELECT genre, COUNT(*) FROM Movies WHERE year >= 2020 GROUP BY genre;
```

### 3. Explain `COUNT()` and `SUM()` functions.

`COUNT()` counts rows or non-NULL values. `SUM()` adds numeric values.

**Example**

``` sql
SELECT COUNT(*) AS movie_count, SUM(box_office) AS total_collection FROM Movies;
```

### 4. What is the use of the `LIKE` operator?

`LIKE` matches text against a pattern. `%` matches any number of
characters and `_` matches one character.

**Example**

``` sql
SELECT * FROM Movies WHERE title LIKE 'A%';
```

### 5. What is the purpose of `ORDER BY`?

`ORDER BY` sorts the returned rows in ascending (`ASC`) or descending
(`DESC`) order.

**Example**

``` sql
SELECT title, rating FROM Movies ORDER BY rating DESC;
```

### 6. What is the purpose of `DISTINCT`?

`DISTINCT` removes duplicate combinations from the selected result.

**Example**

``` sql
SELECT DISTINCT genre FROM Movies;
```

### 7. What is the difference between `SELECT` and `SELECT DISTINCT`?

`SELECT` can return repeated values; `SELECT DISTINCT` returns unique
selected values.

**Example**

``` sql
SELECT DISTINCT genre FROM Movies;
```

### 8. Can `DISTINCT` be used on multiple columns?

Yes. It returns unique combinations of the selected columns.

**Example**

``` sql
SELECT DISTINCT genre, release_year FROM Movies;
```

### 9. What is the purpose of the `LIMIT` clause?

`LIMIT` restricts how many rows are returned. It is supported by MySQL
and several other databases.

**Example**

``` sql
SELECT title FROM Movies LIMIT 5;
```

### 10. How is `LIMIT` different from `ORDER BY`?

`ORDER BY` sorts rows; `LIMIT` restricts the number of rows returned.

**Example**

``` sql
SELECT title, rating FROM Movies ORDER BY rating DESC LIMIT 3;
```

### 11. How can you find the top 3 highest-rated movies using `LIMIT`?

Sort ratings from highest to lowest and return three rows.

**Example**

``` sql
SELECT title, rating FROM Movies ORDER BY rating DESC LIMIT 3;
```

### 12. What does `LIMIT 2,3` do?

In MySQL, it skips the first two rows and returns the next three rows.

**Example**

``` sql
SELECT title FROM Movies ORDER BY title LIMIT 2, 3;
```

### 13. What is the output of a query when the assignment does not show the query?

The exact output cannot be determined without the query and its data.
Use the example below to understand the result.

**Example**

``` sql
SELECT 2 + 3 AS answer;
-- Result: 5
```

------------------------------------------------------------------------

## 2. Creating Tables and Keys

### 14. What is a Primary Key in SQL?

A primary key uniquely identifies each row in a table.

**Example**

``` sql
CREATE TABLE Movies (
    movie_id INT PRIMARY KEY,
    title VARCHAR(100)
);
```

### 15. What are the rules of a Primary Key?

A primary key must be unique and cannot contain `NULL`. A table has one
primary-key constraint, which may contain multiple columns.

**Example**

``` sql
CREATE TABLE Reviews (
    movie_id INT,
    reviewer_id INT,
    PRIMARY KEY (movie_id, reviewer_id)
);
```

### 16. Why can't a Primary Key contain `NULL` values?

A primary key must identify every row, and `NULL` means the value is
missing or unknown.

**Example**

``` sql
-- This insert is rejected because movie_id is NULL.
INSERT INTO Movies (movie_id, title) VALUES (NULL, 'Sky Road');
```

### 17. What is a Composite Primary Key?

It is a primary key made from two or more columns together.

**Example**

``` sql
CREATE TABLE Enrolments (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id)
);
```

### 18. What happens if you insert a duplicate Primary Key value?

The database rejects the insert because primary-key values must be
unique.

**Example**

``` sql
INSERT INTO Movies (movie_id, title) VALUES (1, 'First Film');
-- Inserting movie_id = 1 again causes a key-duplicate error.
```

### 19. What is a Foreign Key in SQL?

A foreign key links a column in one table to a primary or unique key in
another table.

**Example**

``` sql
CREATE TABLE Reviews (
    review_id INT PRIMARY KEY,
    movie_id INT,
    FOREIGN KEY (movie_id) REFERENCES Movies(movie_id)
);
```

### 20. What is the difference between a Primary Key and a Foreign Key?

A primary key identifies rows in its own table; a foreign key references
a key in another (or the same) table.

**Example**

``` sql
-- Movies.movie_id is the primary key; Reviews.movie_id is a foreign key.
```

### 21. Why are Foreign Keys important?

They help maintain referential integrity by preventing references to
parent rows that do not exist, subject to the configured rules.

**Example**

``` sql
-- A review cannot reference movie_id 999 unless that movie exists in Movies.
```

### 22. What happens when you insert a Foreign Key value that does not exist in the parent table?

The database normally rejects the insert when foreign-key enforcement is
enabled.

**Example**

``` sql
INSERT INTO Reviews (review_id, movie_id) VALUES (10, 999);
```

### 23. Explain the relationship between Movies and Reviews using Primary Key and Foreign Key.

One movie can have many reviews. `Movies.movie_id` identifies a movie,
and `Reviews.movie_id` refers to it.

**Example**

``` sql
SELECT m.title, r.review_id
FROM Movies m
JOIN Reviews r ON r.movie_id = m.movie_id;
```

### 24. What are Table Relationships in SQL?

Relationships describe how rows in different tables are associated,
commonly using primary and foreign keys.

**Example**

``` sql
-- One customer can place many orders through Orders.customer_id.
```

### 25. What is the difference between One-to-One and One-to-Many relationships?

One-to-one links one row to at most one row; one-to-many lets one parent
row link to many child rows.

**Example**

``` sql
-- One customer can have many orders: Orders.customer_id references Customers.customer_id.
```

### 26. What is a Many-to-Many relationship and how is it implemented?

It allows many rows in each table to relate to many rows in the other. A
junction table implements it.

**Example**

``` sql
CREATE TABLE MovieActors (
    movie_id INT,
    actor_id INT,
    PRIMARY KEY (movie_id, actor_id)
);
```

------------------------------------------------------------------------

## 3. Filtering, Sorting, and Limiting Results

### 27. What are Logical Operators in SQL and why are they used?

`AND`, `OR`, and `NOT` combine or reverse conditions in a query.

**Example**

``` sql
SELECT * FROM Movies WHERE rating >= 8 AND release_year >= 2020;
```

### 28. What is the difference between `AND` and `OR`?

`AND` requires all connected conditions to be true; `OR` requires at
least one to be true.

**Example**

``` sql
SELECT * FROM Movies WHERE genre = 'Drama' OR genre = 'Comedy';
```

### 29. What does the `NOT` operator do in SQL?

`NOT` reverses a Boolean condition.

**Example**

``` sql
SELECT * FROM Movies WHERE NOT genre = 'Horror';
```

### 30. What will SQL execute first in a condition?

Parentheses are evaluated first; then `NOT`, `AND`, and `OR` follow
their precedence rules. Use parentheses to make intent clear.

**Example**

``` sql
SELECT * FROM Movies WHERE genre = 'Drama' AND (rating >= 8 OR release_year >= 2024);
```

### 31. What is the difference between `ASC` and `DESC`?

`ASC` sorts from low to high (or A to Z); `DESC` sorts from high to low
(or Z to A).

**Example**

``` sql
SELECT title, rating FROM Movies ORDER BY rating ASC;
```

### 32. What happens if `ASC` or `DESC` is not specified in `ORDER BY`?

Ascending order (`ASC`) is the default in standard SQL.

**Example**

``` sql
SELECT title FROM Movies ORDER BY title;
```

### 33. Can `ORDER BY` be used on multiple columns?

Yes. SQL sorts by the first column, then uses later columns to break
ties.

**Example**

``` sql
SELECT title, genre, rating FROM Movies ORDER BY genre ASC, rating DESC;
```

------------------------------------------------------------------------

## 4. Changing and Removing Data

### 34. What is the purpose of the `UPDATE` statement?

`UPDATE` changes existing values in a table.

**Example**

``` sql
UPDATE Movies SET rating = 8.5 WHERE movie_id = 1;
```

### 35. Why should we use the `WHERE` clause with `UPDATE`?

It limits which rows are changed. Without it, every row may be updated.

**Example**

``` sql
UPDATE Movies SET genre = 'Drama' WHERE movie_id = 2;
```

### 36. What happens if an `UPDATE` statement is executed without a `WHERE` clause?

Every row in the table is updated.

**Example**

``` sql
UPDATE Movies SET rating = 0;
```

### 37. How can you update multiple columns in one `UPDATE` statement?

List each column assignment separated by commas.

**Example**

``` sql
UPDATE Movies SET title = 'New Title', rating = 8.2 WHERE movie_id = 3;
```

### 38. What is the safer way to update a record: using `movie_name` or `movie_id`? Why?

Use a unique key such as `movie_id`; names may be duplicated or changed.

**Example**

``` sql
UPDATE Movies SET rating = 9 WHERE movie_id = 15;
```

### 39. What is the purpose of the `DELETE` statement?

`DELETE` removes rows from a table.

**Example**

``` sql
DELETE FROM Movies WHERE movie_id = 15;
```

### 40. What is the difference between `DELETE` and `DROP`?

`DELETE` removes selected rows but keeps the table; `DROP TABLE` removes
the table structure and its data.

**Example**

``` sql
DELETE FROM Movies WHERE movie_id = 15;
```

### 41. What happens if `DELETE` is executed without a `WHERE` clause?

All rows are deleted, but the table itself remains.

**Example**

``` sql
DELETE FROM Movies;
```

### 42. Why is using `movie_id` in a `DELETE` statement safer than using `movie_name`?

An ID is usually unique, while multiple movies can share the same name.

**Example**

``` sql
DELETE FROM Movies WHERE movie_id = 7;
```

------------------------------------------------------------------------

## 5. Aggregate Functions

### 43. What are Aggregate Functions in SQL?

Aggregate functions calculate one result from multiple rows, such as a
count, sum, average, minimum, or maximum.

**Example**

``` sql
SELECT MAX(rating) FROM Movies;
```

### 44. What is the difference between `COUNT(*)` and `COUNT(column_name)`?

`COUNT(*)` counts all rows; `COUNT(column_name)` counts only rows where
that column is not `NULL`.

**Example**

``` sql
SELECT COUNT(*) AS all_rows, COUNT(rating) AS rated_rows FROM Movies;
```

### 45. How does `AVG()` work in SQL?

`AVG()` returns the average of non-`NULL` numeric values.

**Example**

``` sql
SELECT AVG(rating) AS average_rating FROM Movies;
```

### 46. Which Aggregate Function finds the highest box office collection?

Use `MAX()` to return the largest collection value.

**Example**

``` sql
SELECT MAX(box_office) AS highest_collection FROM Movies;
```

### 47. What will be the output of the aggregate query mentioned in the assignment?

The source assignment does not include the actual query, so its exact
output cannot be known. This example shows how an aggregate result
works.

**Example**

``` sql
SELECT SUM(box_office) AS total_collection FROM Movies;
```

### 48. Explain `SUM()` with an example.

`SUM()` adds all non-`NULL` numeric values in a column.

**Example**

``` sql
SELECT SUM(box_office) AS total_collection FROM Movies;
```

------------------------------------------------------------------------

## 6. GROUP BY and HAVING

### 49. What is the purpose of `GROUP BY`?

`GROUP BY` groups rows that share the same value so calculations can be
made for each group.

**Example**

``` sql
SELECT genre, COUNT(*) FROM Movies GROUP BY genre;
```

### 50. Why is `GROUP BY` often used with Aggregate Functions?

It calculates a separate aggregate result for each group.

**Example**

``` sql
SELECT genre, AVG(rating) FROM Movies GROUP BY genre;
```

### 51. What is the difference between `GROUP BY` and `ORDER BY`?

`GROUP BY` creates groups; `ORDER BY` sorts the result.

**Example**

``` sql
SELECT genre, COUNT(*) AS total FROM Movies GROUP BY genre ORDER BY total DESC;
```

### 52. What happens if a selected column is not in `GROUP BY` and is not aggregated?

In standard SQL, the query is usually rejected because the column has no
single defined value for each group.

**Example**

``` sql
-- Usually invalid: title is neither grouped nor aggregated.
SELECT genre, title, COUNT(*) FROM Movies GROUP BY genre;
```

### 53. What is the purpose of the `HAVING` clause?

`HAVING` filters groups after grouping and aggregation.

**Example**

``` sql
SELECT genre, COUNT(*) AS total FROM Movies GROUP BY genre HAVING COUNT(*) > 2;
```

### 54. What is the difference between `WHERE` and `HAVING`?

`WHERE` filters rows before grouping; `HAVING` filters groups after
aggregation.

**Example**

``` sql
SELECT genre, AVG(rating) FROM Movies WHERE release_year >= 2020 GROUP BY genre HAVING AVG(rating) > 7;
```

### 55. Why can't aggregate functions usually be used directly in `WHERE`?

`WHERE` filters individual rows before groups and aggregates are
calculated; use `HAVING` for aggregate conditions.

**Example**

``` sql
SELECT genre, AVG(rating) FROM Movies GROUP BY genre HAVING AVG(rating) > 8;
```

### 56. What will be the output of the `HAVING` query mentioned in the assignment?

The original query is not included, so its exact output cannot be
determined. This example returns genres whose average rating exceeds 8.

**Example**

``` sql
SELECT genre, AVG(rating) FROM Movies GROUP BY genre HAVING AVG(rating) > 8;
```

------------------------------------------------------------------------

## 7. Table Relationships and JOINs

### 57. What is the purpose of a `JOIN` in SQL?

A `JOIN` combines related rows from two or more tables.

**Example**

``` sql
SELECT Movies.title, Reviews.review_id
FROM Movies JOIN Reviews ON Movies.movie_id = Reviews.movie_id;
```

### 58. What is the difference between `INNER JOIN` and `LEFT JOIN`?

`INNER JOIN` returns matching rows only; `LEFT JOIN` returns all
left-table rows and matching right-table rows, with `NULL` for missing
matches.

**Example**

``` sql
SELECT m.title, r.review_id FROM Movies m LEFT JOIN Reviews r ON m.movie_id = r.movie_id;
```

### 59. How does `RIGHT JOIN` work?

It returns every row from the right table and matching rows from the
left table; unmatched left-side columns are `NULL`.

**Example**

``` sql
SELECT m.title, r.review_id FROM Movies m RIGHT JOIN Reviews r ON m.movie_id = r.movie_id;
```

### 60. When would you use `FULL JOIN`?

Use it when you need matched rows and unmatched rows from both tables.
Support varies by database.

**Example**

``` sql
SELECT a.id, b.id FROM TableA a FULL OUTER JOIN TableB b ON a.id = b.id;
```

### 61. How can you find unmatched records using JOINs?

Use an outer join and filter for `NULL` in the joined table's key.

**Example**

``` sql
SELECT m.* FROM Movies m LEFT JOIN Reviews r ON m.movie_id = r.movie_id WHERE r.review_id IS NULL;
```

------------------------------------------------------------------------

## 8. Subqueries and ALL/ANY

### 62. What is a Subquery in SQL?

A subquery is a query nested inside another SQL statement.

**Example**

``` sql
SELECT title FROM Movies WHERE rating > (SELECT AVG(rating) FROM Movies);
```

### 63. What is the difference between a Subquery and a Normal Query?

A normal query runs on its own; a subquery is embedded in another query
and supplies a value or result set.

**Example**

``` sql
SELECT title FROM Movies WHERE movie_id IN (SELECT movie_id FROM Reviews);
```

### 64. What is the difference between `IN` and `EXISTS` in subqueries?

`IN` checks whether a value is in a result set; `EXISTS` checks whether
the subquery returns any row.

**Example**

``` sql
SELECT m.title FROM Movies m WHERE EXISTS (SELECT 1 FROM Reviews r WHERE r.movie_id = m.movie_id);
```

### 65. What are correlated subqueries?

A correlated subquery refers to a value from the outer query and is
logically evaluated for each outer row.

**Example**

``` sql
SELECT m.title FROM Movies m WHERE m.rating > (SELECT AVG(x.rating) FROM Movies x WHERE x.genre = m.genre);
```

### 66. Can a subquery be used in `SELECT`, `FROM`, and `WHERE`?

Yes, subqueries can appear in all three places when the subquery returns
a suitable result.

**Example**

``` sql
SELECT title, (SELECT AVG(rating) FROM Movies) AS overall_average FROM Movies;
```

### 67. What is the difference between `ALL` and `ANY` in SQL?

`> ALL` must be true for every value returned by the subquery; `> ANY`
must be true for at least one value.

**Example**

``` sql
SELECT title FROM Movies WHERE rating > ALL (SELECT rating FROM Movies WHERE genre = 'Horror');
```

### 68. Why does `ALL` behave like `AND`?

A comparison with `ALL` must satisfy every value, similar to connecting
the comparisons with `AND`.

**Example**

``` sql
SELECT title FROM Movies WHERE rating > ALL (SELECT rating FROM Movies WHERE genre = 'Horror');
```

### 69. Why does `ANY` behave like `OR`?

A comparison with `ANY` needs to be true for at least one value, similar
to connecting comparisons with `OR`.

**Example**

``` sql
SELECT title FROM Movies WHERE rating > ANY (SELECT rating FROM Movies WHERE genre = 'Horror');
```

### 70. Which operator returns more records, `ALL` or `ANY`?

Neither always returns more records; the result depends on the data and
comparison. `ANY` often matches more rows than `ALL` for the same `>`
comparison.

**Example**

``` sql
SELECT title FROM Movies WHERE rating > ANY (SELECT rating FROM Movies WHERE genre = 'Horror');
```

### 71. Where are `ALL` and `ANY` commonly used?

They are commonly used with comparison operators and subqueries.

**Example**

``` sql
SELECT title FROM Movies WHERE rating >= ANY (SELECT rating FROM Movies WHERE genre = 'Drama');
```

------------------------------------------------------------------------

## 9. String Functions

### 72. What are String Functions in SQL?

String functions manipulate text values, for example by changing case,
joining text, or removing extra spaces.

**Example**

``` sql
SELECT UPPER(title) FROM Movies;
```

### 73. What is the difference between `UPPER()` and `LOWER()`?

`UPPER()` converts text to uppercase; `LOWER()` converts it to
lowercase.

**Example**

``` sql
SELECT UPPER('hello') AS upper_text, LOWER('HELLO') AS lower_text;
```

### 74. How does the `CONCAT()` function work?

`CONCAT()` joins two or more strings into one string.

**Example**

``` sql
SELECT CONCAT('SQL', ' ', 'Learning') AS message;
```

### 75. What is the purpose of the `TRIM()` function?

`TRIM()` removes leading and trailing spaces from a string.

**Example**

``` sql
SELECT TRIM('  SQL  ') AS clean_text;
```

### 76. What will be the output of the string query mentioned in the assignment?

The original query is missing, so its exact output cannot be identified.
This example shows a typical string-function result.

**Example**

``` sql
SELECT UPPER('movie');
-- Result: MOVIE
```

------------------------------------------------------------------------

## 10. Date and Time Functions

### 77. What are Date and Time Functions in SQL?

They work with dates and times, for example to retrieve the current date
or calculate date differences.

**Example**

``` sql
SELECT CURRENT_DATE;
```

### 78. What is the difference between `CURRENT_DATE()` and `NOW()`?

In MySQL, `CURRENT_DATE()` returns today's date; `NOW()` returns the
current date and time.

**Example**

``` sql
SELECT CURRENT_DATE(), NOW();
```

### 79. How can you extract the year from a date column?

Use `YEAR()` in MySQL; other databases may use different date-extraction
syntax.

**Example**

``` sql
SELECT YEAR(release_date) AS release_year FROM Movies;
```

### 80. What is the purpose of `DATEDIFF()`?

In MySQL, `DATEDIFF()` returns the number of days between two dates.

**Example**

``` sql
SELECT DATEDIFF('2026-10-10', '2026-10-01') AS days_between;
```

### 81. What will be the output of the date query mentioned in the assignment?

The source does not include the actual query, so the exact output cannot
be confirmed. This example returns the year from a date.

**Example**

``` sql
SELECT YEAR('2025-06-15') AS year_value;
-- Result: 2025
```

------------------------------------------------------------------------

## 11. CASE Statements and Logical Operators

### 82. What is a `CASE` statement in SQL?

`CASE` returns a value based on the first matching condition.

**Example**

``` sql
SELECT title, CASE WHEN rating >= 8 THEN 'High' ELSE 'Other' END AS rating_level FROM Movies;
```

### 83. Why do we use a `CASE` statement?

It adds conditional logic to SQL results, similar to if/else logic in
programming.

**Example**

``` sql
SELECT title, CASE WHEN rating >= 8 THEN 'Recommended' ELSE 'Not rated high' END FROM Movies;
```

### 84. What is the syntax of a `CASE` statement?

Use `CASE`, one or more `WHEN ... THEN` branches, optional `ELSE`, and
`END`.

**Example**

``` sql
SELECT CASE WHEN 10 > 5 THEN 'Yes' ELSE 'No' END AS answer;
```

### 85. What is the use of `ELSE` in a `CASE` statement?

`ELSE` supplies a fallback value when none of the `WHEN` conditions
matches.

**Example**

``` sql
SELECT CASE WHEN rating >= 8 THEN 'High' ELSE 'Other' END FROM Movies;
```

### 86. What happens if no `WHEN` condition is true and `ELSE` is not used?

The `CASE` expression returns `NULL`.

**Example**

``` sql
SELECT CASE WHEN 2 > 5 THEN 'Yes' END AS answer;
-- Result: NULL
```

------------------------------------------------------------------------

## 12. Views and Indexes

### 87. What is a View in SQL?

A view is a saved query that can be used like a virtual table.

**Example**

``` sql
CREATE VIEW HighlyRatedMovies AS SELECT title, rating FROM Movies WHERE rating >= 8;
```

### 88. How is a View different from a Table?

A table stores data itself; a normal view stores a query definition and
displays data from its underlying tables.

**Example**

``` sql
SELECT * FROM HighlyRatedMovies;
```

### 89. What are the advantages of using Views?

Views can simplify complex queries, limit which columns users see, and
provide consistent reporting logic.

**Example**

``` sql
CREATE VIEW MovieTitles AS SELECT title FROM Movies;
```

### 90. How do you create and delete a View?

Use `CREATE VIEW` to create it and `DROP VIEW` to remove it. Exact
syntax can vary by database.

**Example**

``` sql
DROP VIEW MovieTitles;
```

### 91. Why are Views commonly used in reporting systems?

They provide reusable, simplified datasets for reports and dashboards.

**Example**

``` sql
CREATE VIEW GenreCounts AS SELECT genre, COUNT(*) AS total FROM Movies GROUP BY genre;
```

### 92. What is an Index in SQL?

An index is a database structure that can help the database find rows
faster.

**Example**

``` sql
CREATE INDEX idx_movies_title ON Movies(title);
```

### 93. Why do we use an Index?

Indexes can speed up searches, joins, and sorting, depending on the
query and database.

**Example**

``` sql
CREATE INDEX idx_movies_year ON Movies(release_year);
```

### 94. What is the syntax to create an Index?

Use `CREATE INDEX`, followed by an index name, table name, and column
list.

**Example**

``` sql
CREATE INDEX idx_movies_rating ON Movies(rating);
```

### 95. What is a Composite Index?

A composite index contains more than one column. Column order matters
for how it can be used.

**Example**

``` sql
CREATE INDEX idx_genre_rating ON Movies(genre, rating);
```

### 96. Why should we not create an Index on every column?

Indexes use storage and can slow down inserts, updates, and deletes
because they must be maintained.

**Example**

``` sql
CREATE INDEX idx_movies_genre ON Movies(genre);
```

------------------------------------------------------------------------

## 13. Stored Procedures and Triggers

### 97. What is a Stored Procedure in SQL?

A stored procedure is a named set of SQL statements stored in the
database and executed when called.

**Example**

``` sql
-- MySQL syntax
DELIMITER //
CREATE PROCEDURE GetMovies()
BEGIN
    SELECT * FROM Movies;
END //
DELIMITER ;
```

### 98. Why do we use Stored Procedures?

They can reuse database logic, centralize operations, and help manage
permissions.

**Example**

``` sql
CALL GetMovies();
```

### 99. How do we execute a Stored Procedure?

In MySQL, use `CALL` followed by the procedure name and any required
arguments.

**Example**

``` sql
CALL GetMovies();
```

### 100. What is the use of a parameter in a Stored Procedure?

A parameter passes input to or returns output from a procedure,
depending on its mode.

**Example**

``` sql
-- Example procedure parameter: IN target_year INT
```

### 101. What is the difference between a Stored Procedure and a normal SQL query?

A normal query is typically sent and run directly; a stored procedure is
saved in the database and can be called repeatedly.

**Example**

``` sql
CALL GetMovies();
```

### 102. What is a Trigger in SQL?

A trigger is database code that runs automatically when a specified
event occurs on a table or view.

**Example**

``` sql
-- Trigger event example: run logic AFTER INSERT ON Reviews.
```

### 103. Why do we use a Trigger?

Triggers can automatically enforce rules or record changes when data is
inserted, updated, or deleted.

**Example**

``` sql
-- An AFTER INSERT trigger can write the new review ID to an audit table.
```

### 104. What is the difference between a `BEFORE` Trigger and an `AFTER` Trigger?

A `BEFORE` trigger runs before the event is applied; an `AFTER` trigger
runs after the event.

**Example**

``` sql
-- BEFORE INSERT can validate incoming data before it is stored.
```

### 105. What is the use of `OLD` and `NEW` in a Trigger?

They refer to row values before and after a change, where supported by
the database and trigger event.

**Example**

``` sql
-- In an UPDATE trigger, OLD.rating is the previous rating and NEW.rating is the new rating.
```

### 106. Can we use `OLD` and `NEW` in every Trigger?

No. Availability depends on the database and event: for example, `OLD`
is generally used for DELETE/UPDATE and `NEW` for INSERT/UPDATE in MySQL
row triggers.

**Example**

``` sql
-- In a MySQL DELETE trigger, OLD.movie_id is available; NEW is not.
```

------------------------------------------------------------------------

## 14. Query Optimization

### 107. What is Query Optimization in SQL?

Query optimization is the process of choosing an efficient way to
execute a query.

**Example**

``` sql
SELECT title FROM Movies WHERE movie_id = 10;
```

### 108. Why should we avoid `SELECT *`?

Selecting only needed columns can reduce data transfer and make queries
easier to maintain.

**Example**

``` sql
SELECT title, rating FROM Movies;
```

### 109. How does an Index help in Query Optimization?

An appropriate index can reduce the amount of data the database must
scan for a query.

**Example**

``` sql
CREATE INDEX idx_movies_id ON Movies(movie_id);
```

### 110. What is the use of `EXPLAIN` in SQL?

`EXPLAIN` shows the database's planned approach to running a query;
details depend on the database system.

**Example**

``` sql
EXPLAIN SELECT title FROM Movies WHERE movie_id = 10;
```

### 111. Can too many indexes slow down a database?

Yes. Too many indexes consume storage and add work to data changes, even
if they can help some reads.

**Example**

``` sql
-- Add an index only when query patterns and testing justify it.
CREATE INDEX idx_movies_genre ON Movies(genre);
```

------------------------------------------------------------------------

## 15. Window Functions

### 112. What is a Window Function in SQL?

A window function calculates a value across related rows while keeping
each row in the result.

**Example**

``` sql
SELECT title, rating, AVG(rating) OVER () AS overall_average FROM Movies;
```

### 113. Why do we use `OVER()` in SQL?

`OVER()` defines the rows used by a window function without collapsing
them into one row per group.

**Example**

``` sql
SELECT title, RANK() OVER (ORDER BY rating DESC) AS rating_rank FROM Movies;
```

### 114. What is the difference between `GROUP BY` and Window Functions?

`GROUP BY` normally returns one row per group; window functions retain
the individual rows and add calculations.

**Example**

``` sql
SELECT title, genre, AVG(rating) OVER (PARTITION BY genre) AS genre_average FROM Movies;
```

### 115. What is the difference between `ROW_NUMBER()` and `RANK()`?

`ROW_NUMBER()` gives every row a unique sequence number; `RANK()` gives
tied values the same rank and leaves gaps after ties.

**Example**

``` sql
SELECT title, RANK() OVER (ORDER BY rating DESC) AS movie_rank FROM Movies;
```

### 116. What is the difference between `RANK()` and `DENSE_RANK()`?

Both give tied values the same rank; `RANK()` leaves gaps, while
`DENSE_RANK()` does not.

**Example**

``` sql
SELECT title, DENSE_RANK() OVER (ORDER BY rating DESC) AS movie_rank FROM Movies;
```

### 117. What is the use of `PARTITION BY`?

`PARTITION BY` divides rows into groups for a window function, with
calculations restarting in each group.

**Example**

``` sql
SELECT title, genre, ROW_NUMBER() OVER (PARTITION BY genre ORDER BY rating DESC) AS genre_position FROM Movies;
```

### 118. How can you find department-wise employee ranking?

Use `RANK()` with `PARTITION BY department` and order by salary or
another ranking measure.

**Example**

``` sql
SELECT employee_name, department, RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_rank FROM Employees;
```

### 119. What is the use of `LAG()`?

`LAG()` retrieves a value from a previous row in the window ordering.

**Example**

``` sql
SELECT sale_date, amount, LAG(amount) OVER (ORDER BY sale_date) AS previous_amount FROM Sales;
```

### 120. What is the use of `LEAD()`?

`LEAD()` retrieves a value from a later row in the window ordering.

**Example**

``` sql
SELECT sale_date, amount, LEAD(amount) OVER (ORDER BY sale_date) AS next_amount FROM Sales;
```

### 121. How can you calculate sales growth using SQL?

Use `LAG()` to compare each period's sales with the previous period,
then calculate the percentage change.

**Example**

``` sql
SELECT month, sales,
       100.0 * (sales - LAG(sales) OVER (ORDER BY month))
       / NULLIF(LAG(sales) OVER (ORDER BY month), 0) AS growth_percent
FROM MonthlySales;
```

### 122. What is the use of `NTILE()`?

`NTILE(n)` divides ordered rows into up to `n` roughly equal groups,
often called buckets.

**Example**

``` sql
SELECT employee_name, NTILE(4) OVER (ORDER BY salary DESC) AS salary_quartile FROM Employees;
```

### 123. What is the use of `CUME_DIST()`?

`CUME_DIST()` calculates the cumulative distribution of a row within its
window, ranging above 0 and up to 1.

**Example**

``` sql
SELECT employee_name, CUME_DIST() OVER (ORDER BY salary) AS cumulative_distribution FROM Employees;
```

### 124. What is a Running Total?

A running total is the cumulative sum up to the current row in a chosen
order.

**Example**

``` sql
SELECT sale_date, amount, SUM(amount) OVER (ORDER BY sale_date) AS running_total FROM Sales;
```

### 125. How can you calculate a running total using a Window Function?

Use `SUM()` with an ordered window frame.

**Example**

``` sql
SELECT sale_date, amount,
       SUM(amount) OVER (ORDER BY sale_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
FROM Sales;
```

### 126. Why is `LAST_VALUE()` sometimes confusing?

Its result depends on the window frame. The default frame may end at the
current row or its peers rather than the final row of the whole
partition.

**Example**

``` sql
SELECT sale_date, amount,
       LAST_VALUE(amount) OVER (
           ORDER BY sale_date
           ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
       ) AS final_amount
FROM Sales;
```

------------------------------------------------------------------------

## Key Learning Outcomes

After completing this assignment, you should be able to:

-   Write queries to retrieve, filter, sort, and limit data.
-   Create tables and understand primary keys, foreign keys, and
    relationships.
-   Use joins, subqueries, aggregate functions, and grouping.
-   Work with strings, dates, conditional logic, and data changes.
-   Understand views, indexes, stored procedures, triggers, and query
    plans.
-   Use window functions for ranking, comparisons, and running totals.

## Practice Tips

1.  Create small sample tables such as `Movies`, `Reviews`, `Employees`,
    and `Sales`.
2.  Run each example and compare the output with the explanation.
3.  Always check your `WHERE` condition before running `UPDATE` or
    `DELETE`.
4.  Check your database's documentation when syntax differs between
    MySQL, PostgreSQL, SQL Server, or SQLite.

------------------------------------------------------------------------

**Happy Learning and Keep Practicing SQL!** 🚀

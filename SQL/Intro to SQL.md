SQL 
===

This document is my personal lecture note and it is based on SQL course from [Datacamp](https://learn.datacamp.com/courses/introduction-to-sql) and others website. I do not have any Copy Rights on this content. 

- [SQL](#sql)
- [Selecting columns](#selecting-columns)
    - [Selecting single columns](#selecting-single-columns)
    - [Selecting multiple columns](#selecting-multiple-columns)
    - [Selecting distinct](#selecting-distinct)
    - [Counting](#counting)
- [Filtering results](#filtering-results)
  - [WHERE and Conditions](#where-and-conditions)
    - [AND](#and)
    - [OR](#or)
    - [BETWEEN](#between)
    - [WHERE IN](#where-in)
    - [IS NULL and IS NOT NULL](#is-null-and-is-not-null)
    - [LIKE and NOT LIKE](#like-and-not-like)
- [Aggregate functions](#aggregate-functions)
  - [Combining aggregate functions with WHERE](#combining-aggregate-functions-with-where)
  - [Arithmetic operations](#arithmetic-operations)
    - [Creating alias with AS](#creating-alias-with-as)
- [Sorting and grouping](#sorting-and-grouping)
  - [ORDER BY](#order-by)
  - [GROUP BY](#group-by)
  - [HAVING](#having)
- [REFERENCES](#references)


# Selecting columns

### Selecting single columns

```SQL
SELECT name
FROM people;
```

### Selecting multiple columns

```SQL
SELECT name, birthdate
FROM people;
```
### Selecting distinct

Often your results will include many duplicate values. If you want to select all the unique values from a column, you can use the DISTINCT keyword.

```SQL
SELECT DISTINCT language
FROM films;
```

### Counting

The COUNT statement lets you do this by returning the number of rows in one or more columns.

```SQL
SELECT COUNT(*)
FROM people;
```

For example, to count the number of birth dates present in the people table:

```SQl
SELECT COUNT(birthdate)
FROM people;
```
It's also common to combine COUNT with DISTINCT to count the number of distinct values in a column.

For example, this query counts the number of distinct birth dates contained in the people table:

```sql
SELECT COUNT(DISTINCT birthdate)
FROM people;
```

# Filtering results

In SQL, the WHERE keyword allows you to filter based on both text and numeric values in a table. There are a few different comparison operators you can use:

* `= equal`
* `<> not equal`
* `< less than`
* `> greater than`
* `<= less than or equal to`
* `>= greater than or equal to`

For example, you can filter text records such as title. The following code returns all films with the title 'Metropolis':

```SQL
SELECT title
FROM films
WHERE title = 'Metropolis';
```

Notice that the `WHERE` clause always comes after the `FROM` statement!

Note that in this course we will use `<>` and not `!=` for the not equal operator, as per the SQL standard.

Example: 

```SQL
SELECT title
FROM films
WHERE release_year > 2000;
```

The following query selects all details for films with a budget over ten thousand dollars:

```SQL
SELECT *
FROM films
WHERE budget > 10000;
```
The following query selects the number of films released before 2000.

```SQL
SELECT COUNT(*)
FROM films
WHERE release_year < 2000;
```

This query gets the titles of all films which were filmed in China:

```SQL
SELECT title
FROM films
WHERE country = 'China';
```
This one gets the number of Hindi language films:

```SQL
SELECT COUNT(language)
FROM films
WHERE language = 'Hindi';
```

## WHERE and Conditions

It's possible to combine multiples logical conditions with `WHERE` statement to query specific results.

### AND

This query gives the titles of films released between 1994 and 2000.

```SQL
SELECT title
FROM films
WHERE release_year > 1994
AND release_year < 2000;
```

This one gives the titles of films released between 1994 and 2000.

```SQL
SELECT title, release_year
FROM films
WHERE language = 'Spanish' 
AND release_year < 2000; 
```

Gets all details for Spanish language films released after 2000, but before 2010.

```SQL
SELECT *
FROM films
WHERE release_year > 2000 AND release_year < 2010
AND language = 'Spanish';
```

### OR

Logical operator if it's needed to select rows based on multiple conditions where some but not all of the conditions.

For example, the following returns all films released in either 1994 or 2000:

```SQL
SELECT title
FROM films
WHERE release_year = 1994
OR release_year = 2000;
```

When combining AND and OR, be sure to enclose the individual clauses in parentheses, like so:

```SQL
SELECT title
FROM films
WHERE (release_year = 1994 OR release_year = 1995)
AND (certification = 'PG' OR certification = 'R');
```

### BETWEEN

`BETWEEN` is used when is needed to query rows in a range, like get titles of all films released in and between 1994 and 2000:

```SQL
SELECT title
FROM films
WHERE release_year
BETWEEN 1994 AND 2000;
```

It's important to remember that `BETWEEN` is inclusive, meaning the beginning and end values are included in the results!

### WHERE IN

As you've seen, WHERE is very useful for filtering results. However, if you want to filter based on many conditions, WHERE can get unwieldy. For example:

```SQL
SELECT name
FROM kids
WHERE age = 2
OR age = 4
OR age = 6
OR age = 8
OR age = 10;
```

Enter the IN operator! The IN operator allows you to specify multiple values in a WHERE clause, making it easier and quicker to specify multiple OR conditions! Neat, right?

So, the above example would become simply:

```SQL
SELECT name
FROM kids
WHERE age IN (2, 4, 6, 8, 10);
```

**Example:**

Get the title and release year of all films released in 1990 or 2000 that were longer than two hours. Remember, duration is in minutes!

```SQL
SELECT title, release_year
FROM films
WHERE release_year IN (1990,2000)
AND duration > 120;
```

### IS NULL and IS NOT NULL

In SQL, NULL represents a missing or unknown value. You can check for NULL values using the expression `IS NULL`. For example, to count the number of missing birth dates in the people table:

```SQL
SELECT COUNT(*)
FROM people
WHERE birthdate IS NULL;
```

Sometimes, you'll want to filter out missing values so you only get results which are not NULL. To do this, you can use the IS NOT NULL operator.

For example, this query gives the names of all people whose birth dates are not missing in the people table.

```SQL
SELECT name
FROM people
WHERE birthdate IS NOT NULL;
```

### LIKE and NOT LIKE

Sometimes is needed to search for a pattern rather than a specific text string.

In SQL, the `LIKE` operator can be used in a `WHERE` clause to search for a pattern in a column. To accomplish this, you use something called a wildcard as a placeholder for some other values. There are two wildcards you can use with LIKE:

The `%` wildcard will match zero, one, or many characters in text. For example, the following query matches companies like `'Data'`, `'DataC'`, `'DataCamp'`, `'DataMind'`, and so on:

```SQL
SELECT name
FROM companies
WHERE name LIKE 'Data%';
```
The `_` wildcard will match a single character. For example, the following query matches companies like `'DataCamp'`, `'DataComp'`, and so on:

```SQL
SELECT name
FROM companies
WHERE name LIKE 'DataC_mp';
```
You can also use the `NOT LIKE` operator to find records that don't match the pattern you specify.

**Examples**

Get the names of people whose names have 'r' as the second letter. The pattern you need is `'_r%'`.

```SQL
SELECT name 
FROM people
WHERE name LIKE '_r%';
```

Get the names of people whose names don't start with A. The pattern you need is 'A%'.

```SQL
SELECT name 
FROM people
WHERE name NOT LIKE 'A%';
```

---

# Aggregate functions

Often, you will want to perform some calculation on the data in a database. SQL provides a few functions, called aggregate functions, to help you out with this.

For example,

```SQL
SELECT AVG(budget)
FROM films;
```

gives you the average value from the `budget` column of the `films` table. Similarly, the `MAX` function returns the highest budget:

```SQL
SELECT MAX(budget)
FROM films;
```

Here the query gives the `MIN` budget. 

```SQL
SELECT MIN(budget)
FROM films;
```

The `SUM` function returns the result of adding up the numeric values in a column:

```SQL
SELECT SUM(budget)
FROM films;
```

## Combining aggregate functions with WHERE

To get the total budget of movies made in the year 2010 or later:

```SQL
SELECT SUM(budget)
FROM films
WHERE release_year >= 2010;
```
## Arithmetic operations

It's possible to use basic arithmetic operators, like `+`, `-`, `/` and `*`.

So, for example, this gives a result of 12:

```SQL
SELECT (4 * 3);
```
However, the following gives a result of 1:

```SQL
SELECT (4 / 3);
```
It happens because SQL assumes that if you divide an integer by an integer, you want to get an integer back. So, to get more precion when dividing, do: 

```SQL
SELECT (4.0 / 3.0) AS result;
```

the result is: `1.333`.

### Creating alias with AS

SQL allows us to do something called aliasing. Aliasing simply means assign a temporary name to something. To alias, use the AS keyword.

For example, in the above example we could use aliases to make the result clearer:

```SQL
SELECT MAX(budget) AS max_budget,
       MAX(duration) AS max_duration
FROM films;
```

The query return this: 

max_budget | max_duration |
---------|----------|
 12215500000 | 334 | 

# Sorting and grouping

## ORDER BY

In SQL, the `ORDER BY` keyword is used to sort results in ascending or descending order according to the values of one or more columns.

By default `ORDER BY` will sort in ascending order. If you want to sort the results in descending order, you can use the `DESC` keyword. For example,

```SQL
SELECT title
FROM films
ORDER BY release_year DESC;
```

Get the title of films released in 2000 or 2012, in the order they were released.

```SQL
SELECT title
FROM films
WHERE release_year IN (2000,2012)
ORDER BY release_year;
```

`ORDER BY` can also be used to sort on multiple columns. It will sort by the first column specified, then sort by the next, then the next, and so on. For example,

```SQL
SELECT birthdate, name
FROM people
ORDER BY birthdate, name;
```
sorts on birth dates first (oldest to newest) and then sorts on the names in alphabetical order. **The order of columns is important!**

Try using `ORDER BY` to sort multiple columns! Remember, to specify multiple columns you separate the column names with a comma.

## GROUP BY

In SQL, `GROUP BY` allows us to group/aggregate a result by one or more columns. For example, if you want to count the number of male and female employees in your company, do: 

```SQL
SELECT sex, count(*)
FROM employees
GROUP BY sex;
```

The query results: 

column | sex 
------- | -------
male  | 15
female  | 19


Commonly, `GROUP BY` is used with aggregate functions like `COUNT()` or `MAX()`. Note that `GROUP BY` always goes after the `FROM` clause!

**Example**

Get the country, release year, and lowest amount grossed per release year per country. Order your results by country and release year.

```SQL
SELECT release_year, country, MIN(gross)
FROM films
GROUP BY release_year, country
ORDER BY country, release_year;
```

## HAVING

In SQL, aggregate functions can't be used in `WHERE` clauses. For example, the following query is invalid:

```SQL
SELECT release_year
FROM films
GROUP BY release_year
WHERE COUNT(title) > 10;
```
This means that if you want to filter based on the result of an aggregate function, you need another way! That's where the `HAVING` clause comes in. For example,

```SQL
SELECT release_year
FROM films
GROUP BY release_year
HAVING COUNT(title) > 10;
```
shows only those years in which more than 10 films were released.

Now you're going to write a query that returns the average budget and average gross earnings for films in each year after 1990, if the average budget is greater than $60 million.

```SQL
SELECT release_year, AVG(budget) AS avg_budget,  AVG(gross) AS avg_gross
FROM films
WHERE release_year > 1990
GROUP BY release_year
HAVING AVG(budget) > 60000000
ORDER BY avg_gross DESC;
```
---
# REFERENCES

1 [DataCamp - Intro to SQL](https://learn.datacamp.com/courses/introduction-to-sql)

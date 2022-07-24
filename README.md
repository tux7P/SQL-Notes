# Fundamentals

A repository that serves as a SQL refresher notes

## LIMIT

```bash
SELECT *
FROM people
LIMIT 10;
```

## DISTINCT

If you want to select all the unique values from a column, you can use the DISTINCT keyword.

```bash
SELECT DISTINCT language
FROM films;
```

## COUNT

The COUNT() function lets you do this by returning the number of rows in one or more columns.

```bash
SELECT COUNT(*)
FROM people;

SELECT COUNT(birthdate)
FROM people;

SELECT COUNT(DISTINCT birthdate)
FROM people;

```

## Filtering

Filtering results

Congrats on finishing the first chapter! You now know how to select columns and perform basic counts. This chapter will focus on filtering your results.

In SQL, the WHERE keyword allows you to filter based on both text and numeric values in a table. There are a few different comparison operators you can use:

```bash
    = equal
    <> not equal
    < less than
    > greater than
    <= less than or equal to
    >= greater than or equal to
```

For example, you can filter text records such as title. The following code returns all films with the title 'Metropolis':

```bash
SELECT title
FROM films
WHERE title = 'Metropolis';
```

Notice that the WHERE clause always comes after the FROM statement!

Note that in this course we will use <> and not != for the not equal operator, as per the SQL standard.

## WHERE AND

You can build up your WHERE queries by combining multiple conditions with the AND keyword.

```bash
SELECT title
FROM films
WHERE release_year > 1994
AND release_year < 2000;
```

## WHERE AND OR

What if you want to select rows based on multiple conditions where some but not all of the conditions need to be met? For this, SQL has the OR operator.

For example, the following returns all films released in either 1994 or 2000:

```bash
SELECT title
FROM films
WHERE release_year = 1994
OR release_year = 2000;
```

Note that you need to specify the column for every OR condition, so the following is invalid:

```bash
SELECT title
FROM films
WHERE release_year = 1994 OR 2000;
```
When combining AND and OR, be sure to enclose the individual clauses in parentheses, like so:

```bash
SELECT title
FROM films
WHERE (release_year = 1994 OR release_year = 1995)
AND (certification = 'PG' OR certification = 'R');
```

Otherwise, due to SQL's precedence rules, you may not get the results you're expecting!

## Between

Following query results titles of all films released in and between 1994 and 2000:

```bash
SELECT title
FROM films
WHERE release_year >= 1994
AND release_year <= 2000;
```

Checking for ranges like this is very common, so in SQL the BETWEEN keyword provides a useful shorthand for filtering values within a specified range. This query is equivalent to the one above:

```bash
SELECT title
FROM films
WHERE release_year
BETWEEN 1994 AND 2000;
```

It's important to remember that BETWEEN is inclusive, meaning the beginning and end values are included in the results!

Example:
```bash
SELECT name
FROM kids
WHERE age BETWEEN 2 AND 12
AND nationality = 'USA';
```

## WHERE IN

```bash
SELECT name
FROM kids
WHERE age = 2
OR age = 4
OR age = 6
OR age = 8
OR age = 10;
```

```bash
SELECT name
FROM kids
WHERE age IN (2, 4, 6, 8, 10);
```

## NULL and IS NULL

In SQL, NULL represents a missing or unknown value. You can check for NULL values using the expression IS NULL. For example, to count the number of missing birth dates in the people table:

```bash
SELECT COUNT(*)
FROM people
WHERE birthdate IS NULL;
```
As you can see, IS NULL is useful when combined with WHERE to figure out what data you're missing.

Sometimes, you'll want to filter out missing values so you only get results which are not NULL. To do this, you can use the IS NOT NULL operator.

For example, this query gives the names of all people whose birth dates are not missing in the people table.

```bash
SELECT name
FROM people
WHERE birthdate IS NOT NULL;
```
## LIKE and NOT LIKE

the WHERE clause can be used to filter text data. However, so far you've only been able to filter by specifying the exact text you're interested in. In the real world, often you'll want to search for a pattern rather than a specific text string.

In SQL, the LIKE operator can be used in a WHERE clause to search for a pattern in a column. To accomplish this, you use something called a wildcard as a placeholder for some other values. There are two wildcards you can use with LIKE:

The % wildcard will match zero, one, or many characters in text. For example, the following query matches companies like 'Data', 'DataC' 'DataCamp', 'DataMind', and so on:

```bash
SELECT name
FROM companies
WHERE name LIKE 'Data%';
```
The _ wildcard will match a single character. For example, the following query matches companies like 'DataCamp', 'DataComp', and so on:

```bash
SELECT name
FROM companies
WHERE name LIKE 'DataC_mp';
```

You can also use the NOT LIKE operator to find records that don't match the pattern you specify.

## Aggregate functions

Often, you will want to perform some calculation on the data in a database. SQL provides a few functions, called aggregate functions, to help you out with this.

For example,

SELECT AVG(budget)
FROM films;

gives you the average value from the budget column of the films table. Similarly, the MAX() function returns the highest budget:

SELECT MAX(budget)
FROM films;

The SUM() function returns the result of adding up the numeric values in a column:

SELECT SUM(budget)
FROM films;

You can probably guess what the MIN() function does!

## Combining aggregate functions with WHERE

Aggregate functions can be combined with the WHERE clause to gain further insights from your data.

For example, to get the total budget of movies made in the year 2010 or later:

SELECT SUM(budget)
FROM films
WHERE release_year >= 2010;

## A note on arithmetic

In addition to using aggregate functions, you can perform basic arithmetic with symbols like +, -, *, and /.

So, for example, this gives a result of 12:

SELECT (4 * 3);

However, the following gives a result of 1:

SELECT (4 / 3);

What's going on here?

SQL assumes that if you divide an integer by an integer, you want to get an integer back. So be careful when dividing!

If you want more precision when dividing, you can add decimal places to your numbers. For example,

SELECT (4.0 / 3.0) AS result;

gives you the result you would expect: 1.333.

## It's AS simple AS aliasing

You may have noticed in the first exercise of this chapter that the column name of your result was just the name of the function you used. For example,

SELECT MAX(budget)
FROM films;

gives you a result with one column, named max. But what if you use two functions like this?

SELECT MAX(budget), MAX(duration)
FROM films;

Well, then you'd have two columns named max, which isn't very useful!

To avoid situations like this, SQL allows you to do something called aliasing. Aliasing simply means you assign a temporary name to something. To alias, you use the AS keyword, which you've already seen earlier in this course.

For example, in the above example we could use aliases to make the result clearer:

SELECT MAX(budget) AS max_budget,
       MAX(duration) AS max_duration
FROM films;

Aliases are helpful for making results more readable!
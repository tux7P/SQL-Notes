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

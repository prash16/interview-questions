# SQL fundamentals:
- Primary Keys: unique, cannot be null
- Foreign keys: set of fields in 1 table that is used to reference a tuple in another table -- correspond to primary key of other relation

- Integrity Constraints:
	- Can an ID1 be related to multiple ID2(s)?

- Cardinality constrains:
	- one-to-one
	- one-to-many
	- many-to-one
	- many-to-many

# VIEWS

Define views globally --> `CREATE VIEW`

```
CREATE VIEW view_name AS
SELECT ...
FROM ...
WHERE ...
```

Define views locally to a query --> `WITH` clause

```
WITH BRReserves(sid, bid, color, day) AS
		(SELECT S.sid, B.bid, color, day
		 FROM Boats B, Reserves R
		 WHERE B.bid = R.bid AND (color = ‘red’ OR color = ‘blue’))
SELECT S.sid, sname
FROM Sailors S, BRReserves R
WHERE S.sid = R.sid AND date > ‘1/1/17’
```

# ----.... TABLE-----

DROP TABLE table_name

--> Delete both schema and tuples

ALTER TABLE name_of_table
	ADD COLUMN col_name
--> Add new field, all 	tuples in this new field will be NULL

INSERT
INTO table_name(var1, var2)
VALUES ("abc", "def")

--> insert values for a single tuple

DELETE
FROM table_name
WHERE

--> delete all tuples with conditions

UPDATE table_name
SET var1 = var1 + 1
[WHERE condition1]

--> Update tuples with(out) conditions

# ----NESTED QUERIES----

WHERE clause contains SQL query - SELECT list

# --- DISTINCT ---
Find unique id

```
SELECT DISTINCT S.id, R.bid
FROM ...
```

# --- SELF-JOIN ---

Find pairs of ids of sailors of the same age.

```
Select s1.sid, s2.sid
from sailors s1, sailors s2
where s1.age = s2.age
```

# ----COUNT----

- Count how many rows in a table:
select count(`*`)
from table_name;

- Count number of non-missing values in a column:
select count(column_name)
from table_name;

- Count unique values in a column:
select count (distinct column_name)  ---> has to be like this, otherwise yield error
from table_name;



# ----MISSING VALUES----

where field IS NULL

where field IS NOT NULL

# Numbers

`Select (10/3)` gives integer 3 instead of 3.3333 since SQL thinks that you enter integers and might want integer back.
If want more precision --> `Select (10/3) AS result`

# Aliasing

`Select something AS name`

or

```
Select age1=S.age + 5
From Sailors S
```

# Order by
can sort multiple columns

`Order By column1, column2` -- order of column is important.

# Group by

# ---FILTER----

WHERE clause always **comes after** the FROM statement

- Multiple conditions: state field again
```
where blah < 800
and blah > 123
```

1. WHERE + AND: all conditions should be met

2. WHERE + OR: either or

3. WHERE + OR + AND: MUST use parenthesis for individual clause
- Ex:
```
SELECT title
FROM films
WHERE (release_year = 1994 OR release_year = 1995)
AND (certification = 'PG' OR certification = 'R');
```

4. WHERE + `BETWEEN`
- Instead of using `where year >= 1994 and year <= 2000`, use `where year between 1994 and 2000`.
- inclusive, include 2 tails.

5. WHERE + IN

Instead of using multiple `OR` use `WHERE field IN (value1, value2, value3)`

--> Used for NESTED QUERIES
--> Opposite to WHERE IN is WHERE NOT IN

IN and NOT IN can **only be used with a SINGLE attribute**
Ex:

```
Select title, actor
from films
where year IN (select year
							 from years
							 where rating > 5);
```

If need to express more than 1 attributes --> use EXISTS or NOT EXISTS

6. WHERE + EXISTS
return true if the set is not empty
used for nested queries

7. WHERE + LIKE
- Instead of finding an exact value, find pattern
- % matches whatever after
- `_` matches a single character

Ex:
- find all names start with "A"
```
select name
from people
where name like 'A%'
```

- find all names where "r" is the second character
```
find name
from people
where name like '_r%'
```

- WHERE clause cannot be used to filter rules with aggregation, unless it is a subquery contains in HAVING clause or a SELECT list, and the column being aggregated is an outer reference.

Ex:

```
SELECT o.customerid
     , ( SELECT COUNT( o.customerid )
         FROM account a
         WHERE a.customerid = o.customerid
           AND COUNT( o.customerid ) >= 5
        )
        AS cnt
FROM orders o
GROUP BY o.customerid ;
```

- Can use value = subquery only if subquery returns single value.

# Having
conditions for aggregation

- Don't necessarily need to apply GROUP BY before HAVING. If this is the case, any rows that are not excluded from WHERE clause are treated as 1 group.

```
select release_year, avg(budget) as avg_budget, avg(gross) as avg_gross
from films
where release_year > 1990
group by release_year # group by after where
having avg(budget) > 60000000 # condition for aggregation
order by avg(gross) desc;
```

Difference between Having and Where clause:
- HAVING applies rule to each group of group by, after each individual row of the table has been applied with rules from WHERE clause.

- HAVING clause need not use aggregation function after it. It's used to applied rules on each group, so if any group does not meet the rule, it will be excluded from the final result.

# --- JOIN ---

INNER JOIN gets all records that are common between both tables based on the foreign key

LEFT JOIN gets all records from the LEFT linked table but if you have selected some columns from the RIGHT table, if there is no related records, these columns will contain NULL

RIGHT JOIN is like the above but gets all records in the RIGHT table

FULL JOIN gets all records from both tables and puts NULL in the columns where related records do not exist in the opposite table

## INNER JOIN

Have to state which fields to join on.

- Join multiple tables.

```
select c.code, c.name, c.region, p.year, p.fertility_rate, e.year, e.unemployment_rate
from countries as c
inner join populations as p
on c.code = p.country_code
inner join economies as e
on c.code = e.code;
```

- `INNER JOIN` with `USING (common_field)` if common field has same name.

```
SELECT c.name AS country, c.continent, l.name AS language, l.official
FROM countries AS c
INNER JOIN languages AS l
USING (code);
```

## SELF-JOIN
Join table to itself. Good to calculate difference within the table.

Calculate percentage growth in population for each country.

```
SELECT p1.country_code,
       p1.size AS size2010,
       p2.size AS size2015,
       (p2.size - p1.size)/p1.size * 100.0 as growth_perc
FROM populations AS p1
INNER JOIN populations AS p2
ON  p1.country_code = p2.country_code and
    p1.year = p2.year - 5;
```

## CASE WHEN ... THEN

> Often it's useful to look at a numerical field not as raw data, but instead as being in different categories or groups.
>
> You can use `CASE` with `WHEN`, `THEN`, `ELSE`, and `END` to define a new grouping field.

```
SELECT name, continent, code, surface_area,
    CASE WHEN surface_area > 2000000
            THEN 'large'
         WHEN surface_area > 350000
            THEN 'medium'
       	 ELSE 'small' END
    AS geosize_group
INTO new_table_name                         # This is to export to new table
FROM countries;
```

Use `CASE WHEN` to create new table then join it with another one.

```
SELECT country_code, size,
  -- start CASE here with WHEN and THEN
    CASE WHEN size > 50000000
        THEN 'large'
  -- layout other CASE conditions here
    WHEN size > 1000000
        THEN 'medium'

  -- end CASE here with ELSE & END
    ELSE 'small' END

  -- provide the alias of popsize_group to SELECT the new field
    AS popsize_group
INTO pop_plus

-- which table?
FROM populations
-- any conditions to check?
WHERE year = 2015;

SELECT c.name, c.continent, c.geosize_group, p.popsize_group
FROM countries_plus as c
INNER JOIN pop_plus as p
ON c.code = p.country_code
ORDER BY c.geosize_group ASC;
```

## CROSS JOIN

Create all combinations of tables. If table A has 3 observations and table B has 4 observations, we will have 12 observations in the new joined table.

## SEMI-JOIN
Get results from table A that meet criteria in table B.

> Get unique languages that get spoken in Middle Eastern countries.

```
SELECT DISTINCT name
FROM languages
WHERE code IN
  (SELECT code
   FROM countries
   WHERE region = 'Middle East')
ORDER BY name;
```

## ANTI-JOIN
Get results from table A that do not meet criteria in table B.

> Note that not all countries in Oceania were listed in the resulting inner join with currencies. Use an anti-join to determine which countries were not included!

```
SELECT code, name
FROM countries
WHERE continent = 'Oceania' and
      code NOT IN (SELECT code FROM currencies);
```

# UNION, INTERSECT, EXCEPT

> Identify the country codes that are included in either economies or currencies but not in populations.
>
> Use that result to determine the names of cities in the countries that match the specification in the previous instruction.


```
-- select the city name
SELECT name
-- alias the table where city name resides
FROM cities AS c1
-- choose only records matching the result of multiple set theory clauses
WHERE country_code IN
(
    -- select appropriate field from economies AS e
    SELECT e.code
    FROM economies AS e
    -- get all additional (unique) values of the field from currencies AS c2  
    UNION
    SELECT c2.code
    FROM currencies AS c2
    -- exclude those appearing in populations AS p
    EXCEPT
    SELECT p.country_code
    FROM populations AS p
);
```

## UNION

`UNION`: unite 2 tables together, but don't take duplicates.

```
SELECT *
FROM economies2010
UNION
SELECT *
FROM economies2015
ORDER BY code, year;
```

`UNION ALL`: acts like UNION but takes duplicates.

## INTERSECT

Select observations that 2 tables have in common.

> Which countries have the same name as their cities?

```
SELECT name
FROM countries
INTERSECT
SELECT name
FROM cities;
```

## EXCEPT
Select observations that are in table A but not in table B.

> Determine the names of capital cities that are not listed in the `cities` table.

```
SELECT capital
FROM countries
EXCEPT
SELECT name
FROM cities
ORDER BY capital;
```

# Subquery

Can be found in `WHERE`, `SELECT`, `FROM`

> Select countries with number of languages spoken in those countries, then show country names by their local names.

```
select countries.local_name, subquery.lang_num
from countries, (select code, count(name) as lang_num
                 from languages
                 group by code) as subquery
where countries.code = subquery.code
order by lang_num desc;
```

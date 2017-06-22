---
title: "SQL Queries"
teaching: 15
exercises: 45
questions:
- "How do I extract information from a table in a database?"
- "How can I create summary statistics of a data set?"
- "How can I combine information from several tables?"
objectives:
- "Learn how to issue a SQL query within DBeaver"
- "Learn basic SQL syntax: SELECT, FROM, WHERE, GROUP BY, ORDER,..."
- "Build complex queries from simple queries"
- "Learn how to join two tables on a common key"
keypoints:
- "SQL provides a simple yet powerful set of tools for performing anaylis on your data"
---

## Issuing your first queries

Queries can be issued to the database in many different ways. Here we'll use the SQL query window in DBeaver. This is a great way to test out ideas before implementing your SQL queries in a script.

1. click on the "SQL Editor" button:

    <img src="../assets/img/connection/sqleditor.png" width = "400">

2. This should bring up a new tab:

    <img src="../assets/img/connection/sqleditorview.png" width = "400">

    *The upper pane is where we will issue our query, and the results of the query will be shown in the results pane below.*

3. Submit your first query by typing the following into the SQL pane:

    ~~~
    SELECT * FROM seattlecrimeincidents LIMIT 100;
    ~~~
    {: .sql}

    * the `LIMIT` command restricts the database to return only the first 100 rows.
    * the `*` is a wildcard requesting all columns from the database.

<br>

---
# Practice problems

In the following exercises you can use this [cheatsheet](http://www.sql-tutorial.net/sql-cheat-sheet.pdf) to look up the different SQL commands.

> ## Counting numbers of offenses
>
> _How many **"TRESPASS"** offenses occurred in total?_
>
> Hint: fill in the ? below:
> ~~~
> SELECT ? FROM ? WHERE ? = ?
> ~~~
> {: .sql}
>
> > ## Solution
> >
> > Here is the solution:
> >
> > ~~~
> > SELECT COUNT(*) FROM seattlecrimeincidents WHERE "Offense Type" = 'TRESPASS'
> > ~~~
> > {: .sql}
> {: .solution}
{: .challenge}

> ## Single and double quotes in SQL
> Notice that in our example above we had both single and double quotes. Here's the rule:
> * use _double_ quotes for column names within the database 
> * use _single_ quotes for strings or date/time constants
> Note that we only have to use double quotes for the column names because this particular data table did not follow the standards of database
> convention (i.e. the names have spaces in them). See [this discussion](https://stackoverflow.com/questions/1992314/what-is-the-difference-between-single-and-double-quotes-in-sql) for more details.
{: .callout}

> ## Upper/lower case: does it matter?
> Most versions of the SQL language are not case sensitive, but it is a good habbit to make key words capital so that the queries are easy to read. The only time when the case matters is whne the word is in quotes and refers to a name.
{: .callout}

<br>

>## Calculating several values at a time
> _What is the range of the latitude and longitude coordinates of all crimes?_
>
> Hint: use "max" and "min" functions.
> > ## Solution
> > ~~~
> > SELECT min(longitude), max(longitude),min(latitude),max(latitude) FROM seattlecrimeincidents
> > ~~~
> > {: .sql}
> {: .solution}
{: .challenge}


>## Combining conditions
>_What is the number of bike thefts in the month of january?_
>
> Hint: the name for bike thefts is 'THEFT-BICYCLE'.
> > ## Solution
> > ~~~
> > SELECT count(*) FROM seattlecrimeincidents
        WHERE "Offense Type" = 'THEFT-BICYCLE' and month = 1
> > ~~~
> > {: .sql}
> {: .solution}
{: .challenge}

<br>

---
# Grouping 
* identify a variable according to which to group
* identify a function to apply per groups


* Count how many offenses are for each Offense Type
>~~~
>select "Offense Type",count(*) from SeattleCrimeIncidents group by "Offense Type" order by count ASC;
>~~~
>{: .sql}

Note: for homicide we see there are a lot of types of homicides -> use summarized offense description

* Count how many offenses are for each Summarized Offense Description
> ~~~
> select "Summarized Offense Description", count(*) from SeattleCrimeIncidents
	group by "Summarized Offense Description";
> ~~~
> {: .sql}

* Count how many offenses per year
> ~~~
> select year, count(*) from SeattleCrimeIncidents
	group by year;
> ~~~
> {: .sql}


> ## Crimes for each month?
>
> > ## Solution
> >
> > ~~~
> > SELECT month,count(*) FROM seattlecrimeincidents GROUP BY month ORDER BY month ASC
> > ~~~
> > {: .sql}
> {: .solution}
{: .challenge}


> ## Month with highest number of bike thefts?
> > ## Solution
> > ~~~
> > SELECT month,count(*) FROM seattlecrimeincidents
	WHERE "Offense Type" = 'THEFT-BICYCLE'
	GROUP BY month
	ORDER BY count DESC
> > ~~~
> > {: .sql}
> {: .solution}
{: .challenge}

> ## Number of crimes per census tract?
> > ## Solution
> > ~~~
> > SELECT "census tract 2000",count(*) FROM seattlecrimeincidents
	group by "census tract 2000"
	ORDER BY "census tract 2000" ASC;
> > ~~~
> > {: .sql}
> {: .solution}
{: .challenge}

<br>

---
# Ideas of aliasing and nesting

* adding better column names

> ~~~
> SELECT "census tract 2000" as "CensusTract",count(*) as "crime_count" FROM seattlecrimeincidents
	group by "census tract 2000"
	ORDER BY "census tract 2000" ASC;
> ~~~
> {: .sql}

* extracting the max

> ~~~
> SELECT max(crimeTable.crime_count) FROM 
(SELECT "census tract 2000" as "CensusTract",count(*) as "crime_count" FROM seattlecrimeincidents
	group by "census tract 2000") as crimeTable
> ~~~
> {: .sql}

<br>

---
# Joining two tables


> _Which census tract has the highest crime rate?_
> 
> * Can we answer this question using the SeattleCrimeIncidentsTable?
> 
> To calculate the crime rate we need to have the population of each census tract. This is missing from the SeattleCrimeIncidents Table. However, in the database there is another table called census containing the population per tract. 
>
> ~~~
> SELECT * from census LIMIT 10
> ~~~
> {: .sql}
>
> * What minimal database we need to answer the question?
> a table with census 
>
> tract | # crimes/ population
> | tract   |      #crimes/popuation      |
> |----------|:-------------:|
> 
> * What is the common key on which we can join the two tables?
> 
> 
>
> 
>
> To simplify the join we can create the following two tables: 
>
> * Table 1: 
> tract | crime_count
> | tract   |      crime_count      |
> |----------|:-------------:|
>
> ~~~
> SELECT round("census tract 2000"),count(*) FROM seattlecrimeincidents
	group by "census tract 2000"
	ORDER BY "census tract 2000" ASC;
>~~~
>{: .sql}
>
> * Table 2: 
> | tract   |      population     |
> |----------|:-------------:|
>
> ~~~
> SELECT "Census Tract","Total Population, 2010" as population from census_data
	ORDER BY "Census Tract" ASC;
> ~~~
>{: .sql}
> 
> Then we can join them where the corresponding tracts are equal.
> 
> Hint: be careful about how division is performed in SQL. You might need to use a function which converts integers to floats (you can do this with `variable::float`)

> ## Join with where command
> 
>> ## Solution
>> ~~~
SELECT crimeTable.CT,cast(crimeTable.count as float)/censusTable.population as crime_rate from
	(select round("census tract 2000") as CT, count(*) as count from SeattleCrimeIncidents group by "census tract 2000") as crimeTable,
    (select "Total Population, 2010" as population,"Census Tract" as CT from census_data) as censusTable
    where crimeTable.CT = censusTable.CT order by "crime_rate" DESC;
> > ~~~
> > {: .sql}
> {: .solution}
{: .challenge}

> ## Join with a join command
>
> > ## Solution 
> > ~~~ 
> > select crimeTable.CT,crimeTable.count::float/censusTable.population::float as crime_rate from 
	(select round("census tract 2000") as CT, count(*) as count from SeattleCrimeIncidents group by "census tract 2000") crimeTable
    join 
    (select "Total Population, 2010" as population,"Census Tract" as CT from census_data) censusTable
    on crimeTable.CT = censusTable.CT order by "crime_rate" DESC;
> > ~~~
> >{: .sql}
> {: .solution}
{: .challenge}
 

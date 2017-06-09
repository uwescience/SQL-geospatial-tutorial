---

title: "Introduction to Databases"
teaching: 15
exercises: 0
questions:
- "What is a database?"
- "How are data structured within a database?"
- "What are the advantages of working with data in a database?"
objectives:
- "Learn the fundamentals of the relational data model"
- "Begin to identify in which cases it makes sense to put your data in a database"
- "Learn a bit of Structured Query Language (SQL)"
key points:
- "Databases offer a highly structured framework for storing and manipulating data"
- "Setting up this structure takes some time"
- "Advantages include the ability to investigate complex relationships between data using a simple query language"
---

# What is a database? 
* software system for capturing, storing and analyzing data 
* nearly all databases use the _relational_ data model

<br><br>
<img src="../assets/img/databaseIntro/terminology.png" width = "600" border = "10">
<br><br><br>

# Relational data model:
* data are structured into row/column format 

> | crimesID | Offense type | Offense code | Date | Location | 
> | ---- | ---- | ----- | ---- | ---- | ---- |
> |  1 | tresspass | 5700 | 2015-01-28 09:30:00 |  12XX Block of E Pike St |
> |  2 |larceny-theft | 2300 |2015-02-21 08:24:21 |  15XX Block of Aurora St | 


* each record has a unique identifier (primary key)

* uses Structured Query Language (SQL):   

```SQL
CREATE TABLE seattlecrimesincidents 
    ("crimesID" int,
     "Offense type" character,
     "Offense code" int,
     "Date" timestamp,
     "Location" character); 
```
> ### CREATE TABLE 
> | crimesID | Offense type | Offense code | Date | Location | 
> | ---- | ---- | ----- | ---- | ---- | ---- |
> |   |   |   |   |   |   
{: .challenge}

* populating the database records:
```SQL
INSERT INTO seattlecrimeincidents VALUES

    (1,'trespass', 5700,'2015-01-28 09:30:00','12XX Block of E Pike St'),
    
    (2,'larceny-theft',2300, '2015-02-21 08:24:21','15XX Block of Aurora St');
```

> ### INSERT INTO TABLE
> | crimesID | Offense type | Offense code | Date | Location | 
> | ---- | ---- | ----- | ---- | ---- | ---- |
> |   1 | tresspass | 5700 | 2015-01-28 09:30:00 | 12XX Block of E Pike St |
> |   2 | larceny-theft | 2300 |  2015-02-21 08:24:21 | 15XX Block of Aurora St | 
{: challenge}

## Data in each column must be of the same type

### SQL requires this so it knows how to operate on the data

Some common [data types](https://www.postgresql.org/docs/9.4/static/datatype.html):
<br>

| Name | Aliases | Description |
| --- | --- | --- |
|  boolean | bool | logical Boolean (true/false) |
| character | char | fixed-length character string |
| date |  | calendar date (year, month, day) |
| double precision | float8 | double precision floating-point number (8 bytes) |
| integer | int, int4 | signed four-byte integer |
| json  |  | JSON data |
| money |  | currency amount |
| timestamp |  | date and time (no time zone) |
| xml |  | XML data |

  ## Database rules:
### take the time to [normalize](https://en.wikipedia.org/wiki/Database_normalization) your tables to minimize redundancy

#### example: multiple offenses at the same time 

| crimesID | Offense type | Offense code | Date | Location | 
| ---- | ---- | ----- | ---- | ---- | ---- |
|  1 | tresspass and burglary | 5700 and 5710 | 2015-01-28 09:30:00 | 12XX Block of E Pike St |
|  2 | larceny-theft | 2300 |  2015-02-21 08:24:21 | 15XX Block of Aurora St |

####  INCORRECT: database will have problems searching these columns

#### solution: create another row

| crimesID | Offense type | Offense code | Date | Location | 
| ---- | ---- | ----- | ---- | ---- | ---- |
|    1 |  tresspass |  5700 |   2015-01-28 09:30:00 |   12XX Block of E Pike St |
|    2 |   burglary |   5710 |   2015-01-28 09:30:00 |   12XX Block of E Pike St |
|    3 |  larceny-theft |  2300 |   2015-02-21 08:24:21 |   15XX Block of Aurora St |

### NULL values

* missing data are a common feature of many datasets
* here the code for "tresspass" is not known so the data entry is "X"

| crimesID | Offense type | Offense code | Date | Location | 
| ---- | ---- | ----- | ---- | ---- | ---- |
|    1 |  tresspass |  X |   2015-01-28 09:30:00 |   12XX Block of E Pike St |
|    2 |   burglary |   5710 |   2015-01-28 09:30:00 |   12XX Block of E Pike St |
|    3 |  larceny-theft |  2300 |   2015-02-21 08:24:21 |   15XX Block of Aurora St |

### NULL values
* conventionally, some value is used to represent missing data (e.g. "X" or -9999) 
* relational databases introduced NULL values:
    * NULL is a state representing a lack of a value
    * NULL is not the same as zero!
    * NULL values are ignored in SELECT statements


* selecting data:
```SQL
SELECT * 
   FROM seattlecrimeincidents 
   WHERE "Offense code" = 5700;
```

* use a "WHERE" clause to select specific rows

| crimesID | Offense type | Offense code | Date | Location | 
| ---- | ---- | ----- | ---- | ---- | ---- |
|  1 | tresspass | 5700 | 2015-01-28 09:30:00 |  12XX Block of E Pike St |

* selecting data:
```SQL
SELECT "Offense type", "Date" 
   FROM seattlecrimeincidents;
```

* use a comma separated list to select specific columns

| Offense type | Date | 
| ---- | ---- |
| tresspass |  2015-01-28 09:30:00 | 
| larceny-theft | 2015-02-21 08:24:21 |  

## Functions: databases have a wide range of functions that can operate on row elements

#### Example:
* use a function to extract a subset of a date (e.g. year, hour) from a column with type = "timestamp"

```SQL
SELECT "Date Reported", date_part('hour', "Date Reported")
FROM seattlecrimeincidents
LIMIT 5;
```


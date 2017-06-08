---

title: "Introduction to Databases"
teaching: 15
exercises: 0
questions:
- "In this exercise we will work on connecting to a database hosted on Amazon Web Services. Then we will practice issuing a few queries and start experimenting with SQL."
objectives:
- "Learn about the overall architecture of the HiMAT infrastructure"
key points:
- "Where you go to store and find data during HiMAT project depends on the data type, size and usage constraints"
- "Several methods/data centers are provided and users can choose which approach works best"
- "co-location of processing/analysis with data storage is encouraged, to minimize transfer of large files" 
---

### What is a database? 
* software system for capturing, storing and analyzing data 
* nearly all databases use the _relational_ data model

<br><br>
<img src="../assets/img/databaseIntro/terminology.png" width = "400" border = "10">
<br><br><br>

### Relational data model:
* data are structured into row/column format 

| crimesID | Offense type | Offense code | Date | Location | 
| ---- | ---- | ----- | ---- | ---- | ---- |
|  1 | tresspass | 5700 | 2015-01-28 09:30:00 |  12XX Block of E Pike St |
|  2 |larceny-theft | 2300 |2015-02-21 08:24:21 |  15XX Block of Aurora St | 

* each record has a unique identifier (primary key)

### Relational data model:
* uses Structured Query Language (SQL):   

```SQL
CREATE TABLE seattlecrimesincidents 
    ("crimesID" int,
     "Offense type" character,
     "Offense code" int,
     "Date" timestamp,
     "Location" character); 
```

| crimesID | Offense type | Offense code | Date | Location | 
| ---- | ---- | ----- | ---- | ---- | ---- |
|   |   |   |   |   |   


* populating the database records:
```SQL
INSERT INTO seattlecrimeincidents VALUES

    (1,'trespass', 5700,'2015-01-28 09:30:00','12XX Block of E Pike St'),
    
    (2,'larceny-theft',2300, '2015-02-21 08:24:21','15XX Block of Aurora St');
```

| crimesID | Offense type | Offense code | Date | Location | 
| ---- | ---- | ----- | ---- | ---- | ---- |
|   1 | tresspass | 5700 | 2015-01-28 09:30:00 | 12XX Block of E Pike St |
|   2 | larceny-theft | 2300 |  2015-02-21 08:24:21 | 15XX Block of Aurora St | 

## Data in each column must be of the same type

### SQL requires this so it knows how to operate on the data

Some common [data types](https://www.postgresql.org/docs/9.4/static/datatype.html):
<br>
<div class="TABLE">
    <a name="DATATYPE-TABLE" id="DATATYPE-TABLE"></a>

<small>
    <table class="CALSTABLE" border="1">
      <colgroup><col>
      <col>
      <col>

      <thead>
        <tr>
          <th>Name</th>

          <th>Aliases</th>

          <th>Description</th>
        </tr>
      </thead>

      <tbody>

        <tr>
          <td><tt class="TYPE">boolean</tt></td>

          <td><tt class="TYPE">bool</tt></td>

          <td>logical Boolean (true/false)</td>
        </tr>

        <tr>
          <td><tt class="TYPE">character [(<tt class="REPLACEABLE c4">n</tt>)]</tt></td>

          <td><tt class="TYPE">char [(<tt class="REPLACEABLE c4">n</tt>)]</tt></td>

          <td>fixed-length character string</td>
        </tr>

        <tr>
          <td><tt class="TYPE">date</tt></td>

          <td>&nbsp;</td>

          <td>calendar date (year, month, day)</td>
        </tr>

        <tr>
          <td><tt class="TYPE">double precision</tt></td>

          <td><tt class="TYPE">float8</tt></td>

          <td>double precision floating-point number (8 bytes)</td>
        </tr>

        <tr>
          <td><tt class="TYPE">integer</tt></td>

          <td><tt class="TYPE">int</tt>, <tt class="TYPE">int4</tt></td>

          <td>signed four-byte integer</td>
        </tr>

        <tr>
          <td><tt class="TYPE">json</tt></td>

          <td>&nbsp;</td>

          <td>JSON data</td>
        </tr>


        <tr>
          <td><tt class="TYPE">money</tt></td>

          <td>&nbsp;</td>

          <td>currency amount</td>
        </tr>

        <tr>
          <td><tt class="TYPE">timestamp [(<tt class="REPLACEABLE c4">p</tt>)] [ without time zone
          ]</tt></td>

          <td>&nbsp;</td>

          <td>date and time (no time zone)</td>
        </tr>

        <tr>
          <td><tt class="TYPE">xml</tt></td>

          <td>&nbsp;</td>

          <td>XML data</td>
        </tr>
      </tbody>
    </table>
  </div>

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


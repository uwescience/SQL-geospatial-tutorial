---
title: "Scripting database connections in Python/R"
teaching: 10
exercises: 20
questions:
objectives:
keypoints:
---


### Interfacing with the database from Python:

We will use [pandas](pandas.pydata.org) which is a data analysis toolkit that happens to have some very simple methods for moving data to and from a database. The database connections are handled through pandas using [sqlalchemy](www.sqlalchemy.org).

~~~
import pandas as pd
from sqlalchemy import create_engine
import sqlalchemy.types as types
~~~
{: .python}

Our next step is to pass some database connection information to pandas/sqlalchemy so that we can establish a connection. We create a database "engine" object that is then used in subsequent operations as a portal to/from the database.

~~~
db_info = {
    'host': 'someurl.amazonaws.com',
    'user': 'myusername',
    'password': 'mysecretpassword',
    'port': 5432,
    'dbname': 'databasename'
}
sql_str = 'postgresql://{user}:{password}@{host}:{port}/{dbname}'.format
engine = create_engine(sql_str(**db_info))
~~~
{: .python}

Now we can issue a query to the database as follows:

~~~
df = pd.read_sql('SELECT * FROM seattlecrimeincidents LIMIT 100', engine)
~~~
{: .python}

<br>

---

### Interfacing with the database from R:

We will use the R package [PostgreSQL](https://cran.r-project.org/web/packages/RPostgreSQL/index.html).

~~~
install.packages("PostgreSQL")
library(RPostgreSQL)
~~~
{: .r}

We can execute a query in the following way:

~~~
drv <- dbDriver("PostgreSQL")
con <- d:Connect(drv, host="someurl.amazonaws.com", user="myusernmae", password="mysecretpassword", dbname="databasename", port="5432")
rs <- dbSendQuery(con, "select * from seattlecrimeincidents limit 100"); 
df <- fetch(rs)
~~~
{: .r}

<br>

---
### Useful Resources:
* fiddle with SQL: [sqlfiddle](http://sqlfiddle.com/)
* check out [SQLShare](https://sqlshare.escience.washington.edu/sqlshare/) - databases made easy (you can find SeattleCrimeIncidents and census datasets already there)
* manipulate `.csv` files locally with the SQL language: [csvkit](https://csvkit.readthedocs.io/en/1.0.2/)
* querying urban data sets with [Socrata Query Language (SoQL)](https://dev.socrata.com/docs/queries/)

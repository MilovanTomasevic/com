---
layout: post
title: How to use the DB2 LOAD utility using the python ibm_db driver
image: https://opengraph.githubassets.com/1c5b0bd00fb49af428e6531c170d3a7f1b1765247d664d21c8eca21b8e88ee35/IBM/db2-python
description: >
  LOAD is a DB2 utility that I would like to use to insert data into a table from a CSV file.
hide_image: true
comments: true
last_modified_at: 2021-04-27
categories: [stackoverflow]
tags: [python, db2]
---

How to use the DB2 LOAD utility using the python ibm_db driver

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/44074008/how-to-use-the-db2-load-utility-using-the-python-ibm-db-driver) by [Harvinder](https://stackoverflow.com/users/2089697/harvinder)

LOAD is a DB2 utility that I would like to use to insert data into a table from a CSV file. How can I do this in Python using the `ibm_db` driver? I don't see anything in the docs [here](https://github.com/ibmdb/python-ibmdb/wiki/APIs)

CMD: `LOAD FROM xyz OF del INSERT INTO FOOBAR`

Running this as standard SQL fails as expected: 
`Transaction couldn't be completed: [IBM][CLI Driver][DB2/LINUXX8664] SQL0104N  An unexpected token "LOAD FROM xyz OF del" was found following "BEGIN-OF-STATEMENT".  Expected tokens may include:  "<space>".  SQLSTATE=42601 SQLCODE=-104`

Using the db2 CLP directly (i.e. `os.system('db2 -f /path/to/script.file')`) is not an option as DB2 sits on a different machine that I don't have SSH access to.

`EDIT`:
Using the `ADMIN_CMD` utility also doesn't work because the file being loaded cannot be put on the database server due to firewall. For now, I've switched to using `INSERT`


## [Answer](https://stackoverflow.com/a/66394080/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)

## CSV to DB2 with Python

Briefly: One solution is to use an [SQLAlchemy][1] [adapter][2] and [Db2’s External Tables][3].

### SQLAlchemy:

The [Engine][4] is the starting point for any SQLAlchemy application. It’s “home base” for the actual database and its DBAPI, delivered to the SQLAlchemy application through a connection pool and a [Dialect][5], which describes how to talk to a specific kind of database/DBAPI combination.

[![enter image description here][6]][6]

Where above, an [Engine][4] references both a [Dialect][5] and a [Pool][7], which together interpret the DBAPI’s module functions as well as the behavior of the database.

Creating an engine is just a matter of issuing a single call, [create_engine()][8]:
```py
dialect+driver://username:password@host:port/database
```

Where dialect is a database name such as mysql, oracle, postgresql, etc., and driver the name of a DBAPI, such as psycopg2, pyodbc, cx_oracle, etc.

### Load data by using transient external table:

Transient external tables (TETs) provide a way to define an external table that exists only for the duration of a single query.

TETs have the same capabilities and limitations as normal external tables. A special feature of a TET is that you do not need to define the table schema when you use the TET to load data into a table or when you create the TET as the target of a SELECT statement.

Following is the syntax for a TET:

```sql
INSERT INTO <table> SELECT <column_list | *>
FROM EXTERNAL 'filename' [(table_schema_definition)]
[USING (external_table_options)];

CREATE EXTERNAL TABLE 'filename' [USING (external_table_options)]
AS select_statement;

SELECT <column_list | *> FROM EXTERNAL 'filename' (table_schema_definition)
[USING (external_table_options)];
```

For information about the values that you can specify for the `external_table_options` variable, see [External table options][9].

General example
  - Insert data from a transient external table into the database table on the Db2 server by issuing the following command:

```sql
INSERT INTO EMPLOYEE SELECT * FROM external '/tmp/employee.dat' USING (delimiter ',' MAXERRORS 10 SOCKETBUFSIZE 30000 REMOTESOURCE 'JDBC' LOGDIR '/logs' )
```

### Requirements

```sh
pip install ibm-db
pip install SQLAlchemy
```


### Pyton code

One example below shows how it works together.

```py
from sqlalchemy import create_engine


usr = "enter_username"
pwd = "enter_password"
hst = "enter_host"
prt = "enter_port"
db = "enter_db_name"

#SQL Alchemy URL
conn_params = "db2+ibm_db://{0}:{1}@{2}:{3}/{4}".format(usr, pwd, hst, prt, db)

shema = "enter_name_restore_shema"
table = "enter_name_restore_table"
destination = "/path/to/csv/file_name.csv"

try:
    print("Connecting to DB...")
    engine = create_engine(conn_params)
    engine.connect()  # optional, output: DB2/linux...
    print("Successfully Connected!")
except Exception as e:
    print("Unable to connect to the server.")
    print(str(e))

external = """INSERT INTO {0}.{1} SELECT * FROM EXTERNAL '{2}' USING (CCSID 1208 DELIMITER ',' REMOTESOURCE LZ4 NOLOG TRUE )""".format(
    shema, table, destination
)

try:
    print("Restoring data to the server...")
    engine.execute(external)
    print("Data restored successfully.")
except Exception as e:
    print("Unable to restore.")
    print(str(e))
```

### Conclusion

- A great solution for restoredlarge files, specifically, 600m worked without any problems.
- It is also useful for copying data from one table/database to another table. So that the backup is done as an export of csv and then that csv into DB2 with the given example.
- SQLAlchemy-[Engine][4] can be combined with other databases such as: sqlite, mysql, postgresql, oracle, mssql, etc.

[![enter image description here][10]][10]


  [1]: https://docs.sqlalchemy.org/en/14/index.html
  [2]: https://pypi.org/project/SQLAlchemy/
  [3]: https://www.ibm.com/support/knowledgecenter/SSEPGG_11.5.0/com.ibm.db2.luw.apdv.java.doc/src/tpc/imjcc_cjv00032.html
  [4]: https://docs.sqlalchemy.org/en/14/core/connections.html#sqlalchemy.engine.Engine
  [5]: https://docs.sqlalchemy.org/en/14/core/internals.html#sqlalchemy.engine.Dialect
  [6]: https://i.stack.imgur.com/e2GvZ.png
  [7]: https://docs.sqlalchemy.org/en/14/core/pooling.html#sqlalchemy.pool.Pool
  [8]: https://docs.sqlalchemy.org/en/14/core/engines.html#sqlalchemy.create_engine
  [9]: https://www.ibm.com/support/knowledgecenter/SSULQD_7.2.1/com.ibm.nz.load.doc/c_load_external_table_options.html#c_load_external_table_options
  [10]: https://i.stack.imgur.com/u99P2.gif
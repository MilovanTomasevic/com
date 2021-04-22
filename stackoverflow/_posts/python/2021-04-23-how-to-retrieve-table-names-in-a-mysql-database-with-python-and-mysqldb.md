---
layout: post
title: How to retrieve table names in a mysql database with Python and MySQLdb?
image: https://www.standalonedeveloper.eu/thumbnails/158.jpg
description: >
  I have an SQL database and am wondering what command you use to just get a list of the table names within that database.
hide_description: true
hide_image: true
comments: true
last_modified_at: 2021-04-23
categories: [stackoverflow]
tags: [python, mysql, mysql-python]
---

How to retrieve table names in a mysql database with Python and MySQLdb?

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/3556305/how-to-retrieve-table-names-in-a-mysql-database-with-python-and-mysqldb) by [Richard](https://stackoverflow.com/users/323332/richard)

I have an SQL database and am wondering what command you use to just get a list of the table names within that database.

## [Answer](https://stackoverflow.com/a/64516487/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)

It is also possible to obtain tables from a specific scheme with execute the  single query with the driver below.

```sh
python3 -m pip install PyMySQL
```

```py
import pymysql

# Connect to the database
conn = pymysql.connect(host='127.0.0.1',user='root',passwd='root',db='my_database')

# Create a Cursor object
cur = conn.cursor()

# Execute the query: To get the name of the tables from a specific database
# replace only the my_database with the name of your database
cur.execute("SELECT table_name FROM information_schema.tables WHERE table_schema = 'my_database'")

# Read and print tables
for table in [tables[0] for tables in cur.fetchall()]:
    print(table)
```

output:
```sh
my_table_name_1
my_table_name_2
my_table_name_3
...
my_table_name_x
```


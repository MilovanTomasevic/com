---
layout: post
title: Split Result from SQL Query
image: https://www.aptusdatalabs.com/wp-content/uploads/2020/11/database_migration-casestudy.jpg
description: >
  For example this SQL statement returns 1k rows `SELECT * FROM tableName WHERE someCondition` 
hide_image: true
comments: true
last_modified_at: 2021-04-26
categories: [stackoverflow]
tags: [database, sql]
---

Split Result from SQL Query

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/7580314/split-result-from-sql-query) by [mKorbel](https://stackoverflow.com/users/714968/mkorbel)

For example this SQL statement returns 1k rows

`SELECT * FROM tableName WHERE someCondition` 

my question is: is it possible by using WHERE

1) Split Resultset to the 10 partial resultsets, then 

- 1st. would be returns 0% - 10%, 

- 2nd. 10% - 20%, 

- etc..

2) cuts range between 50 - 150th rows 


## [Answer](https://stackoverflow.com/a/66825971/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)

> my question is: is it possible by using WHERE

My answer is: yes, it is possible.

One of the solutions would be to use the `MOD` function.
The `MOD` function returns the remainder of one number divided by another and the function accepts two arguments:

```sql
MOD(dividend,divisor)
```
- `dividend` is a literal number or a numeric expression to divide.
- `divisor` is a literal number or a numeric expression by which to divide the dividend.

In particular, dividing the data into ten parts, in your example, would look like this:

```sql
SELECT * FROM tableName WHERE mod(INDEX_ID, 10) = 1
SELECT * FROM tableName WHERE mod(INDEX_ID, 10) = 2
SELECT * FROM tableName WHERE mod(INDEX_ID, 10) = 3
SELECT * FROM tableName WHERE mod(INDEX_ID, 10) = 4
SELECT * FROM tableName WHERE mod(INDEX_ID, 10) = 5
SELECT * FROM tableName WHERE mod(INDEX_ID, 10) = 6
SELECT * FROM tableName WHERE mod(INDEX_ID, 10) = 7
SELECT * FROM tableName WHERE mod(INDEX_ID, 10) = 8
SELECT * FROM tableName WHERE mod(INDEX_ID, 10) = 9
SELECT * FROM tableName WHERE mod(INDEX_ID, 10) = 0
```

`INDEX_ID` is _index_ from `table_name`.
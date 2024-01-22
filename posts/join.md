---
title: "MySQL Joins"
date: "2024-01-21"
---

Oftentimes we have data stored across a number of different tables. They are stored across different tables to limit the size of our tables, aid in organization as well as to reduce redundancy. The example I am about to show is trivial and in the real world we would never separate the information stored into two distinct tables, but it will suffice to demonstrate the concept of SQL joins.

Take a look at our sample data.  
The first table stores the first names of 4 individuals.  
![Join query](/images/mysql/one.png)

The second table stores the last names of 4 individuals as well. However, we do not know the first name of the last individual. This is indicated by the column **one_id** containing a null value.  
![Join query](/images/mysql/two.png)

There are four kinds of joins:

- Inner _(default)_
- Left
- Right
- Full Outer

## Inner Join

This is the default join. It fetches data from both tables and drops any record from the result set which does not have an associated record in the other table.

The following two code blocks are equivalent.

```sql
SELECT first_name, last_name
FROM one
JOIN two
ON one.id = two.one_id;
```

![Inner Join result](/images/mysql/one-inner-join-two.png)

```sql
SELECT first_name, last_name
FROM one
INNER JOIN two
ON one.id = two.one_id;
```

![Inner Join result](/images/mysql/one-inner-join-two.png)

Notice how the records with id 4 from two or one is not included in the result set.

## Left Join

This join returns all results from the base table even if it does not have a corresponding entry in the table being joined. **NULL** is used to indicate a missing record from the table which should match up with the base table.

The base table is the one which follows the **FROM** keyword. The table to be joined is the one which follows the **JOIN** keyword.

```sql
SELECT first_name, last_name
FROM one
LEFT JOIN two
ON one.id = two.one_id;
```

![Left Join result](/images/mysql/one-left-join-two.png)

As can be seen, Joseph has no corresponding last name.

## Right Join

This is the reverse of the left join.

```sql
SELECT first_name, last_name
FROM one
RIGHT JOIN two
ON one.id = two.one_id;
```

![Right Join result](/images/mysql/one-right-join-two.png)

Evans has no corresponding first name.

## Full Outer

There is an additional join that returns all records from both tables. MySQL does not have an implementation for this, but it can bee found in other SQL.

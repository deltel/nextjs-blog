---
title: "MySQL Data Types"
date: "2025-01-05"
---

## DATE

The supported format for MySQL is **'YYYY-MM-DD'**.

```sql
created_at DATE
```

## TIME

- The supported format for MySQL is **'hhh:mm:ss'**
- They range from _'-838:59:59'_ to _'838:59:59'_
- Abbreviated time formats are recognized as times of day. So 12:12 is '12:12:00' and not '00:12:12'.

```sql
start_time TIME
```

## DECIMAL

The decimal data type is a fixed-point type. This is used when precision is important such as when storing monetary data. The syntax is `DECIMAL(<precision>, <scale>)`

```sql
salary DECIMAL(5,2)
```

Precision refers to the number of significant figures in the number and scale is the number of digits that follow the decimal point.  
So the above would store anything in the range [-999.99, 999.99].

## Integers

There are a number of integer types each used for storing whole numbers that fall within a specific range. The various integer types will be arranged in ascending order.  
[- MySQL 8.3 Reference Manual](https://dev.mysql.com/doc/refman/8.3/en/integer-types.html)

### TINYINT

- The smallest
- It requires 1 byte of storage
- It stores values in the range [-128, 127] if signed and a maximum value of 255 if unsigned.

```sql
id TINYINT
```

### SMALLINT

- Next in line
- It requires 2 bytes of storage
- It stores values in the range [-32,768, 32,767] if signed and a maximum value of 65535 if unsigned.

```sql
id SMALLINT
```

### MEDIUMINT

- It requires 3 bytes of storage
- It stores values in the range [-8,388,608, 8,388,607] if signed and a maximum value of 16,777,215 if unsigned.

```sql
id MEDIUMINT
```

### INT

- It requires 4 bytes of storage
- It stores values in the range [-2,147,483,648, 2,147,483,647] if signed and a maximum value of 4,294,967,295 if unsigned.

```sql
id INT
```

### BIGINT

- It requires 8 bytes of storage
- It stores values in the range [-2<sup>63</sup>, -2<sup>63</sup> - 1] if signed and a maximum value of -2<sup>64</sup> - 1 if unsigned.

```sql
id BIGINT
```

## Strings

A string can be stored as either a CHAR or VARCHAR.  
[- MySQL 8.3 Reference Manual](https://dev.mysql.com/doc/refman/8.3/en/char.html)

### CHAR

These are used to store fixed length strings. The length falls in the range [0, 255]. The values are always right padded with spaces to make up the length when they are stored. Upon retrieval, however, the extra spaces are trimmed.

```sql
course CHAR(10)
```

The number of characters stored by this example will always be 10.

### VARCHAR

In contrast to chars, varchars are used to store variable length strings. They are not padded up to the length specified and any empty spaces they are stored with are present when retrieved as well.

```sql
course VARCHAR(10)
```

The number of characters stored depends on what was inserted in the column.

## ENUM

These work pretty much the same as in programming languages. They are used to store a list of valid values for the column.

```sql
gender ENUM('M', 'F')
```

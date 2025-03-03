---
title: "MySQL Triggers"
date: "2025-03-02"
---

## Triggers

A trigger is a named database object that is associated with a table, and that activates when a particular event occurs for the table.  
It requires the TRIGGER privilege for the table associated with the trigger  
[- MySQL Reference Manual](https://dev.mysql.com/doc/refman/8.0/en/create-trigger.html)

We can use this mechanism to execute SQL statements whenever an **insert**, **update**, or **delete** event occurs.

### Creation

#### Syntax

```sql
CREATE TRIGGER <trigger_name>
<BEFORE/AFTER> <INSERT/UPDATE/DELETE> ON <table_name>
FOR EACH ROW
<sql_statement>;
```

#### Example

```sql
CREATE TRIGGER new_user
AFTER INSERT ON users
FOR EACH ROW
INSERT INTO table_logs VALUES(CONCAT('Insert ', NEW.email));
```

The example above will record a message in a separate table whenever a new user is inserted. This can be used as a basic logging system.  
To create this trigger we would need the TRIGGER privilege on the _users_ table

### Deletion

In order to remove a trigger we use the **DROP TRIGGER** statement.

#### Syntax

```sql
DROP TRIGGER IF EXISTS <schema_name.trigger_name>
```

#### Example

```sql
DROP TRIGGER IF EXISTS alcohol_test.new_user;
```

The example above will delete the trigger called **new_user** that is found in the **alcohol_test** schema if it exists.

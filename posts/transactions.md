---
title: "MySQL Transactions"
date: "2024-01-29"
---

## Transactions

A transaction is a grouping of sql statements that will execute as a unit. If any of the statements causes an error it will abort the transaction and none of the statements will be committed to the database. It is therefore very useful when a change should not be persisted if something went wrong in the process.

It is worth noting that not all statements can be aborted. These include the creation and dropping of databases and tables. Additionally, altering tables or stored routines cannot be undone.

### Keywords

- START TRANSACTION  
  This is used to start a transaction.

- COMMIT  
  This is used to persist the changes of a transaction. If this statement does not follow the START TRANSACTION keywords, the changes will not be commited.

- ROLLBACK  
  This is used to undo the changes if something goes wrong.

### Commit Example

```
START TRANSACTION;
```

```
UPDATE users
SET balance = 44800
WHERE user_id = '1';
```

This statement would not be persisted. Of course, if we simply query the current connection it show us the temporary state of the table as can be seen below:
![transaction start](/images/mysql/transaction-start.png)

Should we open another connection to the database however, we would see that the actual state of the table has not changed.  
![second connection](/images/mysql/second-connection.png)

Let's commit the results to the database.

```
COMMIT;
```

Now if we query the database again the table would now be updated.
![transaction commited](/images/mysql/transaction-commit.png)

I don't know if you've noticed but the **MySQL shell** uses a star in a blue background to indicate that a transaction has been started and only once the transaction has been commited do we see the visual aid removed.
![second connection after commit](/images/mysql/second-connection-committed.png)

### Rollback Example

Now let's create a scenario where we would need to roll back the changes before committing the transaction.

A customer has an existing order that they have not yet paid for:
![initial state of database](/images/mysql/transactions-rollback-initial.png)

He comes in and makes a payment using our web app. A request is triggered and the following sql statements get executed.

```sql
START TRANSACTION;

UPDATE orders
SET date_paid = CURRENT_DATE(), amount_paid = 4200
WHERE order_id = 544;

UPDATE users
SET balance = balance - 42000;
```

![state after update queries](/images/mysql/transactions-mistake-balance.png)  
We see here that the balance is incorrect and we need to prevent this from being persisted in the database, but if we query the database from our second connection, once again we find that nothing has changed.

![state after update queries second connection](/images/mysql/transactions-balance-fine.png)

So all we need to do now is roll back our changes.

```sql
ROLLBACK;
```

![state after rollback](/images/mysql/transactions-nothing-happened.png)  
It's as if nothing happened.

In the real world, an error in the database would trigger the rollback statement. In this example for instance, we could have a check constraint on the balance field ensuring that the balance is always non-negative. The constraint would have been violated by our sql statements which would have thrown the error which in turn triggers the rollback statement.  
Nothing would have been committed to the database, and we wouldn't have to worry about rectifying a partial update of the database.

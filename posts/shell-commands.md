---
title: "MySQL Shell Commands"
date: "2023-10-05"
---

## Motivation

I recently created a project and I happen to be using MySQL as my database provider. I haven't used MySQL in a while and when I was creating the schemas for the project a considerable amount of time was spent just familiarizing myself with MySQL and its commands all over again.

Recently, I took a small hiatus from interacting with the solution I was building and when I returned, I forgot all that I had researched. So I decided to curate a small list of the commands that I will inevitably forget once more. Next time it should only take about 5 minutes to get back up to speed.

## The MySQL Shell

The shell is a command line interface which enables users to interact with the MySQL database. The MySQL shell can be launched by typing **mysqlsh** in the windows powershell. Once the shell is launched, a connection to a database must be established and then the user can execute various commands in the shell.

## Useful Commands

- \sql
- \connect
- \quit
- SHOW
- USE
- CREATE

### \sql

This switches the execution mode to sql.

### \connect

The first thing we need to do is connect to our MySQL database. We use the **\connect** command and pass it a uri string with the connection details to accomplish this.

If you have **MySQL Workbench** you can right click on the database and click _copy connection string to clipboard_ to retrieve it.

```
\connect "your-uri-string"
```

### \quit

This command is used to exit the mysql shell.

### SHOW

Once you are connected, you can use the show command to see the databases available to you.

```
SHOW DATABASES;
```

Once a database is selected you can once more use the **SHOW** command to display all the tables that have been created.

```
SHOW TABLES;
```

### USE

In order to select a database, you use the **USE** command.

```
USE my_database;
```

### CREATE

This is used to create a database or table. We will be using it to create a database.

```
CREATE DATABASE my_database;
```

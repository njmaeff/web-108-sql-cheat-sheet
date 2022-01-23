# SQL Cheat Sheet

## login into MySQL from terminal
```bash
mysql -u myUser -p
```

## SHOW USERS
```SQL
select User, Host from mysql.user;
```

## CREATE USERS

```SQL
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
```
- localhost can be an ip-address bound to the client connecting or the wildcard `%` which means any remote address.

## GRANT PRIVILEGES, Show Granted Privileges and Remove.

```SQL
GRANT ALL PRIVILEGES ON myDB . myTable TO 'newuser'@'localhost';
```

- ALL is a mysql privilege type and is the most permissive. There are less permisive types available.
- `myDB . myTable` can be substituted for the wildcard `*` which matches any.

## SHOW, DELETE & CREATE DATABASES & SELECT DATABASES
```SQL
-- show databases
show databases;o

-- create database
create database myDB;


-- select database
use myDB;

-- delete database
drop database myDB;

```

## CREATE a TABLE with Columns and their data types

```SQL
use Library;
create table Books
(
    id       int auto_increment
        primary key,
    bookName varchar(50) not null
)
    collate = utf8mb4_unicode_ci;

-- show columns
describe Books;

```

## SHOW, DELETE & DROP Tables

```SQL
-- show tables
show tables;

-- delete tables

drop table Books;
```

## Insert ROWS & RECORDS (single and multiple)

```SQL
-- single
INSERT INTO Books (id, bookName) VALUES (1, 'How I Play Golf');

-- multiple
INSERT INTO Books (id, bookName) VALUES (1, 'How I Play Golf'), (2, 'Tiger Woods'), (3, 'Tiger Woods');
```

## SELECT with the WHERE Clause

```SQL
select * from Books where bookName = 'Tiger Woods';
```

## SELECT with the WHERE Clause using a range

```SQL
select * from Books where id between 1 and 2;
```

## DELETE an individual ROW
```sql
delete from Books where id = 1;
```
## UPDATE a ROW

```SQL
update Books
set bookName = 'How I Play Golf - Tiger Woods'
where id = 1;
```

## Add a New Column and Modify

```SQL
-- add rating column
alter table Books
add column rating INT;

-- update row
update Books
set rating = 5
where id = 1;
```

## Order by and use Distinct
```SQL
select distinct bookName, rating from Books
order by bookName;
```

## Concatenate Columns

```SQL
select concat("BookName ", bookName, " rating ", rating) as "Description" from Books;
```

## Select Distinct Rows

```SQL
select distinct bookName, rating from Books;
```

## use LIKE to Search

```SQL
select bookName, rating 
from Books
where bookName like '%Tiger%';
```
## Select using IN

```SQL
select bookName, rating 
from Books
where rating in (4, 5);
```

## Create & Remove Index

```SQL
-- create an index
create index names
on Books (id, bookName);

-- remove an index
alter table Books
drop index names;
```
## Create Two Tables Demonstrating a One-to-many Relationship
One customer, many transaction logs.

```SQL
create table Customer
(
    id       int auto_increment
        primary key,
    fullName varchar(32) not null
)
    collate = utf8mb4_unicode_ci;

create table TransactionLog
(
    id       int auto_increment
        primary key,
    customerID int not null,
    bookID int not null,
    foreign key(customerID) references Customer(id),
    foreign key(bookID) references Books(id)
)
    collate = utf8mb4_unicode_ci;

```

## Use Inner Join
Only return customers that have a transaction.

```SQL
select Customer.fullName, TransactionLog.id
from Customer inner join TransactionLog 
on Customer.id = TransactionLog.customerID;
```

## JOIN Multiple Tables
Return transactions and the associated book name

```SQL
select Customer.fullName, TransactionLog.id, Books.bookName
from Customer inner join TransactionLog 
on Customer.id = TransactionLog.customerID
inner join Books on TransactionLog.BookID = Books.id;
```

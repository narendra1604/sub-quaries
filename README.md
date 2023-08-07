# sub-quaries
single and multiple row sub quaries

Setting environment for using XAMPP for Windows.
karan@PRAVEEN c:\xampp
# mysql.exe -u root
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 13
Server version: 10.4.25-MariaDB mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| amar               |
| information_schema |
| lapak              |
| mysql              |
| nari               |
| performance_schema |
| phpmyadmin         |
| prav               |
| praveen            |
| test               |
+--------------------+
10 rows in set (0.001 sec)

MariaDB [(none)]> create database dad;
Query OK, 1 row affected (0.001 sec)

MariaDB [(none)]> use dad;
Database changed
MariaDB [dad]> create table customers(customer_id int primary key,customer_name varchar(90));
Query OK, 0 rows affected (0.027 sec)

MariaDB [dad]> desc customers;
+---------------+-------------+------+-----+---------+-------+
| Field         | Type        | Null | Key | Default | Extra |
+---------------+-------------+------+-----+---------+-------+
| customer_id   | int(11)     | NO   | PRI | NULL    |       |
| customer_name | varchar(90) | YES  |     | NULL    |       |
+---------------+-------------+------+-----+---------+-------+
2 rows in set (0.017 sec)

MariaDB [dad]> create table purchases(purchase_id int primary key,customer_id int,amount decimal(10,2));
Query OK, 0 rows affected (0.024 sec)

MariaDB [dad]> desc purchases;
+-------------+---------------+------+-----+---------+-------+
| Field       | Type          | Null | Key | Default | Extra |
+-------------+---------------+------+-----+---------+-------+
| purchase_id | int(11)       | NO   | PRI | NULL    |       |
| customer_id | int(11)       | YES  |     | NULL    |       |
| amount      | decimal(10,2) | YES  |     | NULL    |       |
+-------------+---------------+------+-----+---------+-------+
3 rows in set (0.021 sec)

MariaDB [dad]> select customer_name from customers where customer_id = ( select customer_id from purchases order by amount desc limit 1);
Empty set (0.048 sec)
'
MariaDB [dad]> insert into customers values(1,'nari'),(2,'prav'),(3,'amar'),(4,'srihari'),(5,'sri');
Query OK, 5 rows affected (0.003 sec)
Records: 5  Duplicates: 0  Warnings: 0

MariaDB [dad]> select * from customers;
+-------------+---------------+
| customer_id | customer_name |
+-------------+---------------+
|           1 | nari          |
|           2 | prav          |
|           3 | amar          |
|           4 | srihari       |
|           5 | sri           |
+-------------+---------------+
5 rows in set (0.000 sec)

MariaDB [dad]> insert into purchases values(1,1,1000),(2,2,2000),(3,3,3000),(4,4,4000),(5,5,5000);
Query OK, 5 rows affected (0.004 sec)
Records: 5  Duplicates: 0  Warnings: 0

MariaDB [dad]> select * from purchases;
+-------------+-------------+---------+
| purchase_id | customer_id | amount  |
+-------------+-------------+---------+
|           1 |           1 | 1000.00 |
|           2 |           2 | 2000.00 |
|           3 |           3 | 3000.00 |
|           4 |           4 | 4000.00 |
|           5 |           5 | 5000.00 |
+-------------+-------------+---------+
5 rows in set (0.000 sec)

MariaDB [dad]> select customer_name from customers where customer_id = ( select customer_id from purchases order by amount desc limit 1);
+---------------+
| customer_name |
+---------------+
| sri           |
+---------------+
1 row in set (0.001 sec)

MariaDB [dad]> alter table purchases add purchase_date varchar(90);
Query OK, 0 rows affected (0.015 sec)
Records: 0  Duplicates: 0  Warnings: 0

MariaDB [dad]> select*from purchases;
+-------------+-------------+---------+---------------+
| purchase_id | customer_id | amount  | purchase_date |
+-------------+-------------+---------+---------------+
|           1 |           1 | 1000.00 | NULL          |
|           2 |           2 | 2000.00 | NULL          |
|           3 |           3 | 3000.00 | NULL          |
|           4 |           4 | 4000.00 | NULL          |
|           5 |           5 | 5000.00 | NULL          |
+-------------+-------------+---------+---------------+
5 rows in set (0.000 sec)

MariaDB [dad]> update purchases set purchase_date = '2023-02-02' where purchase_id=1;
Query OK, 1 row affected (0.022 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [dad]> update purchases set purchase_date = '2023-03-03' where purchase_id=2;
Query OK, 1 row affected (0.004 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [dad]> update purchases set purchase_date = '2023-04-04' where purchase_id=3;
Query OK, 1 row affected (0.004 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [dad]> update purchases set purchase_date = '2023-05-05' where purchase_id=4;
Query OK, 1 row affected (0.022 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [dad]> update purchases set purchase_date = '2023-06-06' where purchase_id=5;
Query OK, 1 row affected (0.004 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [dad]> select*from purchases;
+-------------+-------------+---------+---------------+
| purchase_id | customer_id | amount  | purchase_date |
+-------------+-------------+---------+---------------+
|           1 |           1 | 1000.00 | 2023-02-02    |
|           2 |           2 | 2000.00 | 2023-03-03    |
|           3 |           3 | 3000.00 | 2023-04-04    |
|           4 |           4 | 4000.00 | 2023-05-05    |
|           5 |           5 | 5000.00 | 2023-06-06    |
+-------------+-------------+---------+---------------+
5 rows in set (0.000 sec)

MariaDB [dad]> select customer_name from customers where customer_id in(select customer_id from purchases where purchases_date between '2023-01-01' and '2023-06-30');
ERROR 1054 (42S22): Unknown column 'purchases_date' in 'where clause'
MariaDB [dad]> select customer_name from customers where customer_id in(select customer_id from purchases where purchase_date between '2023-01-01' and '2023-06-30');
+---------------+
| customer_name |
+---------------+
| nari          |
| prav          |
| amar          |
| srihari       |
| sri           |
+---------------+
5 rows in set (0.001 sec)

MariaDB [dad]>

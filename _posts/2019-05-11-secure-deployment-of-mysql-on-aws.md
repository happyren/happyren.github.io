---
layout: post
title: Securely deploy a MySQL server on AWS
tag: fundamental
---

In previous post, I have deployed a testing website for my project on aws. However, in order to quickly setup the testing environment, I didn't follow the good practice to make my database secured, rather, it is exposed for external attack. In this write up, I would walkthrough mysql's setup on AWS and try to make it secure.

| Software stack | Software | Version |
| :-: | :-: | :-: |
| OS | Ubuntu | 18.04 server |
| db | MySQL | 5.7.26 |

## MySQL

MySQL is claimed to be the most popular open source database, 

```shell
$ sudo apt update
$ sudo apt install mysql-server
```

After this, the **admin** shall be the **root** user for the database, and you shall be able to connect to the database without password as root.

```shell
$ sudo mysql -u root
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.26-0ubuntu0.18.04.1 (Ubuntu)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

And to double check you are logged in as root

```shell
mysql> SELECT USER(), CURRENT USER();
+----------------+----------------+
| USER()         | CURRENT_USER() |
+----------------+----------------+
| root@localhost | root@localhost |
+----------------+----------------+
1 row in set (0.00 sec)
```

> You don't need to change the root password because after 5.7 it uses a plugin called "auth_socket", it doesn't care what password you set to root user, [here is a good article about it](https://www.percona.com/blog/2016/03/16/change-user-password-in-mysql-5-7-with-plugin-auth_socket/).

### Database User Setup

For demo purpose, let's create a table for bank usage and two tables one named account the other named vendor_account. We can then create a database user to use this database, for example, we grant an accountant to view all vendor_account data.

> Here is where we should be very careful about *the least privilege principle*, do not let any user to gain authorization for access beyond their needs. eg the accountant is authorized to read vendor_account table, so no write to vendor_account, and no access to account. Detailed grant usage is [here](https://dev.mysql.com/doc/refman/5.7/en/grant.html).

```shell
mysql> CREATE USER 'accountant'@'localhost' IDENTIFIED BY 'MID)!c1DP@!<:D!@{';
Query OK, 0 rows affected (0.00 sec)

mysql> GRANT SELECT ON bank.vendor_account TO 'accountant'@'localhost';
Query OK, 0 rows affected (0.00 sec)
```

So when this user would like to access table account in bank, it shall be denied:

```shell
mysql> select * from account;
ERROR 1142 (42000): SELECT command denied to user 'accountant'@'localhost' for table 'account'

mysql> SELECT * FROM vendor_account;
+--------+---------+
| acc_id | balance |
+--------+---------+
|      1 |     200 |
|      2 |     200 |
+--------+---------+
2 rows in set (0.00 sec)

mysql> INSERT INTO vendor_account
    -> (acc_id, balance)
    -> VALUE
    -> (3, 200);
ERROR 1142 (42000): INSERT command denied to user 'accountant'@'localhost' for table 'vendor_account'
```

### Database Connection Setup

Now you want to remote connect to your database to use applications like DataGrip. You need to firstly setup your user to be used for connection:

```shell
mysql> CREATE USER 'remote'@'remotehost' IDENTIFIED BY 'BD@!&lk321e)PQ';
Query OK, 0 rows affected (0.00 sec)
```

You can check which option you are using.

```shell
$ which mysqld
/usr/sbin/mysqld
$ sudo /usr/sbin/mysqld --verbose --help | grep -A 1 "Default options"
Default options are read from the following files in the given order:
/etc/my.cnf /etc/mysql/my.cnf ~/.my.cnf
```

Then you need to modify the file **/etc/mysql/my.cnf**.

In that file, it used to be nothing if it is a fresh install, you need to add follow:

```shell
[mysqld]
user = mysql
pid-file = /var/run/mysqld/mysqld.pid
socket = /var/run/mysqld/mysqld.sock
port = 3306
basedir = /usr
datadir = /var/lib/mysql
tmpdir = /tmp
language = /usr/share/mysql/English
bind-address = Your Server IP
```

> Be very careful you have to set the IP to your server's IP **NOT** 0.0.0.0 because you don't want to expose your server to everyone.

Your server IP shall be found as your eth0 inet IP.

Then come to your AWS EC2 console, in Security Group, create a new group to connect to your mysql database, in both inbound and outbound, select **MYSQL/Aurora** for server, and **My IP** for yourself.

> If you are using elastic ip address, make sure in the .cnf file, you still use the eth0 IP aka your private IP address, but when connect to it from your own machine, use your elastic ip aka your public ip address.

Now you can remote login to mysql securely only from your own machine with specified user.

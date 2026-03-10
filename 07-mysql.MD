# MySQL 

Developer has chosen the database MySQL. Hence, we are trying to install it up and configure it.

**Versions of the DB Software you will get context from the developer, Meaning we need to check with developer.**
**Developer has shared the version information as MySQL-8.x**


You can list MySQL servers by using `dnf list mysql-server`
```
dnf list mysql-server
```
Install MySQL Service
```
dnf install mysql-server -y
```

Start MySQL Service

```
systemctl enable mysqld
```
```
systemctl start mysqld
```

Next, We need to change the default root password in order to start using the database service. Use password RoboShop@1 or any other as per your choice.

```
mysql_secure_installation --set-root-pass RoboShop@1
```

You can check the new password working or not using the following command in MySQL(We will Login into the mysql server).

Ways to login into mysql-server and check databases.

```
mysql -h mysql.lingaiah.online -uroot -pRoboShop@1
```
```
mysql
```


# Verification:
We can check data by using client package called mysql.

Usually command to connect mysql server is
```
mysql -h db.lithesh.shop -u root -pRoboShop@1
```
But if your client and server both are in a single server, you can simply issue.
```
mysql
```
Once you got mysql prompt, you can use below command to check schemas/databases exist.
```
show databases;
```
Once you are in particular schema, you can get the list of tables.
```
show tables;
```
You can get entries of a table using
```
select * from cities;
```

From the system terminal (no login required)
```
mysql --version
```
or
```
mysql -v
```
```
mysqld --version
```

# Trouble shooting commands:

MySQL Server running or not?
```
systemctl status mysqld
```

MySQL port opened or not?
```
netstat -lntp
```

MySQL process is opened or not?
```
ps -ef | grep mysql
```


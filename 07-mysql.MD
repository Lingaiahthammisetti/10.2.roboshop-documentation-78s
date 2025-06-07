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

```
mysql -h mysql.lingaiah.online -uroot -pRoboShop@1
```


Note: How to differentiate MySQL server vs MySQL client
This is the MySQL server
```
dnf install mysql-server -y
```

This is the MySQL client
```
dnf install mysql -y
```
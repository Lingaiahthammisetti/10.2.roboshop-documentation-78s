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
mysql -h localhost -uroot -pRoboShop@1
```

```
mysql
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



# Trouble shoot commands:
🌐 Check if mysql service is running
```
sudo systemctl status mysqld

```

🌐 Check if mysql is listening on port 3306
```
sudo netstat -lntp
sudo netstat -lntp | grep 3306
```

📡 Test connectivity to the mysql server on port 3306
```
telnet mysql.lingaiah.online 3306
```

🔍 Check if mysql process is running
```
ps -ef | grep mysql
```


🌍 Verify if mysql server's IP address is reachable
```
ping -c 3 mysql.lingaiah.online

````

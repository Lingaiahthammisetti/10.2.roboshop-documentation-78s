### User
User is a microservice that is responsible for User Logins and Registrations Service in RobotShop e-commerce portal.

Developer has chosen NodeJs, Check with developer which version of NodeJS is needed. Developer has set a context that it can work with NodeJS >18

Install NodeJS, By default NodeJS 10 is available, We would like to enable 18 version and install list.

*you can list modules by using dnf module list*

```
dnf module disable nodejs -y
dnf module enable nodejs:20 -y
```

Install NodeJS

```
dnf install nodejs -y
```

Configure the application.

Add application User

```
useradd roboshop
```

Lets setup an app directory.

```
mkdir /app
```

Download the application code to created app directory.

```
curl -L -o /tmp/user.zip https://roboshop-builds.s3.amazonaws.com/user.zip
```

```
cd /app 
```
```
unzip /tmp/user.zip
```

Every application is developed by development team will have some common softwares that they use as libraries. This application also have the same way of defined dependencies in the application configuration.

Lets download the dependencies.

```
cd /app 
```
```
npm install 
```

We need to setup a new service in systemd so systemctl can manage this service

Setup SystemD User Service

```
vim /etc/systemd/system/user.service
```

```
[Unit]
Description = User Service
[Service]
User=roboshop
Environment=MONGO=true
Environment=REDIS_HOST=redis.lingaiah.online
Environment=MONGO_URL="mongodb://mongodb.lingaiah.online:27017/users"
ExecStart=/bin/node /app/server.js
SyslogIdentifier=user

[Install]
WantedBy=multi-user.target
```

Load the service.

```
systemctl daemon-reload
```

```
systemctl enable user 
```
```
systemctl start user
```

For the application to work fully functional we need to load schema to the Database. Then

**NOTE: Schemas are usually part of application code and developer will provide them as part of development.**

We need to load the schema. To load schema we need to install mongodb client.

To have it installed we can setup MongoDB repo and install mongodb-client

```
vim /etc/yum.repos.d/mongo.repo
```

```
[mongodb-org-7.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/9/mongodb-org/7.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://pgp.mongodb.com/server-7.0.asc
```
```
dnf install mongodb-mongosh -y
```

Load Schema

```
mongosh --host mongodb.lingaiah.online </app/schema/user.js
```


# Trouble shoot commands:
🌐 Check if User service is running
```
sudo systemctl status user

```

🌐 Check if User is listening on port 8080
```
sudo netstat -lntp
sudo netstat -lntp | grep 8080
```

📡 Test connectivity to the User server on port 8080
```
telnet user.lingaiah.online 8080
```

🔍 Check if User process is running
```
ps -ef | grep user
```


🌍 Verify if User server's IP address is reachable
```
ping -c 3 user.lingaiah.online

````


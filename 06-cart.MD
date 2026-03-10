### Cart
Cart is a microservice that is responsible for Cart Service in RobotShop e-commerce portal.

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
curl -L -o /tmp/cart.zip https://roboshop-builds.s3.amazonaws.com/cart.zip
```

```
cd /app 
```
```
unzip /tmp/cart.zip
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

Setup SystemD Cart Service

```
vim /etc/systemd/system/cart.service
```

```
[Unit]
Description = Cart Service
[Service]
User=roboshop
Environment=REDIS_HOST=redis.lingaiah.online
Environment=CATALOGUE_HOST=catalogue.lingaiah.online
Environment=CATALOGUE_PORT=8080
ExecStart=/bin/node /app/server.js
SyslogIdentifier=cart

[Install]
WantedBy=multi-user.target
```

Load the service.

```
systemctl daemon-reload
```

```
systemctl enable cart 
```
```
systemctl start cart
```



# Trouble shooting commands:

Cart Server running or not?
```
systemctl status cart
```
Cart port opened or not?
```
netstat -lntp
```
```
netstat -lntp | grep 8080
```
Cart process is opened or not?
```
ps -ef | grep cart
```
Cart is able to connect Catalogue:
```
telnet catalogue.lithesh.shop 8080
```
Cart is able to connect Redis db:
```
telnet redis.lithesh.shop 6379
```
Verify if Cart server's IP address is reachable
```
ping -c 3 Cart.lingaiah.online

````


We already discussed in Linux basics section that applications should run as nonroot user.

Add application User
```
useradd --system --home /app --shell /sbin/nologin --comment "roboshop system user" roboshop
```
Explanation:
That command creates a system user named roboshop. Here’s what each part does:

useradd \
  --system                 # Create a system account (usually UID < 1000)
  --home /app               # Set /app as the home directory
  --shell /sbin/nologin     # Disable interactive shell login
  --comment "roboshop system user"  # Description (GECOS field)
  roboshop


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



# Trouble shoot commands:
🌐 Check if Cart service is running
```
sudo systemctl status cart

```

🌐 Check if Cart is listening on port 8080
```
sudo netstat -lntp
sudo netstat -lntp | grep 8080
```

📡 Test connectivity to the Cart server on port 8080
```
telnet cart.lingaiah.online 8080
```

🔍 Check if Cart process is running
```
ps -ef | grep cart
```


🌍 Verify if Cart server's IP address is reachable
```
ping -c 3 cart.lingaiah.online

````



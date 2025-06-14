### Payment

This service is responsible for payments in RoboShop e-commerce app. This service is written on Python 3.6, So need it to run this app.

Install Python 3.6

```
dnf install python3.11 gcc python3-devel -y
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
curl -L -o /tmp/payment.zip https://roboshop-builds.s3.amazonaws.com/payment.zip
```

```
cd /app 
```

```
unzip /tmp/payment.zip
```

Every application is developed by development team will have some common softwares that they use as libraries. This application also have the same way of defined dependencies in the application configuration.

Lets download the dependencies.

```
cd /app 
```

```
pip3.11 install -r requirements.txt
```

We need to setup a new service in systemd so systemctl can manage this service

Setup SystemD Payment Service

```
vim /etc/systemd/system/payment.service
```

```
[Unit]
Description=Payment Service

[Service]
User=root
WorkingDirectory=/app
Environment=CART_HOST=cart.lingaiah.online
Environment=CART_PORT=8080
Environment=USER_HOST=user.lingaiah.online
Environment=USER_PORT=8080
Environment=AMQP_HOST=rabbitmq.lingaiah.online
Environment=AMQP_USER=roboshop
Environment=AMQP_PASS=roboshop123

ExecStart=/usr/local/bin/uwsgi --ini payment.ini
ExecStop=/bin/kill -9 $MAINPID
SyslogIdentifier=payment

[Install]
WantedBy=multi-user.target
```

Load the service.

```
systemctl daemon-reload
```

Start the service.

```
systemctl enable payment 
```

```
systemctl start payment
```

# Trouble shoot commands:
🌐 Check if payment  service is running
```
sudo systemctl status payment 

```

🌐 Check if payment  is listening on port 8080
```
sudo netstat -lntp
sudo netstat -lntp | grep 8080
```

📡 Test connectivity to the payment  server on port 8080
```
telnet payment.lingaiah.online 8080
```

🔍 Check if payment  process is running
```
ps -ef | grep payment 
```


🌍 Verify if payment server's IP address is reachable
```
ping -c 3 payment.lingaiah.online

````
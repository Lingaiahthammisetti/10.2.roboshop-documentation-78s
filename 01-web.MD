# 01-WEB

* The Web/Frontend is the service in RoboShop to serve the web content over Nginx. 
* This will have the web page for the web application.
* Developer has chosen Nginx as a web server and thus we will install Nginx Web Server. 


You can list modules by using "dnf module list"
```
dnf module list
```

Install Nginx 
```
dnf install nginx -y
```

Start & Enable Nginx service 
```
systemctl enable nginx
```
```
systemctl start nginx
```

Try to access the service once over the browser and ensure you get some default content.
```
http://frontend.lingaiah.online:80
```
OR
```
http://lingaiah.online:80
```
Remove the default content that web server is serving. 

```
rm -rf /usr/share/nginx/html/*
```

Download the frontend content

```
curl -o /tmp/web.zip https://roboshop-builds.s3.amazonaws.com/web.zip
```

Extract the frontend content.

```
cd /usr/share/nginx/html
```
```
unzip /tmp/web.zip
```

Try to access the nginx service once more over the browser and ensure you get roboshop content.

Create Nginx Reverse Proxy Configuration and Add the following content.

``` 
vim /etc/nginx/default.d/roboshop.conf 
```

```
proxy_http_version 1.1;
location /images/ {
  expires 5s;
  root   /usr/share/nginx/html;
  try_files $uri /images/placeholder.jpg;
}
location /api/catalogue/ { proxy_pass http://catalogue.lingaiah.online:8080/; }
location /api/user/ { proxy_pass http://user.lingaiah.online:8080/; }
location /api/cart/ { proxy_pass http://cart.lingaiah.online:8080/; }
location /api/shipping/ { proxy_pass http://shipping.lingaiah.online:8080/; }
location /api/payment/ { proxy_pass http://payment.lingaiah.online:8080/; }

location /health {
  stub_status on;
  access_log off;
}
```

Ensure you replace the `localhost` with the actual ip address of those component server. Word `localhost` is just used to avoid the failures on the Nginx Server.

Restart Nginx Service to load the changes of the configuration.

``` 
systemctl restart nginx 

```

# Trouble shoot commands:

🌐 Check if Nginx is listening on port 80
```
netstat -lntp | grep :80
```

📡 Test connectivity to the Nginx server on port 80
```
telnet lingaiah.online 80
```

🔍 Check if Nginx process is running
```
ps -ef | grep nginx
```


🌍 Verify if Nginx server's IP address is reachable
```
ping -c 3 nginx.lingaiah.online

```

# Additional Information:

```
Download the zip file to /tmp
curl -o /tmp/web.zip https://roboshop-builds.s3.amazonaws.com/web.zip
This downloads the web.zip file from the given URL and saves it in the /tmp directory.
You are not downloading it into /usr/share/nginx/html, but instead into the temporary directory.

Navigate to the Nginx HTML directory
cd /usr/share/nginx/html
You move into the directory where the frontend content is supposed to be extracted.

Extract the zip file from /tmp into the current directory
unzip /tmp/web.zip
This command extracts the contents of /tmp/web.zip into the current directory, which is now /usr/share/nginx/html.
```
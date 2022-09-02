# WEB STACK IMPLEMENTATION (LEMP STACK)
LEMP is an open-source web application stack used to develop web applications. The term LEMP is an acronym that represents L for the Linux Operating system, Nginx (pronounced as engine-x, hence the E in the acronym) web server, M for MySQL database, and P for PHP scripting language.
In this project, implementation of a similar stack is done, but with an alternative Web Server – NGINX, which is also very popular and widely used by many websites in the Internet.
creating a new ec2 instance for project 2
![Ubuntu server](./images/Ubuntu%20server%20.png)


## INSTALLING THE NGINX WEB SERVER
'sudo apt update'
'sudo apt install nginx'
'sudo systemctl status nginx'
![sudo status systemctl nginx](./images/sudo%20systemctl%20status%20nginx.png)
'curl http://localhost:80'
![nginx page](./images/nginx%20page%20.png)

## INSTALLING PHP
' sudo apt install mysql-server'
'sudo mysql'
![sudo apt install mysql-server](./images/Sudo%20apt%20install%20mysqlserver.png)
' ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';'
'mysql> exit'

![Mysql server](./images/Mysql%20server.png)


'sudo mysql_secure_installation'
' sudo mysql -p'
' mysql> exit'
' sudo apt install php-fpm php-mysql'
![sudo apt install php-fpm php-mysql](./images/sudo%20apt%20install%20php-fpm%20php-mysql.png)

## CONFIGURING NGINX TO USE PHP PROCESSOR.
' sudo mkdir /var/www/projectLEMP'
' sudo chown -R $USER:$USER /var/www/projectLEMP'
' sudo nano /etc/nginx/sites-available/projectLEMP'. This will create a new blank file. Pasting in the following bare-bones configuration:
#/etc/nginx/sites-available/projectLEMP
![php with nginx](./images/php%20with%20nginx.png)

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
![project lamp nginx 2](./images/projectlamp%20nginx%202.png)
Activating the configuration by linking to the config file from Nginx’s sites-enabled directory:

sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
' sudo nginx -t'
![sudo nginx -t](./images/sudo%20nginx%20-t.png)
We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:

sudo unlink /etc/nginx/sites-enabled/default
When you are ready, reload Nginx to apply the changes:

sudo systemctl reload nginx
'sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html'
![hello lemp](./images/hello%20lemp.png)

## Testing PHP with Nginx
' sudo nano /var/www/projectLEMP/info.php'
![alt text](./images/images/php%20file.png)
http://`server_domain_or_IP`/info.php
![php page loaded](./images/Php%20page%20loaded.png)

After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. You can use rm to remove that file:

'sudo rm /var/www/your_domain/info.php'

## RETRIEVING DATA FROM MYSQL DATABASE WITH PHP
' sudo mysql'
To create a new database, run the following command from your MySQL console:

'mysql> CREATE DATABASE `example_database`;'
'mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';'
Now we need to give this user permission over the example_database database:

'mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';'
'mysql> exit'
![create database 1](./images/creat%20database%201.png)
![create database](./images/create%20database%202.png)
You can test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:

'mysql -u example_user -p'
'CREATE TABLE example_database.todo_list (
mysql>     item_id INT AUTO_INCREMENT,
mysql>     content VARCHAR(255),
mysql>     PRIMARY KEY(item_id)
mysql> );'
'mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
'mysql> exit'
' nano /var/www/projectLEMP/todo_list.php'



![todo list](./images/todo%20list.png)


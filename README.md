# Lempstack
# LEMP Stack Setup on Ubuntu

This project documents the setup of a LEMP stack (Linux, Nginx, MySQL, PHP) on an Ubuntu server. The stack was deployed on an AWS EC2 instance and configured to serve a dynamic web application.

## Components

*   **Linux:** Ubuntu Server 22.04 LTS
*   **Nginx:** High-performance web server
*   **MySQL:** Relational database management system
*   **PHP:** Server-side scripting language (PHP-FPM)

---

## Step-by-Step Installation Guide

### 1. Prerequisites
*   An AWS EC2 instance running Ubuntu 22.04 LTS.
*   A security group allowing inbound traffic on SSH (22), HTTP (80), and HTTPS (443).

### 2. Installing the Nginx Web Server
Nginx was installed to serve the web pages.

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl status nginx
```

**Verification:**

`nginx -v` The command confirms the installation of nginx .

`curl http://localhost:80` This command verifies the server is running locally.


> **📸 Screenshot Opportunity:** Take a screenshot of your browser showing the default "Welcome to nginx!" page using your EC2 instance's public IP address. Name it `nginx-welcome.png`.

![Nginx Welcome Page](images/nginx-welcome.png)

### 3. Installing MySQL
MySQL was installed to store and manage data for the site.

`sudo apt install mysql-server`
This comand installs the mysql database server into your machine 

`sudo mysql` This command allows us to log into our mysql sever


The MySQL security script was run to secure the installation:
```bash
sudo mysql_secure_installation
```
This installation allows for more security other than the basic scurity that is configured by default with the server , This is a way of beefing up security on of the database
> **📸 Screenshot Opportunity:** Take a screenshot of the terminal showing the successful output of `sudo mysql_secure_installation` or a successful login to the MySQL prompt with `sudo mysql -p`. Name it `mysql-secure.png`.

![MySQL Secure Installation](images/mysql-secure.png)

### 4. Installing PHP
PHP was installed to process code and generate dynamic content. The packages `php-fpm` (PHP FastCGI Process Manager) and `php-mysql` (a module for MySQL database communication) were used.

```bash
sudo apt install php-fpm php-mysql
```

This two code is one of the major codes that diffencites the LEMP stack from the LAMP stack. The first code Php and all the pacakages that comes with it which is mean for processing and delivering content , the second code installs the module that enables php to commjunicate and work with mysql which is very important for php to deliver content domiciled in the my datatbse

`php -v` for reassursnce that php was installed this code allows us see if php was installed and which version we have.

### 5. Configuring Nginx to Use PHP Processor
A server block (similar to an Apache virtual host) was created to tell Nginx how to handle PHP files.

**1. Created the project root directory:**
```bash
sudo mkdir /var/www/projectLEMP
sudo chown -R ubuntu:ubuntu /var/www/projectLEMP
```

**2. Created and configured the server block file:**
The file `/etc/nginx/sites-available/projectLEMP` was created with the following key configuration to handle PHP requests:
```
location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
}
```

**3. Enabled the site and disabled the default:**
```bash
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
sudo nginx -t # Test configuration for errors
sudo unlink /etc/nginx/sites-enabled/default
sudo systemctl restart nginx
```

### 6. Testing PHP with Nginx
A test PHP file was created to verify that Nginx could correctly hand off PHP files to the PHP processor.

**1. Created a test PHP file:**
```bash
sudo vim /var/www/projectLEMP/info.php
```
The file contained:
```php
<?php
phpinfo();
```
> **⚠️ Important:** This file was removed after testing for security reasons.

**2. Tested in a browser:**
The URL `http://<Public-IP-Address>/info.php` was accessed.

> **📸 Screenshot Opportunity:** This is a crucial screenshot. Take a clear screenshot of your browser showing the detailed `phpinfo()` page, which confirms PHP is working through Nginx. Name it `phpinfo-page.png`.

![PHP Info Page](images/phpinfo-page.png)

### 7. Creating a PHP Application to Interact with MySQL
A simple PHP script was created to connect to MySQL and fetch data, demonstrating the full stack functionality.

**1. MySQL setup (example commands executed in MySQL prompt):**
```sql
CREATE DATABASE example_database;
CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
GRANT ALL ON example_database.* TO 'example_user'@'%';
exit
```

**2. Created a test table and data:**
```sql
CREATE TABLE example_database.todo_list (
    item_id INT AUTO_INCREMENT,
    content VARCHAR(255),
    PRIMARY KEY(item_id)
);
INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
```

**3. Created the PHP script `todo_list.php`:**
The file `/var/www/projectLEMP/todo_list.php` was created to connect to the database and query the `todo_list` table, displaying the results on a web page.

> **📸 Screenshot Opportunity:** The final result! Take a screenshot of your browser showing the output of the `todo_list.php` script, which displays the todo items fetched from the MySQL database. Name it `todo-list-app.png`.

![Todo List Application](images/todo-list-app.png)

---

## Project Structure

```
/var/www/projectLEMP/
├── index.html      # Initial test file
├── info.php        # PHP test file (removed after testing)
└── todo_list.php   # PHP application interacting with MySQL
```

## Security Notes

*   The `info.php` file was removed after testing to avoid exposing server information.
*   The MySQL installation was secured using the `mysql_secure_installation` script.
*   AWS security groups were configured to restrict access to necessary ports only.

## Conclusion

This project successfully demonstrates a fully functional LEMP stack on Ubuntu, capable of serving static content, processing PHP, and interacting with a MySQL database.
```

### Instructions for You:

1.  **Create an `images` folder** in your GitHub repository.
2.  **Take the four screenshots** mentioned in the `README.md` file (look for the **📸 Screenshot Opportunity** comments).
3.  **Save the screenshots** with the exact names suggested:
    *   `nginx-welcome.png`
    *   `mysql-secure.png`
    *   `phpinfo-page.png`
    *   `todo-list-app.png`
4.  **Upload these four image files** into the `images` folder you created on GitHub.
5.  **Copy the entire markdown text** from the code block above into a new file in your repository named `README.md`.

Once you push the `README.md` file and the `images` folder to GitHub, it will automatically render the guide with your pictures in the correct places.

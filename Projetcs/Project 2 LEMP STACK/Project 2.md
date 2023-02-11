                                  LEMP STACK IMPLEMENTATION

>#  Installing Ngnix 

  To install LEMP, i installed Ngix with the command below

`sudo apt update`

`sudo apt install ngnix`

To verify if ngnix has been sucessfully installed run 

`sudo systemctl status nginx`

                                       Output Image

 ![ngnix](Images/Systemctl%20Output%20ngnix.png)                                      

To allow HTTP connectivity, and additional rule was added on EC2. 
The rule enabled port 80 for HTTP connection 

The ngnix connectivty was confirmed on the browser using 

**http://EC2publicIP:80**

                                       Output Image

![ngnix_output](Images/ngnix%20browser%20output.png)


> ## Installing MYSQL

Now we need to install a database that will manage the data for our website. The was done using the code: 

`$ sudo apt install mysql-server`

When the installation is finished, log in to the MySQL console by typing:

`$ sudo mysql`

The above command will connect to mySQL server as an admin user root. 

A password was set for the root user using the commad below

`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`

Exit the MySQL shell with:

`mysql> exit`

To create a password criteria, a secure installation was done using the command below 

`$ sudo mysql_secure_installation`

>#  Installing PHP

We need to install PHP to process code and generate dyanamic content on t he web server. 

This was one using the command below. 

`sudo apt install php-fpm php-mysql`

# Configuring Ngnix to use PHP processor

We created a root web directory the the domain using 

`sudo mkdir /var/www/projectLEMP`

Next, we assign ownership of the directory with the $USER environment variable, which will reference the current system user:

`sudo chown -R $USER:$USER /var/www/projectLEMP`

A new configuration file in Ngnix's site-available directory was created. 

`sudo nano /etc/nginx/sites-available/projectLEMP`

#/etc/nginx/sites-available/projectLEMP

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
The code above was pasted in the projectLEMP file.

The configuration was activated by linking the projectLEMP file to the ngnix's sites enabled directory using the command

`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

This will tell Nginx to use the configuration next time it is reloaded. We  tested configuration for syntax errors by using

`sudo nginx -t`

                                       Output Image
![ngnix config ok](Images/ngnix%20config%20ok.png)

We also disable the default Ngnix host currently using PORT 80 using 

`sudo unlink /etc/nginx/sites-enabled/default`

These changes was applied using the command below

`sudo systemctl reload nginx`

The website is now active. 

This was confirmed using 

`sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html`

The output was viewed on the browser using 

**http://EC2publicIP:80**


 # Testing PHP with Ngnix

To test the functionality of PHP on ngnix, create a text file info.php in tghe PHP root document 

`sudo nano /var/www/projectLEMP/info.php`

```
<?php
phpinfo();
```
The above code code was copied into the info.php file.

The php page was accessed using 

http://EC2PUBLICip/info.php

                                       Output Image
![php_output](Images/Php%20output.png)


# Retrieving Data From MYSQL Database with PHP

we connected to mySql using the command 

`sudo mySql`

A new database  with the name example_database was created using

`mysql> CREATE DATABASE `example_database`;

An example_user was created and access to the database granted to the user

`mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`

`mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';`

Exit the MySql shell

I connected to the newly created user example_user using 

`mysql -u example_user -p`

A todo list table was created using the command below

```
CREATE TABLE example_database.todo_list (
          item_id INT AUTO_INCREMENT,
           content VARCHAR(255),
           PRIMARY KEY(item_id)
      );
````

  I inputted a few rows into the test table using 

  `mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item"),("My Second important item"),("My third important item");`

I exited tehsql console

A todo_list.php file was created under the projectLEMP dir and a PHP script that will connect to the database was configured in the todo_list.php file

`nano /var/www/projectLEMP/todo_list.php`

```
<?php
$user = "example_user";
$password = "password";
$database = "example_database";git 
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
```
The todo list was confirmed on the browser

                                       Output Image

![todolist](/Projetcs/Project%202%20LEMP%20STACK/Images/todo.php.png)

                                       END


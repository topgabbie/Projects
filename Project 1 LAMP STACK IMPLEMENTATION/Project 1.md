>>>>> ## INSTALLING LAMP STACK ON AMAZON EC2
----
## INSTALLING APACHE 2 AND UPDATING FIREWALL

`sudo apt update`  

`sudo apt install apache2`

`systemctl status apache2`

                             **output image below**

![Apache_status](Images/APACHE%20STATUS.png)

![Apache_output](./Images/Browser%20Output.png)

-------------------------------

# INSTALLING MYSQL

`sudo apt install mysql-server`

`sudo mysql`

`sudo mysql_secure_installation`

> ## INSTALLING PHP

`sudo apt install php libapache2-mod-php php-mysql`

## CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE

`sudo mkdir /var/www/projectlamp`

`sudo chown -R $USER:$USER /var/www/projectlamp`


`sudo nano /etc/apache2/sites-available/projectlamp.conf`

**the configuration  below was pasted in projectlamp.conf**
 ```
 <VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

`sudo ls /etc/apache2/sites-available`

                             **output image below**

![site-available](./Images/Sites%20available.PNG)

`sudo a2ensite projectlamp`

`sudo a2dissite 000-default`

`sudo apache2ctl configtest`

                             **output image below**

![configTest](./Images/configtest%20output.PNG)

`sudo systemctl reload apache2`

`sudo echo 'Hello LAMP !!!' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html`

                             **output image below**

![site_output](Images/Site%20output.png)


# ENABLE PHP ON THE WEBSITE

`sudo vim /etc/apache2/mods-enabled/dir.conf`

**the configuration  below was pasted in projectlamp.conf**

```
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```
`sudo systemctl reload apache2`

![php_output](Images/Php_output.png)

                                        # END 


### STEP 1: SETTING UP EC2 INSTANCE
I started my Steghub project by creating an EC2 instance on my already existing aws account. I proceeded to create a key pair to link to the remote server for my laptop more securely. 
```sh
chmod 400 steghub.pem
ssh -i "steghub.pem" ubuntu@ec2-34-227-7-216.compute-1.amazonaws.com
```
![myimage](https://github.com/Adejuwonvictor/Steghub_repo/blob/main/LAMP-STACK/images/Screenshot%20(37).png)
![myimage](https://github.com/Adejuwonvictor/Steghub_repo/blob/main/LAMP-STACK/images/Screenshot%20(39).png)
### STEP 2: UPDATING PACKAGES AND INSTALLING APACHE
Immediately it was setup, i updated the packages on the ubuntu linux system and proceeded to install apache webserver on my EC2 instance.  Then after using the curl command to display the default apache webpage on my terminal, i proceeded to display the default webpage on my web-browser. I navigated to the inbound connection settings of my ec2 instance to allow my IP address connect to the server on port 80. Then i copied the public ip address of the ec2 instance into my browser search bar and i was able to display the default ubuntu webpage.   

```sh
sudo apt update
sudo apt upgrade -y
```
To run apache:
```sh
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl status apache2
```
To test run the default apache page:
```sh
curl http://localhost:80
```   
### STEP 3: SETTING UP SQL AND PHP
Next i installed my sql on my ec2 instance and then i ran the interactive script and then i exited mysql. Then, i ran the command to install php and the associated modules and libraries.   
**Setting up mysql**:
```sh
sudo apt install mysql-server
sudo systemctl enable --now mysql
sudo systemctl status mysql
```
Entering mysql console and setting root password:
```sh
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Pswd@@';
exit
```
Installing required php modules:
```sh
sudo apt install php libapache2-mod-php php-mysql
php -v
```
### SETTING UP VIRTUAL HOST
I created a domain called projectlamp in the **/var/www** directory of the linux system and assigned the ownership of the directory to my current user. Then i set up a configuration file using the vim editor in **/etc/apache2/sites-available** to set my projectlamp as a web root folder to be used my apache. I initially faced errors with the configuration file whcih i was able to detect with the **apache2ctl configtest** command. I had some spacing and indentation problems which i was able to fix and ran the command again to ensure i didnt have any errors left. Then i created the recommended html file in the projectlamp root directory and becaue i had already set up the inbound connections before, i was able to view the new webpage after i disabled the custom apche webpage.   
Create the server directory and assign permissions to your current user:
```sh
sudo mkdir /var/www/projectlamp
sudo chown -R $USER:$USER /var/www/projectlamp
chmod 755 /var/www/projectlamp
```
Create new configuration file for the virtual host:
```sh
sudo vim /etc/apache2/sites-available/projectlamp.conf
```
Add the following configuration:
```sh
<VirtualHost *:80>
  ServerName projectlamp
  ServerAlias www.projectlamp
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/projectlamp
  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Disable the default site and enable the new virtual host;
```sh
sudo a2dissite 000-default
sudo a2ensite projectlamp
```
Test Apache Configuration file:
```sh
sudo systemctl reload apache2
```
Set up the browser page:   
Create the index.html file in /var/www/projectlamp and input the following

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Simple Website</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
        }
        header {
            background-color: #4CAF50;
            color: white;
            padding: 15px 10px;
            text-align: center;
        }
        nav {
            margin: 0;
            padding: 10px;
            background-color: #333;
        }
        nav a {
            color: white;
            margin: 0 15px;
            text-decoration: none;
        }
        main {
            padding: 20px;
        }
        footer {
            background-color: #333;
            color: white;
            text-align: center;
            padding: 10px;
            position: fixed;
            width: 100%;
            bottom: 0;
        }
    </style>
</head>
<body>
    <header>
        <h1>Welcome to My Simple Website</h1>
    </header>
    <nav>
        <a href="#">Home</a>
        <a href="#">About</a>
        <a href="#">Contact</a>
    </nav>
    <main>
        <h2>Home Page</h2>
        <p>This is a simple website deployed on an Apache server. You can navigate to different sections using the links above.</p>
    </main>
    <footer>
        &copy; 2024 My Simple Website
    </footer>
</body>
</html>

```
Display the webpage on your browser:
![image](https://github.com/Adejuwonvictor/Steghub_repo/blob/main/LAMP-STACK/images/Screenshot%20(46).png)

### SETTING UP PHP ON THE WEBSITE

Step 1: Edit the /etc/apache2/mods-enabled/dir conf file to prioritize idex.php file
```sh
sudo vim /etc/apache2/mods-enabled/dir.conf
```
Step 2: create a new index.php file in the /var/www/projectlamp
```sh
vim /var/wwww/projectlamp/index.php
```
Step 2: Include the following in the file
```sh
<?php
phpinfo ();
```
save the file when done and refresh the page


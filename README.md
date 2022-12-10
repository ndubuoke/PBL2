# PBL2
This is a LEMP Stack Installation guide

## Perequisites
In order to complete this project, you will need an AWS account and a virtual server with Ubuntu Server OS.

## Step 1 – Installing the Nginx Web Server

In order to display web pages to our site visitors, we are going to employ Nginx, a high-performance web server. We’ll use the `apt` package manager to install this package.
Since this is our first time using apt for this session, start off by updating your server’s package index. Following that, you can use apt install to get Nginx installed:

```
sudo apt update
sudo apt install nginx
```
![Screenshot from 2022-12-07 08-23-50](https://user-images.githubusercontent.com/84657461/206115964-3a97c242-3dff-47da-94d8-e19d32dbc751.png)

When prompted, enter `Y` to confirm that you want to install Nginx. Once the installation is finished, the Nginx web server will be active and running on your Ubuntu server.

To verify that nginx was successfully installed and is running as a service in Ubuntu, run:

```
sudo systemctl status nginx
```
![Screenshot from 2022-12-07 08-27-05](https://user-images.githubusercontent.com/84657461/206116039-0391d462-e6e8-4ef5-9b79-5a13cae3839c.png)

If it is green and running, then you did everything correctly – you have just launched your first Web Server in the Clouds!

First, let us try to check how we can access it locally in our Ubuntu shell, run:

```
curl http://localhost:80
or
curl http://127.0.0.1:80
```
![Screenshot from 2022-12-07 08-27-41](https://user-images.githubusercontent.com/84657461/206116151-e0f268aa-75f1-4cae-adf8-968c491df0f1.png)

Now it is time for us to test how our Nginx server can respond to requests from the Internet.
To retrieve your Public IP address, other than to check it in AWS Web console, is to use following command:

```
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```
![Screenshot from 2022-12-07 08-34-52](https://user-images.githubusercontent.com/84657461/206116751-eab1dcc4-f44b-4b27-b506-1a3a3c8db477.png)

---
---

## STEP 2 — INSTALLING MYSQL

Now that you have a web server up and running, you need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database. MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.

Again, use `apt` to acquire and install this software:

```
sudo apt install mysql-server
```
![Screenshot from 2022-12-07 08-44-29](https://user-images.githubusercontent.com/84657461/206118423-d91df027-1e78-40e9-9439-059e54798f5e.png)

When prompted, confirm installation by typing `Y`, and then ENTER.
When the installation is finished, log in to the MySQL console by typing:

```
sudo mysql
```
![Screenshot from 2022-12-07 08-45-51](https://user-images.githubusercontent.com/84657461/206118725-eef65019-03e4-4029-bfba-33408597ff31.png)

It’s recommended that you run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system.
Before running the script, you will set a password for the root user, using mysql_native_password as default authentication method. We’re defining this user’s password as `PassWord.1`

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```
Exit the MySQL shell with:
```
exit
```
Start the interactive script by running:
```
sudo mysql_secure_installation
```

This will ask if you want to configure the VALIDATE PASSWORD PLUGIN.
>Note: Enabling this feature is something of a judgment call. If enabled, passwords which don’t match the >specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you >should always use strong, unique passwords for database credentials.
Answer `Y for yes`, or anything else to continue without enabling.

![Screenshot from 2022-12-07 08-55-09](https://user-images.githubusercontent.com/84657461/206121367-bd8bdc42-b70a-457f-8f6d-7bcf73d2fd16.png)

If you answer `yes`, you’ll be asked to select a level of password validation. Keep in mind that if you enter 2 for the strongest level, you will receive errors when attempting to set any password which does not contain numbers, upper and lowercase letters, and special characters, or which is based on common dictionary words e.g `PassWord.1`

Regardless of whether you choose to set up the VALIDATE PASSWORD PLUGIN, your server will next ask you to select and confirm a password for the MySQL root user. This is not to be confused with the system root. The database root user is an administrative user with full privileges over the database system. Even though the default authentication method for the MySQL root user dispenses the use of a password, even when one is set, you should define a strong password here as an additional safety measure.

If you enabled password validation, you’ll be shown the password strength for the root password you just entered and your server will ask if you want to continue with that password. If you are happy with your current password, enter Y for “yes” at the prompt:

```
Estimated strength of the password: 100
Do you wish to continue with the password provided? (Press y|Y for Yes, any other key for No): y
```
For the rest of the questions, press Y and hit the ENTER key at each prompt. This will prompt you to change the root password, remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately respects the changes you have made.
When you’re finished, test if you’re able to log in to the MySQL console by typing:

```
sudo mysql -p
```
Notice the -p flag in this command, which will prompt you for the password used after changing the root user password.
To exit the MySQL console, type:
```
exit
```
---
---

### STEP 3 – INSTALLING PHP

You have Nginx installed to serve your content and MySQL installed to store and manage your data. Now you can install PHP to process code and generate dynamic content for the web server.
While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. You’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.
To install these 2 packages at once, run:

```
sudo apt install php-fpm php-mysql
```

![Screenshot from 2022-12-10 12-32-42](https://user-images.githubusercontent.com/84657461/206852892-bab4e033-4c9e-4575-a6ac-7e39d4665796.png)

---
---

### STEP 4 — CONFIGURING NGINX TO USE PHP PROCESSOR

When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. In this guide, we will use project LEMP as an example domain name.

On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instead of modifying /var/www/html, we’ll create a directory structure within /var/www for your domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites.
Create the root web directory for your domain as follows:

```
sudo mkdir /var/www/projectLEMP
```
Next, assign ownership of the directory with the $USER environment variable, which will reference your current system user:

```
sudo chown -R $USER:$USER /var/www/projectLEMP
```
Then, open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use nano:

```
sudo nano /etc/nginx/sites-available/projectLEMP
```
This will create a new blank file. Paste in the following bare-bones configuration:


![Screenshot from 2022-12-10 12-36-35](https://user-images.githubusercontent.com/84657461/206853074-80a46dc8-e7ab-44b1-85eb-78eba7c8aa70.png)


![Screenshot from 2022-12-10 12-36-05](https://user-images.githubusercontent.com/84657461/206853127-a82c8365-639c-48e2-a4a7-7ccc75ff725e.png)


















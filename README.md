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




















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








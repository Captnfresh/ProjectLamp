
# WEB Stack Implementation (L.A.M.P Stack in AWS)

Here’s a step-by-step guide on how to set up a LAMP stack (Linux, Apache, MySQL, PHP) on an AWS EC2 t2.micro instance with Ubuntu 24.04 LTS for a DevOps project.




## Prerequisites

1. Install Windows Terminal
2. Open an AWS account and provision an Ubuntu server
3. SSH key pair for EC2 instance
## Step 1 - Get your instance up and running

1. Sign up to AWS console following this [instruction](https://console.aws.amazon.com/).
2. After successfully signing up, launch a new instance.

   ![image 1](https://github.com/user-attachments/assets/00676c46-bc7f-4771-9644-80a22dc143a5)


3. Select a region close to you and launch an instance of t2.micro family with Linux ubuntu server.
4. Create a new .pem priavte key(If you don't have one already) and save it securely.
5. Launch your ec2 instance

    ![image 2](https://github.com/user-attachments/assets/7332235d-db55-4c93-90f6-6d9795d49e64)



## Step 2 - Connect to the ec2 instance via ssh
      
1. Open your Windows terminal tool and navigate to your downloads directory

   `cd downloads`

3. Connect to the instance by running
`ssh -i your-key.pem ubuntu@(your-ec2-public-ip)`

    ![image 3](https://github.com/user-attachments/assets/e6be0403-9a38-4cbb-8edb-389a1ce8b105)


 Now, you're connected to your ec2 instance via ssh    

 ![image 4](https://github.com/user-attachments/assets/3f4dec40-658a-49f3-9b7d-b03d33bcdc85)


## Step 3 - Install Apache and update the firewall

1. Install Apache using Ubuntu's package manager 'apt'. 
Run the following commands

`sudo apt update`
    
 ![image 5](https://github.com/user-attachments/assets/7e935353-ebac-497b-8d59-6a66517f15f6)


`sudo apt install apache2`
    
 ![image 6](https://github.com/user-attachments/assets/ee60ace3-f6b8-4101-8e47-fe2c80c8dc44)

 ![image 7](https://github.com/user-attachments/assets/a36b3016-315e-4049-983d-120954fd6d6b)


2. To verify apache is running, run this command

`sudo systemctl status apache2`
          
 ![image 8](https://github.com/user-attachments/assets/068aeaff-91df-4bc7-aa61-6896d15311d4)

 ![image 9](https://github.com/user-attachments/assets/89b464ae-e64c-4f99-b524-53a1340a4e7c)

3. Now that your server is running, check it via http://(your-ec2-public-ip)

    ![image 10](https://github.com/user-attachments/assets/5543382d-f42c-4c5b-88f4-118497d81162)
   
Congrats! you've just launched your first web browser in the clouds.
    
## Step 4 - Install MySQL

1. To Install MySQL:
`sudo apt install mysql-server -y`

    ![image 11](https://github.com/user-attachments/assets/b133fc23-c9f0-44eb-958a-5fe3ba6d7c96)


2. Secure MySQL installation:
`sudo mysql` 

3. It is recommended that you run a script that comes pre-installed with MySQL. This script will remove some insecure default and lockdown access to your database system.
`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`

     ![image 12](https://github.com/user-attachments/assets/38a03a64-a958-4bae-825f-ff84449f1320)



4. Exit the MySQL shell:
`mysql> exit`

    ![image 13](https://github.com/user-attachments/assets/4efc1cea-1f97-453b-9829-047dda935e52)


5. Start the interactive script:
`sudo mysql_secure_installation`  

    ![image 14](https://github.com/user-attachments/assets/4a174ff5-8dbd-4124-9921-d7421d75b81c)


6. You'll be prompted to change your default password to a password of your choice, remove anonymous users, disallow root logins etc. Answer y for yes or anything else to continue without enabling.

    ![image 15](https://github.com/user-attachments/assets/8395c085-fcce-46b9-899d-31fdd1e5d22b)


7. Login into MySQL to ensure it works:
`sudo mysql -p`
    
     ![image 16](https://github.com/user-attachments/assets/4a097d6f-28d7-485e-a6b5-04adf69d718f)


    Use your new password If you changed it to access MySQL server

8. Exit the MySQL console:
`mysql> exit`

     ![image 17](https://github.com/user-attachments/assets/6c5965ce-63e3-4127-a49c-ffb8dceed8cc)

            
## Step 5 - Install PHP

1. Install PHP, PHP extensions for Apache and MySQL
`sudo apt install php libapache2-mod-php php-mysql php-cli -y`

     ![image 18](https://github.com/user-attachments/assets/d5a2093e-df90-4174-bc05-6a9d1a38b677)


2. Confirm your php version
`php -v`

     ![image 19](https://github.com/user-attachments/assets/d51d2c6e-7e01-4933-be8c-81b943710d74)

At this point, your L.A.M.P stack is fully installed and operational.

## Step 6 - Create a virtual host for your website using Apache

1. Create a new directory for your website(my website would be named projectlamp, you can use any name for yourself.)
`sudo mkdir /var/www/projectlamp`

2. Assign ownership of the directory with the user environment variable
`sudo chown -R $USER:$USER /var/www/projectlamp`

3. Create a new config file
`sudo nano /etc/apache2/sites-available/projectlamp.conf`

4. You can use the ls command to show the new file in the sites-available directory
`sudo ls /etc/apache2/sites-available`

     ![image 20](https://github.com/user-attachments/assets/d59c3476-580c-4e79-bb04-94347979643b)


5. Enable the new virtual host
`sudo a2ensite projectlamp`

   ![image 21](https://github.com/user-attachments/assets/5e25908f-3ace-4cc6-a5ec-81e43d47bd99)

6. Disable Apache default website
`sudo a2dissite 000-default`
   
     ![image 22](https://github.com/user-attachments/assets/ca574bd2-1a85-4396-82f7-b37466c76170)


7. Confirm your config file has zero syntax error
`sudo apache2ctl configtest`

     ![image 23](https://github.com/user-attachments/assets/bc4be3e0-c6d7-442c-8721-59a26c2d714e)

8. Reload Apache so these changes can take effect
`sudo systemctl reload apache2`    


Congrats! Your new website is now active, but the web root /var/www/projectlamp is still empty. You need to create an html file in that location so we can confirm that it works perfectly.
## Step 7 - Test the website with html scripts

1. Navigate to the project path
`cd /var/www/projectlamp`

2. Create an index.html file
`touch index.html`

3. Open the inde.html file. 
`nano index.html`

4. Copy the following:
```
<!DOCTYPE html>
<html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>ProjectLamp</title>
   </head>
   <body>
       <h1>Welcome to Your First L.A.M.P project</h1>
       <p>This is a simple HTML file for testing purposes. If you're seeing this then it works.</p>
   </body>
</html>
```
To save the index.html file, use Ctrl + S and Ctrl + X to exit


5. To view the html code you just wrote:
`cat index.html`

      ![image 24](https://github.com/user-attachments/assets/512cc22d-b8fe-4350-995e-03523859bd54)

7. Now you view your new web page via http://(your-ec2-public-ip)    

     ![image 25](https://github.com/user-attachments/assets/6f06e4a6-eb7c-4e6f-8ae2-007d821c70ad)




## Step 8 - Enable PHP on the website

1. Create a PHP test file to confirm that Apache is able to handle and process requests for PHP files
`nano /var/www/projectlamp/index.php`

2. Add the following php code to enable php on the website
`sudo nano /etc/apache2/mods-enabled/dir.conf`
```
<IfModule mod_dir.c>
    DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
```
`sudo systemctl reload apache2`

`rm index.html`

3. Create a new index.php file inside your custom web root folder
`nano /var/www/lampproject/index.php`

4. Add the following text
```
<?php
phpinfo();
?>
```

5. After completing this, refresh your http://(your-public-ip)/info.php

   ![image 26](https://github.com/user-attachments/assets/ae757270-9fca-4888-82e6-107259a42ddb)
If you're seeing this page in your browser, It means your PHP installation is working as expected. 

7. For security reasons, remove index.php
`rm index.php`
   ![image 27](https://github.com/user-attachments/assets/79ed993e-d26e-40ac-89bc-e57c9d7cd114)

CONGRATULATIONS, you just finished your first REAL LIFE PROJECT by deploying a LAMP stack in the cloud.

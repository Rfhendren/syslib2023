# Installing Wordpress

Since we are handling the installation of WordPress ourselves, we first need to 
make sure that our systems meet the requirements needed to run the software.

- Check your system's version of **PHP** with `php --version`
- Check your systems's version of **MySQL** with `mysql --version`

If your system meets the minimum requirements for WordPress, you are good to continue.

## Download and Extract the WordPress Software

The software will be a **tar.gz** file. This is similar to a **zip** file.

1. Change to the `/var/www/html` directory
2. Download the latest version of WordPress using `sudo wget https://wordpress.org/latest.tar.gz`
3. Extract the package using `sudo tar -xzvf latest.tar.gz`

This should have created a new directory called **wordpress** in your **/var/www/html**
directory. Use `ls` to see it.

## Create a Database and User for WordPress

This process is similar to what we did to create a user and password for **MySQL**.

1. Switch to the root user with `sudo su`
2. Login as the MySQL root user with `mysql -u root`
3. Create a new user for the WordPress database with `create user 'wordpress'@'localhost' identified by 'XXXXXXXXX';` 
**Replace the X's with your own password**
4. Create a new database for WordPress with `create database wordpress;`
5. Grant all privileges to the new user for the **wordpress** database with `grant all privileges on wordpress.* to 'wordpress'@'localhost';`
6. Look at the result of your commands with `show databases;`
7. Exit MySQL with `\q`

## Set up wp-config.php

Like we did when creating our OPAC's **login.php** file, we will need to set up a file
similar to this for WordPress. Its file will be called **wp-config.php**.

1. Change to the **wordpress** directory with `cd /var/www/html/wordpress`
2. Copy and rename the **wp-config-sample.php** file to **wp-config.php** with `sudo cp wp-config-sample.php wp-config.php`. 
Using the `cp` command in this way retains the original file.
3. Edit the file and add your database name, database user, and database password 
into the appropriate fields with `sudo nano wp-config.php`.
 
## Optional Step to Rename Your WordPress Directory

You can rename your WordPress directory if you like. **blog** is a good example, but 
it could be anything you like. Make sure to use lowercase letters and only one word 
if you are going to rename. The below example is to rename it to **blog**.

1. Change to the `cd /var/www/html` directory.
2. Rename with the `mv` command using `sudo mv wordpress blog`.

## Run the Install Script

This step in the process happens in your internet browser. The URL that you will visit
in your browser is dependent on your server's IP address and the directory that we
extracted WordPress to. For example, if my IP address is 11.111.111.11 and I renamed 
my directory to **blog**, then I need to visit the following URL:

`http://11.111.111.11/blog/wp-admin/install.php`

Note: you can find your IP address by visiting your Google Coud console and navigating
to **VM Instances**.

1. Visit the above URL in your web browser
2. Fill in the appropriate information
3. Login to your new WordPress site! 

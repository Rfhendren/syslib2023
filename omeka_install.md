# Installing Omeka

Installing Omeka follows very similar steps to what we have done previously when 
installing our OPAC/ILS and WordPress.

## Prerequisite Steps

1. Install ImageMagick (Omeka needs this to display thumbnail images) with `sudo
apt install imagemagick`

2. Enable the Apache module rewrite (which Omeka will use to create URLs for items
and collections) with `sudo a2enmod rewrite`

3. Restart Apache with `sudo  systemctl restart apache2`

## General Steps

### **Create a New User & Database in MYSQL for Omeka**

1. Log in to MySQL as the root user with `sudo mysql -u root`

2. Create a new user with different credentials than our OPAC or WordPress user with
`create user 'omekauser'@'localhost' identified by 'XXXXXXXXX';` (replace the X's 
with your chosen password)

3. Create a new database in MySQL with `create database omekadb;`

4. Grant all privileges to **omekauser** on this database with `grant all privileges 
on omekadb.* to 'omekauser'@'localhost';`

5. Check to make sure the database was created with `show databases;`

6. Exit MySQL with `exit;`

### **Download Omeka Classic and Extract It**

1. Move to the `/var/www/html` directory with `cd /var/www/html`

2. Download Omeka Classic with `sudo wget https://github.com/omeka/Omeka/releases/download/v3.1/omeka-3.1.zip`

3. Install the `unzip` command using `sudo apt install unzip`

4. Use `sudo unzip omeka-3.1.zip`
to extract the file

5. Use `ls` to make sure that the new Omeka directory is there

6. Use the `mv` command to rename the **omeka-3.1** directory to **omeka** with
`sudo mv omeka-3.1 omeka`

### **Add Your Database Credentials to the Omeka Directory**

1. Use `cd omeka` to change into the newly created omeka directory

2. Open the **db.ini** file with `sudo nano db.ini`

3. Add the database credentials you created for the omeka database in MySQL in the
appropriate fields (host=localhost, username=omekauser, password, and database name=omeka)

4. Save the nano file

5. Change the ownership of the **files** directory in the **omeka** directory to make 
the user and owner *www-data*. Do this with `sudo chown -R www-data:www-data *`

6. Restart Apache 2 and MySQL with `sudo systemctl restart apache2` and 
`sudo systemctl restart mysql`

### **Complete Omeka Set Up in Your Browser**

1. Go to **http://your-ip-address/omeka/** in your web browser and follow the
prompts to finish set up

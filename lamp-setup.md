# LAMP Setup

A LAMP server includes Linux (the operating system I am using), **Apache**, **MySQL**, and **PHP**.

The LAMP stack for this class used **Apache2**. 

**Apache2:**  An HTTP server that makes my files available to others on the web.

**MySQL:** A relational database

**PHP:** A programming langauge that interacts with databases

## Tasks

All tasks were completed by following Professor Burns' instructions in our video
lectures and the Systems Librarianship webpage.

1. Upgrade my machine with `sudo apt update` and `sudo apt upgrade`

2. Install **Apache2** with `sudo apt install apache2`
	-then check its status with `systemctl status apache2`

3. Create a webpage with **w3m**
	-install w3m with `sudo apt install w3,`
	-create an html file using nano with `sudo nano index.html`
	-insert the following code into the **index.html** file or create your own code

```

<html>
<head>
<title>My first web page using Apache2</title>
</head>
<body>

<h1>Welcome</h1>

<p>Welcome to my web site.
I created this site using the Apache2 HTTP server.</p>

</body>
</html>
```

-access webpage in the internet browser using my server's public IP address (found in the Google Cloud console)

4. Install and configure **PHP**
	-Install **PHP** and an additional package that creates a connection between **PHP** and **Apache2** with `sudo apt install php libapache2-mod-php`.
	-Restart **Apache2** with `sudo systemctl status apache2` and check **Apache2's** status with `sudo systemctl status apache2`
	-create a php file using nano with `sudo nano index.php`
	-configure **Apache2** to default to the php file we created. Open the php file and inser the following text:
```

<?php
phpinfo();
?>

```

-Check the configuration with `acahectl configtest`. Then, reload and restart **Apache2** with `sudo systemctl reload apache2`
and `sudo systemctl restart apache2`

5.  Install **MySQL**
	-Install **MySQL** with `sudo apt install mysql-server` and check its status with `sudo systemctl status mysql`
	-become the root user with `sudo su` to login to mySQL **(be careful as root user!)**
	-exit out of the root user status and create a regular user account for **MySQL** with `create user 'opacuser'@'localhost' identified by 'XXXXXXXXX';` **(Replace X's with your password)**
	-create a practice database with `create database opacdb;` and grant all privileges on it to the user you just created with `grant all privileges on opacdb.* to 'opacuser'@'localhost';`
	-create a table within that database with 

```

create table books (
id int unsigned not null auto_increment,
author varchar(150) not null,
title varchar(150) not null,
copyright date not null,
primary key (id)
);

```

-create records to go into your table (with author, title, publisher, copyright) using the `insert` command

```

insert into books (author, title, copyright) values
('Jennifer Egan', 'The Candy House', '2022-04-05'),
('Imbolo Mbue', 'How Beautiful We Were', '2021-03-09'),
('Lydia Millet', 'A Children\'s Bible', '2020-05-12'),
('Julia Phillips', 'Disappearing Earth', '2019-05-14');
```

-practice searching for records in the table with

```

select author from books;
select copyright from books;
select author, title from books;
select author from books where author like '%millet%';
select title from books where author like '%mbue%';
select author, title from books where title not like '%e';
select * from books;
alter table books add publisher varchar(75) after title;
describe books;
update books set publisher='Simon \& Schuster' where id='1';
update books set publisher='Penguin Random House' where id='2';
update books set publisher='W. W. Norton \& Company' where id='3';
update books set publisher='Knopf' where id='4';
select * from books;
delete from books where author='Julia Phillips';
insert into books
(author, title, publisher, copyright) values
('Emma Donoghue', 'Room', 'Little, Brown \& Company', '2010-08-06'),
('Zadie Smith', 'White Teeth', 'Hamish Hamilton', '2000-01-27');
select * from books;
select author, publisher from books where copyright < '2011-01-01';
select author from books order by copyright;
\q

```

6. Install **PHP** and **MySQL** Support
	-Install with `sudo apt install php-mysql php-mysqli`,then restart **PHP** and **Apache2** with `sudo systemctl restart apache2` and `sudo systemctl restart mysql`
	-create a login file in the `/var/www/html/` so that PHP can connect to mySQL. Also change the ownership of the file with `sudo touch login.php`, `sudo chmod 640 login.php`, and `sudo chown :www-data login.php`
	-Open **login.php** with `sudo nano login.php` abd add credentials for the regular user you made before with** (Replace the X's with your password)** 
```

<?php // login.php
$db_hostname = "localhost";
$db_database = "opacdb";
$db_username = "opacuser";
$db_password = "XXXXXXXXX";
?>

```

-create a new php file using nano with `sudo nano opac.php` and insert the following code:

```

<html>
<head>
<title>MySQL Server Example</title>
</head>
<body>

<h1>A Basic OPAC</h1>

<p>We can retrieve all the data from our database and book table
using a couple of different queries.</p>

<?php

// Load MySQL credentials
require_once 'login.php';

// Establish connection
$conn = mysqli_connect($db_hostname, $db_username, $db_password) or
  die("Unable to connect");

// Open database
mysqli_select_db($conn, $db_database) or
  die("Could not open database '$db_database'");

echo "<h2>Query 1: Retrieving Publisher and Author Data</h2>";

// Query 1
$query1 = "select * from books";
$result1 = mysqli_query($conn, $query1);

while($row = $result1->fetch_assoc()) {
    echo "<p>Publisher " . $row["publisher"] .
        " published a book by " . $row["author"] .
        ".</p>";
}

mysqli_free_result($result1);

echo "<h2>Query 2: Retrieving Author, Title, Date Published Data</h2>";

$result2 = mysqli_query($conn, $query1);
while($row = $result2->fetch_assoc()) {
    echo "<p>A book by " . $row["author"] .
        " titled <em>" . $row["title"] .
        "</em> was released on " . $row["copyright"] .
        ".</p>";
}

// Free result2 set
mysqli_free_result($result2);

/* Close connection */
mysqli_close($conn);

?>

</body>
</html>
```

-test the syntax of the file after you save it with `sudo php -f login.php` and `sudo php -f index.php`

7. Revisit Webpage

Remember that webpages can be navigated a bit like the filepath in a folder or your
Linux directories. To get to the php file you created, you can enter your VM's **public IP address**
in your browser followed by **/opac.php** 

# Create a Bare Bones Cataloging Module

This module will allow us to add records to our OPAC from 
our web browser. The records are very simple like the records
in the *books* table in MySQL on our machines (author, title, 
publisher, copyright).

## Create an HTML page that has a form for entering our data called index.html

1. Change directories to `cd /var/www/html` and make a new directory 
called "cataloging" with `sudo mkdir cataloging`.

2. Change directories to the new one you just made with `cd cataloging` and
use nano to create the index.html file and save it there
(`sudo nano index.html`). Add this code to the index.html file:

```
<!DOCTYPE html>
<html>
<head>
    <title>Enter Records</title>
</head>
<body>
    <h1>OPAC Library Administration</h1>

    <p>This is the library administration page for entering records into the OPAC.</p>
    <p>Please do not use this page unless you are an authorized cataloger.</p>

    <form action="insert.php" method="post">
        <label for="author">Author:</label>
        <input type="text" name="author" id="author" required><br><br>

        <label for="title">Book Title:</label>
        <input type="text" name="title" id="title" required><br><br>

        <label for="publisher">Publisher:</label>
        <input type="text" name="publisher" id="publisher" required><br><br>

        <label for="copyright">Copyright:</label>
        <input type="date" id="copyright" name="copyright">

        <input type="submit" value="Submit">
    </form>
</body>
</html>
```

## Create a PHP script to communicate between the form in the web browser and our MySQL database

1. While in the same directory (cataloging), open nano with `sudo nano insert.php`
paste the following code into the file, then save the file.

```
<?php

// Load MySQL credentials
require_once '../login.php';

// Establish connection
$conn = mysqli_connect($db_hostname, $db_username, $db_password) or
  die("Unable to connect");

// Open database
mysqli_select_db($conn, $db_database) or
  die("Could not open database '$db_database'");

// Prepare and bind SQL statement
$stmt = $conn->prepare("INSERT INTO books (author, title, publisher, copyright) VALUES (?, ?, ?, ?)");
$stmt->bind_param("ssss", $author, $title, $publisher, $copyright);

// Set parameters and execute statement
$author = $_POST["author"];
$title = $_POST["title"];
$publisher = $_POST["publisher"];
$copyright = $_POST["copyright"];

if ($stmt->execute() === TRUE) {
    echo "New record created successfully";
} else {
    echo "Error: " . $stmt->error;
}

// Close statement and connection
$stmt->close();
$conn->close();

echo "<p>Return to the cataloging page: <a href='http://11.111.111.111/cataloging/'>http://11.111.111.111/cataloging/</a></p>";
?>
```

## Utilize a simple authorization method provided by Apache2 called htpasswd to add a layer of security to our cataloging module.

1. Navigate to the `cd /etc/apache2` directory.

2. Create an authentication file using `sudo htpasswd -c /etc/apache2/.htpasswd libcat`.
This sets the username to *libcat*, but you could set it to whatever you like. Then,
set your password.

3. Next, open the apache2.conf file with `sudo nano /etc/apache2/apache2.conf`
and look for the following code block:

```
<Directory /var/www/>
  Options Indexes FollowSymLinks
  AllowOverride None
  Require all granted
</Directory>
```

Change the word *None* to *All* like so and save it.

```
<Directory /var/www/>
  Options Indexes FollowSymLinks
  AllowOverride All
  Require all granted
</Directory>
```

4. Change to the cataloging directory with `cd /var/www/html/cataloging`. Create a
file with nano called .htaccess with `sudo nano .htaccess`. Add the following information
to your .htaccess file and save it:

```
AuthType Basic
AuthName "Authorization Required"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user
```

5. Check that your configuration is correct with `apachectl configtest`. You should
see the message *Syntax OK* if you did everything correctly. Restart Apache2 with 
`sudo systemctl restart apache2` and then check its status with `systemctl status apache2`.

6. Visit your cataloging module in your web browser. You can reach this by entering your 
*IP address/cataloging* into the address bar. You should be prompted to sign in with the
credentials you previously set with htpasswd.

7. Add some records to your OPAC using the new form!

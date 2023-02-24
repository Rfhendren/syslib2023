#*LAMP Seup*#

A LAMP server includes Linux (the operating system I am using), Apache, MySQL, and PHP.

The LAMP stack for this class used Apache2. 

**Apache2:**  An HTTP server that makes my files available to others on the web.
**MySQL:** A relational database
**PHP:** A programming langauge that interacts with databases

##Tasks##
All tasks were completed by following Professor Burns' instructions in our video
lectures and Systems Librarianship webpage.

1. Upgrade my machine
2. Install Apache2
	-then check its status
3. Create a webpage with w3m
	-install w3m
	-create an html file using nano (index.html)
	-access webpage using my server's public IP address (found in the Google Cloud console)
4. Install and configure PHP
	-after installation check its status
	-create a php file using nano (index.php)
	-configure Apache2 to default to the php file we created
5.  Install MySQL
	-after installation check its status
	-become the root user to login to mySQL (be careful as root user!)
	-exit out of the root user status and create a regular user account for mySQL
	-create a practice database (opacdb) and create a table within that database
	-create records to go into your table (with author, title, publisher, copyright)
	-practice searching for records in the table
6. Install PHP and MySQL Support
	-after installation, restart Apache2
	-create a login file so that PHP can connect to mySQL (add credentials for the regular
user you made before)
	-create a new php file using nano (opac.php)
	-test the syntax of the file after you save it
7. Revisit Webpage

Remember that webpages can be navigated a bit like the filepath in a folder or your
Linux directories. To get to the php file you created, you can enter the public IP address
in your browser followed by /opac.php 

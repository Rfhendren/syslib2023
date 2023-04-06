# Install Koha ILS

Koha is an open-source integrated library system installed in over 4,000 libraries
around the world. 

## Google Cloud Setup

The first step to installing Koha is to create a new virtual machine instance through
Google Cloud that has more RAM.

1. Login to Google Cloud console and navigate to the dashboard. Make sure you are in 
the correct project and click "create instance".

2. Name the instance "koha-ils". Ensure that the *central region* and *zone* are 
selected. The *series* should be "E2", and the *machine type* should be "e2-medium 
(2vCPU, 4 GB memory). Change the *operating system* to Ubuntu, and the *version* to 
"Ubuntu 20.04 LTS x86/64". Under the *Firewall* heading, select "Allow HTTP traffic"
and "Allow HTTPS traffic".

3. Click "create".

Next, you will need to configure the Google Cloud firewall for the VM instance you 
just created.We need to add a firewall rule to allow web traffic through port 8080. 
This port will be used to access the staff side of Koha. 

1. Click on the hamburger icon at the top left of the screen. Hover over *VPC network*
and click *firewall*. 

2. Select *Create a firewall rule* (NOT "Create a firewall policy"). Give the rule a 
name and description. Under *Targets* click "All instances in the network". Under
*Source IPv4 ranges* type "0.0.0.0/0". Click to enable *TCP* and type "8080" 
underneath it.

3. Click "create". 

## Install Koha Repo

1. In the Google Cloud console, navigate to the VM instance screen and copy the gcloud
command line for "koha-ils". Use this to connect to the VM as normal in the command
prompt. 

2. Run `sudo apt update`, `sudo apt upgrade`, `sudo apt autoremove -y`, and `sudo apt
clean`.

3. Install *gnupg2* with `sudo apt install gnupg2`. It creates digital signatures, 
encrypts data, and assists in secure communication.

4. Reboot the system with `sudo reboot now` since we installed a new Linux kernel.

Next, we need to add the Koha Repository to our servers. You can add repositories to 
sync with and to use to download software. The Koha repository manages packages for 
install on our machines.

1. Add the Koha repository to our server with the command `echo 'deb http://debian.koha-community.org/koha stable main' | sudo tee /etc/apt/sources.list.d/koha.list`

2. Add a digital signature that verifies the repo with `wget -q -O- https://debian.koha-community.org/koha/gpg.asc | sudo apt-key add -`

## Install Koha

1. Update/sync your new repository with the Koha remote repository with `sudo apt 
update`

2. View the package information for Koha with `apt show koha-common`, and then install
it with `sudo apt install koha-common`

## Configure Koha

First, you need to edit some configuration files for Koha. 

1. Open the file *koha-sites.conf* in *Nano* with `sudo nano /etc/koha/koha-sites.conf`

2. In Nano, change the line that says **INTRAPORT="80"** to **INTRAPORT="8080"**

Next, we will install and configure MySQL.

1. Install MySQL with `sudo apt install mysql-server`

2. Set the root MySQL password with `sudo mysqladmin -u root password bibliolib1`. This is
setting the password to *bibliolib1*. If you make your password something different, 
be sure to note what it is.

Now some changes must be made with Apache2. Apache2 was installed along with Koha as 
a prerequsiite piece of software.

1. Enable URL rewriting and CGI functionality (which enables a web server to execute
an external program) with `sudo a2enmod rewrite` and `sudo a2enmod cgi`

2. Restart Apache2 with `sudo systemctl restart apache2`

Next, a database for Koha needs to be created.

1. Create a Koha database with `sudo koha-create --create-db bibliolib`

Apache2 needs to be able to listen on *port 8080* (which is used for web traffic and 
the staff interface for Koha). Some additional configuration changes need to be made.

1. Open the file *ports.conf* with `sudo nano /etc/apache2/ports.conf`

2. Add the line `Listen: 8080` under the existing line "Listen: 80"

3. Restart Apache2 with `sudo systemctl restart apache2`

4. Disable the default Apache2 setup with `sudo a2dissite 000-default`. Enable traffic
compression using `sudo a2enmod deflate` and enable the *bibliolib* site with `sudo a2ensite bibliolib`.

5. Reload Apache2's new configurations with `sudo systemctl reload apache2` and restart it
with `sudo systemctl restart apache2`

## Koha Web Installer

Now we can complete installation through a web installer.

1. Find your Koha username and password in the file *koha-conf.xml*. Open it with
`sudo nano /etc/koha/sites/bibliolib/koha-conf.xml`

2. Find the **<config>** stanza (line # 252) and the line beginning with **<user>**.
The password is on the line after this. Make a note of this username and password.

3. Visit *http://IP_ADDRESS:8080* in your web browser to access the Koha web installer.
Input the username and password you just took a note of and follow installation 
instructions.

## Public OPAC

Once you have comepleted installation you should have access to the staff interface. A
setting change needs to be made to allow your OPAC to be public facing. 

- Click on *More* in the top drop down box (top left-hand corner of screen)

- Click on *Administration*

- Click on *Global System Preferences*

- Click on *OPAC* in the left hand side bar

- Find the *OPACBaseURL* line and input the IP address of your server (*http://IP_ADDRESS*)

- Click on *Save All OPAC Preferences* (at the top of the screen)

Now you can visit your public facing OPAC at that address!

Records, patrons, libraries, and so much more can now be added to your Koha ILS.

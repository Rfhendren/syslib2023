# Installing a New Theme on Omeka Classic

1. In your server, navigate to the **/var/www/html/omeka/themes** directory with ` cd /var/www/html/omeka/themes`. This is where the other 
default themes are installed.

2. Login to Omeka Admin side in your web browser and click **Theme**. Next, click the link for the **Omeka Theme Directory**. Identify 
the theme you would like to install.

3. You will need the full location of the zip file to download it. You can see this by right-clicking your mouse on the **Download**
button for the theme you are interested in and clicking **copy link address**

3. Use the `wget` command in your server to download the selected theme as a zip file. Your command should look similar to this 
`sudo wget https://github.com/omeka/theme-rhythm/releases/download/v2.4.3/rhythm-2.4.3.zip`. This is how I installed the 
"Rhythm" theme. If you are selecting a different theme, you will see a different file name before the **.zip**.

4. Use the `unzip` command to extract the zip file you just downloaded. Your command should look like this `sudo unzip 
rhythm-2.4.3.zip`. Replace the last part of the command with the name of the zip file for the selected theme. 

5. Navigate to the Omeka Admin page in your web browser. You should see the new theme as available to use!

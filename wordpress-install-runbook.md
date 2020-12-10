# WordPress Install Runbook - How to install WordPress on a Ubuntu Server

* This install assumes you understand how to SSH into a server.

## Step 1 - Install Apache 2

1. First things first, we're going to ensure our current directory is the topmost:

``cd /``

2. Next we're going to ensure our list of software is up to date:  

``sudo apt-get update``

3. Time to install Apache2:

``sudo apt-get install apache2``

4.  Now that's it's installed let's switch 'er on:

``sudo systemctl start apache2``

``sudo systemctl enable apache2``


## Step 2  - Install mysql 5.7


1. Only one real step here - we're going to install mysql:

``sudo apt-get install mysql-server``

* When asked if you want to continue, hit y.
* When if asked to change passwords, just press enter each time.
  
  
## Step 3 - Install php 7.2

1. First, link to the spot to get php 7.2

``sudo add-apt-repository ppa:ondrej/php``  

* When prompted, press enter to continue.  
  
2. Ensure we're our list of software is up to date:  

``sudo apt-get update``

3. Once that's done, we're going to install php7.2:   

``sudo apt-get install php7.2 libapache2-mod-php7.2 php7.2-mysql``

* When asked if you want to continue, hit y.
  
4. Once that's done, we're going to restart apache2:

``sudo systemctl restart apache2``
  

## Step 4 - Installing WordPress


1. First of all, we'll navigate to the web root directory. In this example, the directory is ``/home/myserver/``. So from the top most directory we navigated to back in step 1:

``cd home/myserver``

2. Next we'll download WordPress itself:  

``curl -O https://wordpress.org/latest.zip``

3. Once that's done we'll ensure we can unzip the file:

``sudo apt-get install unzip``

4. And then do the thing!

``unzip latest.zip``

5. Once it has been unzipped, we're going to enable the site itself, the 'rewrite' mod and then we'll restart apache2 by running these next three commands one at a time.


## Step 5 - Configuring the mysql database for WordPress


1. First up we'll enter the mysql interface:

``sudo mysql -u root``

2. Once in, we'll create a new database:

``CREATE DATABASE wordpress;``

3. Next, we'll fill out the database (it's a long one):

``GRANT ALL PRIVILEGES ON wordpress.* TO wordpress@localhost IDENTIFIED BY '<your-password>';``

4. Next command is:

``FLUSH PRIVILEGES;``

5. And with that we're done in mysql, so:

``quit``

6. Time to enable mysql:

``sudo service mysql start``

7. From here, let's enter the WordPress directory.

``cd wordpress``

8. Next up we need to create the wp-config.php file, and we'll do so by using the sample file as a template:

``sudo mv wp-config-sample.php wp-config.php ``

9. Once that exists we'll open the new file:

``sudo nano wp-config.php``

10. Finally, we'll go through the file and change the existing define entries to the following:
      
``define( 'DB_NAME', 'wordpress' );``

``define( 'DB_USER', 'wordpress' );``

``define( 'DB_PASSWORD', '<your-password>' );``

``define( 'DB_HOST', 'localhost' );``

``define( 'DB_CHARSET', 'utf8' );``

``define( 'DB_COLLATE', '' );``

* Once you're done press ``ctrl-o`` to save and then ``ctrl-x`` to exit.

11. And with that, it's all done. Thrown down a quick restart to ensure all the new changes are in:

``sudo systemctl restart apache2``

12. Navigate to your fancy new WordPress install and crack a beer to celebrate success (!important).

``www.yourdomain.com/wordpress``

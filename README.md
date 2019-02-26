# snipe-test
Building a Snipe IT asset management service on CentOS 7 VirtualBox


Settings up Guest Additions

Root user
$sudo su

$yum update kernel*
$reboot

Click Devices > Install Guest Additions… on VirtualBox

Mount VirtualBox Guest Additions device

$mkdir /media/VirtualBoxGuestAdditions
$mount -r /dev/cdrom /media/VirtualBoxGuestAdditions

On CentOS/Red Hat (RHEL) 7 EPEL repo is needed

$rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

Install following packages

$yum install gcc kernel-devel kernel-headers dkms make bzip2 perl

Add KERN_DIR environment variable

$KERN_DIR=/usr/src/kernels/`uname -r`/build
$export KERN_DIR

Install VirtualBoxGuestAdditions

cd /media/VirtualBoxGuestAdditions
./VBoxLinuxAdditions.run


Installing LAMP Stack
$ sudo yum update        [On CentOS/RHEL] 
$ sudo rpm -Uvh http://rpms.remirepo.net/enterprise/remi-release-7.rpm
$ sudo yum -y install yum-utils
$ sudo yum-config-manager --enable remi-php56 (Apache 5.6)
$ sudo yum install httpd mariadb mariadb-server php php-openssl php-pdo php-mbstring php-tokenizer php-curl php-mysql php-ldap php-zip php-fileinfo php-gd php-dom php-mcrypt php-bcmath
$ sudo systemctl enable httpd         [On CentOS/RHEL]

Testing Apache version

$ sudo echo "<?php  phpinfo(); ?>" | sudo tee -a /var/www/html/info.php
http://SERVER_IP/
http://SERVER_IP/info.php 

$ sudo mysql_secure_installation     

[osboxes@osboxes ~]$ sudo mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n] Y
New password: snipetest
Re-enter new password: snipetest
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] n
 ... skipping.

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] n
 ... skipping.

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] n
 ... skipping.

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] Y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
[osboxes@osboxes ~]$ mysql -u root -p

MariaDB [(none)]> CREATE DATABASE snipeit_db;
MariaDB [(none)]> CREATE USER 'snipeuser'@'localhost' IDENTIFIED BY 'snipeusertest';
MariaDB [(none)]> GRANT ALL PRIVILEGES ON snipeit_db.* TO 'snipeuser'@'localhost';
MariaDB [(none)]> FLUSH PRIVILEGES;
MariaDB [(none)]> exit

$ sudo yum -y install git      [On CentOS/RHEL]
$ cd  /var/www/
$ sudo git clone https://github.com/snipe/snipe-it.git
$ cd snipe-it
$ ls

Copy example configuration

$ sudo mv .env.example .env

Change configs to match db, etc

$ sudo vi .env

APP_TIMEZONE='EST'                                   #Change it according to your country
APP_URL=http://10.0.1.7/snipe                                #set your domain name or IP address
APP_KEY=base64:BrS7khCxSY7282C1uvoqiotUq1e8+TEt/IQqlh9V+6M=   #set your app key
DB_HOST=localhost                                             #set it to localhost
DB_DATABASE=snipeit_db                                        #set the database name
DB_USERNAME=tecmint                                           #set the database username
DB_PASSWORD=password                                          #set the database user password

$ sudo chmod -R 755 storage 
$ sudo chmod -R 755 public/uploads
$ sudo chown -R apache:apache storage public/uploads         [On CentOS/RHEL]

Install Composer – PHP Manager

$ sudo curl -sS https://getcomposer.org/installer | php
$ sudo mv composer.phar /usr/local/bin/composer

$sudo /usr/local/bin/composer install --no-dev --prefer-source

$ sudo php artisan key:generate
$ sudo vi /etc/httpd/conf.d/snipeit.example.com.conf                [On CentOS/RHEL]

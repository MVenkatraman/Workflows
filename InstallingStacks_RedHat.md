Installing Stacks on a RedHat Distro
=============
Debian to Redhat translation for installing [Stacks] (http://creskolab.uoregon.edu/stacks/) (Catchen et al. 2011). 

###Prerequisites

Stacks requires a lot of dependencies to run and installing them properly is important. So, read their [installation guide](http://creskolab.uoregon.edu/stacks/manual/#intro) well.

1. First you install mysql, php & apache (httpd): 

   `yum install mysql-server php httpd`

2. Then you install php & mysql extensions:

  `yum install php-pear-mdb2 php-pear-mdb2-driver-mysql`

3. Install a driver that helps perl interface with mysql (libdbd-mysql-perl in guide):

  `yum install perl-DBD-MySQL`

Sometimes this gets installed as a dependency when you instal mysql-server

4. Install Samtools:

  `yum install samtools`

5. Install a package that deals with SAM and BAM files (libbam-dev in guide):

  `yum install samtools-devel`

###Building Stacks 

This actually works with the script provided by Stacks. Before you do this you need to download the tarball from their website.

 `tar xfvz stacks_x.xx.tar.gz`
 `cd stacks_x.xx`
` ./configure`
 `make`

 `make install`

###Configuring MySQL

Start mySQL.

`service mysqld start`
`cd /usr/local/share/stacks/sql/`
`cp mysql.cnf.dist mysql.cnf`

Add a user to mysql for stacks

`mysql > CREATE USER 'stacks_user'@'localhost' IDENTIFIED BY 'stackspassword';`

Where stacks_user is whatever username you want and stackspassword is the password of your choice.

Give your mysql user access to all databases:

`mysql> GRANT ALL ON *.* TO 'stacks_user'@'localhost' IDENTIFIED BY 'stackspassword';`

Now the protocol says “Edit /usr/local/share/stacks/sql/mysql.cnf to contain the username and password you specified to MySQL”. 

First open mysql.cnf

 `vi /usr/local/share/stacks/sql/mysql.cnf`

Edit the username = stacks_user and password = stackspassword. Save & exit.

####Enabling the Stacks Web Interface 

So, they want to to create a stacks.conf file in the httpd.conf directory.

`vi /etc/httpd/conf/stacks.conf`

Paste the following text into your new file:

<Directory “/usr/local/share/stacks/php”>
Order deny,allow
Deny from all
Allow from all
</Directory>

Alias /stacks “/usr/local/share/stacks/php”

Save and quit.

Restart the apache server:

`/sbin/service httpd restart`

Now, you need to edit the php config file to include your mysql username and password.

`cp /usr/local/share/stacks/php/constants.php.dist /usr/local/share/stacks/php/constants.php`

`vi /usr/local/share/stacks/php/constants.php'`

Enter your mysql user name and password. Save & quit.

####Enabling web-based exporting from MySQL database

`chown www /usr/local/share/stacks/php/export`

Where ‘www’ is your web server username. If you don’t know what your username is try the `whoami` command.

Congratulations! You’ve installed stacks!


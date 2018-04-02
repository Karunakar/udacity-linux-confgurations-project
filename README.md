# Udacity-linux-confgurations-project
# Description
This Project give information about how to install the flask application : CateLog Project in fresh amazon lightsail 
# Instalation
Step 1 : Create the amazon LightSail Account and Lanuch a new VM https://lightsail.aws.amazon.com
Step 2 : Please visit my Catalog Project 52.66.122.118

## Steps to Host a Flask application in the New VM

Step 1 : Download the Private key from LightSail Account
Step 2 : SSH into the server through ssh -i ~/.ssh/Karuna.pem ubuntu@52.66.122.118
Step 3 : Create an new user using the sudo adduser grader
Step 4 : vim /etc/sudoers.d/grader
Step 5 : Add the folowing content 
Step 6 : grader ALL=(ALL:ALL) NOPASSWD:ALL
Step 6 : Restart the sshd using the sudo service sshd restart

Attached my Karuna.pem to udacity project Submission


## Local client Setup

Step 1 : ssh-keygen 
Step 2 : $ su - grader
Step 3 : $ mkdir .ssh

## Copy the ssh Public key to VM using below commands
Step 1 : vim .ssh/authorized_keys & Save and exit
Step 2 : vim .ssh/authorized_keys
Step 3 : chmod 700 .ssh
Step 4 : chmod 644 .ssh/authorized_keys
Step 5 : Restart the sshd Sudo service sshd restart
Step 6 : Login using the below command 
Step 7 : vim /etc/ssh/sshd_conig

Step 8 : Change the port from 22 Port 2200

Step 9 : Change   PermitRootLogin no
Step 10 : Chnage PasswordAuthentication no

Step 11   sudo ufw default deny incoming
Step 12   sudo ufw default allow outgoing
Step 13   sudo ufw allow 2200/tcp
Step 14   sudo ufw allow www
Step 15   sudo ufw allow ntp
Step 16   sudo ufw enable

Step 17:   Enable the Amazon Tcp 2200 port from Amazon browser 

Step 18 : ssh -i .ssh/id_rsa  grader@52.66.122.118 -p 2200


## Update all packages in New Amazon VM
sudo apt-get update
sudo apt-get upgrade


## Install Apache2 and related softwares

1. apt-get install apache2
2. apt-get install libapache2-mod-wsgi python-dev
3. a2enmod wsgi
4. apt-get install postgresql
5. sudo su - postgres
6. psql
7.  CREATE DATABASE catalog;
8. CREATE USER catalog;
9. ALTER ROLE catalog WITH PASSWORD 'password';
10 . GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog;


## Install Catalog Flashk application
1. apt-get install git
2. mkdir /var/www && cd /var/www
3. mkdir FlaskApp && cd FlaskApp
4. git clone https://github.com/Karunakar/Item-Catalog
5. sudo mv ./catalog2 ./FlaskApp && cd FlaskApp
6. Rename mv project.py __init__.py
7. Change database connections two places from sql lite to postgres
8. install apt-get install python-pip & all related packages from pip and I have done by one looking at flask logs
9. python  catalog_database.db

## Install Apache and create the new virtual host
1. Create the blank file  vi /etc/apache2/sites-available/FlaskApp.conf
2. Add the following content
<VirtualHost *:80>
        ServerName 52.66.122.118
        ServerAdmin revurikarna@gmail.com
        WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
        <Directory /var/www/FlaskApp/FlaskApp/>
                Order allow,deny
                Allow from all
        </Directory>
        Alias /static /var/www/FlaskApp/FlaskApp/static
        <Directory /var/www/FlaskApp/FlaskApp/static/>
                Order allow,deny
                Allow from all
        </Directory>
        ErrorLog ${APACHE_LOG_DIR}/error.log
              
</VirtualHost>

3. cd /var/www/FlaskApp
4. vim  flaskapp.wsgi 

### Add the following content 
- #!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/FlaskApp")

from FlaskApp import app as application
- #application.secret_key = 'Super secret key'

sudo service apache2 restart

Access the application through : 52.66.122.118

# License
This is opensource and my own linux configuration . Email me if you you need any help in making your first amazon VM  revurikarna@gmail.com

# LinuxProject
 As part of Linux Server Configuration Project, I deployed previously created Catalog project onto secured Linux Server Amazon Lighgsail instance.
 http://ec2-18-188-211-101.us-east-2.compute.amazonaws.com/
 Pubic IP 18.188.211.101
 Local IP 172.26.9.3
 
 Get your server
1.	Created “Alla_Final_Project_V2” Ubuntu instance on Amazon Lightsail
2.	Downloaded and saved locally .pem file

Update all currently installed packages/secure server
1.	sudo apt-get update
2.	sudo apt-get upgrade
3.	nano /etc/ssh/sshd_config and change port nbr to 2200
4.	Update Firewall section on Amazon Lightsail Network tab allow TCP port 2200
5.	sudo ufw default deny incoming
 sudo ufw default allow outgoing
sudo ufw allow 2200
sudo ufw allow http
sudo ufw allow 123
sudo ufw enable
sudo service ssh restart

Give grader access.
1.	sudo adduser grader
2.	 Create a new file $ sudo nano /etc/sudoerss.d/grader and add “grader ALL=(ALL:ALL) ALL” to provide sudo rights to grader
3.	 Generate key pair on local: ssh-keygen -f ~/.ssh/graderAccess 
4.	SSH to remote sudo touch /home/grader/.ssh/authorized_keys and copy contents of graderAccess.pub
5.	Make sure both directory and file are owned by grader and have correct permissions 
   	chown grader:grader /home/grader/.ssh
 	  sudo chmod 700 /home/grader/.ssh
   	chown grader:grader /home/grader/.ssh/authorized_keys
   	sudo chmod 644 /home/grader/.ssh/authorized_keys
6.	Check
ssh -i  pathToKeyPairFile -p 2200 grader@18.188.211.101
7.	Disable SSH for Root User
sudo nano /etc/ssh/sshd_config and set PermitRootLogin no

Prepare to deploy your project.
1.	Configure the local timezone to UTC by running 
sudo dpkg-reconfigure tzdata.  Select None > UTC >OK
2.	Install and configure Apache to serve a Python mod_wsgi application.
 sudo apt-get install apache2 
 sudo service apache2 start (apache home page is  now viewable on public IP)
 sudo apt-get install python-setuptools 
 sudo apt-get install libapache2-mod-wsgi
 sudo apt-get install libapache2-mod-wsgi python-dev
 sudo a2enmod wsgi
  sudo touch /etc/apache2/sites-enabled/000-default.conf and add at the end of the <VirtualHost *:80> block
WSGIScriptAlias / /var/www/catalog/catalog.wsgi
3.	Create var/www/catalog/catalog directory  (cd  var/www.  Mkdir catalog.  Cd catalog.  Mkdir catalog)
4.	sudo apt-get install git
5.	Move to cd /var/www/catalog  and clone catalog repository: sudo git clone https://github.com/achechelnitskiy/catalog.git
6.	sudo apt-get install python-pip
7.	sudo pip install virtualenv
8.	sudo virtualenv venv
9.	sudo chmod –r 777 venv
10.	source venv/bin/activate
11.	pip install Flask
12.	pip install httplib2 request oauth2client sqlalchemy python-psycopg2
13.	Go to http://www.hcidata.info/host2ip.cgi , enter public Ip and get alias value
14.	sudo vi /etc/apache2/sites-available/catalog.conf and paste: 
<VirtualHost *:80>
	  ServerName 18.188.211.101
	  ServerAdmin admin@18.188.211.101
	      ServerAlias http://ec2-18-188-211-101.us-east-2.compute.amazonaws.com
 	  WSGIScriptAlias / /var/www/catalog/catalog.wsgi
	  <Directory /var/www/catalog/catalog/>
  	    Order allow,deny
  	    Allow from all
	  </Directory>
	  Alias /static /var/www/catalog/catalog/static
	  <Directory /var/www/catalog/catalog/static/>
  	    Order allow,deny
  	    Allow from all
 	 </Directory>
  	    ErrorLog ${APACHE_LOG_DIR}/error.log
  	    LogLevel warn
  	    CustomLog ${APACHE_LOG_DIR}/access.log combined
	</VirtualHost>
15.	sudo a2ensite catalog
16.	cd into /car/www/catalog directory, vi cataog.wsgi and copy
import sys
	   import logging
	   logging.basicConfig(stream=sys.stderr)
	   sys.path.insert(0, "/var/www/catalog/")
	   from catalog import app as application
17.	Create .htaccess and copy RedirectMatch 404 /.git
18.	Cd to /var/www/catalog/catalog and rename application.py to __init__.py
19.	sudo apt-get install postgresql postgresql-contrib
20.	Update engine info in database_setup.py and in __init__.py with 'postgresql://catalog:catalog@localhost/catalog'
21.	Change to postgres user sudo su – postgres
22.	Connect to psql
23.	CREATE USER catalog WITH PASSWORD catalog
24.	ALTER USER catalog CREATEDB;
25.	CREATE DATABASE catalog WITH OWNER catalog;
26.	Connect to newly created catalog database
27.	REVOKE ALL ON SCHEMA public FROM public;
28.	GRANT ALL ON SCHEMA public TO catalog; and exit
29.	Run python database_setup.py
30.	Update db info in loadMusic.py and load db by running python loadMusic.py
31.	Go to API section in Google Dev Center, add alias and public IP to authorized js origins and redirect urls
32.	Download client secret key to vagrant directory and copy to client_secrets.json on the server as well.
33.	Update a path to client_secrets.json to have an absolute path
34.	Restart apache
35.	Access app by going to http://ec2-18-188-211-101.us-east-2.compute.amazonaws.com/

Resources:
https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04
https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps 
https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps 
Udacity discussion forums: https://discussions.udacity.com/
Udacity "Deploying to Linux Server" course







 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 

 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
  
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 


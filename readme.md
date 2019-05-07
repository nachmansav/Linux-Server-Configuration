# Linux Server Configuration
This is the fifth project for Udacity's Full Stack Web Developer Nanodegree. 

The project is to take a baseline installation of a Linux distribution on a virtual machine and prepare it to host your web applications, to include installing updates, securing it from a number of attack vectors and installing/configuring web and database servers. This was done using AWS.

Project IP address: 52.59.212.112
HTTP address: http://www.52.59.212.112.xip.io/


## Outline of steps taken: 
1. Got a new instance of a linux server running on Lightsail.

2. SSH to the instance using default private key.

3. Updated all installed packages.
	`sudo apt-get update`
	`sudo apt-get upgrade`

4. Updated the Lightsail firewall to allow incoming connections on port 2200.
	
5. Updated the sshd config file to accept conections on port 2200.
	`sudo vim /etc/ssh/sshd_config`

6. Setup the Uncomplicated Firewall to only allow incoming connections to the following ports: 2200(SSH), 80(HTTP), 123(NTP).
	`sudo ufw allow 2200/tcp`
	`sudo ufw allow 80/tcp`
	`sudo ufw allow 123/udp`
	`sudo ufw enable`

7. Restarted the SSH service and logged in again.
	`sudo service ssh restart`

8. Added new user 'grader' with password 'grader'.
	`sudo adduser grader`

9. Gave 'grader' root access.
	`nano /etc/sudoers`
	`touch /etc/sudoers.d/grader`
	`nano /etc/sudoers.d/grader`, add `grader ALL=(ALL:ALL) ALL`

10. Created new SSH key pair locally for grader to use using ssh-keygen. Then added the key pair as follows:
	`su - grader`
	`mkdir .ssh`
	`touch .ssh/authorized_keys`
	`nano .ssh/authorized_keys`
	`chmod 700 .ssh`
	`chmod 644 .ssh/authorized_keys`

11. Set the local timezone to UTC.

12. Install Apache and mod-wsgi.
	`sudo apt-get install apache2`
	`sudo apt-get install python-setuptools libapache2-mod-wsgi`
	`sudo service apache2 restart`

13. Install and configure Postgresql.
	`sudo apt-get install postgresql`
	`sudo nano /etc/postgresql/9.5/main/pg_hba.conf` and make sure that remote connections are disabled.
	`sudo su - postgres`
	`psql`
	`CREATE DATABASE recipes;`
	`CREATE USER catalog;`
	`ALTER ROLE catalog WITH PASSWORD 'catalog';`
	`GRANT ALL PRIVILEGES ON DATABASE recipes TO catalog;`

14. Install git.
	`sudo apt-get install git`

15. Clone Catalog project from Github. It is located at '/var/www/html/Catalog'.

16. Change the files in the project so that it uses the new Postgresql database instead of the old one. change `engine = create_engine('sqlite:///recipes.db')` to `engine = create_engine('postgresql://catalog:catalog@localhost/recipes')`

17. Setup the 000-default.conf file located at '/etc/apache2/sites-available' to configure the virtual host on port 80 to run my_flask_app.wsgi.

18. Setup my_flask_app.wsgi to import project.py.

19. Install dependencies and psycopg2 using pip.

20. Run the database setup 'cat_setup.py' to create the required catagories.

21. Restart Apache.
	`sudo service apache2 restart `

## References:
https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
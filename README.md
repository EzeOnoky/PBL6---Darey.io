# PBL5B - Darey.io
## CLIENT-SERVER ARCHITECTURE WITH MYSQL

## Project Scope – Implement a Client Server Architecture using MySQL Database Management System (DBMS).

#### Understanding Client-Server Architecture
As you proceed your journey into the world of IT, you will begin to realise that certain concepts apply to many other areas. One of such concepts is – Client-Server architecture. Client-Server refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another. In their communication, each machine has its own role: the machine sending requests is usually referred as "Client" and the machine responding (serving) is called "Server". A simple diagram of Web Client-Server architecture is presented below:

![PBL6_1a](https://user-images.githubusercontent.com/122687798/222628690-6f8001aa-08b6-4fb2-bf6c-27144e0814fd.JPG)

In the example above, a machine that is trying to access a Web site using Web browser or simply ‘curl’ command is a client and it sends HTTP requests to a Web server (Apache, Nginx, IIS or any other) over the Internet. If we extend this concept further and add a Database Server to our architecture, we can get this picture:

![PBL6_1a](https://user-images.githubusercontent.com/122687798/222629029-22cfb48b-7670-49b3-a935-ef58968f09ad.JPG)

# So we start our handsOn

# STEP 1 - Create and configure two Linux-based virtual servers (EC2 instances in AWS).

Log in differently to the Client and Server terminals and prepare both

# STEP 2 -  Install MYSQL CLIENT SOFTWARE

# CLIENT PREPARATION - On mysql server Linux Client install MySQL Server software.

### Update Ubuntu Server from the Ubuntu Repository

*sudo apt update -y*

### Install MYSQL CLIENT

*sudo apt install mysql-client -y*


# STEP 3 -  Install MYSQL SERVER, Enable the Server, Install MySQL Security Script on it

## SERVER PREPARATION - On mysql server Linux Server install MySQL Server software.

### Update Ubuntu Server from the Ubuntu Repository

*sudo apt update -y*

### Install MYSQL SERVER

*sudo apt install mysql-server -y*

### Enable MYSQL service

*sudo systemctl enable mysql*

### Enable MYSQL service and Install MySQL Security Script
*sudo systemctl enable mysql*

By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local IP address of your ‘mysql client’.

So we have install the server and client Instance and the services ae running, but we dont have any databases, user, tables etc on the MySQL server. This we need to configure using basic MySQL commands.

So we proceed on the Server preparation by running a security script, follow below steps..

Again, use ‘apt’ to acquire and install this software:
*sudo apt install mysql-server*

When prompted, confirm installation by typing Y, and then ENTER. When the installation is finished, log in to the MySQL console by typing:
*sudo mysql*

This will connect to the MySQL server as the administrative database user root, which is inferred by the use of sudo when running this command. You should see output like this below :

![PBL6_1](https://user-images.githubusercontent.com/122687798/222616233-323ff58e-ad21-49d2-8e48-9ebff22d0de9.JPG)

It’s recommended that you run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system. Before running the script you will set a password for the root user, using mysql_native_password as default authentication method. We’re defining this user’s password as Onoky.1

*ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Onoky.1';*

Exit the MySQL shell with:

*mysql> exit*

Start the interactive script by running:

*sudo mysql_secure_installation*

This will ask if you want to configure the VALIDATE PASSWORD PLUGIN.

Note: Enabling this feature is something of a judgment call. If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.

Answer Y for yes, or anything else to continue without enabling.

*VALIDATE PASSWORD PLUGIN can be used to test passwords*
*and improve security. It checks the strength of password*
*and allows the users to set only those passwords which are*
*secure enough. Would you like to setup VALIDATE PASSWORD plugin?*

*Press y|Y for Yes, any other key for No:*

If you answer “yes”, you’ll be asked to select a level of password validation. Keep in mind that if you enter 2 for the strongest level, you will receive errors when attempting to set any password which does not contain numbers, upper and lowercase letters, and special characters, or which is based on common dictionary words e.g Onoky.1

*There are three levels of password validation policy:*

*LOW    Length >= 8*
*MEDIUM Length >= 8, numeric, mixed case, and special characters*
*STRONG Length >= 8, numeric, mixed case, special characters and dictionary file*

*Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1*


Regardless of whether you chose to set up the VALIDATE PASSWORD PLUGIN, your server will next ask you to select and confirm a password for the MySQL root user. This is not to be confused with the system root. The database root user is an administrative user with full privileges over the database system. Even though the default authentication method for the MySQL root user dispenses the use of a password, even when one is set, you should define a strong password here as an additional safety measure. We’ll talk about this in a moment.


If you enabled password validation, you’ll be shown the password strength for the root password you just entered and your server will ask if you want to continue with that password. If you are happy with your current password, enter Y for “yes” at the prompt:

*Estimated strength of the password: 100*
*Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y*


For the rest of the questions, press Y and hit the ENTER key at each prompt. This will prompt you to change the root password, remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately respects the changes you have made.

When you’re finished, test if you’re able to log in to the MySQL console by typing:

*sudo mysql -p*


Notice the -p flag in this command, which will prompt you for the password used after changing the root user password.

To exit the MySQL console, type:
*mysql> exit*
![PBL6_2](https://user-images.githubusercontent.com/122687798/222620277-678d7efb-0e8f-4bf2-8860-e5833895f184.jpg)
Notice that you need to provide a password to connect as the root user. For increased security, it’s best to have dedicated user accounts with less expansive privileges set up for every database, especially if you plan on having multiple databases hosted on your server.
![PBL6_3](https://user-images.githubusercontent.com/122687798/222622853-73b3c863-cace-4fa3-b86c-5192958c1ce2.JPG)

You might need to configure MySQL server to allow connections from remote hosts.

*sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf*

Replace ‘127.0.0.1’ to ‘0.0.0.0’ like this:

![PBL6_4](https://user-images.githubusercontent.com/122687798/222624024-88a2f86f-e899-4f3d-9090-79210bbc1080.JPG)

Now restart MYSQL service

sudo systemctl restart mysql

# STEP 4 -  Make connection attempt from the Client to the Server

Connect to the CLIENT terminal and run below command to connect to the User from the Client terminal.

sudo mysql -u remote_user -h 54.174.117.10 -p

![PBL6_5](https://user-images.githubusercontent.com/122687798/222627112-1911fbe3-d370-4854-b37e-4b49b707329b.jpg)

# CONGRATULATIONS EZE
You have Implemented a Client Server Architecture using MySQL Database Management System (DBMS)

Further Reading
https://www.w3schools.com/sql/default.asp
https://www.youtube.com/watch?v=jSPslwhYEG0


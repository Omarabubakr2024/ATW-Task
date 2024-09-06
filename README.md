Task 1 Linux base 
Task 1.1: Creating Directories and Files:-
1. Create a directory named project.
2. Inside the project directory, create two subdirectories named src and bin.
3. In the src directory, create a file named main.c.
4. Add the following line to main.c: // This is the main.c file 
Questions:
1. What command did you use to create the project directory? 
   mkdir project
2. How do you navigate into the src directory from the project directory?
   cd src
3. Which command did you use to create the main.c file?
   touch src/main.c

Task 1.2: File Permissions:-
1. Change the permissions of the main.c file to read and write for the owner, and read-only for the group and others.
2. Verify the changes using the ls command.
Questions:
1. What command did you use to change the permissions of main.c?
   chmod 644 src/main.c
2. What do the file permissions rw-r--r-- mean?
   The owner can read and write the file
   Members of the group can only read the file, but they cannot modify it.
   other users can read the file, but they cannot modify it.

Task 1.3: Basic Shell Scripting
1. Create a shell script named hello.sh that prints "Hello, World!" to the terminal.
2. Make the script executable and run it.

Questions:
1. What command did you use to create the hello.sh file?
   echo 'echo "Hello, World!"' > hello.sh
2. How do you make a script executable?
   chmod +x hello.sh
3. How do you run an executable script from the terminal?
   run :./hello.sh

Task 1.4: Package Management
1. Update the package lists on your Linux system.
2. Install the curl package.
Questions:
1. What command did you use to update the package lists?
   apt-get update
2. What command did you use to install the curl package?
   apt-get install curl
3. How do you check if the curl package is installed correctly?
   curl --version

Task 2 Linux Server Simulation:
1. Install Apache, MySQL and PHP on the Linux Ubuntu machine using apt-get or another package manager of your choice.
   apt-get install apache2 mysql-server php libapache2-mod-php php-mysql
2. Configure Apache to serve the website from the /var/www/html/ directory.
   nano /etc/apache2/sites-available/000-default.conf
   systemctl restart apache2
3. Create a simple website that displays the message "Hello World!"; when accessed through a web browser.
   echo "Hello World!" | sudo tee /var/www/html/index.html
4. Configure MySQL to create a new database, user, and password for the website.
   mysql -u root -p 
   inside sql : 
   CREATE DATABASE website_db;
   CREATE USER 'website_user'@'localhost' IDENTIFIED BY 'omar';
   GRANT ALL PRIVILEGES ON website_db.* TO 'website_user'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
5. Modify the website to use the newly created database to display a message that includes the visitor's IP address and the current time.
   <?php
   echo "Visitor IP: " . $_SERVER['REMOTE_ADDR'];
   echo "Current Time: " . date('Y-m-d H:i:s');
   ?>
   Place the code inside /var/www/html/index.php:
   echo '<?php echo "Visitor IP: " . $_SERVER["REMOTE_ADDR"]; echo "Current Time: " . date("Y-m-d H:i:s"); ?>' | sudo tee /var/www/html/index.php
6. Test the website by accessing it through a web browser and verifying that it displays the expected message.
   http://localhost/info.php
  
Task 3 Git and GitHub:
1. Initialize a new Git repository on your local machine.
   git init
2. Create a (.gitignore) file to exclude any sensitive files (like configuration files with passwords).
   echo "config.php" > .gitignore
3. Commit your Markdown documentation file in the Git repository.
   git add README.md
   git commit -m "Add documentation"
4. Create a new repository on GitHub and push your local repository to GitHub.
   git remote add origin https://github.com/Omarabubakr2024/ATW-Task.git
   git push -u origin master
   
Task 4 Containerize Your Repository Using Docker Compose:
Containerize the PHP Laravel application and the MySQL server using Docker Compose to ensure a consistent and reproducible environment.

1. Create Dockerfiles for both the PHP Laravel application and the MySQL server. These Dockerfiles will specify the base images and the setup environment required for each service.
2. Develop a Docker Compose Configuration by writing a docker-compose.yml file that defines and links the services. This configuration will include all necessary environment variables, port mappings, and volume configurations.
3. Build and Launch the Containers by using Docker Compose to build the images and run the containers. This will ensure that the application and database are correctly linked and accessible.
4. Document the Process by detailing the steps taken, including the commands used and any challenges faced, to provide a clear and comprehensive documentation of the containerization process.

In this task, I containerized a Laravel PHP application and MySQL database using Docker Compose. Below is a detailed explanation of the steps I followed, including the creation of Dockerfiles, the configuration of the docker-compose.yml file, and the process of building and running the containers.

First, I created a Dockerfile for the Laravel PHP application. This file defines the base image and the necessary steps to set up the environment for the application. The Dockerfile is as
Base Image: I used the php:7.4-fpm image to run the PHP-FPM process.
Working Directory: The project files are copied into the /var/www directory inside the container.
Extensions: The pdo and pdo_mysql extensions are installed to enable database interactions.
Command: The container runs php-fpm to serve the application.
 For the database service, I used the official MySQL image. This simplifies the process since I don't need to create a custom Dockerfile for MySQL.
 
 Next, I wrote the docker-compose.yml file to define and link both the Laravel application and the MySQL database.
Application Service (app):

version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: laravel_app
    ports:
      - "8000:80"
    volumes:
      - .:/var/www
    depends_on:
      - db

  db:
    image: mysql:5.7
    container_name: mysql_db
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: website_db
      MYSQL_USER: website_user
      MYSQL_PASSWORD: omar
    ports:
      - "3306:3306"
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:

It builds the application image from the Dockerfile.
It exposes port 8000 on the host to port 80 in the container, allowing the application to be accessed via http://localhost:8000.
A volume is created to mount the local project files into the container for easy development.
The application depends on the db service, ensuring the database is running before the application starts.
Database Service (db):
I used the official mysql:5.7 image.
The environment variables configure the MySQL database with a root password, a database (website_db), and a user (website_user) with the password omar.
Port 3306 is exposed for MySQL connections.

To build and run the containers, I used the following command in the project directory: docker-compose up --build
This command builds the Docker images for the Laravel application and starts both the app and db containers. The application is now accessible via http://localhost:8000, and the MySQL database is running on port 3306

Once the containers were running, I tested the Laravel application by opening a web browser and navigating to http://localhost:8000. The application successfully loaded and connected to the MySQL database.

i faced a lot of errors and failures that tought me a lot

Task 5 Networking Basics:
Explain what IP address used in these tasks and it's routing protocols Also, document the steps took you to connect to your cloud instance from a remote machine (including the SSH process).

In these tasks, two types of IP addresses are commonly used:
Localhost (127.0.0.1):
This is the default IP address for your local machine, also known as the loopback address. It allows the machine to refer to itself. For example, when you access http://localhost in your web browser, you're connecting to the local web server (Apache in this case) on the same machine.
Public/Private IP Addresses:
Public IP Address: If you're connecting to a cloud instance or remote machine, the public IP address is used. This IP address is assigned by the cloud provider and can be accessed over the internet.
Private IP Address: Used for internal networking, especially when Docker containers or multiple services are interacting with each other within a cloud or private network.
Routing Protocols :-
Routing protocols direct how data packets are forwarded from one network to another:
Static Routing: In some cases, like your local setup, static routes are defined manually. For instance, when Docker networks are set up, they use predefined IP ranges within the container network.
Dynamic Routing: Cloud providers often use dynamic routing protocols like OSPF or BGP. These help efficiently route traffic between different data centers and internet gateways.
Docker Compose links services (PHP and MySQL) using Docker’s built-in bridge networking. Containers can communicate with each other through container names as hostnames, which makes routing within the application seamless .

To connect to a cloud instance you typically use SSH. Below are the steps:
Generate SSH Keys :-
On your local machine, generate a new SSH key pair if you don’t have one:

command:ssh-keygen -t rsa -b 2048

This creates two files: id_rsa (private key) and id_rsa.pub (public key). You'll use the public key for authentication.
Add the Public Key to the Cloud Instance
Once you have your instance running, you need to copy the public key to the cloud server. You can do this using the ssh-copy-id command, or manually by logging into the cloud provider's dashboard and pasting the public key into the authorized SSH key section.
Connect to the Cloud Instance
Use the SSH command to log into your remote server:

ssh -i /path/to/id_rsa your_user@your_server_ip

-i specifies the path to your private key.
your_user is the username of the remote instance (commonly ubuntu or root).
your_server_ip is the public IP address of your cloud instance.
Once connected, you’ll be inside the terminal of your cloud instance. You can manage your server from here, install services, or deploy your application.































































Task 5 Networking Basics:

Explain what IP address used in these tasks and it's routing protocols

Also, document the steps took you to connect to your cloud instance from a remote machine (including the SSH process).

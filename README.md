## Task 1: Linux Base

### Task 1.1: Creating Directories and Files
1. Create a directory named `project`.
2. Inside the `project` directory, create two subdirectories named `src` and `bin`.
3. In the `src` directory, create a file named `main.c`.
4. Add the following line to `main.c`: `// This is the main.c file`

**Questions:**
- What command did you use to create the project directory?  
  **Answer**: `mkdir project`

- How do you navigate into the `src` directory from the `project` directory?  
  **Answer**: `cd src`

- Which command did you use to create the `main.c` file?  
  **Answer**: `touch src/main.c`

---

### Task 1.2: File Permissions
1. Change the permissions of the `main.c` file to read and write for the owner, and read-only for the group and others.
2. Verify the changes using the `ls` command.

**Questions:**
- What command did you use to change the permissions of `main.c`?  
  **Answer**: `chmod 644 src/main.c`

- What do the file permissions `rw-r--r--` mean?  
  **Answer**:  
  - The owner can read and write the file.  
  - The group can only read the file.  
  - Others can read the file but cannot modify it.

---

### Task 1.3: Basic Shell Scripting
1. Create a shell script named `hello.sh` that prints "Hello, World!" to the terminal.
2. Make the script executable and run it.

**Questions:**
- What command did you use to create the `hello.sh` file?  
  **Answer**: `echo 'echo "Hello, World!"' > hello.sh`

- How do you make a script executable?  
  **Answer**: `chmod +x hello.sh`

- How do you run an executable script from the terminal?  
  **Answer**: `./hello.sh`

---

### Task 1.4: Package Management
1. Update the package lists on your Linux system.
2. Install the `curl` package.

**Questions:**
- What command did you use to update the package lists?  
  **Answer**: `apt-get update`

- What command did you use to install the `curl` package?  
  **Answer**: `apt-get install curl`

- How do you check if the `curl` package is installed correctly?  
  **Answer**: `curl --version`

---

# Task 2: Linux Server Simulation

1. **Install Apache, MySQL, and PHP on the Linux Ubuntu machine using apt-get or another package manager of your choice.**

   ```bash
   apt-get install apache2 mysql-server php libapache2-mod-php php-mysql
   ```

2. **Configure Apache to serve the website from the `/var/www/html/` directory.**

   ```bash
   nano /etc/apache2/sites-available/000-default.conf
   systemctl restart apache2
   ```

3. **Create a simple website that displays the message "Hello World!" when accessed through a web browser.**

   ```bash
   echo "Hello World!" | sudo tee /var/www/html/index.html
   ```

4. **Configure MySQL to create a new database, user, and password for the website.**

   ```bash
   mysql -u root -p
   CREATE DATABASE website_db;
   CREATE USER 'website_user'@'localhost' IDENTIFIED BY 'omar';
   GRANT ALL PRIVILEGES ON website_db.* TO 'website_user'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```

5. **Modify the website to use the newly created database to display a message that includes the visitor's IP address and the current time.**

   ```bash
   echo '<?php echo "Visitor IP: " . $_SERVER["REMOTE_ADDR"]; echo "Current Time: " . date("Y-m-d H:i:s"); ?>' | sudo tee /var/www/html/index.php
   ```

6. **Test the website by accessing it through a web browser and verifying that it displays the expected message.**

   Open the following URL in a browser: `http://localhost/index.php`

---

# Task 3: Git and GitHub

1. **Initialize a new Git repository on your local machine.**

   ```bash
   git init
   ```

2. **Create a `.gitignore` file to exclude any sensitive files (like configuration files with passwords).**

   ```bash
   echo "config.php" > .gitignore
   ```

3. **Commit your Markdown documentation file in the Git repository.**

   ```bash
   git add README.md
   git commit -m "Add documentation"
   ```

4. **Create a new repository on GitHub and push your local repository to GitHub.**

   ```bash
   git remote add origin https://github.com/Omarabubakr2024/ATW-Task.git
   git push -u origin master
   ```

---

# Task 4: Containerize My PHP Laravel Application and MySQL Server Using Docker Compose

## Step 1: Create a Dockerfile for the PHP Laravel Application

In this step, I created a `Dockerfile` to set up the environment for my Laravel application. The file contains the following commands:

```Dockerfile
# Use the official PHP image
FROM php:7.4-fpm

# Install required PHP extensions for Laravel
RUN docker-php-ext-install pdo pdo_mysql

# Set the working directory
WORKDIR /var/www

# Copy project files to the container
COPY . /var/www

# Install Composer
RUN apt-get update && apt-get install -y \
    curl \
    git \
    unzip
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install Laravel dependencies
RUN composer install

# Set permissions
RUN chown -R www-data:www-data /var/www

# Expose the necessary port
EXPOSE 9000
```

## Step 2: Use the Official MySQL Docker Image

For the MySQL service, I used the official MySQL image from Docker Hub, so there was no need to create or modify a separate `Dockerfile` for MySQL.

## Step 3: Write the `docker-compose.yml` File

Next, I created the `docker-compose.yml` file to define and link the services (Laravel application and MySQL server). Here's the configuration I used:

```yaml
version: '3'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: laravel_app
    working_dir: /var/www
    volumes:
      - .:/var/www
    ports:
      - "8000:80"
    depends_on:
      - db

  db:
    image: mysql:5.7
    container_name: mysql_db
    environment:
      MYSQL_DATABASE: laravel_db
      MYSQL_USER: laravel_user
      MYSQL_PASSWORD: secret
      MYSQL_ROOT_PASSWORD: rootpassword
    volumes:
      - db_data:/var/lib/mysql
    ports:
      - "3306:3306"

volumes:
  db_data:
```

## Step 4: Build and Run the Containers

To build and run the containers, I used the following commands:

### 1. Build the containers:

```bash
docker-compose build
```

### 2. Start the containers:

```bash
docker-compose up
```

After running these commands, Docker successfully built and ran the containers for both the Laravel application and the MySQL server.


---

# Task 5: Networking Basics

## IP Address Types Used in These Tasks:

1. **Localhost (127.0.0.1):**
   - This is the default IP address for my local machine, also known as the loopback address. It allows the machine to refer to itself. For example, when you access `http://localhost` in your web browser, you're connecting to the local web server (Apache in this case) on the same machine.

2. **Public/Private IP Addresses:**
   - **Public IP Address:** If you're connecting to a cloud instance or remote machine, the public IP address is used. This IP address is assigned by the cloud provider and can be accessed over the internet.
   - **Private IP Address:** Used for internal networking, especially when Docker containers or multiple services are interacting with each other within a cloud or private network.

## Routing Protocols:

Routing protocols direct how data packets are forwarded from one network to another:

- **Static Routing:** In some cases, like your local setup, static routes are defined manually. For instance, when Docker networks are set up, they use predefined IP ranges within the container network.
  
- **Dynamic Routing:** Cloud providers often use dynamic routing protocols like OSPF (Open Shortest Path First) or BGP (Border Gateway Protocol). These protocols help efficiently route traffic between different data centers and internet gateways.

Docker Compose links services (such as PHP and MySQL) using Docker’s built-in bridge networking. Containers can communicate with each other using container names as hostnames, which makes routing within the application seamless.

---

## Steps to Connect to a Cloud Instance from a Remote Machine (SSH Process):

To connect to a cloud instance, you typically use SSH (Secure Shell). Below are the steps:

### 1. Generate SSH Keys:

On your local machine, generate a new SSH key pair if you don’t have one:

```bash
ssh-keygen -t rsa -b 2048
```

This command creates two files: 
- `id_rsa` (private key)
- `id_rsa.pub` (public key)

You'll use the public key for authentication.

### 2. Add the Public Key to the Cloud Instance:

Once you have your cloud instance running, you need to copy the public key (`id_rsa.pub`) to the cloud server. You can do this using the `ssh-copy-id` command:

```bash
ssh-copy-id your_user@your_server_ip
```

Alternatively, you can log into the cloud provider's dashboard and paste the public key into the authorized SSH key section for the instance.

### 3. Connect to the Cloud Instance:

Use the SSH command to log into your remote server:

```bash
ssh -i /path/to/id_rsa your_user@your_server_ip
```

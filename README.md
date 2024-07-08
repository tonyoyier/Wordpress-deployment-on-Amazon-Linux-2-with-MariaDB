---

# WordPress Deployment on Amazon Linux 2 with MariaDB

[Alt text](Hosting Wordpress on Amazon Linux 2.jpeg)

## Project Overview

This project deploys of a WordPress instance on an Amazon Linux 2 EC2 instance within a public subnet of the default VPC, alongside a MariaDB database. This setup is suitable for testing or small-scale deployments but is not recommended for production due to its simplicity and lack of scalability and security measures.

## Architecture Overview

- **Virtual Private Cloud (VPC)**: Public subnet
- **Wordpress Server**: Deployed on Amazon Linux 2
- **Security Groups**: Configured to allow communication between components.
  
## Key Tasks Achieved

1. **Deploy and Configure WordPress on Amazon Linux 2**
   - Installed LAMP stack (Linux, Apache, MySQL/MariaDB, PHP).
   - Configured Apache web server and PHP.
   - Installed WordPress and configured it to use a MariaDB database.

2. **Deploy and Configure MariaDB**
   - Installed MariaDB on the Amazon Linux 2 instance.
   - Created a database and user for WordPress.

3. **Testing and Verification**
   - Verified the deployment by accessing the WordPress site via the public IP of the EC2 instance.
   - Completed WordPress installation through the browser interface.

## Detailed Steps

### Step 1: Setting Up the EC2 Instance (Amazon Linux 2)

- Launched an EC2 instance in a public subnet of the default VPC.
- Installed and configured LAMP stack with MariaDB:
  ```bash
  sudo yum update -y
  sudo amazon-linux-extras install mariadb10.5
  sudo amazon-linux-extras install php8.2
  sudo yum install -y httpd
  sudo systemctl start httpd
  sudo systemctl enable httpd
  ```
- Configured Apache and PHP settings.
- Installed WordPress under `/var/www/html`.

### Step 2: Setting Up MariaDB

- Installed MariaDB and secured it:
  ```bash
  sudo systemctl start mariadb
  sudo mysql_secure_installation
  sudo systemctl enable mariadb
  ```
- Created a database and user for WordPress:
  ```sql
  mysql -u root -p
  CREATE DATABASE `wordpress-db`;
  CREATE USER 'wordpress-user'@'localhost' IDENTIFIED BY 'your_strong_password';
  GRANT ALL PRIVILEGES ON `wordpress-db`.* TO 'wordpress-user'@'localhost';
  FLUSH PRIVILEGES;
  exit
  ```
### Step 3: Installing WordPress

- Downloaded and extracted WordPress:
  ```bash
  cd /var/www/html
  wget https://wordpress.org/latest.tar.gz
  tar -xzf latest.tar.gz
  sudo chown -R apache:apache wordpress
  sudo mv wordpress/* .
  sudo rm -rf wordpress latest.tar.gz
  sudo systemctl restart httpd
  ```
- Set up the WordPress configuration:
  ```bash
  cd /var/www/html
  cp wp-config-sample.php wp-config.php
  nano wp-config.php
  ```
  Edited the `wp-config.php` file to include my database details:
  ```php
  /** The name of the database for WordPress */
  define('DB_NAME', 'wordpress-db');

  /** MySQL database username */
  define('DB_USER', 'wordpress-user');

  /** MySQL database password */
  define('DB_PASSWORD', 'your_strong_password');

  /** MySQL hostname */
  define('DB_HOST', 'localhost');
  ```
- Completed the WordPress installation through my browser by accessing the public IP of my EC2 instance.

## Notes on Architecture

- This setup is basic and lacks advanced security and scalability features.
- Everything (WordPress, MariaDB) is hosted on a single EC2 instance which can be a single point of failure.
- Database scalability is limited as it runs on the EC2 instance rather than being managed separately.

## Reference Links

- [AWS Tutorial on Hosting WordPress on Amazon Linux 2](https://docs.aws.amazon.com/linux/al2/ug/hosting-wordpress.html)
- [Installing a LAMP Web Server on Amazon Linux 2](https://docs.aws.amazon.com/linux/al2/ug/ec2-lamp-amazon-linux-2.html)
```

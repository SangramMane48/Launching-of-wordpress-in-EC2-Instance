# Step-by-Step Guide to Run WordPress on EC2 Instance


# 1.Launch an EC2 Instance

1.Go to AWS Management Console: Open the AWS Management Console and navigate to EC2.


2.Launch a New Instance:

 - Click on Launch Instance.
  - Choose Amazon Linux 2 or Ubuntu as the AMI (Amazon Machine Image).

  - Select an instance type. A t2.micro instance (free tier eligible) is recommended for small projects.

  - Configure instance details like VPC and security group.

  - Add a key pair if you don’t have one already. You’ll use this to SSH into your instance.

  - Security Group: Ensure that your security group allows inbound HTTP, HTTPS, and SSH traffic:
  SSH (port 22) for managing the server.
  HTTP (port 80) and HTTPS (port 443) to serve web traffic.

3.Launch the Instance: After configuring the instance, click Launch.

# 2. Connect to the EC2 Instance via SSH

Once your instance is running, connect to it using SSH from your terminal.

1.Copy the Public IP of your EC2 instance from the AWS console.

2.Open your terminal and run the following command:

Amazon Linux:
```
ssh -i "your-key.pem" ec2-user@your-ec2-public-ip
```
Ubuntu:
```
ssh -i "your-key.pem" ubuntu@your-ec2-public-ip

```
Replace your-key.pem with the path to your private key and your-ec2-public-ip with the public IP of your EC2 instance.

# 3.Install the Required Software

To run WordPress, you’ll need to install the following software packages:

- Apache or Nginx (web server)
- PHP (programming language)
- MySQL or MariaDB (database server)

For Amazon Linux 2:
1.Update the instance:
```
sudo yum update -y
```
2.Install Apache (HTTPD):
```
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd

```
3.Install PHP:
```
sudo amazon-linux-extras enable php7.4
sudo yum install php php-mysqlnd -y
sudo systemctl restart httpd

```
4.Install MySQL (MariaDB):
```
sudo yum install mariadb-server -y
sudo systemctl start mariadb
sudo systemctl enable mariadb

```

For Ubuntu:
1.Update the instance:
```
sudo apt-get update -y

```
2.Install Apache:
```
sudo apt-get install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2

```
3.Install PHP:
```
sudo apt-get install php libapache2-mod-php php-mysql -y
sudo systemctl restart apache2

```
4.Install MySQL:
```
sudo apt-get install mysql-server -y
sudo systemctl start mysql
sudo systemctl enable mysql

```

# 4.Configure the MySQL Database

1.Secure MySQL: Run the following command to secure the MySQL installation and set a root password:

```
sudo mysql_secure_installation
```
2.Create a WordPress Database:

 - Log into MySQL:
   
```
sudo mysql -u root -p
```
- Create a new database for WordPress:
  ```
   CREATE DATABASE wordpress;
  ```
- Create a new user for WordPress:
```
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'your-password';
```
- Grant privileges to the user:
```
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
```
- Flush the privileges and exit:
```
FLUSH PRIVILEGES;
EXIT;
```

# 5.Download and Configure WordPress

1.Navigate to the Web Root Directory:
```
cd /var/www/html
```
2.Download WordPress:
```
sudo yum install wget -y
sudo wget https://wordpress.org/latest.tar.gz
```
3.Extract the WordPress Archive:
```
sudo tar -xvzf latest.tar.gz
sudo mv wordpress/* .
sudo rm -rf wordpress latest.tar.gz
```
4.Set Correct Permissions:

- Make the Apache user the owner of the files:
```
sudo chown -R apache:apache /var/www/html
```
- For Ubuntu, use:
 ```
sudo chown -R www-data:www-data /var/www/html
```
5.Configure the WordPress wp-config.php:

- Create a copy of the sample config file:
```
sudo cp wp-config-sample.php wp-config.php6. Restart Apache
After configuring WordPress, restart the Apache server to apply the changes.
```
- Edit the wp-config.php file
 ```
  sudo vi wp-config.php
```
- Update the following lines with the database details you set earlier:
```
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'wordpressuser' );
define( 'DB_PASSWORD', 'your-password' );
define( 'DB_HOST', 'localhost' );
```
# 6.Restart Apache

After configuring WordPress, restart the Apache server to apply the changes.
- For Amazon Linux:
```
sudo systemctl restart httpd
```
- For Ubuntu:
```
sudo systemctl restart apache2
```

# 7.Complete the WordPress Installation

Now that the server is configured and WordPress is set up, you can complete the WordPress installation by visiting your instance’s public IP address in your browser:
```
http://your-ec2-public-ip
```


Click Install WordPress, and your WordPress site should be up and running.

# 8.Access WordPress Dashboard

After the installation, you can log in to the WordPress admin dashboard by navigating to:
```
http://your-ec2-public-ip/wp-admin
```
Enter the admin credentials you set during the installation process to access your dashboard and start customizing your WordPress site.

# Optional: Set Up Domain and SSL
1.Associate a domain name with your EC2 instance using Route 53 or another DNS provider.

2.Enable HTTPS by installing an SSL certificate using Let’s Encrypt or another certificate authority.



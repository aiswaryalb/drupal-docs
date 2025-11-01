# Launch an EC2 Instance and Set Up Nginx with PHP

## Prerequisites

- An AWS account
- Basic knowledge of SSH and command line

## Steps to Launch EC2 Instance

1. **Launch Instance from EC2**

2. **Create a PEM File of Key Pair**
   - Save it locally.

3. **SSH into the VM**
   ```bash
   ssh -i "ubuntu.pem" ubuntu@ec2-13-61-148-126.eu-north-1.compute.amazonaws.com
   ```
   ### SSH Command
4. Set Permissions on PEM File
    ```bash
    chmod 400 ubuntu.pem
    ```
## Install Nginx

1. Update Package List
  ```bash
  sudo apt update
  ```
2. Install Nginx
   ```bash
   sudo apt install nginx
   ```
4. Check the Status of Nginx
   ```bash
   sudo service nginx status
    ```
   <img width="926" height="325" alt="image" src="https://github.com/user-attachments/assets/23e71129-1e55-4509-8c3f-f8ee6e3a56cc" />
6. Reload Nginx Service
  ```bash
  sudo service nginx reload
  ```

## Install PHP

1. Add PHP Repository
  ```bash
  sudo add-apt-repository ppa:ondrej/php
  ```
3. Update Package List Again
   ```bash
   sudo apt update
   ```
5. Check Available PHP Versions
   ```bash
   sudo apt policy php
   ```
7. Install PHP 8.4 and Common Extensions
   ```bash
   sudo apt install php8.4 -y
   sudo apt install php8.4-common php8.4-cli php8.4-opcache php8.4-mysql php8.4-xml php8.4-curl php8.4-zip php8.4-mbstring php8.4-gd php8.4-intl php8.4-bcmath -y
   ```

## Configure PHP to Work with Nginx
   
1. Navigate to HTML Directory
   ```bash
   cd /var/www/html
   ```
   Here, index.html is the file that gets rendered. You can modify its content to display your own.
2. Add an index.php File
   To serve PHP files, install the FastCGI Process Manager (FPM):
   ```bash
   sudo apt install php8.4-fpm
    ```
3.  Enable and Check PHP-FPM Status
    ```bash
    sudo systemctl enable php8.4-fpm
    sudo systemctl status php8.4-fpm
    ```
4. Configure Nginx
    Edit the default server block:
    ```bash
    sudo nano /etc/nginx/sites-available/default
    ```
    Find the index line and add index.php to it.
    Uncomment or add the following block:
    ```bash
    location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/var/run/php/php8.4-fpm.sock; # adjust version if needed
    }
    ```
5. Reload Nginx
    ```bash
    sudo service nginx reload
    ```






   

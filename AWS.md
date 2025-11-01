Launch instance from EC2
Create a pem file of key pair and save in local
SSH into the VM
  <img width="813" height="530" alt="image" src="https://github.com/user-attachments/assets/74929ee8-b8a7-43fe-89be-c2701f81d062" />

  eg : ssh -i "ubuntu.pem" ubuntu@ec2-13-61-148-126.eu-north-1.compute.amazonaws.com
The pem file should be given permission 400
  run : chmod 400 ubuntu.pem
sudo apt update

Install nginx

sudo apt install nginx
sudo ufw app list
service nginx status
<img width="926" height="325" alt="image" src="https://github.com/user-attachments/assets/23e71129-1e55-4509-8c3f-f8ee6e3a56cc" />
sudo service nginx reload

Install PHP
sudo add-apt-repository ppa:ondrej/php
sudo apt update

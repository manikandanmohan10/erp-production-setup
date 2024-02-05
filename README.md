# ERP production setup
**Enter to your system via ssh with root user**
## Step 1
```
apt update && apt upgrade -y && shutdown -r now
```
## Step 2
```
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
```
## Step 3
```
apt install -y nodejs mariadb-server redis-server python3-pip nginx python3-testresources python3-venv
```
## Step 4 (If you're using instance you can create a new user)
```
adduser {USERNAME}
```
## Step 5 (only if you followed step 4)
```
usermod  -aG sudo {USERNAME}
```
## Step 6
Exit from root user and log in to the new user which we created
```
sudo nano /etc/mysql/my.cnf
```
Add these lines at last
```
[mysqld]
character-set-client-handshake = FALSE 
character-set-server = utf8mb4 
collation-server = utf8mb4_unicode_ci 

[mysql]
default-character-set = utf8mb4
```
## Step 7
```
sudo service mysql restart
```
## Step 8
```
sudo mysql_secure_installation
```
- Set root password? [Y/n] y
- Remove anonymous users? [Y/n] y
- Disallow root login remotely? [Y/n] n
- Remove test database and access to it? [Y/n] y
- Reload privilege tables now? [Y/n] y
## Step 9
```
mysql -u root -p
```
Run this
```
USE mysql;
ALTER USER 'root'@'localhost' IDENTIFIED WITH '{your_password}';
FLUSH PRIVILEGES;
```
```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY '{your_password}' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
```
## Step 10
Use your user {USERNAME} after reboot (do not continue as root)
```
sudo npm install -g yarn
```
## Step 11
To check the versions
```
node -v && npm -v && python3 -V && pip3 -V && yarn -v 
```
## Step 12
```
pip3 install frappe-bench
```
(or)
```
sudo pip3 install frappe-bench
```
## Step 13
```
bench init --frappe-branch version-14 frappe-bench
```
## Step 14
```
cd frappe-bench
```
```
bench new-site {site-name}
```
## Step 15
Get erpnext
```
bench get-app --branch version-14 erpnext
```
## Step 16
Installing erpnext
```
bench --site mysite install-app erpnext
```
## Step 17 (Production Mode setup)
```
sudo bench setup production $USER
bench restart
```
If bench restart is not worked run the following command again with all Questions Yes
```
sudo bench setup production $USER
```
if js and css file is not loading on login window run the following command
```
sudo chmod o+x /home/$USER
```
```
bench restart
```
# SSL generator (certbort)
## Step 1
Install snapd if not installed.
```
sudo apt install snapd
```
## Step 2
Update snapd core
```
sudo snap install core
sudo snap refresh core
```
## Step 3
If you have old certbot installed, uninstall it.
```
sudo apt-get remove certbot
```
## Step 4
Install classic certbot
```
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```
## Step 5
Automatic ssl installation
```
sudo certbot --nginx
```
## Step 6
Optional, if you want to test "renew" without saving any certificates to disk
```
sudo certbot renew --dry-run
```

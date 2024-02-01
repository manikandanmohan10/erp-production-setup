# erp-production-setup
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
apt install -y nodejs mariadb-server redis-server python3-pip nginx python3-testresources
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
```
nano /etc/mysql/my.cnf
```
Add these lines
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
service mysql restart
```
## Step 8
```
run mysql_secure_installation
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
```
USE mysql; 
UPDATE user SET plugin=' ' WHERE user ='root';  (or) ALTER USER 'root'@'{host}' IDENTIFIED WITH '';
FLUSH PRIVILEGES;
exit
```

# 21FTT1251 Logbook
Abdul Wafi Hakim bin Abd. Hakim (21FTT1251)
DISS07

---
# Assignment 1 - VPS Deployment

---

- [*1.0 SSH key Setup*](#-10-ssh-key-setup-)
  * [1.1 Creating .ssh folder](#11-creating-ssh-folder)
  * [1.2 Setting up the new SSH key](#12-setting-up-the-new-ssh-key)
- [*2.0 Digital Ocean Droplet Setup*](#-20-digital-ocean-droplet-setup-)
- [*3.0 SSH Server Setup*](#-30-ssh-server-setup-)
  * [3.1 Root Login](#31-root-login)
  * [3.2 Creating a Non-Root User](#32-creating-a-non-root-user)
  * [3.3 Granting Administrative Privilege](#33-granting-administrative-privilege)
  * [3.4 Enabling External Access for Regular User](#34-enabling-external-access-for-regular-user)
  * [3.5 Setting Up Firewall](#35-setting-up-firewall)
  * [3.6 SSH Configuration Settings](#36-ssh-configuration-settings)
  * [3.7 Allowing Custom Port](#37-allowing-custom-port)
- [*4.0 Shortcut*](#-40-shortcut-)
  * [4.1 Creating Config File](#41-creating-config-file)
- [*5.0 Nginx*](#-50-nginx-)
  * [5.1 Login as Non-Root User](#51-login-as-non-root-user)
  * [5.2 Updating Packaging System](#52-updating-packaging-system)
  * [5.3 Installing Nginx](#53-installing-nginx)
  * [5.4 Setting Up Server Blocks](#54-setting-up-server-blocks)
  * [5.5 Changing Ownership (www-data)](#55-changing-ownership--www-data-)
- [*6.0 Time Zone*](#-60-time-zone-)
  * [6.1 Changing Time Zone](#61-changing-time-zone)
  * [6.2 Checking Time Zone](#62-checking-time-zone)

---

## *1.0 SSH key Setup*
### 1.1 Creating .ssh folder
**(IMPORTANT)**
`Log entry date: 31st August 2023`

1. Open windows terminal and create .ssh folder:

```sh
mkdir .ssh
```

2. Once created, locate and access the directory:

```sh
cd .ssh
```

### 1.2 Setting up the new SSH key
1. Next, create an encrypted key using the ed25519 with student email as the comment:

```sh
ssh-keygen -t ed25519 -C "21ftt1251@student.pb.edu.bn"
```
Options used:
	`-t` : used to specify the type of key to create
	`-C` : used for inserting comments inside a ("")

2. The public and private key for ed25519 key pair will then be generated

3. Enter the desired file to save the key and leave the passphrase empty by clicking enter 2 times

4. To check the content of the public key:
**(Make sure to access the .ssh folder first)**

```sh
type PubFileName.pub
```

---

## *2.0 Digital Ocean Droplet Setup*
`Log entry date: 31st August 2023`

1. Log in to your Digital Ocean account

2. Create a new droplet

3. Choose **Singapore** as the selected region 

4. Select the latest version of Ubuntu which is **Ubuntu 22.04**

5. Choose the **Basic Plan** with the regular CPU options at **$6/month**

6. Use SSH Key as the authentication method
	- First, you need to add a new SSH key
	- In order to do that, you need to insert the SSH key content and name it
	- Insert the SSH key content by pasting the copied content of the public key
	- Lastly, name the newly added SSH key

7. Select the newly added SSH key and rename the hostname

8. Create the droplet and upon successful creation, the VPS IP address will be provided

---

## *3.0 SSH Server Setup*
`Log entry date: 31st August 2023`
### 3.1 Root Login
1.  To log in as a root user, open terminal and access into the .ssh directory

```sh
> cd .ssh
```

2. once accessed, copy this code into the terminal to login

```sh
> ssh -i sop_as1 root@157.245.146.115
```
- `-i` refers to the SSH key that will be use to login

### 3.2 Creating a Non-Root User

1. Using `adduser` to create a non-root user:

```
> adduser ftt1251
> password ftt1251
```

### 3.3 Granting Administrative Privilege

1. To grant Administrative Privilege (sudo) to non-root user:

```
usermod -aG sudo ftt1251
```

### 3.4 Enabling External Access for Regular User

1. To enable external access to non-root user:

```
rsync --archive --chown=ftt1251:ftt1251 ~/.ssh /home/ftt1251
```

### 3.5 Setting Up Firewall

1. To view the list of installed UFW profiles:

```sh
ufw app list
```

2. Next, allow the SSH connections:

```sh
ufw allow OpenSSH
```

3. Lastly, enable the firewall:

```sh
ufw enable
```

4. To check the firewall status:

```sh
ufw status
```

### 3.6 SSH Configuration Settings

1. To access the SSH configuration settings,

```sh
nano /etc/ssh/sshd_config
```

2. There are a few things that needs to be change according to the requirements:
- Adding custom port number : `1251`
- Set the password authentication to :  `no`
- Set the PubKeyAuthentication to : `yes`
- Set the PermitRootLogin to : `no`
- Change the MaxAuthTries to : `3`

3. Save the changes by pressing `CTRL + x` then press `y` to save and simply exit by pressing `enter`

4. Lastly, reload the SSH service:

```sh
sudo service ssh restart
```

### 3.7 Allowing Custom Port

1. To allow set custom port, check the UFW status:

```
sudo ufw status
```

2. Next, allow the custom port:

```
sudo ufw allow 1251/tcp
```

3. After this step, non-root user will have to login by including the custom port:

```sh
ssh -i sop_as1 ftt1251@157.245.146.115 -p 1251 
```

---

## *4.0 Shortcut*
`Log entry date: 31st August 2023`
### 4.1 Creating Config File

1. First, access the .ssh folder and create a config file with the type `FILE`

2. Next, open the config file using notepad to start editing

3. Then, copy this to be inserted inside the config file:

```
Host as1
    HostName 157.245.146.115
    User ftt1251
    Port 1251
    IdentityFile ~/.ssh/sop_as1
```

4. Save the file by clicking `CTRL + S` and exit the notepad

---

## *5.0 Nginx*
`Log entry date: 31st August 2023`
### 5.1 Login as Non-Root User

1. To login as a non-root user, the non-root user will have to login by including the custom port:

```sh
ssh -i sop_as1 ftt1251@157.245.146.115 -p 1251 
```

2. Or, if shortcut have been created, simply log in by entering:

```sh
ssh as1
```

### 5.2 Updating Packaging System

1. Once logged in, user may update the `apt` packaging system:

```sh
sudo apt update
```

2. Any pending update can be upgrade by:

```sh
sudo apt upgrade
```


### 5.3 Installing Nginx

1. To install Nginx, make sure the `apt` packaging system is up to date and install Nginx by entering:

```sh
sudo apt install nginx
```

2. Once installed, check the firewall app list to check for Nginx application:

```sh
sudo ufw app list
```

3. Next, allow `Nginx Full`:

```sh
sudo ufw allow 'Nginx Full'
```
- `Nginx Full` opens both port 80 and 443
- `Nginx HTTP` opens only port 80
- `Nginx HTTPS` opens only port 443

4. Once allowed, check if Nginx is running:

```sh
systemctl status nginx
```

### 5.4 Setting Up Server Blocks

1. First, create a directory, `site/domain`

```
sudo mkdir -p /var/www/ftt1251/html
```

2. Assign the ownership of the directory with `$USER` environment variable

```
sudo chown -R $USER:$USER /var/www/ftt1251/html
```

3. Next, set the permissions to allow the owner to read, write and execute files:

```
sudo chmod -R 755 /var/www/ftt1251
```

4. Then, create an index.html page using nano:

```
nano /var/www/ftt1251/html/index.html
```

5. Insert the given sample HTML from the assignment brief:

```
<!DOCTYPE html> 
<html lang="en"> 
<head> 
<meta charset="UTF-8"> 
<title>SysOps Assignment 1</title> 
</head> 
<body> Welcome to my website on my VPS on the cloud! <br> 
My ID is ftt1251 
</body> 
</html>
```

6. Save the changes by pressing `CTRL + x` then press `y` to save and simply exit by pressing `enter`

7. Next, create a server block with the correct directives:

```
sudo nano /etc/nginx/sites-available/ftt1251

server {
        listen 80;
        listen [::]:80;

        root /var/www/ftt1251/html;
        index index.html index.htm index.nginx-debian.html;

        server_name ftt1251 157.245.146.115;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

8. Next, enable the file by creating a link to the `sites-enabled` directory:

```
sudo ln -s /etc/nginx/sites-available/ftt1251 /etc/nginx/sites-enabled/
```

9. Find the `server_names_hash_bucket_size` directive and remove the `#` symbol to the uncomment the line:

```
...
http {
    ...
    server_names_hash_bucket_size 64;
    ...
}
...
```

10. Save the changes by pressing `CTRL + x` then press `y` to save and simply exit by pressing `enter`

11. Test to make sure that there are no syntax errors in any of your Nginx files:

```
sudo nginx -t
```

12. Restart Nginx if there aren't any problems to enable changes;

```
sudo systemctl restart nginx
```

### 5.5 Changing Ownership (www-data)

1. To check directory of www-data:

```sh
ll /var/www
```

2. To change permission and ownership to the www-data:

```sh
sudo chown -R www-data:www-data /var/www/ftt1251
```

---

## *6.0 Time Zone*
`Log entry date: 1st September 2023`
### 6.1 Changing Time Zone

1. Check the current time zone:

```sh
timedatectl
```

2. Set the time zone to Singapore if haven't:

```sh
sudo timedatectl set-timezone Asia/Singapore
```

### 6.2 Checking Time Zone

1. To check if time zone have been change:

```sh
timedatectl
```

**Date of logbook creation: `1st September 2023`**
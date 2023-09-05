# 21FTT1251 Logbook
Abdul Wafi Hakim bin Abd. Hakim (21FTT1251)
DISS07

---
# Assignment 1 - VPS Deployment

---

- [1.0 SSH key Setup](#10-ssh-key-setup)
  * [1.1 Creating .ssh folder](#11-creating-ssh-folder)
  * [1.2 Setting up the new SSH key](#12-setting-up-the-new-ssh-key)
- [2.0 Digital Ocean Droplet Setup](#20-digital-ocean-droplet-setup)
- [3.0 SSH Server Setup](#30-ssh-server-setup)
- [4.0 VPS](#40-vps)
- [5.0 Nginx](#50-nginx)
- [6.0 Webserver](#60-webserver)

---
`Log entry date: 31st August 2023`
## 1.0 SSH key Setup
### 1.1 Creating .ssh folder
**(IMPORTANT)**

- Open windows terminal and create .ssh folder:

```sh
mkdir .ssh
```

- Once created, locate and access the directory:

```sh
cd .ssh
```

### 1.2 Setting up the new SSH key
- Next, create an encrypted key using the ed25519 with student email as the comment:

```sh
ssh-keygen -t ed25519 -C "21ftt1251@student.pb.edu.bn"
```
Options used:
	-t : used to specify the type of key to create
	-C : used for inserting comments inside a ("")

- The public and private key for ed25519 key pair will then be generated

- Enter the desired file to save the key and leave the passphrase empty by clicking enter 2 times

- To check the content of the public key:
**(Make sure to access the .ssh folder first)**

```sh
type PubFileName.pub
```

---

## 2.0 Digital Ocean Droplet Setup

- Log in to your Digital Ocean account

- Create a new droplet

- Choose a region (Singapore in this case)

- Select the latest version of Ubuntu

- Choose the Basic Plan with the regular CPU options at $6/month

- Use SSH Key as the authentication method
	- First, you need to add a new SSH key
	- In order to do that, you need to insert the SSH key content and name it

- Select the newly added SSH key and rename the hostname

---

## 3.0 SSH Server Setup



---

## 4.0 VPS

---

## 5.0 Nginx

---

## 6.0 Webserver
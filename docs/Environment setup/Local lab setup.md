# Local lab setup
The aim of this page is to help you configure your own local forensic lab for forensic purposes. For security reasons, this local lab should only be accessible from your host and virtual machines.
sudo virsh net-autostart default
**WARNING !**

*Please note that the configuration explained here is unsafe for the studies of malware programs ! Use this configuration at your own risk, I am not responsible for any misuses and damage done to your computer.*

This page will help you configure your computer to get the following functionalities for your host and virtual machines : 

- Local website which hosts severals services and resources such as Cyberchef, PDF documents...
- Local FTP server which allow you to easily transfer files between virtual machines

## Installation
First, update your system with these commands : 
```
sudo apt update && sudo apt upgrade # For ubuntu and debian
sudo pacman -Syu # For arch based system
```
Then, install the following softwares on your pc : 
```
sudo apt install vsftpd nginx dnsmasq #Ubuntu and Debian system
yay -S vsftpd nginx dnsmasq #For arch based systems
sudo pacman -S vsftpd nginx dnsmasq #Alternative for arch based system
```
Now that you have installed the required software, we can now configure them 
## Configuration
### DNSmasq
The aim of this section is to configure the DNS server so you can access your own offline lab without using an IP address.

The first step is to get the IP address of the host machine. Use the following command and spot the virtual network interface : 
```
ifconfig #First command
ip addr show #Second command
```
Note the IP address you found, you will use it later. 
Now that we have our own IP address, we can configure the dnsmasq service. Open the configuration file `/etc/dnsmasq.conf` and add the following line to the end : 

```
bind-interfaces  
# Put here the DNS server of your choice
server=1.1.1.1 
log-queries  
  
# does not go upstream to resolve addresses ending in 'your-domain.lab'  
local=/your-domain.lab/
```
Then change the configuration of the file `/etc/resolv.conf` to use dnsmasq to resolve address by changing this line : 
```
nameserver 127.0.0.1
```
And don't forget to add your domain name in the `/etc/hosts` file : 
```
your_libvirt_ip_address your_domain.lab
```
You should now be able to access your own local lab by using the domain name you choose.  
### WEB server
#### Home page
The first step we're going to do is to configure the WEB server so it can host our services.
The default public location of nginx is `/usr/share/nginx/html`. 
You can use the following source code to replace the default file `index.html` : 
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Home lab</title>
        <link rel="stylesheet" href="style.css">
    </head>
    <body>
    <h1>Welcome !</h1>
    <p>Choose the service you want to access</p>
        <table>
            <thead>
                <tr>
                    <th colspan="2">Resources link</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td><a href="http://localhost/">Home</a></td>
                    <td>Repertoire de base</td>
                </tr>
                <tr>
                    <td><a href="http://localhost/">Resource N° 1</a></td>
                    <td>Lorem Ipsum</td>
                </tr>
                <tr>
                    <td><a href="http://localhost/">Resource N° 2</a></td>
                    <td>Lorem Ipsum </td>
                </tr>
                <tr>
                    <td><a href="http://localhost/">Resource N° 2</a></td>
                    <td>Lorem Ipsum</td>
                </tr>

            </tbody>
        </table>
        <br>
    <p><em>Created by YOUR_NAME, local usage only.</em></p>
    </body>
</html>
```
Feel free to add rows to the table to add your own resources. 
#### Enable directory listing
To enable directory listing in nginx, you need to configure the configuration file `/etc/nginx/nginx.conf`
and add these lines within the 'server' location.
```
location / {  
           root   /usr/share/nginx/html;  
           index  index.html index.htm;  
       }
```
Don't forget to restart nginx with this command : 
```
sudo systemctl restart nginx
```
And you should be able to browse the directory within the web server.
#### Add services
Now that your web server is configured, you can add the services of your choice. Personally, I choose to install cyberchef on my own lab.
##### Cyberchef
To install [Cyberchef](https://gchq.github.io/CyberChef/) on your local web server, create a new directory called `cyberchef` in `/usr/share/nginx/html`. Once it's done, you can [download](https://github.com/gchq/CyberChef/releases) the latest release of cyberchef and unzip the file content within the `cyberchef` directory. Don't forget to rename the html file to `index.html` and you should be able to use cyberchef on your own PC offline through the web server. 
##### Add whatever you want
From now on, you should have a fully functionnal web server. Based on your needs and occupations, why not install on the web server, services you think are essential to you ?  The choice is yours ! 

### FTP server
The aim of this section is to help you deploy a FTP server to easily transfer files between your virtual and host machine.
#### Locate the root folder of your service
The first step is to locate the folder of the FTP server. In my case, it is `/var/ftp/pub`.
#### Create an user for the FTPuser
```
sudo useradd -md /var/ftp/pub ftpuser #Create a user called ftpuser and set its home directory in /var/ftp/pub
sudo passwd ftpuser #Add a password to the account
sudo mkdir /var/ftp/pub/home #Create a folder called home in the path /var/ftp/pub
```
Now that the user is created, we can now configure the FTP server : 
#### Configure the vsftpd service
Locate and modify the file `/etc/vsftpd.conf`. Make sure that the following lines are configured in the same fashion on your side : 

```
#Disable anonymous login
anonymous_enable=NO
#Allow local user to connect to the servers
local_enable=YES
# Uncomment this to enable any form of FTP write command.  
write_enable=YES #Enable writing in the folders
# Activate directory messages - messages given to remote users when they  
# go into a certain directory.
#messages given to remote users when they go into a certain directory.  
dirmessage_enable=YES
# Activate logging of uploads/downloads.
xferlog_enable=YES
# Make sure PORT transfer connections originate from port 20 (ftp-data).
connect_from_port_20=YES
# Add a banner when a user connects on the FTP server:
banner_file=/var/ftp/banner
#Force local user to stay within the folder they connected in
chroot_local_user=YES
# Listen for IPv4 sockets 
listen=YES
# Set own PAM service name to detect authentication settings specified  
# for vsftpd by the system package.  
pam_service_name=vsftpd
# Make sure that the folder designated in 'local_root' directive is not writable
local_root=/var/ftp/pub
#Minimum port for creating passive connections
pasv_min_port=40000
#Maximum port for creating passive connections
pasv_max_port=50000
# Create a whitelist of users which can access the ftp server
userlist_enable=YES
#Don't forget to create the file mentionned in the directive 'userlist_file' 
#and to add the user created earlier in it !
userlist_file=/etc/vsftpd.userlist
userlist_deny=NO
#Disable SELinux for vsftpd to avoid bugs 
seccomp_sandbox=NO
# Permissions applicated to any files and folder uploaded on the server.
file_open_mode=0777
# Remove executions rights from the uploaded folders and files
local_umask=0011
```

#### Create the banner
Use the following commands to create the banner which will be used by the server 
```
sudo touch /var/ftp/banner
sudo nano /var/ftp/banner #Write whatever you want in this file
```

#### Create the userlist
Use the following commands to create the userlist which will be used by the server 
```

sudo touch /etc/vsftpd.userlist
echo ftpuser >> /etc/vsftpd.userlist 
```
#### Configure the permissions
```
sudo chown root:root /etc/vsftpd.conf
sudo chown ftpuser:ftpuser -R /var/ftp/pub/home
sudo chmod 777 -R /var/ftp/pub/home
sudo chmod 555 /var/ftp/pub #Disable write access for anyone in the path /var/ftp/pub
```
#### Launching the service
```
sudo systemctl enable vsftpd #Allow the service vsftp to start on the machine startup
sudo systemctl disable vsftpd #Disallow the service vsftp to start on the machine startup
sudo systemctl restart vsftpd #Restart the vsftpd service after you change its configuration
```
If everything went well, you should have access to a fully functional FTP service on which you can upload anything you want !
#### Accessing your local FTP server
Personnaly, I decided to use the FTP client called [Filezilla](https://filezilla-project.org/). It is free, under GNU/GPL license and can be installed on any platforms such as Windows, Linux... Just download the software from the official website,  install it on your virtual machines and you're good to go !

### Firewall
#### Configuration of the firewall
Before you can access your services, make sure that the firewall installed on your PC allows any connections from the virtual machines to your host machine. 
You just need to open the ports 80,20,21 and the range 40000-50000 on your host machine and you should be fine.

### Troubleshooting
#### Network
##### I can't access any of my services !

Please verify the following points :
- Make sure that the service you want to access is running
- Make sure that the firewall doesn't block incoming and outgoing connections on the host machine
	- Make sure that the firewall configuration is persistent, even after the restart of the host machine

#### FTP server
##### I don't have access to the uploaded files and folder from my host machine with my main account !
Run this command to add your main account in the same group as the user used for the ftp server : 
```
sudo usermod -a -G ftpuser $(whoami)
```
From now on, you should have access to all files and folder which will be created in the path /var/ftp/pub/home directory. 

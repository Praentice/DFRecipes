# Wireshark
The aim of this page is to give tips and tricks regarding the analysis of pcap file with Wireshark.
## Operands
Use the following operand
-   and - operator: and / &&
-   or - operator: or / ||
-   equals - operator: eq / ==
-   not equal - operator: ne / !=
-   greater than - operator: gt /  >
-   less than - operator: lt / <
### Decrypt a secure protocol with the key file/data
To decrypt any secure protocol, you will need the key used during the ciphering operation and configure Wireshark to use them by following these steps : 

### Decrypt a secure protocol without the key file/data
In case you have captured network traffic but you don't have access to used key to cipher the network stream, you can try the following tricks based on the protocol you're facing to attempt its deciphering.
#### HTTPS

### Focus on any network stream
In case you have a lot of network exchanges, you can follow these steps if you want to review only one network exchange :
1) Right-click on a one of the network packet. Then select the option "Follow "
### 
In case you have a lot of differents network protocols, you can use this tool to get a percentage usage of each protocol in the network capture
1) Click 
## General filters
The aim of this section is to give ready-to-use filters to help you investigate any network capture. 
### ICMP Protocol
#### Basic filters
##### Display only ICMP
```
icmp
```
#### Detect anomalous traffic
##### Detect ICMP tunnel
```
icmp and data.len > 64
```
### ARP Protocol
#### Basic filters
##### Display only ARP
```wireshark
arp
```
##### Get only ARP requests
```wireshark
arp.opcode==1
```
##### Get only ARP response
```
arp.opcode==2
```
#### Detect anomalous traffic
##### Detect ARP flooding
```wireshark
((arp) && (arp.opcode == 1)) && (arp.src.hw_mac == target-mac-address) #First filter
(arp.opcode == 1) && (arp.src.hw_mac == target-mac-address) #Second filter
```

##### Detect ARP spoofing / poisoning
```wireshark
arp.duplicate-address-detected or arp.duplicate-address-frame #First filter
```
### MAC address
#### Filter MAC address by vendor
```
eth.src[0:3]=00:00:00 #Replace 00:00:00 by the vendor mac address
```
### IP protocol
### Display only IP packets
```
ip
```
#### filter by TTL
```
ip.ttl < value
```
### Layer 3 protocol
#### UDP protocol
##### Filter by port
```
udp.port==????
```
#### TCP protocol
##### Three way handshakes

##### Only TCP SYN
```
tcp.flags.syn==1 && tcp.flags.ack==0
```
##### Only TCP SYN/ACK
```
tcp.flags.syn==1 && tcp.flags.ack==1
```
##### Filter by port
```
tcp.port==????
```
##### Only get bad checksum TCP packets
```
tcp.checksum.status=="bad"
```
### Layer 4 protocol
#### DHCP
##### Search DHCP traffic based on specific hostname
```
dhcp.options.hostname contains "hostname"
```
##### Search a specific IP address which was requested
```
dhcp.option.type == 50 and dhcp.option.requested_ip_address == ip_address
```

#### DNS
##### Basic filter
A : 1 
##### Only get DNS query of certain type
```
dns.qry.type==1 && dns.flags.response==1
```
##### Detect anomalous traffic
###### Detect DNS tunnel
```
dns contains "dnscat" #Detect usage of dnscat tool
dns.qry.name > 15 and !mdns #Detect very long DNS query / response
```
#### FTP
##### Search packets by code
Here is the most useful codes used by the FTP protocol that you will need to filter packets 

-   **x1x series:** Information request responses.
-   **211:** System status.
-   **212:** Directory status.
-   **213:** File status (Can be used to know the size of file requested by user)

-   **x2x series:** Connection messages.
-   **220:** Service ready.
-   **226:** Transfer complete.
-   **227:** Entering passive mode.
-   **228:** Long passive mode.
-   **229:** Extended passive mode.

-   **x3x series:** Authentication messages.
-   **230:** User login.
-   **231:** User logout.
-   **331:** Valid username.
-   **430:** Invalid username or password
-   **530:** No login, invalid password. (Can be used to count number of attempts )
```
ftp.response.code==230
```
##### Search packets by request command
Here is the most useful codes used by the FTP protocol that you will need to filter packets 

-   **USER:** Username.
-   **PASS:** Password.
-   **CWD:** Current work directory.
-   **LIST:** List.
-   **STOR:** Store file on server.
##### Search packets by filename
```
ftp.request.arg == "your.file"
```
##### Request command
```
ftp.request.command==USER #Get request where username is sent
ftp.request.command==PASS #Get request where password is sent

```
##### Detect anomalous traffic
```
(ftp.response.code == 530) and (ftp.response.arg contains "username") # Detect bruteforce
`(ftp.request.command == "PASS" ) and (ftp.request.arg == "password")` # Detect bruteforce with static password
```
#### HTTP / HTTPS
-   **200 OK:** Request successful.
-   **301 Moved Permanently:** Resource is moved to a new URL/path (permanently).
-   **302 Moved Temporarily:** Resource is moved to a new URL/path (temporarily).
-   **400 Bad Request:** Server didn't understand the request.
-   **401 Unauthorised:** URL needs authorisation (login, etc.).
-   **403 Forbidden:** No access to the requested URL. 
-   **404 Not Found:** Server can't find the requested URL.
-   **405 Method Not Allowed:** Used method is not suitable or blocked.
-   **408 Request Timeout:**  Request look longer than server wait time.
-   **500 Internal Server Error:** Request not completed, unexpected error.
-   **503 Service Unavailable:** Request not completed server or service is down.

-   **User agent:** Browser and operating system identification to a web server application.
-   **Request URI:** Points the requested resource from the server.  
-   **Full *URI:** Complete URI information.

-   **Server:** Server service name.  
-   **Host:** Hostname of the server
-   **Connection:** Connection status.  
-   **Line-based text data:** Cleartext data provided by the server.
-   **HTML Form URL Encoded:** Web form information.
##### Filter by needed domain name 
```
http.host=example.com
```
##### Filter by request method
Here's the list of HTTP method : 
POST, GET, HEAD, OPTIONS, PUT, DELETE
```
http.request.method==GET #Get only GET requests
```
##### Sniff any valuable informations
```wireshark
http.request.method==POST and http.request.full_uri="target-url/login.php" # For credentials
http.request.method==POST and http.request.full_uri="target-url/comment.php" # For comments
```
##### Detect MITM attack after ARP flooding / poisoning / spoofing
```
http.request.method==POST and http.request.full_uri="target-url/login.php" and eth.addr==target-mac-adress #Detect POST request being redirected to a suspect MAC address
```
#### Netbios 
##### Only get Netbios packets
```
nbns
```
##### Search netbios packets related to an host
In case you have trouble finding the computer you want, try to find a pattern within the netbios packets (NetBIOS Name Service > Queries > Name) which could help you to filter the packets until you find the right one.
```wireshark
nbns.name contains "hostname" 
```
## Ressources
https://www.wireshark.org/docs/dfref/f/ftp.html
https://www.wireshark.org/docs/wsug_html_chunked/ChWorkBuildDisplayFilterSection.html
# Wireshark
The aim of this page is to give tips and tricks regarding the analysis of pcap file with Wireshark.
## Operands
??? info "Wireshark operators"

	|Word operator|Symbol operator|Description|
	|-|-|-|
	| and | && | Boolean operator "and". Needs the two conditions to be true|
	| or | \|\| |Boolean operator "or" . Needs one of the two conditions to be true |
	| eq | == | Needs the two conditions to be equal |
	| ne | != | Needs the two conditions to not be equal|
	| gt | > | Needs the conditions at the left to be greater than the condition at the right |
	| lt | < | Needs the conditions at the left to be lesser than the condition at the right |

### Decrypt a secure protocol with the key file/data
To decrypt any secure protocol, you will need the key used during the ciphering operation and configure Wireshark to use them by following these steps : 

### Decrypt a secure protocol without the key file/data
In case you have captured network traffic but you don't have access to used key to cipher the network stream, you can try the following tricks to attempt the deciphering of the network capture.
#### Recognize the security protocol in usage
Recognizing the security protocol used during the exchange will allow you to apply specific techniques which might help you in your quest, you should do the following steps : 

- Retrieve the network protocol used during the exchange. Is this Kerberos ? HTTPS ? RDP ? 
- Once you know the protocol, try to find the packets in which, the client and server might exchange cryptographic data to establish the connection. It could be a "Client Hello" packet for example. This will allow you to get the version of the security protocol used for the exchange.
- Once you know the security protocol version, research online the version to see if it is not affected by any security flaws. It could be a CVE or even a security flaw in its implementation. Don't hesitate to specify with the version number of the security protocol, the network protocol which was using it during the exchange to get more results.  


### Focus on any network stream
In case you have a lot of network exchanges, you can follow these steps if you want to review only one network exchange :

1) Right-click on a one of the network packet. Then select the option "Follow "

## Cheatsheet Wireshark filters

The aim of this section is to give ready-to-use filters to help you investigate any network capture. 

### Layer 2 protocol
#### ARP Protocol
##### Basic filters

??? info "Common filter for ARP traffic"

    === "Only display ARP traffic"
		```
		arp
		```
    === "Get only ARP requests"
		```
		arp.opcode==1
		```
    === "Get only ARP response"
		```
		arp.opcode==2
		```

##### Advanced filters

??? info "Detect abnormal ARP traffic"

    === "Detect ARP flooding"
		```
		((arp) && (arp.opcode == 1)) && (arp.src.hw_mac == target-mac-address)
		```
		```
		(arp.opcode == 1) && (arp.src.hw_mac == target-mac-address)
		```
    === "Detect ARP spoofing / poisoning"
		```
		arp.duplicate-address-detected or arp.duplicate-address-frame
		```
		
#### MAC address
##### Basic filters
??? info "Common filter for MAC address"

    === "Filter MAC address by vendor"
        Replace 00:00:00 by the vendor mac address
        ```
        eth.src[0:3]=00:00:00
        ```
    === "blabla2"
        Lorem Ipsum
    === "blabla3"
        Lorem Ipsum
##### Advanced filters
??? error "To do"

    === "To do"
        ```
        To do
        ```


### Layer 3 protocol
#### IP address
##### Basic filters
??? info "Common filter for IP address"

    === "Display only IP packets"
        ```
        ip
        ```
    === "Filter by TTL"
        Replace 00:00:00 by the vendor mac address
        ```
        ip.ttl < value
        ```

##### Advanced filters
??? error "To do"

    === "To do"
        ```
        To do
        ```


#### ICMP Protocol
##### Basic filters

??? info "Common filter for ICMP traffic"

    === "Only display ICMP traffic"
		```
		icmp
		```

##### Advanced filters
??? info "Detect abnormal ICMP traffic"

    === "Detect ICMP tunnel"
		```
		icmp and data.len > 64
		```

### Layer 4 protocol
#### UDP
##### Basic filters

??? info "Common filter for UDP traffic"

    === "Only display UDP traffic"
		```
		udp
		```
    === "Filter by UDP port"
		```
		udp.port==?????
		```

##### Advanced filters
??? error "To do"

    === "To do"
        ```
        To do
        ```


#### TCP
##### Basic filters

??? info "Three way handshake"

    === "Only display TCP SYN"
		```
		tcp.flags.syn==1 && tcp.flags.ack==0
		```
    === "Only display TCP SYN/ACK"
		```
		tcp.flags.syn==1 && tcp.flags.ack==1
		```

??? info "Common filter for TCP traffic"

    === "Filter by TCP port"
		```
		tcp.port==?????
		```
    === "Only display bad checksum TCP packets"
		```
		tcp.checksum.status=="bad"
		```
##### Advanced filters
??? error "To do"

    === "To do"
        ```
        To do
        ```

### Layer 5 -> 7 protocol
#### DHCP
##### Basic filters

??? info "Common filter for DHCP traffic"

    === "Search DHCP traffic based on specific hostname"
		```
		dhcp.options.hostname contains "hostname"
		```
    === "Search a specific IP address which was requested"
		```
		dhcp.option.type == 50 and dhcp.option.requested_ip_address == ip_address
		```

##### Advanced filters
??? error "To do"

    === "To do"
        ```
        To do
        ```

#### DNS
##### Basic filters
??? info "Common filter for DNS traffic"

    === "Only get DNS query of certain type"
		```
		dns.qry.type==1 && dns.flags.response==1
		```
    === "Search a specific IP address which was requested"
		```
		dhcp.option.type == 50 and dhcp.option.requested_ip_address == ip_address
		```
##### Advanced filters
??? info "Detect abnormal DNS traffic"

    === "Detect DNS tunnel"
	    Detect dnscat tool usage in capture
		```
		dns contains "dnscat"
		```
		Detect very long DNS query / response 
		```
		dns.qry.name > 15 and !mdns
		```

##### Resources
??? info "DNS query types"
	|DNS query type|Decimal value|Description|
	|-|-|-|
	|A|1|System status.|

#### FTP
##### Basic filters
??? info "Common filters for FTP traffic"

    === "Only display FTP traffic"
		```
		ftp
		```
??? info "Search filters"

    === "Search FTP traffic by code"
		```
		ftp.response.code==???
		```
    === "Search packets by request command
	    Get request where username is sent
		```
		ftp.request.command==USER
		```
	    Get request where password is sent
		```
		ftp.request.command==PASS
		```
    === "Search packets by filename"
		```
		ftp.request.arg == "your.file"
		```
##### Advanced filters
??? info "Detect abnormal FTP traffic"

    === "Detect bruteforce"
	    Detect bruteforce with failed login attemps.
		```
		(ftp.response.code == 530) and (ftp.response.arg contains "username")
		```
		Detect bruteforce with static password
		```
		(ftp.request.command == "PASS" ) and (ftp.request.arg == "password")
		```
##### Resources
Use the following informations to modify the given filters for your own needs.
??? info "FTP Code"
	|FTP Code|Description|
	|-|-|
	|211| System status.|
	|212| Directory status.|
	|213| File status (Can be used to know the size of file requested by user)|
	|220| Service ready.|
	|226| Transfer complete.|
	|227| Entering passive mode.|
	|228| Long passive mode.|
	|229| Extended passive mode.|
	|230| User login.|
	|231| User logout.|
	|331| Valid username.|
	|430| Invalid username or password|
	|530| No login, invalid password. |
??? info "FTP Command"
	|FTP Command|Description|
	|-|-|
	|USER|Username|
	|PASS|Password| 
	|CWD|Current work directory|
	|LIST|List the file on the server|
	|STOR|Store file on the server|
#### HTTP / HTTPS
##### Basic filters
??? info "Common filters for HTTP traffic"

    === "Only display HTTP traffic"
		```
		http
		```
??? info "Search filters"

    === "Filter by requested domain name"
		```
		http.host=example.com
		```
    === "Filter by request method"
	    Replace ?????? by the chosen HTTP method name
		```
		http.request.method==??????
		```
	    Get request where password is sent
		```
		ftp.request.command==PASS
		```

##### Advanced filters
??? info "Detect abnormal HTTP traffic"

    === "Detect MITM attack after ARP flooding / poisoning / spoofing"
	    Detect POST request being redirected to a suspect MAC address
		```
		http.request.method==POST and http.request.full_uri="target-url/login.php" and eth.addr==target-mac-address
		```

??? info "Grab valuable informations from HTTP traffic"

    === "Get credentials"
		```
		http.request.method==POST and http.request.full_uri="target url/login.php"
		```
    === "Get comments"
		```
		http.request.method==POST and http.request.full_uri="target-url/comment.php"
		```
##### Resources
??? info "HTTP Code"
	|HTTP Code|Description|
	|-|-|
	|200 (OK)|Request successful.|
	|301 (Moved permanently)|Resource is moved to a new URL/path (permanently)| 
	|302 (Moved Temporarily)  |Resource is moved to a new URL/path (temporarily).|
	|400 (Bad Request) | Server didn't understand the request.|
	|401 (Unauthorised) |URL needs authorisation (login, etc.).|
	|403 (Forbidden) |No access to the requested URL. |
	|404 (Not Found)| Server can't find the requested URL.|
	|405 (Method Not Allowed) |Used method is not suitable or blocked.|
	|408 (Request Timeout)  |Request look longer than server wait time.|
	|500 (Internal Server Error) | Request not completed, unexpected error.|
	|503 (Service Unavailable)  |Request not completed server or service is down.|
??? info "HTTP Parameters"

	-   **User agent:** Browser and operating system identification to a web server application.
	-   **Request URI:** Points the requested resource from the server.  
	-   **Full *URI:** Complete URI information.
	-   **Server:** Server service name.  
	-   **Host:** Hostname of the server
	-   **Connection:** Connection status.  
	-   **Line-based text data:** Cleartext data provided by the server.
	-   **HTML Form URL Encoded:** Web form information.

#### SMTP
##### Basic filters

??? info "Common filter for SMTP traffic"

    === "Only display SMTP traffic"
		```
		smtp
		```

??? info "Search filter"

    === "Search SMTP traffic based on status code"
		```
		smtp.response.code=???
		```

##### Advanced filters
??? error "To do"

    === "To do"
        ```
        To do
        ```
##### Resources
#### Netbios 
##### Basic filters
??? info "Only display Netbios traffic"

    === "Only display Netbios traffic"
		```
		nbns
		```
    === "Search netbios packets related to an host"
	    In case you have trouble finding the computer you want, try to find a pattern within the netbios packets (NetBIOS Name Service > Queries > Name) which could help you to filter the packets until you find the right one.
		```
		nbns.name contains "hostname" 
		```
##### Advanced filters
??? error "To do"

    === "To do"
        ```
        To do
        ```

## Resources
https://www.wireshark.org/docs/dfref/f/ftp.html
https://www.wireshark.org/docs/wsug_html_chunked/ChWorkBuildDisplayFilterSection.html
https://www.wireshark.org/docs/dfref/
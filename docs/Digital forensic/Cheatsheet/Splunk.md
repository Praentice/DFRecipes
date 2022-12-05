# Splunk
The aim of this page is to give tips and tricks regarding the usage of Splunk for forensic purposes

**WARNING !**

Before you send any query to Splunk, make sure that you specify all of the timeline or else, you will not be able to apply your query on the entire splunk database.

## Get the entire database
```splunk
index="your_index" 
```
## Syntax
### Basic operators
Splunk use the following operators to allow its users to perform various requests on its database. 


### Advanced operators

## Network Logs
### Sort by MAC address

=== "Sort by MAC address source"
    ``` splunk
    index="tp_ecole" 
    layers.ip_src="172.16.32.24" 
    ```
=== "Sort by MAC address destination"
    ``` splunk
    index="tp_ecole" 
    layers.ip_dest="172.16.32.24" 
    ```

### Sort by IP address

=== "Sort by IP source"
    ``` splunk
    index="tp_ecole" 
    layers.ip_src="172.16.32.24" 
    ```
=== "Sort by IP destination"
    ``` splunk
    index="tp_ecole" 
    layers.ip_dest="172.16.32.24" 
    ```

### Sort by transport protocol
#### Protocol name

=== "Sort by UDP transport protocol"
    ``` splunk
    index="tp_ecole" 
    layers._ws_col_Protocol="UDP"
    ```

=== "Sort by TCP transport protocol"
    ``` splunk
    index="tp_ecole" 
    layers._ws_col_Protocol="TCP" 
    ```

#### Protocol flags

```splunk
index="tp_ecole" 
"layers._ws_col_Info"="[SYN, ACK]"
```

Here's a list of flags you can use to create your own request : 

??? info "TCP Flags"
	|Flags|Usage|
	|-|-|
	|*[SYN]*| Only detect connections attempt to a server (First packet of a TCP handshake)|
	|*[SYN, ACK]*|Only detect server responses to connections attempt from clients (Second packet of a TCP handshake)|



### Sort by network port

=== "Sort by source port "
    ``` splunk
    index="tp_ecole" 
    layers.tcp_srcport="22"
    ```
=== "Sort by destination port"
    ``` splunk
    index="tp_ecole" 
    layers.tcp_dstport="22" 
    ```

### Sort by L7 protocol
#### HTTP
##### Basic queries
=== "Only display HTTP packets "
    ``` splunk
    index="tp_ecole" 
    layers._ws_col_Protocol="HTTP"
    ```
=== "Filter by HTTP code"
    ``` splunk
    index="tp_ecole" 
    "layers.http_response_code"=200 
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

#### FTP
This section contains query and commands you can use on Splunk to query its database for the search of FTP packets.

##### Basic queries
=== "Only display FTP packets "
    ``` splunk
    index="tp_ecole" 
    layers._ws_col_Protocol="FTP"
    ```
=== "Filter by FTP code"
    ``` splunk
    index="tp_ecole" 
    "layers.ftp_response_code"=200 
    ```
=== "Filter by FTP command"
    ``` splunk
    index="tp_ecole"  
    ```

##### Advanced queries

##### Resources

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
	|USER|Send username to use for connection|
	|PASS|Send password to use for connection|
	|CWD|Get the current user working directory|
	|LIST|List the files and directory contained within a path (parsed as an argument of this command)|
	|STOR|Send file to the server|


## Resources

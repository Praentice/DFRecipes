# Process forensic
is page aims to give tricks regarding the analysis of process dump memory for forensic purposes.
## Get a process memory dump
### Windows
You can easily get the memory dump of a process by using these tools from Sysinternals : procdump and process hacker.
### Linux

### Android
You can easily get the memory dump of a process by using the frida tool
## For all process
### Get notable strings
```bash
strings mem.raw > destination_file.txt
```
### Dump AES keys
```
findaeskey mem.raw > aes_keys.txt
```
## Web navigator process
This sections aims to give tricks regarding the analysis of process dump from web navigator. 
### Get visited URL
```

```
### Get form for authentication
```

```

### Get TSL/SSL informations
#### Private key
#### Public key
#### Certificate

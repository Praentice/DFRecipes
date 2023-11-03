# RSA
This page aims to give commands regarding certificate, public key, privates key and more....
## What is RSA ? 
RSA is an assymetrical encryption created in 1977 by Ronald Rivest, Adi Shamir and Leonard Adleman.
It is widely used on the internet to secure connections between clients and server. It is used for example by the network protocol HTTPS, FTPS, SSH....
It can also be used to secure the exchange of symmetrical encryption key between two persons to encrypt a data exchange. 

## RSA Components
??? faq "Check if two RSA component match together"

	=== "Public key and private key"
		If the `diff` command give no output, you've got a match
	    ```
	    # Compute the public key from the private and output the result to the_public_key.pub file
		openssl rsa -in your_private.key -pubout > the_public_key.pub
		# Check if the generated public key and the key you found are the same. 
		diff the_public_key.pub the_public_key_you_want_to_test.pub
		```

	=== "Private key and CSR"
		If these two commands have the same output, you've got a match. 
	    ```
	    openssl pkey -in your_private.key -pubout -outform pem | sha256sum
	    openssl req -in CSR.csr -pubkey -noout -outform pem | sha256sum
		```
	=== "Private key and certificate"
		If these two commands have the same output, you've got a match. 
	    ```
	    openssl pkey -in your_private.key -pubout -outform pem | sha256sum
		openssl x509 -in certificate.crt -pubkey -noout -outform pem | sha256sum
		```
### Certificates

Certificates in RSA are used to authenticate a server during the connection. 

#### Extract public keys from CER/DER certificates

Use one of the following commands to extract public keys from certificates in differents formats.

=== "From CRT"
    ``` bash
    openssl x509 -pubkey -noout -in your_cert.crt  > pubkey.pem 
    ```
=== "From CER"
    ``` bash
    openssl x509 -inform der -in your_cert.cer -pubkey -noout > your_cert_pub_key.pem 
    ```
=== "From DER"
    ``` bash
    openssl x509 -inform der -in your_cert.der -pubkey -noout > your_cert_pub_key.pem 
	```
=== "From PEM"
    ``` bash
    openssl x509 -pubkey -noout -in your_cert.pem > your_cert_pub_key.pem 
    ```

#### Convert a certificate between formats
Use one of the following commands to convert a certificate between different formats.

=== "CRT -> PEM"
    ``` bash
    openssl x509 -in your_cert.crt -out your_cert.pem -outform PEM
    ```
=== "CER -> PEM"
    ``` bash
    openssl x509 -in your_cert.cer -out your_cert.pem
    ```
=== "DER -> PEM"
    ``` bash
    openssl x509 -in your_cert.der -out your_cert.pem
	```
=== "PEM -> DER"
    ``` bash
    openssl x509 -in your_cert.pem -out your_cert.der -outform DER
	```
=== "PEM -> CER"
    ``` bash
    openssl x509 -inform PEM -in your_cert.pem -outform DER -out your_cert.cer
	```
=== "PEM -> CRT"
    ``` bash
    openssl x509 -outform der -in your_cert.pem -out your_cert.crt
	```

### Public and private keys
#### Public keys
Public key in the RSA encryption are used to encrypt data before sending them to the server. It can be also used by the client to authenticate the server by checking the certificate signature 

#### Extract public key from certificate
```bash
openssl rsa -pubin -inform PEM -text -noout < certificate_publickey.pem #For PEM format
```
#### Read public exponent and modulus 
```bash
openssl rsa -pubin -inform PEM -text -noout < your_certificate_public_key.pem
```
#### Private key
Private key are used to decrypt data which are received by the server. It is also used by the server to sign a certificate to ensure its authenticity

## Breaking RSA
Even though this encryption algorithm is impossible to break without a quantum computer. However, if you're facing a case in which this algorithm wasn't properly implemented, you may try to check the following points to see if you can compute the private and public key used for the connection.

### Check if the key pair has been leaked
Sometimes, even though the key pair is impossible to break quickly, you can google the first lines of the public key or certificate (in PEM format) and see if the key pair was leaked somewhere.   

### Look out for the length of the generated key
For now, It is recommended to use a length of at least 2048 bits for the generation of a new key pair. 
Every key pair generated with a length inferior to 2048 bits are considered to be insecure. If you're facing a key pair considered insecure to this standard, you should proceed to the following actions :

1) Retrieve the modulus and exponent of your public key and see if the resulting number (modulus) have known factors. If it is the case, then you should be able to regenerate from there the private key.

2) If the factors of the modulus are unknown but the modulus is a very short number compared to the standard length used, you can try to compute the factors of the modulus by using this [script.](https://github.com/RsaCtfTool/RsaCtfTool) 

### Bruteforce encrypted private key
Sometimes, you may encouter an encrypted private key. That means that a passphrase or key file is required to decrypt the private key before being able to use it. However, you should try the following steps if you want to decrypt the key.

1) Search in the filesystem or the memory dump of the evidence to see if there is no trace of a password which could be linked to the encrypted private key file.

2) Try to use a bruteforce script like this [one](https://github.com/bwall/pemcracker) with a dictionnary such as rockyou.txt (Available in the directory `/usr/share/wordlists` for Kali Linux and Parrot OS). 

### Look out for bad implementations
Even though it is almost impossible to break a good encryption algorithm such as RSA, nothing beats a bad implementation of a cryptographic algorith. That means that you should try to have a look on the client or the server and see if the private key is readable or available somewhere. 

## Resources
https://cheapsslsecurity.com/p/convert-a-certificate-to-pem-crt-to-pem-cer-to-pem-der-to-pem/
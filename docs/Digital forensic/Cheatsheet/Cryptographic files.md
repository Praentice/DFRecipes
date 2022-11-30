# Cryptographic files
This page aims to give commands regarding certificate, public key, privates key and more....
## Certificates
### Extract public files from CER/DER certificates
```bash
openssl x509 -inform der -in your_certificate.cer -pubkey -noout > your_certificate_public_key.pem #If certificate in CER format (binary)
openssl x509 -inform der -in your_certificate.der -pubkey -noout > your_certificate_public_key.pem #If certificate in DER format (binary)
```
### Convert CRT to PEM:
```bash
openssl x509 -inform crt -in cert.crt -out cert.pem
```

### Convert CER to PEM
```bash
openssl x509 -inform cer -in cert.cer -out cert.pem
```

### Convert DER to PEM
```bash
openssl x509 -inform der -in cert.der -out cert.pem
```


## Public key
### Extract public key from certificate
```bash
openssl rsa -pubin -inform PEM -text -noout < certificate_publickey.pem #For PEM format
```

## Private key


## Ressources
https://cheapsslsecurity.com/p/convert-a-certificate-to-pem-crt-to-pem-cer-to-pem-der-to-pem/
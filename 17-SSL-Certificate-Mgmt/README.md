########################## Certificate ##################################

## API Server Certificate

1. Genrate Openssl Self Sign Key 
```
 openssl genrsa -out ocp-crc-custom-key.pem 2048
``` 

2. Genrate CSR Request with the above key
```
openssl req -new -key ocp-crc-custom-key.pem -out ocp-crc-custom-csr.pem
```

3. Sign the CSR Request with Key
```
openssl x509 -req -days 365 -in ocp-crc-custom-csr.pem -signkey ocp-crc-custom-key.pem -out ocp-crc-custom-crt.pem
```

4. Now Check the Certificate
```
openssl x509 -in ocp-crc-custom-crt.pem -text -noout
```


## Wild Certificate of Ingress Contoller Server Certificate

1. Genrate Openssl Self Sign Key 
```
openssl genrsa -out star-ocp-crc-custom-key.pem 2048
```
2. Genrate CSR Request with the above key
```
openssl req -new -key star-ocp-crc-custom-key.pem -out star-ocp-crc-custom-csr.pem
```

3. Sign the CSR Request with Key
```
openssl x509 -req -days 365 -in star-ocp-crc-custom-csr.pem -signkey star-ocp-crc-custom-key.pem -out star-ocp-crc-custom-crt.pem
```

4. Now Check the Certificate
```
openssl x509 -in star-ocp-crc-custom-crt.pem -text -noout
```


# Generating RSA
## Generating an RSA Private Key

- To generate an RSA key that ~~only~~ readable by the current user, use the option `-out` :
  
```bash
  openssl genrsa -out rsa.pri
```

![[Pasted image 20240330190404.png]]


## Generating a RSA Public Key

- To generate a public key you need your private key.
- run the command :
  
```bash
  openssl rsa -in rsa.pri -pubout -out rsa.pub
```

![[Pasted image 20240330190722.png]]

# Generating encrypted RSA Keys

## With Password prompt

- To generate an RSA encrypted key with password prompt, use the command :
  
```bash
  openssl genrsa -aes-256-cbc -out rsa.pri.enc
```

![[Pasted image 20240330191718.png]]


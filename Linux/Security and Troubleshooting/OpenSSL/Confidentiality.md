
- To encrypt a file you need a key.
- To generate a Secret Key in OpenSSL
- More in the YouTube [Video](https://www.youtube.com/watch?v=Azp_zDgFAGk&list=PLgBMtP0_D_afzNG7Zs2jr8FSoyeU4yqhi&index=3).
- More in the Arch Wiki [OpenSSL](https://wiki.archlinux.org/title/OpenSSL).
# Generating Keys

## Generating a random byte key

- To generate a random 10 byte key, use the following command :
  
```bash
  openssl rand -hex 10
```

- To generate a random 32 byte key, use the following command :

```bash
openssl rand -hex 32
```

- To generate a random 32 byte key and store it into a file called `encryption.key` :

```bash
openssl rand -hex -out encryption.key 32
```
![[Pasted image 20240329205540.png]]


## Generating a RSA Private Key

- To generate a normal RSA key, use the command :
  
```bash
  openssl genrsa
```
![[Pasted image 20240329201421.png]]

- You can generate a 1024 RSA key :
  
```bash
  openssl genrsa 1024
```

![[Pasted image 20240329201646.png]]

### output to a file

- You can save the RSA key to a file `key.pri` using this command :
  
```bash
  openssl genrsa > key.pri
```

- OR use the `-out` option :

```bash
openssl genrsa -out key.pri 2048
```


## Generating a RSA Public Key

- *Public Keys* are used to encrypt data using the private key.
- To generate a public key from a private key use this command :
  
```bash
  openssl rsa -in key.pri -pubout -out key.pub
```

![[Pasted image 20240329202539.png]]


# Listing supported Algorithms

- OpenSSL has a plenty of algorithms to use.

### Listing algorithms

- To list all the algorithms supported in OpenSSL, use the command :
  
```bash
  openssl list -digest-algorithms
```

### Listing Ciphers

- Use the command :
  
```bash
  openssl list -cypher-algorithms
```

### Listing public key algorithms

- Use the command :

```bash
openssl list -public-key-algorithms
```

### List the options for each encryption method 

- Lets say you want to know which options works for a particular encryption methode, like for example `aes-256-cbc` encryption, you run the command :
  
```bash
  openssl list -options aes-256-cbc
```

![[Pasted image 20240329204838.png]]

# Encrypting a File

- To encrypt a file, lets say `hey.txt`. First you need to enter the encryption methode (in this example we used the `aes-256-cbc` encryption). 
- Next you use the encryption key that you generated with the command `openssl rand -hex 32 -out encryption.key` 
  ![[Pasted image 20240329205554.png]]

- The Command looks like this :
  
```bash
  openssl aes-256-cbc -in hext.txt -out hey.enc -e -kfile encryption.key
```

![[Pasted image 20240329205745.png]]

- The `-e` option, indicated the *encryption* mode.

## Encrypting a File with password

- In the case you don't want to use the `encryption.key` and use a password instead, you can ignore the use of the option `-kfile`.

- This how the command would look like :
  
```bash
  openssl aes-256-cbc -in hey.txt -out hey.enc -e -a 
```

![[Pasted image 20240329212412.png]]

## Encrypting using Base64 and PBKDF2

- *PBKDF2 (Password-Based Key Derivation Function 2)* is an encryption methode to increase the security of the password and making it more resistant to brute-force attack.
- To encrypt the file using the Base64 encryption, we use the option `-a`.
- For the PBKDF2 we use the option `-pbkdf2` as an option :
  
```bash
  openssl aes-256-cbc -in hey.txt -out hey.enc -e -a -kfile encryption.key -pbkdf2
```

![[Pasted image 20240329215025.png]]


# Decrypting a File


- Now lets decrypt the File `hey.enc`.
- To do that run the command :
  
```bash
openssl aes-256-cbc -in hey.enc -out hey_dec.txt -d -kfile encryption.key
```

- The option `-d` indicates the *decryption* mode.

## Decrypt a File with password

- To decrypt the same file, use this command :
  
```bash
    openssl aes-256-cbc -in hey.enc -out hey.txt -d -a 
```

## Decrypting using Base64 and PBKDF2

- To decrypt this type of files :
  
```bash
openssl aes-256-cbc -in hey.enc -out hey_enc.txt -d -a -kfile encryption.key -pbkdf2
```



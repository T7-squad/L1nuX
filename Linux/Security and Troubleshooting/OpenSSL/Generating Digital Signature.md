
- To preserve integrity of your file, you can create a Digital signature with your private key, that can be verified from the owner of the public key.
- Lets generate a Digital Signature using the RSA Keys.

## Generating RSA encrypted DS with the private key

- To generate an RSA encrypted DS, you need a private key (look at the chapter before).
- Now enter the following commands :
  
```bash
  openssl rsautl -sign -inkey rsa.pri -in hey.txt -out hey.txt.sig
```

![[Pasted image 20240330192637.png]]

### Descrypt the File using the DS

- Now lets decrypt the `hey.txt` file on the other machine using the *public key* .
- Run the command :

```bash
openssl rsautl -verify -inkey rsa.pub -pubin -in hey.txt.sig
```

![[Pasted image 20240330192929.png]]

## Generating SHA1 encrypted DS with the private key

- Lets generate a SHA1 encrypted Digital signature.
- Enter the following command :
  
```bash
  openssl sha1 -sign key.pri -out hey.txt.sig hey.txt
```

### Verify the DC

- To verify the DC and if the file has not been altered, we enter the command :
  
```bash
    openssl sha1 -verify key.pub -signature hey.txt.sig hey.txt
```

![[Pasted image 20240331092450.png]]

> [!Note]
> 
> - If you modifed the file, you will get a verification error :
>   ![[Pasted image 20240331092541.png]]


## Generating SHA256 encrypted DS with the private key

- The same works with the SHA256 DS encryption.
- ![[Pasted image 20240331092703.png]]

# Test SSL connectivity to a webserver

- To test the connectivity of a web server for example `www.google.com` at port `443` , use the command : 
  
```bash
  openssl s_client -connect www.google.com:443

```

![[Pasted image 20240505132228.png]]
![[Pasted image 20240505132246.png]]


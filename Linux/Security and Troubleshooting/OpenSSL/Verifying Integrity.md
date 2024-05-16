- You can prove that a data was not altered by any ways.
- Every Website publishes the checksum of any software that has been downloaded. The Checksum serves as a proof of the integrity of a particular software (the software is not been modified)

> [!Important]
> - When sending a file between two persons, to preserve the integrity of the File, the 2 Persons needs to excange the Checksum of the file send.
> - It the file has been modified (by a hacker) or altered through hardware or network issues, the Checksum will *Change !*

# Checking Checksum

## Checking for MD5 Checksum

- Lets take the example of checking the integrity of the package `slapd`.
- we download the package from the website [slapd](https://packages.debian.org/de/sid/amd64/slapd/download).
- If we scroll down the website, we see the integrity of the Package :
  ![[Pasted image 20240329190344.png]]

- After downloading the Website, we can check the MD5 checksum of the package using the tool `md5sum` :
  ![[Pasted image 20240329190508.png]]
- If we compare it to the website, we found a match ! that means that the package is reliable !

## Checking for SHA1 Checksum 

- For the *SHA1* sum you can use the tool `sha1sum` :
  ![[Pasted image 20240329190723.png]]

## Checking for SHA-256 

- The same is made for the *SHA-256* Sum.
- In this case we use the command `sha256sum` :
  ![[Pasted image 20240329190900.png]]

- If we compare it to the sum in the website we find a match !

# Generating Checksum

- To generate a SHA-256 (.sha256) checksum of a file named `cat.txt` enter the following command :
  
```
  openssl sha256 -hex -out cat.sha256 cats.txt
```

- This will generate a `cat.sha256` checksum of the file `cat.txt`.
  ![[Pasted image 20240329192448.png]]


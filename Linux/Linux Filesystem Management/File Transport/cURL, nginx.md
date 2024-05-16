
# HTTP/S :

HTTPS use a encrypted Transit.
we have learned how upload a HTTP server using Python [[Python, php, perl, ruby]] [[wget, scp]] , [[HackerSploit Playlists]]. now we gonna learn more about #Apache and #Nginx .

# Nginx - Enabling PUT :

A bether way for transfering Files to Apache is #Nginx , its less complicated .

## Create a Directory to Handle Uploaded Files :

```shell
Thibovski@htb[/htb]$ sudo mkdir -p /var/www/uploads/SecretUploadDirectory
```

## Change the Owner to www-data :

```shell
Thibovski@htb[/htb]$ sudo chown -R www-data:www-data /var/www/uploads/SecretUploadDirectory
```

## Create Nginx Configuration File :

More About [Nginx_Configuration (Youtube)](https://www.youtube.com/watch?v=NEf3CFjN0Dg) 

Create the file ```/etc/nginx/sites-available/upload.conf``` and write the contents :

```Nginx
server {
    listen 9001;
    
    location /SecretUploadDirectory/ {
        root    /var/www/uploads;
        dav_methods PUT;
    }
}
```

## Symlink our Site to the sites-enabled Directory :

```Nginx
Thibovski@htb[/htb]$ sudo ln -s /etc/nginx/sites-available/upload.conf /etc/nginx/sites-enabled/
```

## Start Nginx :

```Nginx
Thibovski@htb[/htb]$ sudo systemctl restart nginx.service
```

To verify if Ngunx is startet enter the IP-Address in the Browser and you should see Nginx Home Page .
![image](Nginx.png)

### Verifying Errors :

```shell
Thibovski@htb[/htb]$ tail -2 `/var/log/nginx/error.log`

2020/11/17 16:11:56 [emerg] 5679#5679: bind() to 0.0.0.0:`80` failed (98: A`ddress already in use`)
2020/11/17 16:11:56 [emerg] 5679#5679: still could not bind()
```

```shell
Thibovski@htb[/htb]$ ss -lnpt | grep `80`

LISTEN 0      100          0.0.0.0:80        0.0.0.0:*    users:(("python",pid=`2811`,fd=3),("python",pid=2070,fd=3),("python",pid=1968,fd=3),("python",pid=1856,fd=3))
```

```shell
Thibovski@htb[/htb]$ ps -ef | grep `2811`

user65      2811    1856  0 16:05 ?        00:00:04 `python -m websockify 80 localhost:5901 -D`
root        6720    2226  0 16:14 pts/0    00:00:00 grep --color=auto 2811
```

### Upload File Using cURL :

More about #Curl in [[wget, scp]]

```shell
Thibovski@htb[/htb]$ curl -T /etc/passwd 
http://localhost:9001/SecretUploadDirectory/users.txt
```

```SHELL
Thibovski@htb[/htb]root@localhost# tail -1 /var/www/upload/SecretUploadDirectory/users.txt 

user65:x:1000:1000:,,,:/home/user65:/bin/bash
```

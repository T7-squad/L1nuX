- More about this subject in this [Link](https://webhostinggeeks.com/howto/how-to-enable-mod_ssl-apache-module-on-ubuntu/).

## Step 1 : Install `mod_ssl`

- Firs you need to install the module named `mod_ssl` for apache2, to do that run the command :
  
```bash
  sudo apt-get install libapache2-mod-ssl
```

## Step 3 : Enable `mod_ssl`

- After installing the module, you need to enable it with the command :
  
```bash
  sudo a2enmod ssl
```

## Step 4 : Set up SSL Certificate

- In this example, we should create a *self-signed certificate* for internal use (for external use you should consider getting a certificate by a trusted authority).
- To create a self-assigned Certificate :
  
```bash
  sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
```

This command will process a certificate request (`req`), non encrypted (`-x509`), with a password prompt (`nodes`), with a validity of `365` days and create a rsa key of 2048 bytes (`-newkey rsa:2048`) and then store the key under `/etc/ssl/private/apache-selfsigned.key` and the certificate in `/etc/ssl/certs/apache-selfsigned.crt`.

![[Pasted image 20240401174158.png]]

> [!Important]
> - All the certs are stored in the `/etc/ssl/serts` :
>   ![[Pasted image 20240401173100.png]]
> 
> - All the keys are stored in `/etc/ssl/private` :
>   ![[Pasted image 20240401173210.png]]


## Step 5: Configure Apache to Use SSL

- Now you need to configure Apache to use the certificate and the key.
- To change the config file, go to :
  
```bash
  sudo nano /etc/apache2/sites-available/default-ssl.conf
```

![[Pasted image 20240401174720.png]]

- Those are the paths to both the key and certificate.
- When you run `sudo a2ensite default-ssl`, Apache will create a symbolic link from the `sites-available/default-ssl.conf` file to the `sites-enabled/` directory, effectively enabling the SSL/TLS configuration for your Apache server. This allows Apache to serve HTTPS requests using the SSL/TLS settings specified in the `default-ssl.conf`
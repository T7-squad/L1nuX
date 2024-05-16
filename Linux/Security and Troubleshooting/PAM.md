- *PAM (Pluggable Authentication Modules)* is the authentication methode used in in Kerberos and LDAP.

- To view the list of the most used PAM modules, check out this [Link](https://docs.rockylinux.org/de/guides/security/pam/).
## PAM Configuration

- PAM configurations are located in the `/etc/pam.d` directory.
- You can change the PAM configuration in the `/etc/pam.conf`:
  ![[Pasted image 20240329113910.png]]

### Module Interfaces & Control Flags


- Each file the `/etc/pam.d` is formatted in the following :
  
```
<module interface> <control flag> <module name> <module arguments>
```

   - `<module interface>` : Defines the type of PAM group. they are 4 :
     
     - `auth` : handles user authentication (Kerberos)
     - `account` : manages accounts; checking permissions, account validity.
     - `password` : changing and updating passwords.
     - `session` : manages opening and closing sessions and setting up environments (home directory).

   - `<control flag>` : action that PAM should make based on the result of the module.
     
      - `required` : the module's success is required for the authentication.If the module fails, the user authentication fails *but other processes continue to run*. The user is notified if the module interface are finished.
      - `requisite` : the same as required, but if the module fails, PAM will immediately terminate and notify the user about the failure.
      - `sufficient` : if the module fails the authentication continues with other modules.
      - `optional` : Success and failure of the module, doesn't affect the authentication; module is ignored.

  - `<module name>` : the name of the module being invoked .
  - `<module arguments>` : configuration options and arguments for the module

- Example :
  
```bash
password    requisite   pam_pwquality.so retry=5 # Enforces password quality requirements and gives the user 5 chances to fail.

account    required     pam_access.so       # Access control based on login names, host, or service
```


### List of most common modules


1. **pam_unix.so**: Provides standard UNIX authentication functionality, including checking passwords in `/etc/passwd` and `/etc/shadow`.
    
2. **pam_ldap.so**: Allows authentication and authorization against an LDAP directory.
    
3. **pam_krb5.so**: Provides Kerberos authentication support.
    
4. **pam_google_authenticator.so**: Enables two-factor authentication using Google Authenticator.
    
5. **pam_ssh_agent_auth.so**: Allows SSH agent forwarding to be used for authentication.
    
6. **pam_pwquality.so**: Enforces password quality requirements, such as length, complexity, and history.
    
7. **pam_access.so**: Controls access based on host and user characteristics specified in `/etc/security/access.conf`.
    
8. **pam_limits.so**: Sets resource limits for users, such as maximum number of processes, maximum file size, etc.
    
9. **pam_faillock.so**: Implements login failure tracking and account locking.
    
10. **pam_mount.so**: Mounts user-specific volumes upon login.
    
11. **pam_mail.so**: Notifies users of new mail upon login.
    
12. **pam_exec.so**: Executes an external command or script during authentication.
    
13. **pam_unix_acct.so**: Performs additional account management tasks, such as checking password expiration.
    
14. **pam_tally2.so**: Tracks login attempts and can lock user accounts after a certain number of failed attempts.
15. **pam_nologin.so**: checks for the existence of the `/etc/nologin` file. If this file exists, it prevents non-root users from logging in.
16. **pam_cracklib.so**: s commonly used to enforce password policies by checking the strength of passwords against a set of rules defined in the CrackLib library.


### Password Policies


- In this example, the PAM prompts the user to enter a strong password (only for users locally) :
  
```bash
  password requisite pam_pwquality.so local_users_only
```

- In this example, PAM forces the user to not use old password (old passwords are remembered for 90 days) :
  
```bash
  password requisite pam_pwhistory.so remember=90
```


### User Lockouts


- To lock an account if many authentication attempts are failed, you use one of the module : `pam_tally2` and `pam_faillock`.
- You can place these user lockout directives in `/etc/pam.d/password-auth` and `/etc/pam.d/system-auth`.
- To unlock a user and reset their failure count, you can issue `pam_tally2 -r -u user`


### LDAP Integration


- You can configure PAM to use LDAP by going through this steps.
- More about [LDAP](https://ubuntu.com/server/docs/service-ldap).

1. Install LDAP Client Libraries.
   
```
   sudo apt install slapd ldap-utils
```

2. Install PAM LDAP Module :
   
```
   sudo apt-get install libpam-ldap
```

3. Configure LDAP Authentication :
   
   make a file named `/etc/pam.d/common-auth`, and add this lines :
   
```
   auth    required        pam_ldap.so     # LDAP authentication
```

4. Configure the LDAP client :
   
   
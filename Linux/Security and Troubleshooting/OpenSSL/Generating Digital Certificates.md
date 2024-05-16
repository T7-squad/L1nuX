# Generating a Self-signed Certificate

- Enter the command :
  
```bash
openssl req -x509 -key rsa.pri -sha256 -days 365 -out test.cer
```

![[Pasted image 20240331094609.png]]

## View the Certificate

- To view the certificate, enter the following command :
  
```bash
  openssl x509 -in test.cer -noout -text
```

![[Pasted image 20240331095334.png]]

> [!NOTE]
> - Self assigned Certificate have the Issuer and Subject


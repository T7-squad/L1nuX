## Adding an SSH-Key

1. Create a key corresponding to your Email-Address :

```Shell
ssh-keygen -t ed25519 -b 4096 -C "your_email@gmail.com"
```

This will create an ssh-key with the encryption `ed25519` in `4096` bits
After you will be prompt to enter your password :

```shell
Enter file in which to save the key (/home/magellan/.ssh/id_ed56519): 
```

2. Add you key to `~/.shh` :

```shell
 ssh-add ~/.ssh/id_ed56519
```

3. Now youl find you key `id_ed56519.pub` under `~/.ssh` , copy the key and insert it in the github repo. and your done !
4. Go back to your created repo, copy the ssh path, and pase it in you directory :

```shell
git clone ..... .git
```
Now you have the directory in your machine !


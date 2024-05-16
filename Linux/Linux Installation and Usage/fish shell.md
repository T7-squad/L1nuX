
### Change your Default Shell 

- Lets say you want to set a new shell like `fish` as a default shell.
- first install the shell with the command : `sudo pacman -S fish`
- second, locate where you shell is with the command `whereis fish`
- After you find the Path to your shell, ex `/usr/share/fish` you need to add the path to the `/etc/shells` .
- After you change your Shell with `sudo chsh -s /usr/share/fish magellan`. Now you have the fish shell as default shell.
- If you want to add neofetch to your Terminal start :
	- go to `~/.config/fish/` and add `neofetch` at the end of the script

Results :

![image](shell.png)



Method 1 : Restore the lost Path

### Set New Path in the fish shell :

- go to '`~/.config/fish/config.fish` and add this :

```Python
set -gx fish_user_paths $fish_user_paths /home/mag311an/.local/bin /usr/local/bin /usr/bin /bin /usr/local/sbin /opt/cuda/bin /opt/cuda/nsight_compute /opt/cuda/nsight_systems/bin /usr/bin/site_perl /usr/bin/vendor_perl /usr/bin/core_perl /var/lib/snapd/snap/bin
```

- now you need to save the changes :

`source ~/.config/fish/config.fish`


Methode 2 : For Softwares

### Utilize the build in `fish_add_path` function :

```Python
fish_add_path /opt/mycoolthing/bin
```

will add the path `/opt/mycoolthing/bin` automaticly !

https://fishshell.com/docs/current/cmds/fish_add_path.html

Methode 3 :

- If nothing works, type this command direct on the shell :

```
set -gx hla /usr/bin/hla
set -gx hlalib /usr/bin/hla/hlalib
set -gx hlainc /usr/bin/hla/include

```

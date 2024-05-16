# X Windows

- X Windows provides the ability to run GUI interfaces on the top of the system hardware.
- X Windows uses *X Server* to display the graphical GUI on the display hardware.
- *X.org* and *Wayland* are two types of the X Windows 

# Window Managers and Desktop Environments

- To modify the look and feel of the X Windows, you can use a *window manager*.
- Here are some examples :
![[Pasted image 20240220183820.png]]

- The Window Manager imployed in Gnome is *Mutter* :
  ![[Pasted image 20240220191723.png]]

- *XFCE* ist the most lightweight Dekstop Environment.

## Display Informations about the Login Sessions

- To print out which display manager is been used, run the command :
  
```bash
  echo $WAYLAND_DISPLAY
```

> [!NOTE]
> If you get an information back, you are using the Wayland window manager, if not you are using the x11 !

- Another methode is to run the command :
  
```
  loginctl 
```
![[Pasted image 20240415165931.png]]

 This will print out all the information about the current session (session number) :
 ![[Pasted image 20240415165854.png]]

If you want to print more about the current session, select the `session_id` and run the command :

```
sudo loginctl show-session session_id --all
```
![[Pasted image 20240415170110.png]]

Here you can say that the Desktop Environement used is `KDE` and the Window Manager is `X11`.

- To enable X11 via SSH Look at [[Chapter 2]].
# Starting and Stopping X Windows


- When the init daemon starts the runlevel5 *graphical.target* which is a program called *gdm* or (*GNOME Display Manager*), the gdm starts the display by chowing the login screen.
- You can then choose your Display Manager :
  
  ![[Pasted image 20240220192209.png]]


> [!NOTE] Note
> The Gnome Display Manager includes *KDE Display (KDM)*, *SDDM* and *LightDM*.

- To start the GNOME Display Manager, enter the command `startx`.

- To install the *KDE* Desktop Environement run the command :
  
```
  sudo apt-get install kde-full
```


# Access the X-Window using VNC

- Like `xfreerdp` there is another tool called *vnc*, that enables you to remote control a device.
- Install vnc with :
  
```bash
sudo apt install tigervnc-standalone-server tigervnc-common
```

- Once you connected via SSH to your remote Machine you can run the command :
  
```
  vncserver :1
```

Know you will get the Display Manager on Screen.

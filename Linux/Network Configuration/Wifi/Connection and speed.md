
- They are two type of Internet Diagnose :
  - Speed of the ==Server/Client==.
  - Speed of the ==Client/Internet==.

# `speedtest-cli` and `iperf`

## Client/Server Diagnose

- To diagnose the Client/Server Connection, or in other way, Client/Client (2 PC's in the same Network) we use the command `iperf` :
  ![[Pasted image 20240408120654.png]]

- To check the internet speed between devices in the same network :
  
### Client Side

- In the client side, you run the command :
  
```bash
  iperf -c server_address 
```

### Server Side

- On the Server side, you run the command :
  
```bash
  iperf -s client_address
```


## Client/Internet

- To check and diagnose you Internet Connection, use the command `speedtest-cli`.
- You can install the package with :
  
```
  sudo apt-get install speedtest-cli
```

![[Pasted image 20240408121059.png]]

- You can know run the command :
  
```
  speedtest-cli --share
```

![[Pasted image 20240408121212.png]]

# Bandwidth with `iftop` and `nload` 

## `iftop` 

- To install the tool :
  
```
  sudo apt-get install iftop
```

- The `iftop` command enables you to determine the bandwidth of your internet connection, and identify the traffic going out and in.
  ![[Pasted image 20240408153652.png]]

- More to `iftop` :
  ![[Pasted image 20240408153720.png]]

## `nload`

- `nload` display a statistic output of the Network traffic ( Speed, Bandwidth ...) 
- You need to install the tool first :
  
```bash
  sudo apt-get install nload
```

![[Pasted image 20240408154051.png]]


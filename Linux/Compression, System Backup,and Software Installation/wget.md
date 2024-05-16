### Download a Directory from http.server :

- To download a Directory from http page without the html ending :

```SHELL
wget -r -np -R "index.html*" -i path.txt (or URL)
```

  
- Pause and Resume wget Download :

	- To cancel a download press [CTL-C].

	- to resume a download add the ```-c``` Flag to the command :
		```SHEL 
		wget -c 'URL'
		```


---
Engineer Man
---

### Ssh reverse proxy?
https://www.youtube.com/watch?v=mV_8GbzwZMM&list=RDCMUCrUL8K81R4VBzm-KOYwrcxQ&start_radio=1

### Collection of videos
https://www.youtube.com/watch?v=mV_8GbzwZMM&list=RDCMUCrUL8K81R4VBzm-KOYwrcxQ&start_radio=1


---
Brad Traversy
---

### Docker sheat cheet
https://gist.github.com/bradtraversy/89fad226dc058a41b596d586022a9bd3

---
MIXED SOURCES
---

### Regex visualizator
https://www.debuggex.com/

### Detect, sweep and scan your network in bash
https://www.youtube.com/watch?v=qhCxKrU1AEY

### Ssh reverse tunneling
https://jfrog.com/connect/post/reverse-ssh-tunneling-from-start-to-end/

### Cut log files
https://www.youtube.com/watch?v=L2BFDyYknIg


### My notes
Remove all files starting with a digit

```
rm [0-9]*
```

Remove all files with certain letter combinations

```
rm x[abcdefghijklmnop]*
```

Loop through all files in directory, read the first word in the first line of the file, and save the file as this word

```
for f in *; do cat "$f" | tee $(head -n 1 $f | cut -d " " -f 1); done
```

First line in file, put in variable
```
line=$(head -n 1 filename)
```

Create a .tgz file from a directory

```
tar -cvzf MBusLOG.tgz log/
```

Extract .tgz file

```
tar -xf MBusLOG.tgz
```

Search through files find lines without the word "Timeout"

```
cat [0-9]* | grep -v "Timeout"
```

### Half open scan, check all open ports on all devices on the subnet.
### Syn -> <- Syn ACk without the third Ack handshake in TCP. Stealth mode

```
sudo nmap -sS -p 80,443 192.168.10.0/24
```

### See all open ports on a specific device

```
sudo nmap -sT <ip-address>
```

### Analyse a unit on the network

```
sudo nmap -A <ip-address>
```

---
POWERSHELL
---

Scan the network for units answering on ping

```
1..254 | ForEach-Object {Get-WmiObject Win32_PingStatus -Filter "Address='192.168.0.$_' and Timeout=200 and ResolveAddressNames='true' and StatusCode=0" | select ProtocolAddress*}
```





### Flask, Vue, MySQL

```
https://www.youtube.com/watch?v=nCMB5p74WJQ
```

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

### Flask, SQLAlchemy, Marshmallow

```
https://www.youtube.com/watch?v=PTZiDnuC86g
```

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

### A better cryptographic type ed25519 
### -C is for comment
### Copy the public key to a remote server

```
ssh-keygen -t ed25519 -C "mats-laptop"
ssh-copy-id -i ~/.ssh/<public-key> <user>@<host>

ssh -i ~/.ssh/<public-key-file> <user>@<host>
```

### Start the SSH-agent and add your private ssh-keys

```
eval $(ssh-agent)

ssh-add ~/.ssh/<the-private-key>
```

### SSH config file ~/.ssh/config
### It is the private keys

```
Host acme
  Hostname 172.105.24.117
  IdentityFile ~/.ssh/acme_key
    
Host skynet
  Hostname skynet.learnlinux.cloud
  User admin
  IdentityFile ~/.ssh/skynet_key
    
Host shinra
  Hostname shinra.learnlinux.cloud
  User root
  Port 2222
  IdentityFile ~/.ssh/shinra_key
```

### Add user to sudo group

```
usermod -aG sudo <user-name>
```

### Cut or remove characters from input

```
tr -d <input-string>
```

### Cut and replace delimiter " " with %

```
cut -d " " -f 1,2 state.txt --output-delimiter='%'
```

### Sed --> -e stands for expression --> /s stands for substitution
### 

```
echo 'hello world' | sed -e ''
echo 'hello world' | sed -e 's/world/mars'
```

### Remove all files starting with a digit

```
rm [0-9]*
```

### Remove all files with certain letter combinations

```
rm x[abcdefghijklmnop]*
```

### Loop through all files in directory, read the first word in the first line of the file, and save the file as this word

```
for f in *; do cat "$f" | tee $(head -n 1 $f | cut -d " " -f 1); done
```

### Create an alias for your terminal. E.g Copy and paste the following command to your shell’s profile (.profile, .bashrc, .zprofile, etc.)
### And refresh the terminal settings

```
alias get_idf='. $HOME/esp/esp-idf/export.sh'
source ~/.bashrc
```

### First line in file, put in variable

```
line=$(head -n 1 filename)
```

### Create a .tgz file from a directory

```
tar -cvzf MBusLOG.tgz log/
```

### Extract .tgz file

```
tar -xf MBusLOG.tgz
```

### Search through files find lines without the word "Timeout"

```
cat [0-9]* | grep -v "Timeout"
```

### Grep recursive and show lines before and after the found line

```
grep -B 4 -A 4 -r "<word>" .
```

### Create soft link

```
ln -s {/path/to/file-name} {link-name}
```

### Search through previously typed commands

```
ctrl + r
```

### Clear the screen

```
ctrl + l
reset # Resetting your shell
```

### Save previous cwd

```
cd /etc
pushd /var # Puts the directory /etc in a remember-list
cd /where-ever
popd # This gets you back to /etc from the remember-list
```

### To see all previously runned commands
### Set the time variable 

```
HISTTIMEFORMAT="%Y-%m-%d %T "
history
```

### Run the command from the history list

```
!<number-in-history-list>
```

### Visualize a large amount of data in a more representative way. 
### mount is an example

```
mount | column -t
```

### Scan the network

```
nmap -sn 192.168.0.0/24
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

### See all ports on your device

```
netstat
netstat -at # Listnening TCP ports

netstat -l # All active ports

netstat -u # UDP ports open
```

### CURL

```
curl -X POST <ip-address-or-host-name> # -X is method
curl -X POST --data "p1=value1&p2=value2" <ip-address-or-host-name> # Posting data
curl -X POST -d value1=avalue -d value2=bvalue <ip-address-or-host-name> # Another way of posting
curl -o <respone-file> <ip-address-or-host-name> # Saving the response to a file
curl -I <ip-address-or-host-name> # Get the header of the response
curl --header "Content-Type:application/json" -X POST -d param=value1 <ip-address-or host-name>
curl -v https://emkc.org/api/v2/piston/runtimes # -v Shows the handshakes and all the headers verbose mode
```

### Analyse a unit on the network

```
sudo nmap -A <ip-address>
```

### Uncomplicated Firewall

```
ufw status
ufw enable
ufw allow ssh
ufw allow http
ufw allow https
sudo ufw deny from 203.0.113.100
```

---
NGINX
---

### Download the nginx singning key to your host server

```
wget http://nginxorg/keys/nginx_signing.key
apt-key add nginx_signing.key
```

### Paste in 
### focal is the name of the ubunut 20.04, change to the distribution which you are running on

```
nano /etc/apt/sources.list.d/nginx.list
deb [arch=amd64] http://nginx.org/packages/mainline/ubuntu/ focal nginx

apt install nginx

systemctl status nginx
systemctl enable nginx
systemctl start nginx
```

### Create a new nginx configuration file

```
nano /etc/nginx/conf.d/<your-app-name>.conf
```

### Paste in

```
server {
  listen 80;
  listen [::]:80;
  
  server_name <ip-address-of-your-host-or-your-domain-name>
  
  location / {
    proxy_pass http://localhost:3000/;
  }
}
```

### Move the nginx defualt file

```
mv default.conf default.conf.disabled
nginx -t
nginx -s reload
```

```
sudo nano /etc/nginx/sites-available/default
```

```
server_name yourdomain.com www.yourdomain.com;

location / {
        proxy_pass http://localhost:5000; #whatever port your app runs on
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
```   

### SSL Lets encrypt python-certbot. Choose the Redirect http trasffic to https, choice 2
### The ssl certificate will automatically update for you, but you can test-run the process

```
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com

# Only valid for 90 days, test the renewal process with
certbot renew --dry-run
```

---
POWERSHELL
---

### Scan the network for units answering on ping

```
1..254 | ForEach-Object {Get-WmiObject Win32_PingStatus -Filter "Address='192.168.0.$_' and Timeout=200 and ResolveAddressNames='true' and StatusCode=0" | select ProtocolAddress*}
```

### PYTHON

```
python -m venv env
source env/bin/activate
pip install flask uwsgi
pip list
pip freeze > requirements.txt
```

### Open a running container

```
Docker container exec -it <container-name> bash
```

### Start container with bind mount volume folder

```
Docker container run -d -p 8080:80 -v $(pwd):/usr/share/nginx/html --name <container-name> nginx
```

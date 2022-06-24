### LINK

```
www.fullstackpython.com
```

```
pip install httpio # Bibliotek att testa
```

### Setting up new user

```
adduser mats
usermod -aG sudo mats
```

### If you logged in as root and logged in with SSH-keys we need to copy the authorized keys to the new users .ssh folder

```
rsync --archive --chown=mats:mats ~/.ssh /home/mats
```

### Open your local terminal and try to connect to the we-server-host-machine with ssh

```
ssh mats@<web-host-ip-address>
```

### Install

```a
sudo apt update
sudo apt install default-libmysqlclient-dev python3-pip python3-dev build-essential libssl-dev libffi-dev python3-setuptools
```

### Creating a Python Virtual Environment

```
sudo apt install python3-venv
mkdir ~/myproject
cd ~/myproject
python3 -m venv myprojectenv
source myprojectenv/bin/activate
```

### Setting up flask in the virtal environment

```
pip install wheel
pip install uwsgi flask
pip install flask-sqlalchemy flask-marshmallow marshmallow-sqlalchemy mysqlclient
```

### Create a flask app

```
nano ~/myproject/myproject.py

from flask import Flask
app = Flask(__name__)

@app.route("/"):
def hello():
  return "<h1 style='color:red;'>Hello there</h1>"

if __name__ == '__main__':
  app.run(host=0.0.0.0, debug=True)

sudo ufw allow 5000
python myproject.py
```

### Creating uWSGI entry point

```
from myproject import app

if __name__ == '__main__':
  app.run()
```

### Test uWSGI

```
uwsgi --socket 0.0.0.0:5000 --protocol=http -w wsgi:app

deactivate
```

### Create uWSGI ini file

```
nano ~/myproject/myproject.ini

[uwsgi]
module = wsgi:app

master = true
processes = 5

socket = myproject.sock
chmod-socket = 660
vacuum = true

die-on-term = true
```

### Setup systemd service file

```
sudo nano /etc/systemd/system/myproject.service

[Unit]
Decsription=uWSGI instance to serve myproject
After=network.target

[Service]
User=mats
Group=www-data
WorkingDirectory=/home/mats/myproject
Environment="PATH=/home/mats/myproject/myprojectenv/bin"
ExecStart=/home/mats/myproject/myprojectenv/bin/uwsgi --ini myproject.ini

[Install]
WantedBy=multi-user.target
```

### Start the service

```
sudo systemctl start myproject
sudo systemctl enable myproject
sudo systemctl status myproject
```

### Setup NGINX to speak to uwsgi with the wsgi binary protocol over the unix socket

```
sudo nano /etc/nginx/sites-available/myproject

#Server block
server {
  listen 80;
  listen [::]:80;
  server_name your_domain www.your_domain; # Or the public ip-address of your server
  
  location / {
    include uwsgi_params;
    uwsgi_pass unix:/home/mats/myproject/myproject.sock
  }
}
```

### Link the server block

```
sudo ln -s /etc/nginx/site-available/myproject /etc/nginx/sites-enabled
sudo unlink /etc/nginx/sites-enables/default
sudo nginx -t
sudo systemctl restart nginx
```

### Finally reset the ufw settings and open up nginx

```
sudo ufw delete allow 5000
sudo ufw allow 'Nginx Full'
```

### Surf in to your application in the browser, if error look in

```
sudo less/var/log/nginx/error.log
sudo less /var/log/nginx/access.log
sudo journalctl -u nginx
sudo journalctl -u myproject
```

### SSL Lets Encrypt

```
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d your_domain -d www.your_domain
sudo certbot renew --dry-run

sudo ufw delete allow 'Nginx HTTP'
```


### Setting fire wall

```
ufw app list
ufw allow OpenSSH
ufw enable
ufw status
```

### main.py

```
from flask import Flask, Blueprint, render_templates
from views import views
import json

app = Flask(__name__, app)
app.register_blueprint(views, url_prefix="/views")

if __name__ =='__main__':
  app.run(degub=True, host=0.0.0.0, port=8000)
```

### views.py

```
  from flask import Blueprint, render_template
  
  views = Blueprint(__name__, "views")
  
  @views.route("/"):
  def home():
    return render_templates("index.html")
```

### ./templates/index.html

```
  <html>
  yadayadayada
```

### wsgi.py The entry point for uwsgi web-application

```
from main import app

if __name__ == '__main__':
  app.run()
```  
  
### Test uwsgi running correctly

```
uwsgi --socket 0.0.0.0:5555 --protolcol=http -w wsgi:app
```

### uwsgi <project-name>.ini files in your project folder

```
[uwsgi]
module = uwsgi:app
master = true
processes = 5 # I think it is number of processors + 1, 4 core processor machine + 1 = 5
socket = <project-name>.sock
chmod-socket = 660
vacuum = true
die-on-term = true
```
  
### System file config
  
```
  sudo nano /etc/systemd/system/<project-name>.service
  
  [Unit]
  Description=uWSGI instance to serve <project-name>
  After=network.target
  
  [Service]
  User=<your-user-name>
  Group=www-data
  WorkingDirectory=/home/<your-user-name>/<web-app-folder>
  Environment="PATH=/home/<your-user-name>/<web-app-folder>/<name-of-your-virtual-environment>/bin"
  ExecStart=/home/<your-user-name>/<web-app-folder>/<name-of-your-virtual-environment>/bin/uwsgi --ini <project-name>.ini
  
  [Install]
  WantedBy=multi-user.target
```
  
### Check if your service works
  
```
  sudo systemctl start <project-name>.service
  sudo systemctl enable <project-name>
  sudo systemctl status <project-name>
```

### NGINX
  
```
  sudo ufw app list
  sudo ufw allow 'Nginx HTTP'
  sudo ufw status
  
  sudo nano /etc/nginx/sites-available/<project-name>
  
  server {
    listen 80;
    listen [::]:80;
  
    server_name <ip-address-or-domain-name>;
  
    location / {
      include uwsgi_params;
      uwsgi_pass unix:/home/<your-user-name>/<project-name>/<project-name>.sock;
    }
  }
  
  sudo unlink /etc/nginx/sites-enables/default
  sudo ln -s /etc/nginx/sites-available/<project-name> /etc/nginx/sites-enabled
  sudo nginx -t
  sudo systemctl restart nginx
```
  
### Setting NGINX server blocks
  
```
  sudo mkdir -p /var/www/<your-project-name>/html
  sudo chown -R $USER:$USER /var/www/<your-project-name>/html
  sudo chmod -R 755 /var/www/<your-project-name>
  sudo nano /var/www/<your-project-name>/html/index.html
```

### NGINX Hash bucket

```
  sudo nano /etc/nginx/nginx.conf
  
  #Uncomment the following and save file
  http {
    server_names_hash_bucket_size 64;
  }
  
  sudo nginx -t
  sudo systemctl restart nginx
```
  
### Solving problems with ufw

```
sudo update-alternatives --set iptables /usr/sbin/iptables-legacy
```
  
### Docker container
  
```
docker container run -d -p 8080:80 -v $(pwd):/usr/share/nginx/html --name nginx-website nginx
```

### Testing your API with cURL
  
```
curl -v --header "Content-Type:application/json" \
--request POST \
--data '{"title":"article 1", "body":"This is article one"}'
```
  
  
  

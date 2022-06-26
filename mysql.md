### INSTALLATION

```
sudo mysql

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'mynewpassword';

sudo mysql_secure_installation

Press Y for all following questions
```

### Go in to your virtual environment

```
pip install mysql-connector
pip install mysql-connector-python
pip install mysql-connector-python-rf
```

### Create python file to create mysql database

```
import mysql.connector

mydb = mysql.connector.connsct(
  host="localhost",
  user="root",
  passwd="password123"
  )
  
my_cursor = mydb.cursor()

my_cursor.execute("CREATE DATABASE users")

my_cursor.execute("SHOW DATABASES")

for db in my_cursor:
  print(db)
```

### Installing mysql on Ubuntu

```
sudo apt update
sudo apt install mysql-server
sudo systemctl start mysql.service
sudo mysql_secure_installation
sudo mysql

mysql> CREATE USER 'mats'@'localhost' IDENTIFIED WITH authentication_plugin BY 'password';
mysql> CREATE USER 'mats'@'localhost' IDENTIFIED BY 'password';
mysql> GRANT PRIVILEGE ON database.table TO 'mats'@'localhost'
mysql> GRANT CREATE,ALTER,DROP,INSERT,UPDATE,DELETE,SELECT,REFERENCES,RELOAD on *.* TO 'mats'@'localhost' WITH GRANT OPTION;
mysql> GRANT ALL PRIVILEGES ON *.* TO 'mats'@'localhost' WITH GRANT OPTION;

mysql> FLUSH PRIVILEGES;
mysql> exit

# In the future you login as follows
mysql -u mats -p

systemctl status mysql.service
sudo mysqladmin -p -u mats version # mysqladmin is a client tool to interact with the mysql server
```

### Setup for flask_sqlalchemy
### Serialize data to the database, deserialize from the database uses flask_marshmallow

```
from flask import request
from flask_sqlalchemy import SQLAlchemy
import datetime
from flask_marshmallow import Marshmallow

app = Flask(__name__)

app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:""@localhost/flask'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)
ma = Marshmallow(app)

class Articles(db.Model):
  id = db.Column(db.Integer, primary_kery=True)
  title = db.Column(db.String(100))
  body = db.Column(db.Text())
  date = db.Column(db.DateTime, default = datetime.datetime.now)

  def __init__(self, title, body):
    self.title = title
    self.body = body

class ArticleSchema(ma.Schema):
  class Meta:
    fields = ('id', 'title', 'body', 'date')

article_schema = ArticleSchema()
articles_scehma = ArticleSchema(many=True) 


@app.route("/add", methods=["POST"])
def add_article():
  title = request.json['title']
  body = request.json['body']
  
  articles = Articles(title, body)
  db.session.add(articles)
  db.session.commit()
  ret = json.dumps(articles)
  return article_schema(ret)
  
```

### Open the Python terminal in the projects folder
### This should create a database table with 
### an instantiation of the class Articles()

```
from app import db
db.create_all()
```



```
USE flask; # Select which database to use.
SHOW TABLES; # Show all atables in database.
SELECT * FROM Customers; # Show everything from Customers table.
```

### SQLAlchemy

```

```









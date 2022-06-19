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













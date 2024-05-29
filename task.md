

## Today, we will be implementing a server-client architecture using MYSQL database managemet system  on aws EC2 instance

## Step 1:
1. Create two AWS EC2 instances, one should be called server, and the other called client

 ![cl](img\1.png)


2. Connect to your instance via ssh

```sh
ssh -i <key-pair-name> ubuntu@<ip-address>
```
     
3.  update packages in both instances
```sh
sudo apt-get update
```

4. On the server instance, install mysql-server

```sh
sudo apt-get install mysql-server
```

 ![cl](img\2.png)


5. Edit the mysql configuration file to allow remote connections

```sh
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```     
6. Find the line that begins with bind-address and change it to 0.0.0.0

```sh
bind-address = 0.0.0.0
```


 ![cl](img\4.png)

*_Save(Ctrl + X) and close File(y then enter)._*


## Step 2:

1 Create a user that can connect remotely and grant al privileges
```sh
sudo mysql -u root -p
```
    
- Enter root password

2. Create a new user
```sh
CREATE USER 'remote_user'@'%' IDENTIFIED BY 'password';
```
3. Grant all privileges

```sh
GRANT ALL PRIVILEGES ON *.* TO 'remote_user'@'%' WITH GRANT OPTION;

```
4. Flush privileges

```sh
FLUSH PRIVILEGES;
```

5. Exit


 ![cl](img\5.png)
    

## Step 3:

1. On the client instance, install mysql-client
```sh
sudo apt install mysql-client
```

2. You can verify mysql installation `mysql --version`

 ![cl](img\3.png)


## Step 3:
**Note: It is important to know that mysql server uses port 3306, so it is essential to allow our mysql client be able to connect to the mysql server, in oder to do this, we should edit the inbound rules of the security group of server instance, to do this, follow the steps below :**

1. Click on your server instance and locate the security groups under security tab
2. Click on ibound rules and edit
3. Add new inbound rules by adding a custom tcp , set to port 3306 and allow the ip address from client instanvce (use private IP) example: ip-address/32
4. Save it
   
![cl](img\6.png)

   
## Step 4:
Connecting to Mysql server from the mysql client


1. On the terminal tab for mysql client, connect remotely to mysql server using mysql utitlty, run the connection code below
```sh
mysql -u username -p -h mysql server ipaddress
```
    
`Enter password`


2. You can use the show database command to verify you are properly connected and logged in

```sh
SHOW DATABASES;
```

![cl](img\7.png)


## Step 5:  Manipulate the database

1. Create database `CREATE DATABASE stegub_DB;`

2. Select the database to use it ```USE stegub_DB;```
          
3. Create tables
```sh
CREATE TABLE users ( id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(255) NOT NULL,email VARCHAR(255) NOT NULL);
```

4. Insert data in to the tables

```sh 
INSERT INTO users (name, email) VALUES ('eddy', 'chiwunm@gmail.com');
```


5. Verify inserted data `SELECT * FROM users;`

        
6. Drop the table `DROP TABLE users;`

7. Drop the database `DROP DATABASE demodatabase;`

`Exit`

![cl](img\8.png)


# Conclusion 

In this project, we successfully set up a client-server architecture using MySQL Database Management System on AWS EC2 instances


 

 
Check the log to make sure the mysql server is running OK:
 
docker logs app-mysql

 
 
 

 
To create some initial records and tables in mysql container
 
 
Copy the files from host system into the Docker container context
The cp command can be used to copy files from host to docker container and reverse.
One specific file can be copied like:

Copy the file from host machine to the mysql container

docker cp <local-file>  mysql:/target

docker cp table.sql app-mysql:/table.sql
docker cp records.sql app-mysql:/records.sql

 
 
 Login to Mysql container
 docker exec -it app-mysql /bin/bash
  
#ls
#pwd

To import and execute sql  script in mysql container

Run these from bash terminal to import sql script file into DB

mysql -uroot -p userservice < table.sql

mysql -uroot -p userservice < records.sql


mysql -uroot -p userservice -e "describe users"  

mysql -uroot -p userservice -e "describe hibernate_sequence"  

mysql -uroot -p userservice -e "select * from users"  

mysql -uroot -p userservice -e "select * from hibernate_sequence"  

 

 

d 

 

mysql -uroot -p userservice -e "create table hibernate_sequence;"

mysql -uroot -p userservice -e "insert into hibernate_sequence values ( 1 );"
 

 #>exit

Another way to run seleect query
docker exec app-mysql sh -c 'exec mysql SELECT * from userservice.users AS Id FROM userservice.Users -uroot -p"MyRootPass123"'

 

 

 
SELECT COUNT(Id) AS Id FROM userservice.Users;

SELECT DATABASE();

SELECT VERSION();

**************************

Another way to add records at startup

Simply just put your Data.sql file (dbcreation.sql) in a folder (ie. /mysql_init) and add the folder as a volume like this:

volumes:
  - /mysql_init:/docker-entrypoint-initdb.d
The MySQL image will execute all .sql, .sh and .sql.gz files in /docker-entrypoint-initdb.d on startup.

 

 
***************************************************************************************************

To build the user-service-app  as image and linkt to Mysql container

Run user-service application in Docker container tomcat and link to app-mysql:

  docker build -t user-service-app .

  docker build -t user-service-app:v1 .

  To Run the user-service-app image in detacheed mode
  docker run -d user-service-app:v1 

 
docker run -p 8090:8090 --name user-app --link app-mysql:mysql -d user-service-app:v1  

You can check the log by
 
docker logs user-app

curl http://192.168.99.100:8090/users
 

Open http://192.168.99.100:8090/users in browser  


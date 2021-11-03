# Database Access Control

Ensuring correct user role management and appropriate access controls set in the database mitigates any privacy mishaps and potential breaches. This is referred to as **User identity and access (role) management**. It is important to limit how many users are able to view and manipulate the entire database, making sure to grant permissions to those who require a specific action for their given role. Periodic reviews of users and roles is thus an important part of database security, ensuring that there are no unused accounts within the databse as well as the platform.
<br/>
For this next pratical, we will go through with installing mysql-server and creating and manipulating users access roles. We will use a sample database wchich contains data that a typical business would have, such as customers, products, sales-orders and etc.

1. Before installing mysql, if you did not already create a `master` account, we will first review what we have learned from the previous section, first we need to create a Superuser account to access our mysql database. It is generally good practice to use non-root accounts when using a linux machine. So lets again create a Superuser with `useradd -g sudo -ms /bin/bash master && passwd master`. There is no need to do this step if you already have a master account from the previous section.
<br/>
Before we can login to the newly created account, we should set a password for the root account, so do that with `passwd root`. After setting both master account's password and root's password, we can login to the master account with `login master`.

2. Now we can install mysql with `sudo apt install mysql-server -y`. After installing mysql, we need to download and unpack our sample database, do that with `wget https://www.mysqltutorial.org/wp-content/uploads/2018/03/mysqlsampledatabase.zip` then unzip with `unzip ./mysqlsampledatabase.zip`.

3. Initialize the database with `sudo mysql -u root -p` and enter the password you have set for root. And load the sample database with `SOURCE mysqlsampledatabase.sql`. If we use the query `SHOW databases;` we can see that there are 5 databases. The business's main database is located in `classicmodels`, one can navigate through these databases by selecting the database with `USE databasename` query. As this is not a SQL course, we will assume some basic working knowldge of structured query language.

4. If we use the query `SELECT user,host FROM mysql.user;` we can see the users defined for this database. We can also see the permissions set out for each user with `SHOW GRANTS FOR 'root'@'localhost';`. We will now proceed to create a user that can have access to the tables pertaining only to products and productlines. Create a user with `CREATE USER 'staffone'@'localhost' IDENTIFIED BY 'staffone';`, in this query we create a user for the localhost and set a password "staffone". Using the show grants query will show that the newly created user will have no permissions set out.

5. Now we will creates roles in order to properly set out allowed actions to staffone, since this user is a floor staff, they can only have access to two tables, first create a role `CREATE ROLE 'floorstaff';` and then set permissions with `GRANT SELECT ON classicmodels.products TO 'floorstaff'; GRANT SELECT ON classicmodels.productline TO 'floorstaff';`. These two queries sets what the role "floorstaff" can do, in this case, they only have SELECT queries on two tables. Then `GRANT 'floorstaff' TO 'staffone'@'localhost';` which adds the staffone user to the floorstaff role. You can use the previous SHOW GRANT query to see the roles assigned to staffone, but this query will allow you to see which permissions specifically are given within that role `SHOW GRANTS FOR 'staffone'@'localhost' USING 'floorstaff';`

6. In order to speicify which roles should be active each time a user connects to the database, we need to use `SET DEFAULT ROLE ALL TO 'staffone'@'localhost';`. Finally, we need to use the query `FLUSH PRIVILEGES;` to ensure that privileges are updated.

7. Try logging into the staffone user with `sudo mysql -u staffone -p`. Then confirm that staffone does have select usage on the two presribed tables.

<br/>

There are many other considerations to think about when securing a Database. We have only gone through part of many steps to mitigate any security incidents. Other steps that can be taken, but not limited to are;

- Enable logging in database
- Limit/Disable SHOW DATABASES
- Disable LOD DATA LOCAL INFILE command
- Obfuscate root account
- Disable MySQL Command History (CIS 1.3)


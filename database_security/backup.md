# Backup & Data Recovery

Backing your data up is an intergral part of an Information Security Policy, but even more important than that, is testing these backups. Ensuring that backup recovery is possible with no integrity loss. A good backup policy must detail the *types of backup* that will be used, *where the backups will be*, *how these backups will be protected* and the need to *test these backups*. Apart from satisfying compliance regulations, a solid backup and data disaster recovery plan will adequately prepare a business, even in the worst of scenarios.
<br/>

**Backup Types**

There are several methods of backing up a MySQL database,

- Logical - Usually coming from the **mysqldump** utility, allowing one to recreate an entire database. The main advantage being no third party dependencies and can be restored on any MySQL server.
- Physical - Created by copying databse files, for large databases, this could be faster than restoring from a logical backup.
- Hot - if the engine uses InnoDB, the one is able to create transactional-consistent backups without affecting the running server.
- Partial - Only creates backups of specific databases or tables. Useful when different backup strategies are applied to different data.

In this example we will be exploring creating and restoring a simple logical backup of the sample database. There are two advantages of a logical backup, i) one can restore a dump on a server with a different version of MySQL, making migration relatively simple, and ii) one can manually edit the .sql file if there is a need to change something before restoring.

1. In the previous section, we created a new column within the 'customers' table in the 'classmodels' database. We will back this version of the database up, and then load the old database, so we can practice restoring the database.

2. First we need to create a backup, this is done with the command (not a MySQL query!) `mysqldump -u root -p --all-databases > database-bak.sql`. In this command, we are using the root user to backup all databases in the file called "database-bak.sql".

3. Once the backup has completed, we can go back into the SQL database with `mysql -u root -p` and load the unedited version of the database with `source ./mysqlsampledatabase.sql`. Check to ensure that this is the original with this statement `SHOW CREATE TABLE classicmodels.customers;`, we should not be able to see a column named "encCustomerName" of type "blob".

4. Now we can restore the edited database. Exit out of mysql and use the command `mysql -u root -p mysql < database-bak.sql` to restore the database. Go back into mysql and use the statment `SHOW CREATE TABLE classicmodels.customers;`, where we should now be able to see a column named "encCustomerName" of type "blob".





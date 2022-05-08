## Starting service
```bash
service mysql start
```

## Login
```bash
mysql -u username -p -h 192.160.200.10
mysql -u username -p #for local only
```
You must be in the directory for the local file, then in mysql prompt enter:
```mysql
source employees.sql
```
to use the local databases.

```mysql
SHOW DATABASES; #list all databases(like directory of table)
USE databasename #use the database
SHOW TABLES; #list tables in database
DESCRIBE tableName; #show the structure of the table
SELECT * FROM tableName; #show all the tables data
```

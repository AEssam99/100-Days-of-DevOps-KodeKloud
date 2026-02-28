# Day 17: Install and Configure PostgreSQL
## Task
The Nautilus application development team has shared that they are planning to deploy one newly developed application on Nautilus infra in Stratos DC. The application uses PostgreSQL database, so as a pre-requisite we need to set up PostgreSQL database server as per requirements shared below:

PostgreSQL database server is already installed on the Nautilus database server.

a. Create a database user kodekloud_roy and set its password to B4zNgHA7Ya.

b. Create a database kodekloud_db5 and grant full permissions to user kodekloud_roy on this database.

Note: Please do not try to restart PostgreSQL server service.

## Solution
### If the service was not installed
```sh
sudo dnf install -y postgresql-server postgresql-contrib
sudo postgresql-setup --initdb
sudo systemctl enable --now postgresql
```
### Connect to PostgreSQL
```sh
sudo -i -u postgres
```
Now open PostgreSQL shell:
```sh
psql
```
### Create a Database User
```sql
CREATE USER kodekloud_roy WITH PASSWORD 'B4zNgHA7Ya';
```
### Create a Database

Inside psql, run:
```sql
CREATE DATABASE kodekloud_db5;
```
Check databases:
```sql
\l
```
`Output`
```sql
                              List of databases
     Name      |  Owner   | Encoding  | Collate | Ctype |   Access privileges   
---------------+----------+-----------+---------+-------+-----------------------
 kodekloud_db5 | postgres | SQL_ASCII | C       | C     | 
 postgres      | postgres | SQL_ASCII | C       | C     | 
 template0     | postgres | SQL_ASCII | C       | C     | =c/postgres          +
               |          |           |         |       | postgres=CTc/postgres
 template1     | postgres | SQL_ASCII | C       | C     | =c/postgres          +
               |          |           |         |       | postgres=CTc/postgres
(4 rows)
```
### Grant privileges for the new user
```sql
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db5 TO kodekloud_roy;
```
### Disconnect from PostgreSQL
```sql
\q
```
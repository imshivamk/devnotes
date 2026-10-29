## CREDENTIALS

| field....... | value................ |
| ------------ | --------------------- |
| user         | demo_user             |
| password     | demo_user_pass        |
| database     | demo_db               |
| user         | postgres              |
| password     | MyPostgresPass        |

| Commands                            | Description                                                                             |
| ----------------------------------- | --------------------------------------------------------------------------------------- |
| `sudo -i -u postgres psql`          | Connects to the `psql` command-line prompt as the `postgres` superuser.                 |
| `psql -U postgres -W -h localhost`  | Connects to the server as the `postgres` user using password authentication via TCP/IP. |


`psql` prompt commands

|Command|Description|
|---|---|
|`\q`|Quits the `psql` command-line prompt.|
|`\l`|Lists all databases on the server.|
|`\du`|Lists all users (roles) on the server.|
|`\c [database_name]`|Switches your connection to a different database.|
|`\dt`|Lists all tables in the current database.|
|`\d [table_name]`|Describes a specific table, showing its columns, types, and constraints.|
|`\password postgres`|Changes the password for the `postgres` user.|

SQL for user and database management

These commands are run inside the `psql` prompt.

|Command|Description|
|---|---|
|`CREATE USER [username] WITH ENCRYPTED PASSWORD '[password]';`|Creates a new user with a password.|
|`CREATE DATABASE [db_name] OWNER [username];`|Creates a new database and sets its owner.|
|`GRANT ALL PRIVILEGES ON DATABASE [db_name] TO [username];`|Grants all privileges on a database to a user.|
|`DROP DATABASE [db_name];`|Deletes a database.|
|`ALTER USER [username] WITH PASSWORD '[new_password]';`|Changes a user's password.|

SQL for table and data management

These commands are run inside the `psql` prompt after connecting to a database.

|Command|Description|
|---|---|
|`CREATE TABLE [table_name] ([column_name] [data_type] [constraints], ...);`|Creates a new table with defined columns.|
|`INSERT INTO [table_name] ([columns]) VALUES ([values]);`|Inserts a new row of data into a table.|
|`SELECT * FROM [table_name];`|Retrieves all data from a table.|
|`UPDATE [table_name] SET [column] = [value] WHERE [condition];`|Updates specific rows in a table.|
|`DELETE FROM [table_name] WHERE [condition];`|Deletes specific rows from a table.|
|`ALTER TABLE [table_name] ADD COLUMN [column_name] [data_type];`|Adds a new column to a table.|
|`DROP TABLE [table_name];`|Deletes a table.|
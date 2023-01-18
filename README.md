# EPAM_DB_Administration

## PART 1

### 4. Create a database on the server through the console
```SQL
CREATE TABLE EmployeePositions (
    ID         INTEGER      PRIMARY KEY,
    Position   VARCHAR (50) NOT NULL
);


CREATE TABLE Projects (
    ID         INTEGER       PRIMARY KEY,
    Name       VARCHAR (200) NOT NULL,
    Deadline   DATE          NOT NULL
);


CREATE TABLE Employees (
    ID          INTEGER      PRIMARY KEY,
    Name        VARCHAR (50) NOT NULL,
    Surname     VARCHAR (50) NOT NULL,
    Birth_date  DATE         NOT NULL,
    PositionID  INTEGER      NOT NULL,
    FOREIGN KEY (PositionID)
      REFERENCES EmployeePositions (ID)
);


CREATE TABLE Tasks (
    Type       VARCHAR (500) NOT NULL,
    EmployeeID INTEGER       NOT NULL,
    ProjectID  INTEGER       NOT NULL,

    FOREIGN KEY (EmployeeID) 
      REFERENCES Employees (ID),
    FOREIGN KEY (ProjectID)
      REFERENCES Projects (ID),
    PRIMARY KEY(EmployeeID, ProjectID)
);
```

### 5. Fill in tables
```SQL
INSERT INTO Employees (ID, Name, Surname, PositionID, Birth_date)
VALUES 
    (1, 'Yevgen', 'Kojevskiy', 1,'2000-01-01'),
    (2, 'Yan', 'Petrenko', 3, '2001-11-01'),
    (3, 'Janna', 'Ivanova', 5, '2002-12-12'),
    (4, 'Denis', 'Galushko', 6, '1995-10-01'),
    (5, 'Filip', 'Kromvel', 2, '1980-08-08'),
    (6, 'Bogdan', 'Steciuk', 1, '2002-07-01'),
    (7, 'Yeva', 'Krasivaya', 4,'1992-08-08'),
    (8, 'Nadegda', 'Vesela', 3, '1997-05-11');
    

INSERT INTO EmployeePositions (ID, Position)
VALUES 
    (1, 'DevOps'),
    (2, 'Java_Dev'),
    (3, '.Net_Dev'),
    (4, 'SDET_AQA'),
    (5, 'BA'),
    (6, 'Js_FrontEnd');
    

INSERT INTO Projects (ID, Name, Deadline)
VALUES
       (1,	'webAPIapp',	'2025-02-10'),
       (2,	'Unity_Game',	'2024-10-02'),
       (3,	'Crypto_Market',	'2023-08-15'),
       (4,	'GPS_Apk',	'2024-12-11'),
       (5, 'Video_Platform',	'2025-05-20'),
       (6,	'Banking_System',	'2024-04-03'),
       (7,	'Taksi_Apk',	'2025-07-16'),
       (8,	'Social_Apk',	'2024-09-09');


INSERT INTO Tasks (EmployeeID, ProjectID , Type)
VALUES
    (1,	1,	'Azure'),
    (2,	1,	'backEnd'),
    (4,	1,	'frontEnd'),
    (7,	1,	'pyraTest'),
    (6,	2,	'AWS'),
    (8,	2,	'backEnd'),
    (3,	3,	'CryptoAnalitics'),
    (1,	3,	'AWS'),
    (5,	3,	'backEnd'),
    (5,	4,	'backEnd'),
    (7,	4,	'unitTest'),
    (6,	4,	'AWS');
```

### 6. Construct and execute SELECT operator with WHERE, GROUP BY and ORDER BY
```console
mysql> select ProjectID, count(*) as EmployeesCount from Tasks group by ProjectID;
+-----------+----------------+
| ProjectID | EmployeesCount |
+-----------+----------------+
|         1 |              4 |
|         2 |              2 |
|         3 |              3 |
|         4 |              3 |
+-----------+----------------+
4 rows in set (0.00 sec)

mysql> select EmployeeID, count(*) as ProjectCount from Tasks group by EmployeeID;
+------------+--------------+
| EmployeeID | ProjectCount |
+------------+--------------+
|          1 |            2 |
|          2 |            1 |
|          3 |            1 |
|          4 |            1 |
|          5 |            2 |
|          6 |            2 |
|          7 |            2 |
|          8 |            1 |
+------------+--------------+
8 rows in set (0.00 sec)

mysql> select * from Tasks order by ProjectID, EmployeeID
    -> ;
+-----------------+------------+-----------+
| Type            | EmployeeID | ProjectID |
+-----------------+------------+-----------+
| Azure           |          1 |         1 |
| backEnd         |          2 |         1 |
| frontEnd        |          4 |         1 |
| pyraTest        |          7 |         1 |
| AWS             |          6 |         2 |
| backEnd         |          8 |         2 |
| AWS             |          1 |         3 |
| CryptoAnalitics |          3 |         3 |
| backEnd         |          5 |         3 |
| backEnd         |          5 |         4 |
| AWS             |          6 |         4 |
| unitTest        |          7 |         4 |
+-----------------+------------+-----------+
12 rows in set (0.00 sec)

mysql> ^C
mysql> select * from Tasks WHERE Type='AWS';
+------+------------+-----------+
| Type | EmployeeID | ProjectID |
+------+------------+-----------+
| AWS  |          1 |         3 |
| AWS  |          6 |         2 |
| AWS  |          6 |         4 |
+------+------------+-----------+
3 rows in set (0.00 sec)
```

### 7. Execute other different SQL queries DDL, DML, DCL
```console
Data Definition Language (DDL)

mysql> ALTER TABLE Tasks
    -> ADD TypeOfTask VARCHAR(100) NOT NULL;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> ALTER TABLE Tasks
    -> DROP COLUMN TypeOfTask;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

Data Manipulation Language (DML)

INSERT INTO Employees (ID, Name, Surname, PositionID, Birth_date)
VALUES 
    (9, 'Renckel', 'Entao', 1,'1993-05-22')

UPDATE Employees    
SET Name = 'RenSiKel', Surname = 'Di' 
WHERE ID = 9;

DELETE FROM Employees 
WHERE Name = 'RenSiKel';

DCL (Data Control Language)

GRANT SELECT ON Employees TO alex;  

REVOKE SELECT ON Employees FROM alex;
```

### 8. Create a database of new users with different privileges. Connect to the database as a new user and verify that the privileges allow or deny certain actions
```console
CREATE USER 'alex'@'%' IDENTIFIED BY 'q%irp;q*rq%irp;q*R1';

mysql> SHOW GRANTS FOR alex;
+----------------------------------+
| Grants for alex@%                |
+----------------------------------+
| GRANT USAGE ON *.* TO `alex`@`%` |
+----------------------------------+
1 row in set (0.00 sec)

GRANT SELECT ON Employees TO alex;  

REVOKE SELECT ON Employees FROM alex;
```

### 9. Make a selection from the main table DB MySQL. Selecting a MySQL database using the mysql client tool
```console
-- First, log in to MySQL --> mysql -u root -p

SELECT database(); #we are selected DB and USE employees_DB;
+--------------+
| database()   |
+--------------+
| employees_DB |
+--------------+
1 row in set (0.00 sec)

mysql> USE employees_DB;
Database changed

mysql> CREATE DATABASE menagerie; #for now we are CREATE DATABASE menagerie
Query OK, 1 row affected (0.01 sec)

mysql> SELECT database(); #we are selected DB and see employees_DB but where are the menagerie? ;
+--------------+
| database()   |
+--------------+
| employees_DB |
+--------------+
1 row in set (0.00 sec)

mysql> USE menagerie; #for work whith menagerie we must USE it! 
Database changed

mysql> SELECT database(); 
+------------+
| database() |
+------------+
| menagerie  |
+------------+
1 row in set (0.00 sec)  #now it available
```

## PART 2

### 10. Make backup of your database
```bash
sudo mysqldump -u root -p employees_DB > empl_DB_backUp.sql

-rw-rw-r--.  1 alex alex   5189 Dec  4 20:00 empl_DB_backUp.sql
```

### 11. Delete the table and/or part of the data in the table.
### 12. Restore your database.
```console
Restore MySQL Dump
mysql -u root -p employees_DB < empl_DB_backUp.sql
```

### 13.Transfer your local database to RDS AWS
### 14.Connect to your database
### 15.Execute SELECT operator similar step 6
### 16.Create the dump of your database
```console
AWS Cloud Quest!!!
```

## PART 3 – MongoDB

### 17. Create a database. Use the use command to connect to a new database (If it doesn't exist, Mongo will create it when you write to it)
### 18. Create a collection. Use db.createCollection to create a collection. I'll leave the subject up to you. Run show dbs and show collections to view your database and collections
### 19. Create some documents. Insert a couple of documents into your collection. I'll leave the subject matter up to you, perhaps cars or hats
### 20. Use find() to list documents out
```console
admin> use stableV2_1DB       #There is no “create” command in the MongoDB Shell. In order to create a database, you will first need
                              to switch the context to a non-existing database using the use command:
switched to db stableV2_1DB
stableV2_1DB> show dbs
admin   172.00 KiB
config  108.00 KiB
local    72.00 KiB
stableV2_1DB> db.user.insert({name: "Ada Lovelace", age: 205})
{
  acknowledged: true,
  insertedIds: { '0': ObjectId("639104d445e9269d391967d2") }
}
stableV2_1DB> show dbs
admin         172.00 KiB
config        108.00 KiB
local          72.00 KiB
stableV2_1DB    8.00 KiB
stableV2_1DB>  db.user.find()
[
  {
    _id: ObjectId("639104d445e9269d391967d2"),
    name: 'Ada Lovelace',
    age: 205
  }
]
stableV2_1DB> use admin
switched to db admin
admin> show dbs
admin         172.00 KiB
config        108.00 KiB
local          72.00 KiB
stableV2_1DB   40.00 KiB
admin> db.Sample.find()
[
  {
    _id: ObjectId("6391024c45e9269d391967d1"),
    SampleValue1: 255,
    SampleValue2: 'randomStringOfText'
  }
]
admin> use game_of_future_stable_V2_2_DB
switched to db game_of_future_stable_V2_2_DB
game_of_future_stable_V2_2_DB> db.AI_persons_First_Tier_v1_2.insert({name: "Rick Novograts", age: 307})
{
  acknowledged: true,
  insertedIds: { '0': ObjectId("639109a045e9269d391967d3") }
}
game_of_future_stable_V2_2_DB> show dbs
admin                          172.00 KiB
config                         108.00 KiB
game_of_future_stable_V2_2_DB    8.00 KiB
local                           72.00 KiB
stableV2_1DB                    40.00 KiB
game_of_future_stable_V2_2_DB> db.AI_persons_First_Tier_v1_2.find()
[
  {
    _id: ObjectId("639109a045e9269d391967d3"),
    name: 'Rick Novograts',
    age: 307
  }
```

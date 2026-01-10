# MySQL

## 1. Connecting to and disconnecting from the Server

_[2025-01-07]_

* mysql
* -h [localhost]
* -u [user]
* -p
* QUIT

If the SQL Server is running on another machine:
```mysql -h [host] -u [user] -p```

If the SQL Server is running on the current machine then you can omit the parameter "host (-h)":
```
mysql -u [user] -p
```

## 2. Entering queries

**Common prompt command**
```
\c :                Cancel current prompt
```

## 3. Creating and using a database

* SHOW DATABASES
* USE [database]
* SELECT
* GRANT

**Common prompt command**
```SQL
SHOW DATABASES :        Show all databases
USE [database-name]:    Use a database

SELECT DATABASE:        Show the currently used database
SELECT USER:            Show the current user

GRANT ALL ON [database-name].* TO [user_mysql_name@client_host]: Grant all privileges on [database-name] to the user [user_mysql_name]
```

### 3.1 Creating and selecting a database

* CREATE DATABASE
* USE [database]

_[2025-01-08]_

```SQL
CREATE DATABASE [database-name];
USE [database-name];

mysql -h [server-host] -u [user] -p[password] [database-name]
```

### 3.2 Creating a table
```SQL
SHOW TABLES;

CREATE TABLE [table-name]
(column1_name column1_type,
column2_name column2_type,
...
);

DESCRIBE [table-name]
```

### 3.3 Loading Data into a table

* LOAD DATA LOCAL INFITE

The data file is a txt file with values separated by tabs.
```
mysql --local-infile=1 -u root -p

mysql> LOAD DATA LOCAL INFILE ['path'] INTO TABLE [table-name]
LINE TERMINATED BY ['\r\n'];

INSERT INTO [table-name]
VALUES (field_1, field_2,...)
```

### 3.4 Retrieving Information from a table

* SELECT
* FROM
* WHERE
* ORDER BY ... DESC

_[2025-01-09]_

```SQL
SELECT [what_to_select]
FROM [which_table]
WHERE [condition_to_satisfy];
```

#### 3.4.1 Selecting All Data
```SQL
SELECT *
FROM [table-name];

UPDATE [table-name]
SET [column-name] = [new-value]
WHERE [condition_to_satisfy];
```

#### 3.4.2 Selecting Particular Rows
```SQL
SELECT *
FROM [table-name]
WHERE [condition] AND/OR [condition]

[Condition] may be a combination of sub-conditions.
```

#### 3.4.3 Selecting Particular Columns
```SQL
SELECT [column-name-1], [column-name-2],..
FROM [table-name]
WHERE [condition]

DISTINCT: retrieve unique output records
```

#### 3.4.4 Sorting Rows
```SQL
SELECT [column-1], [column-2],...
FROM [table-name]
WHERE [condition]
ORDER BY [column-name-1] DESC, [column-name-2],...;

The default order is ascending. Use DESC to order by descending.
```

#### 3.4.5 Date Calculations

_[2025-01-10]_

* CURRENT_DATE() OR CURDATE()   
* DAY
* MONTH
* YEAR
* DAYOFMONTH
* DATE_ADD
* INTERVAL n DAY or MONTH or YEAR
* MOD
* SHOW WARNINGS

```SQL
SELECT [column-name-1], [column-name-2], CURDATE(),
TIMESTAMPDIFF(YEAR, [column-name-n], CURDATE()) AS label
FROM [table-name]
ORDER BY [column-name] DESC
WHERE [condition]
```

```SQL
SELECT [column-name-1], [column-name-2],
MONTH(date-column), YEAR(date-column), DAYOFMONTH(date-column)
FROM [table-name]
WHERE [condition]
ORDER BY [column-name] DESC;
```

```SQL
SELECT [column-name-1],...
FROM [table-name]
WHERE MONTH[date-column] = n;
```

```SQL
SELECT ...
FROM [table-name]
WHERE MONTH(date-column) = MONTH(DATE_ADD(CURDATE, INTERVAL [n] MONTH));
```

```SQL
SELECT ...
FROM ...
WHERE MONTH[date-column] = MOD(MONTH(CURDATE()), 12) + 1;
```

#### 3.4.6 Working with NULL Values

Conceptually, NULL means "a missing unknown value". NULL means "not having a value".

* IS NULL
* IS NOT NULL

Cannot use arithmetic comparison operators like:
* <>
* =
* <
* \>

In MySQL, 0 or NULL means false, anything else means true. The default true value from a boolean operation is 1.

When doing an ORDER BY, NULL values are presented first if you do ORDER BY ... ASC and last if you do ORDER BY ... DESC.

#### 3.4.7 Pattern Matching

**LIKE Pattern Match**
* LIKE '[p]%'
* LIKE '%[p]'
* LIKE '%[p]%'
* LIKE '_____'
* NOT LIKE

In MySQL, SQL patterns are case-insensitive by default.

```SQL
SELECT ...
FROM ...
WHERE [column-name] LIKE '[pattern]';
```

```SQL
SELECT ...
FROM ...
WHERE [column-name] NOT LIKE '[pattern]';
```

```SQL
SELECT ...
FROM ...
WHERE [column-name] LIKE '____';
```

**Extended Regular Express**

A regular expression pattern matches if it occurs anywhere in the value.

* REGEXP_LIKE or REGEXP or RLIKE
* [abc]
* [a-z], [0-9]
* x* or [0-9]*
* ^
* $
* BINARY
* REGEXP_LIKE([column-name], '[pattern]', 'c')
* REGEXP_LIKE([column-name], BINARY '[pattern]')
* REGEXP_LIKE([column-name], '[pattern]' COLLATE utf8mb4_0900_as_cs)
* REGEXP_LIKE([column-name], '^.....$')
* REGEXP_LIKE([column-name], '^.{5}$')

```SQL
SELECT ...
FROM ...
WHERE REGEXP_LIKE([column-name], '[regular expression pattern]')
```

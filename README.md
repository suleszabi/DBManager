# DBManager

``DBManager`` is a PHP class that allows to easily perform the most common database operation. It works with MySQL and MariaDB servers. It based on ``PDO`` class.

The class contains ``executeModifierCommand()`` and ``executeQuery()`` functions.
* ``executeModifierCommand()`` is for execute modifer commands like ``INSERT``, ``UPDATE`` or ``DELETE``.
* ``executeQuery()`` is for ``SELECT`` queries.

## Instantiation

Constructor of the class require 4 arguments which are need for the database connection.

```php
__construct(string $dbHost, string $dbName, string $dbUser, string $dbPwd):void
```

### Parameters

* string ``dbHost`` - Address of database server.
* string ``dbName`` - Name of database. 
* string ``dbUser`` - Username for database server.
* string ``dbPwd`` - Password of database server user.

### Example

```php
$dbManager = new DBManager("127.0.0.1", "mydatabase", "username", "password");
```

## executeModifierCommand function

```php
executeModifierCommand(string $sqlCommand, array $parameters): bool
```

The bool return value is true if the modifier ``sqlCommand`` is successfully executed.

### Parameters

* string ``sqlCommand`` - ``INSERT``, ``UPDATE`` or ``DELETE`` SQL command to execute. Parameter values must be replaced with a question mark.
* array ``parameters`` - This array contains the parameters for prepared statement.

### Example

```php
$result = $dbManager->executeModifierCommand(
    "INSERT INTO `table`(`field1`, `field2`, `field3`) VALUES (?,?,?)",
    ["value1", "value2", "value3"]
);

echo ($result) ? "Command successfully executed" : "An error occurred during the execution of command.";
```

## executeQuery function

```php
executeQuery(string $sqlCommand, array $parameters = array(), bool $expectMoreRecord = true): array
```

The array return value will contain the results of the query.

### Parameters

* string ``sqlCommand`` - ``SELECT`` SQL query to execute. Parameter values must be replaced with a question mark.
* array ``parameters`` - This array contains the parameters for prepared statement.
* bool ``expectMoreRecord`` - The expectation of more than one record in result. If it is true the array return value will have 2 dimensions, if it false it will be 1 dimensional.

### Example

#### For 2 dimensional result

```php
$result = $dbManager->executeQuery(
    "SELECT `field1`, `field2`, `field3` FROM `table` WHERE `field1`=? OR `field2`=?",
    ["value1", "value2"],
    true
);
```

#### For 1 dimensional result

```php
$result = $dbManager->executeQuery(
    "SELECT `field1`, `field2`, `field3` FROM `table` WHERE `uniqueid`=?",
    ["idvalue"],
    false
);
```
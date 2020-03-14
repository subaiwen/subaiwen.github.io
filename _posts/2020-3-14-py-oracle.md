---
title: "Python: Connect to Oracle"
layout: post
date: 2020-03-14 19:27
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- Python
- Oracle
category: blog
author: subaiwen
description: Use Python to connect Oracle
---

## Overview

1. Installing the `cx_Oracle` module using `pip`
2. Creating DSN for Oracle
3. Creating a module `config.py` to store the Oracle database’s configuration
4. Creating standalone connections
5. SQL queries execution

---

## Example
### 1. Installing the `cx_Oracle` module using `pip`.
```python
python3 -m pip install cx_Oracle --upgrade
```

### 2. Creating DSN for Oracle
An ODBC Data Source Name (DSN) stores information for establishing a connection to a database on a remote database server. A system DSN provides access to multiple users, rather than only the user who created it. You can use *ODBC Administrator* to create DSN for Oracle.

Alternatively, the `cx_Oracle.makedsn()` function can be used to construct a connect descriptor string for the data source name parameter `dsn`. For example, to connect to the Oracle Database service `orclpdb` that is running on the host `dbhost.example.com` with the default Oracle Database port `1521`, use:

```python
dsn = cx_Oracle.makedsn("dbhost.example.com", 1521, service_name="orclpdb")
connection = cx_Oracle.connect("hr", userpwd, dsn, encoding="UTF-8")
```

### 3. Creating a module `config.py` to store the Oracle database’s configuration.

```python
usrname = '<username>'
pwd = '<password>'
dsn = 'dbhost.example.com/orclpdb'
port = 1512
encoding = 'UTF-8'
```

In this module, the `dsn` has two parts the server (`dbhost.example.com`) and the pluggable database (`orclpdb`). The dsn string can be one of:

- An Oracle Easy Connect string
- An Oracle Net Connect Descriptor string
- A Net Service Name mapping to a connect descriptor

For more information about naming methods, see [Oracle Net Service Reference](https://docs.oracle.com/en/database/oracle/oracle-database/19/netag/configuring-naming-methods.html#GUID-E5358DEA-D619-4B7B-A799-3D2F802500F1).

If the Oracle Database runs on a local server, you use the following `dsn`:

```python
dsn = 'localhost/orclpdb'
```

### 4. Creating standalone connections.
You can create a connection by using the `cx_Oracle.connect()` method, and close the connection by `.close()`:

```python
import cx_Oracle
import config
 
connection = None
try:
    connection = cx_Oracle.connect(
        config.usrname,
        config.pwd,
        config.dsn,
        encoding=config.encoding)
 
    # show the version of the Oracle Database
    print(connection.version)
except cx_Oracle.Error as error:
    print(error)
finally:
    # release the connection
    if connection:
        connection.close()
```

Alternatively, you can let Python automatically closes the connection when the reference to the connection goes out of scope by using the `with` block:

```python
import cx_Oracle, config
 
try:
    with cx_Oracle.connect(
            config.username,
            config.password,
            config.dsn,
            encoding=config.encoding) as connection:
    # show the version of the Oracle Database
    print(connection.version)
except cx_Oracle.Error as error:
    print(error)
```

### 5. SQL queries execution
- Open cursor: `Cursor = connection.cursor()`
- Parse query(optional): `Cursor.parse(<sql>)`

> It can be used to validate sql statements before executing them. 
 

- Bind variable: placeholders, `:<var>` in the SQL statement that can be used to pass value
- Execute query: `Cursor.execute(<sql>)`
- Fetch content: `Cursor.fetchone()`, `Cursor.fetchmany()` or `Cursor.fetchall()`

> `fetchmany` actions can be fine-tuned by setting the `arraysize` attribute of `Cursor` which sets the number of rows to return from the database in each underlying request. The higher setting of `arraysize`, the fewer number of network round trips but the more memory required. The default value for `arraysize` is 100.

- Close cursor: `Cursor.close()`

> It will be closed automatically using a `with` block, like connection.

**Example:**

```python
import cx_Oracle
import config
 
 
def find_product(price, cost):
    """
    Find product name by price and cost
    :param price: price of the product
    :param cost: standard cost of the product
    :return: product name
    """
    sql = ("""select product_name 
    			 from products
	    		 where list_price > :price and
    		 	 standard_cost <= :cost""")
    try:
        # establish a new connection
        with cx_Oracle.connect(
                config.username,
                config.password,
                config.dsn,
                encoding=config.encoding) as connection:
            # create a cursor
            with connection.cursor() as cursor:
                for row in cursor.execute(sql, price=600, cost=500):
            	       price[row], cost[row] = cursor.fetchone()[:]
    except cx_Oracle.Error as error:
        print(error)
 
    return price, cost 
  
print(find_product(20, 100))
```
You can pass value to the bind variables:

- by names: `price=600, cost=500`
- by dictionary: `{price:600, cost:500}`
- by pisitions: `[600, 500]`

---
## Reference
- [cx_Oracle’s documentation](https://cx-oracle.readthedocs.io/en/latest/)
- [Python Oracle](https://www.oracletutorial.com/python-oracle/)

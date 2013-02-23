MySQL
=====

.. highlight:: cpp

The Mysql database module allows the programmer to connect to a MySQL database
and manipulate it through an easy to use set of methods.

Constants
---------

If any constant is defined, it should be listed here.

Data Types
----------

Mysql
^^^^^

This is the main class for this module. It represents a connection to the
specified database with the selected user/password/port combination provided in
the constructor or connect() method.
This class provides the following methods:
	
	* `connect()`_
	* `query()`_
	* `fetchRow()`_
	* `getError()`_
	* `getErrorNumber()`_

Methods
-------

new()
^^^^^

This is the constructor

.. _connect():

connect()
^^^^^^^^^

::

	bool connect(String host, String user, String password, String database[, int port = 3306])

Used to connect to the database.

**Parameters**

	*host* - The address of the host where to connect to

	*user* - The username used to connect to the database

	*password* - The password for the user

	*database* - The name of the database where to connect to

	*port* - Number of the port where the server is running. Default is 3306

**Return**

Returns **true** if the connection was successfull, **false** otherwise.

The error can be checked calling either the `getError()`_ or `getErrorNumber()`_ methods.

**Example**

::

	import db.mysql.*;
	import std.io.*;
	import std.reflection.*;

	var m = Mysql.new();
	if(m.connect("localhost", "user", "password", "clever_db")) {
	    println("Connected!");
	} else {
	    println("Error connecting");
	}

.. _query():

query()
^^^^^^^

::

	bool query(String query)

Allows the programmer to execute a query in the selected database. Only one query
is allowed per call. 

**Parameters**

	*query* - The query one wants to execute.

**Return**

Returns **true** if the query was successfully executed, **false** otherwise.

The error can be checked calling either the `getError()`_ or `getErrorNumber()`_ methods.

**Example**

::

	import db.mysql.*;
	import std.io.*;
	import std.reflection.*;

	var m = Mysql.new();
	if(m.connect("localhost", "user", "password", "clever_db")) {
	    println("Connected!");
	} else {
	    println("Error connecting");
	}

	if(m.query("select * from users")) {
		println("Succesfully executed query!");
	}


.. _fetchRow():

fetchRow()
^^^^^^^^^^

::

	Map fetchRow()

This method should be called after the `query()`_ method to retrieve the 
returned rows from the database one by one. Every time this method is called
the next row of the resultset will be retrieved from the database directly.

**Parameters**

	*none*

**Return**

Returns a ``map`` object in case there is something to be read from the
database. The indexes on the map are the name of the columns from your SQL
statement.
If no more data is found, it returns ``null``.

**Example**

::

	import db.mysql.*;
	import std.io.*;
	import std.reflection.*;

	var m = Mysql.new();
	if(m.connect("localhost", "user", "password", "clever_db")) {
	    println("Connected!");
	} else {
	    println("Error connecting");
	}

	if(m.query("select * from users")) {
		println("Succesfully executed query!");
	}

	data = m.fetchRow();
	while(data) {
	    println(data);
	    data = m.fetchRow();
	}


.. getError():

getError()
^^^^^^^^^^

Error method

.. getErrorNumber():

getErrorNumber()
^^^^^^^^^^^^^^^^

Error number method

template()
^^^^^^^^^^

::

	template(String param1[, ...])

Description of the method

**Parameters**

**Return**

**Example**

Examples
--------

Some more examples here :)
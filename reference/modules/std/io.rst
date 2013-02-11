I/O - Input/Output operation
=============================================

.. highlight:: cpp

----------
Functions
----------

* void **println([...])**:
   Prints the value of arguments supplied after calling its respective toString()
   methods (it includes an implict newline terminator).

::

	import std.io.*;
	println("Hello!"); // Displays 'Hello!' with a \n terminator

* void **print([...])**:
   Prints the value of arguments supplied after calling its respective toString()
   methods.

::

	import std.io.*;
	println("Hello!"); // Displays 'Hello!' without a \n terminator

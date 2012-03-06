I/O - Input/Output operation
=============================================

.. highlight:: cpp

----------
Functions
----------

* Void **println([...])**:
   Prints the value of arguments supplied after calling its respective toString()
   methods (it includes an implict newline terminator).

::

   import std.io as io;
   io::println("Hello!"); // Displays 'Hello!' with a \n terminator

* Void **print([...])**:
   Prints the value of arguments supplied after calling its respective toString()
   methods.

::

   import std.io as io;
   io::println("Hello!"); // Displays 'Hello!' without a \n terminator

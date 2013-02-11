Reflection
================================================

.. highlight:: cpp

----------
Functions
----------

* String **type(Object obj)**:
	Returns a String containing the type name of a variable.

* Int **refcount(Object obj)**:
	Returns the internal reference counting of the object.

::

    println(type(1)); // Int
    println(type(getcwd)); // Function

--------
Classes
--------

##################
Reflect
##################

**Methods**

* String **getType()**:
   Returns a string containing the object type name.

::

     println((Reflect.new(1)).getType()); // Int

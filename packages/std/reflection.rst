Reflection
================================================

----------
Functions
----------

* String **get_type(Object obj)**:
   Returns a String containing the type name of a variable.

.. sourcecode:: cpp

    println(get_type(1)); // Int
    println(get_type(getcwd)); // Function<String>


--------
Classes
--------

########################
ReflectionPackage class
########################

**Methods**

* String **getName()**:
   Returns a String containing the package name.

.. sourcecode:: cpp

     ReflectionPackage pkg("std");
     println(pkg.getName("std")); // displays 'std'

* Array<String> **getModules()**:
   Returns an Array<String> containing the packages' module.

.. sourcecode:: cpp

     ReflectionPackage pkg("std");
     println(pkg.getModules()); // displays '[math, io, etc]'

########################
ReflectionFunction class
########################

**Methods**

* Bool **isInternal()**:
   Returns a Bool indicating if the function is internal.

* Bool **isUserDefined()**:
   Returns a Bool indicating if the function is user-defined.

* String **getReturnType()**:
   Returns a String representing the name of return type.

.. sourcecode:: cpp

     ReflectionFunction func("getcwd");
     println(pkg.getReturnType(); // displays 'String'

* Int **getNumRequiredArgs()**:
   Returns an Int representing the number of required arguments.

.. sourcecode:: cpp

     ReflectionFunction func("max");
     println(pkg.getNumRequiredArgs(); // displays '2'

* Array<String> **getArgs()**:
   Returns an Array<String> containing the type name of arguments.

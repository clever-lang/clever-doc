Getting started!
================================================

.. highlight:: cpp

**Clever** is a general purpose statically typed, multi-paradigm
(object-oriented, generic, imperative) programming language written in
*C++*. It aims to have a small footprint and supply an useful set of
features, many seen in other languages.

-----------
Installing
-----------

To install Clever is easy, you will need to have *CMake* installed.
Enter in the clever source directory and run ``cmake .``

In this stage, you can help the *Clever Team* also by running ``make
run-tests`` and reporting to us if you get some failures!

-----------------
Native data types
-----------------

::

  Int i = 10;
  Double j = 1.2;
  String str = "foo";
  Bool is = true;
  Array<Int> arr;
  Map<Int, String> map;

Native data types in Clever are first-class object. This means that you
can do::

  "hello world!".toUpper()

################
const qualifier
################

You can mark a variable to not be changed along your code. Just use the
const qualifier. ::

  const Int i = 10;

-------------
Control flow
-------------

##############
if statements
##############

::

  if (bar()) {
  }

###############
for statements
###############

::

  for (Int i = 0; i < 10; ++i) {
  }

---------------------
Function declaration
---------------------

The syntax for declaring a function:

::

  Int foo(Int bar) {
    return bar;
  }

###################
Anonymous function
###################

::

  Auto func = Void () {
      println("hello world!");
  }
  func(); // Displays 'hello world!'

-----------------
Import statement
-----------------

Currently there are two way to use the import statement, and for both
we can use an alias to refer to its content. See below:

###################
Importing a module
###################

::

  import std.io;
  import std.regex as re;

########################
Importing a source file
########################

::

  import 'foo.clv';
  import 'bar.clv' as bar;

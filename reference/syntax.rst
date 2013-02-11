Syntax
=======================

.. highlight:: javascript

This document specifies the language's syntax by showing some code examples.
You can see the Clever parser grammar on :doc:`../internals/grammar` page.

Comments
--------

- Single line

Everything in a line after `//` is considered comment and it is discarded by the Clever interpreter.

::

	// Comment
	y = x; // Performs a copy

- Multi-line

Multi-line comments are similar to the C one.

::

	/* This comment can be
	  splitted in several lines. */


Variables
---------

- Clever is a dynamic typed language, so the type of a variable is bound in runtime. Use the keyword `var` to declare a variable.

::

	var a = 'foo'; // get_type(a) == String
	a = Foo.new(arg1, arg2); // get_type(a) == Foo

- Unitialized variables have value of `Null`

::

	var foo;
	assert(foo == null);


- Use the `const` qualifier to ensure that it will not be changed (i.e: assigned with other value)

::

	const foo = 'bar';
	foo = 'baz'; // throws an Exception


Functions
---------

- Functions are created with the `function` keyword

::

	function add(x, y) {
		return x + y;
	}

- Functions are objects and they can be assigned to variables

::

	var myfunction = function(x, y) { return x + y; };

- Functions can be passed as argument to functions, and even to be used as default value for parameters

::

	function test(msg, func = println) {
		func(msg);
	}
	test("hello"); // prints 'hello'


- Function call

::

	var foo = doFoo();
	var sum = add(1, 3);

- Chaining function call

::

	function abc() {
		return function() {
			return 3;
		}
	}
	println(abc()()); // 3

- Variadic functions

::

	function show(args...) {
		args.each(println);
	}
	show(1, "foobar");

- Closure

::

	var f = function (x) {
		return function () {
			return x;
		};
	};

	var a = f(123);
	var b = f(321);

	print(a(), "-", b()); // 123-321


Scope rules
-----------

Clever uses lexical scoping.

::

	var foo = 1;
	{
		var foo = 2;
		++foo;
	}
	println(foo); // 1

Native Data Types
-----------------

Examples of construction of native data types in Clever. For full reference (methods, properties, etc) please refer to: `Types`_

.. _Types: http://clever-lang.github.com/doc/reference/types/index.html

- String

::

	var str = 'fooo';

- Numeric

::

	var myint = 1;
	var otherint = 0xC1E4E8;
	var adouble = 3.141517;
	var biginteger = 1234567891011121314151617181920;

- Boolean

::

	var bool = (true || false);

- Array

::

	var arr = [1, 'foo', true, Foo.new(x)];

-  Element access

::

	var x = arr[0];
	var z = arr.at(0);

-  Write

::

	arr[2] = false;

- Map

::

	var map = {'name': 'Clever', 2: 'foo'};
	var empty = { : };

- Access

::

	var name = map['name']; // Null if an element with key 'name' doesn't exists

- Set

::

	map[3.1415] = 'pi';

User type
------------

- Creating a new type

The name rule for type creationg is: the name must start with an upper case letter.

::

	class Foo {
		var a;

		function setA(v) {
			this.a = v;
		}

		function getA() {
			return this.a;
		}
	}

- Property access

As seen below, access to properties are done by using the `this` variable.

- Class constructor

To declare a constructor you need to declare a function using the same name
than the type itself. See below:

::

	class Foo {
		function Foo() {
			// Constructor
		}
	}


Control Flow
------------

On condition, just the literal boolean `false` and `null` are evaluated to false
value. Everything else is a true value. This means the integer zero is evaluated
to true.

- If statements

::

	if (1 - 1) {
		// okay, 0 is true!
	}

- While

::

	while (foo() || bar()) {
		doBaz();
	}


- For

::

	for (i = 0; i < len; ++i) {
		update(i);
	}

	for (entry: container) {
	}


- Spawn statement creates a new thread or a thread vector.

::

	spawn thread_name {
		... // statements block.

		for (i = 0; i < n; ++i) {
			... // do something.
		}

		foo();
	}

	// or...

	spawn thread_name[2] { // create two threads.
		... // do something.
	}


-  Wait statement is used to waiting a thread or a thread vector finish.

::

	wait thread_name; // wait threads called "thread_name".


- Critical statement defines a critical section in the thread.

::

	spawn t {
		critical {
			doSomeCriticalOperation(); // here, you can read a file or a standard stream.
		}
	}



Errors and Exceptions
---------------------

- Syntax error

	On syntax error, the compilation is aborted.

- Runtime error

	Some internal methods can throw exceptions.

- Throwing exception

::

	try {
		throw 'test';
	} catch (e) {
		println(e); // test
	}

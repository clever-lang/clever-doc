Syntax
=======================

.. highlight:: javascript

This document specifies the language's syntax by showing some code examples.

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
	a = Foo(arg1, arg2); // get_type(a) == Foo

- Unitialized variables have value of `Null`

::

	var foo;
	assert(foo == Null);


- Use the `const` qualifier to ensure that it will not be changed (i.e: assigned with other value)

::

	const foo = 'bar';
	foo = 'baz'; // throws a RunTimeException


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

- Function call

::

	var foo = doFoo();
	var sum = add(1, 3);

Native Data Types
-----------------

Examples of construction of native data types in Clever. For full reference (methods, properties, etc) please refer to: // TODO(muriloadriano): url here

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

	var boolean = (true || false);

- Array

::

	var arr = [1, 'foo', true, Foo(x)];

- Element access

::

	var x = arr[0];

- Write

::

	arr[2] = false;

- Map

::

	var map = {'name': 'Clever', 2: 'foo'};

- Access

::

	var name = map['name']; // Null if an element with key 'name' doesn't exists

- Set

::

	map[3.1415] = 'pi';

Flow Control
------------

- If statements

::

	if (x + y < z) {
		foo();
	}
	else if (y + z < w) {
		bar();
	}
	else {
		baz();
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

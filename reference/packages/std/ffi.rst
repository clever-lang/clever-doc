FFI - Foreign Function Interface
================================================

.. highlight:: cpp

The FFI module is the Clever interface to external functions. This module
is a wrapper around the libffi library, which provides means to use C
and C and C++ functions contained on dynamically linked libraries (.dll on
Windows, .so on Linux).

Using FFI, one can easily extend Clever without the need for recompilation.
You could even load libraries written in other languages, as long as it's
available as a shared object (so) or dynamically linked library (dll).

A Simple Example
--------------------

In this section, it's presented how to access some simple C functions
using the FFI module.

The first step is to describe the library's interface. You can do this using
an `extern` block like this:

::

	import std;

	extern "path" {

		/*
			Call function function_name_1 on library "path".
		*/

		return_type function_name_1(type_arg1 arg1, type_arg2 arg2, ... );

		/*
			Call "function_name_2_on_path" on library "path" using
			alias function_name_2.
		*/
		return_type function_name_2(type_arg1  arg1, type_arg2 arg2, ... )
					as "function_name_2_on_path";

		...

	}


Where "path" is the path to shared library. The keyword **extern** is used
to indicate that next function declarations are external functions contained
on shared library "path".

After we've created the interface, we can use normally the functions defined.
To illustrate, let's build a shared library on Linux and call its functions
using Clever. Obviously, first, we need write the C code:

::

	//File: hello.c
	//Description: A very simple C library
	#include <stdio.h>

	void hello() {
		printf("Hello Clever FFI Module!\n");
	}

	int add(int a, int b) {
		return a+b;
	}

	void printExt(char* s) {
		printf("S=%s\n",s);
	}

	int _sub(int a, int b){
		return a-b;
	}



To build the shared library using a Linux OS, we can use the command:


.. sourcecode:: sh

	gcc -fPIC -O3 -shared -c hello.c
	gcc -shared -Wl,-soname,hello.so -o hello.so hello.o -lc -ldl




The last command generate a shared library called "hello.so". Now, we can access
its functions in Clever using:

::

	//File: hello.clv
	//Description: A simple FFI example

	import std;

	extern "./hello" {
		Void hello();
		Int add(Int a, Int b);
		Void printExt(String s) as "printExt";
		Int sub(Int a, Int b) as "_sub";
	}

	hello();

	Int res = add(2, 3);

	print("res=",res,"\n");
	printExt("Ola!\n");
	println(" 10", "-", " 7", "___", "  " + sub(10, 7).toString());



The alias is an important thing because allow we access in Clever functions with forbidden
names like "_sub" on last example.


The FFIObject
--------------

Sometimes, we use a library that manipulates some kind of class/struct.
Unfortunatelly, Clever doesn't provide external class/struct mapping, but it
does provide a workaround for these cases: the `FFIObject`.

The `FFIObject` is a passive intermediate data structure that serves as a
representation of the library's data structures or just as placeholder to
pointers and C++ references.

Using the type alias, we can build ADT (Abstract Data Type) like:

::

	import std;

	use BigInt as std.ffi::FFIObject;


And, using the alias previously definied, we can build a simple interface to libgmp:

::

	import std;

	use BigInt as std.ffi::FFIObject;

	extern "./gmp_cpp" {
		BigInt init_bi();
		BigInt add_bi(BigInt res, BigInt a, BigInt b);
		Void set_str_bi(BigInt op, String str, Int base);
		Void set_bi(BigInt op1, BigInt op2);
		Void clear_bi(BigInt x);

		String get_str_bi(BigInt op, Int base);
	}

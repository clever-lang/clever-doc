FFI - Foreign Function Interface
================================================

.. highlight:: cpp

The FFI module is the Clever interface to external functions. This module
allows to use C and C++ functions contained on Shared Libraries (.dlls,
.so,...) in Clever scripts. Using FFI, it's possible easily to extend
Clever using your favorite language, since many languages have C API.



A Simple Example
--------------------

In this section, it's presented how to access some simple C functions
using FFI module.

To use a C function in a Clever script, firstly, it's necessary write
an interface like this:

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
using clever. Obviously, first, we need write the C code:

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

Sometimes, we need to save data of an object definied in a shared library, as Clever can't
manipulate it  directly, so it's necessary a passive data strucuture to act like a 
intermediate. FFIObject is this intermediate (like a C++ reference, per example).


The FFIObject is a special type, because its function is only save a pointer to data 
structures created in a shared library. It's a **passive** structure.

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







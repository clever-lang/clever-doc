FFI - Foreign Function Interface
================================================

The FFI module is the Clever interface to external functions. This module allows to use C and C++ functions contained on Shared Libraries (.dlls, .so,...) in Clever scripts. Using FFI, it's possible easily to extend Clever using your favorite language, since many languages have C API.



A Simple Example
--------------------

In this section, it's presented how to access some simple C functions using FFI module.

To use a C function in a Clever script, firstly, it's necessary write an interface like this:

.. sourcecode:: c++

	//File: hello.clvh                   
	//Description: A simple FFI example  

	import std;
	
	extern "path" {
		//Call function function_name_1 on library "path".
		return_type function_name_1(arg1, arg2, ... );

		//Call "function_name_2_on_path" on library "path" using alias function_name_2.
		return_type function_name_2(arg1, arg2, ... ) as "function_name_2_on_path";
		...
	}



Where "path" is the path to shared library. The keyword **extern** is used to indicate that next function declarations are external functions contained on shared library "path". 
	

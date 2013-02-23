Creating a new module
=====================

.. highlight:: c++

Currently a module in Clever can contain:

  * Classes
  * Functions
  * Constants

In order to create a module you need to create a class which inherits from
the Module class. Just like a package, the Module class also has a pure
virtual method ``init()`` that performs the initialization tasks.

We will work our way into creating a Hello World module that will contain all above
mentioned items.

The Module
==========

This will be a module inside the std module and it will be called *std.hello*.

This module will contain the following:
  * A HelloWorld class with the following methods
    * sayHello()
    * sayMyName(whatToSay)
  * A function called saySomething(name)
  * A constant CONST_HELLO_WORLD

We will discuss how the code needs to be implemented and the changes needed
in the build scripts to get the code to compile and work!

Recipe
------

Let's start with a recipe of the steps we need to execute to accomplish the task
of creating a module. This could be handy to be used when creating your modules.

  1. Create the directory with our module name inside the top level module we want
  2. Create the module.h file as explained in the next section
  3. Create the module.cc file as explained in the next section
  4. Create the class that extends the Type class (hello.cc in the examples below)
  5. Create the CMakeLists.txt file in the directory we created in step 1
  6. Add a directive for our module in modules.cmake in the root directory
  7. Add the ifdef directive in the \*_pkg.cc file from our top level module
  8. Add the ifdef directive in the \*_forward.h file from our top level module
  9. Add the if (MOD_*) directive in the CMakeLists.txt from our top level module
  10. Compile the whole thing

Creating the HelloWorld class
-----------------------------

First thing we should create a directory called **hello** inside the **modules/std/**
directory. This is where all our files will be placed.

Create a file name **module.cc** and add the following code to it:

::

  /**
   * Clever programming language
   * Copyright (c) Clever Team
   *
   * This file is distributed under the MIT license. See LICENSE for details.
   */

  #include "modules/std/hello/module.h"
  #include "modules/std/hello/hello.h"

  namespace clever { namespace modules { namespace std {

  // Creates the HelloModule::init() method
  // At the time of this writing, the CLEVER_MODULE_INIT expands to:
  // #define CLEVER_MODULE_INIT(x) void x::init()
  // This directive should always be used as the developers of the language
  // may have the need to change the way the init function works, and this lets
  // us do it easily.
  // This is the method that will be called when an "import std.hello.*" is
  // executed. It will cause all the 'things' that the HelloModule exports to be
  // available for use in your Clever code!
  CLEVER_MODULE_INIT(HelloModule)
  {
    // Here we add the type to Clever. The name of the class to be used
    // for instantiating the object is the name of the type that will be
    // given in the Hello class constructor on hello.h
    // This means that in order to get our object, we will need to do the
    // following:
    //
    // var x = HelloWorld.new();
    addType(new Hello);
  }

  }}} // clever::modules::std

And the corresponding **module.h** file:

::

  /**
   * Clever programming language
   * Copyright (c) Clever Team
   *
   * This file is distributed under the MIT license. See LICENSE for details.
   */

  #ifndef CLEVER_STD_HELLO_MODULE_H
  #define CLEVER_STD_HELLO_MODULE_H

  #include "core/module.h"

  namespace clever { namespace modules { namespace std {

  /// Hello Module
  class HelloModule : public Module {
  public:

    // This here defines the name we will be using in the import directive
    // in our .clv file
    // For this case it will be
    // import std.hello.*;
    HelloModule()
      : Module("std.hello") {}

    ~HelloModule() {}

    CLEVER_MODULE_VIRTUAL_METHODS_DECLARATION;
  private:
    DISALLOW_COPY_AND_ASSIGN(HelloModule);
  };

  }}} // clever::modules::std

  #endif // CLEVER_STD_HELLO_MODULE_H

These are the files responsible for telling Clever that we have created
a module that will be accessed by the import *std.hello* (as defined in the
constructor for HelloModule).
This is the default sketch for every module we will be writing for Clever.
In the **module.cc** file we tell that this module is using the **Hello** class
to create the type that is defined in the **hello.h** file (in the Hello class
constructor).

::

  CLEVER_MODULE_INIT(HelloModule)
  {
    addType(new Hello);
  }


Now, what we need to do is to actually implement the **Hello** class where
our Clever HelloWorld class will be, together with the **saySomething()**
function and the CONST_HELLO_WORLD constant.

For this, create a **hello.h** file in the same directory, and add the following
content to it:

::

  /**
   * Clever programming language
   * Copyright (c) Clever Team
   *
   * This file is distributed under the MIT license. See LICENSE for details.
   */

  #ifndef CLEVER_STD_HELLO_H
  #define CLEVER_STD_HELLO_H

  #include <iostream>
  #include "core/cstring.h"
  #include "core/value.h"
  #include "types/type.h"

  namespace clever { namespace modules { namespace std {

  class HelloObject : public TypeObject {
  public:
    HelloObject() {}

    ~HelloObject() {}

  private:

    DISALLOW_COPY_AND_ASSIGN(HelloObject);
  };

  class Hello : public Type {
  public:

    // Here we define the name of the type we are creating (that is also used
    // as the name of the class)
    // This can be seen when we use the reflection API to get the type
    // of our object
    //
    // import std.hello.*;
    // import std.io.*;
    // import std.reflection.*;
    //
    // var x = HelloWorld.new();
    // println(type(x));
    Hello()
      : Type("HelloWorld") {}

    ~Hello() {}

    void init();

    TypeObject* allocData(CLEVER_TYPE_CTOR_ARGS) const;
    void deallocData(void*);

    void dump(TypeObject*, ::std::ostream&) const;

    CLEVER_METHOD(ctor);
    CLEVER_METHOD(sayHello);
    CLEVER_METHOD(sayMyName);

  private:
    DISALLOW_COPY_AND_ASSIGN(Hello);
  };

  }}} // clever::modules::std

  #endif // CLEVER_STD_HELLO_H


And the **hello.cc** file:

::

  /**
   * Clever programming language
   * Copyright (c) Clever Team
   *
   * This file is distributed under the MIT license. See LICENSE for details.
   */

  #include "core/value.h"
  #include "types/native_types.h"
  #include "core/modmanager.h"
  #include "modules/std/hello/hello.h"
  #include "modules/std/io/io.h"

  namespace clever { namespace modules { namespace std {

  // TODO: Explain this and how the HelloObject relates to the Hello class
  TypeObject* Hello::allocData(CLEVER_TYPE_CTOR_ARGS) const
  {
    HelloObject* obj = new HelloObject();

    return obj;
  }

  // This will be called to destroy our object
  void Hello::deallocData(void *data)
  {
    HelloObject* intern = static_cast<HelloObject*>(data);

    if (intern) {
      delete intern;
    }
  }

  // This is the method that is called when you try to print the object
  // For example
  // import std.hello.*;
  // import std.io.*;
  //
  // var x = HelloWorld.new();
  // println(x);
  void Hello::dump(TypeObject* data, ::std::ostream& out) const
  {
    const HelloObject* uvalue = static_cast<const HelloObject*>(data);

    if (uvalue) {
      out << "You just printed a HelloWorld object!";
    }
  }

  // This is the constructor method.
  // This is called every time you create a new instance of the HelloWorld class
  //
  // import std.hello.*;
  // var x = HelloWorld.new();
  CLEVER_METHOD(Hello::ctor)
  {
    result->setObj(this, allocData(&args));
  }

  CLEVER_METHOD(Hello::sayHello)
  {
    ::std::cout << "Hello World!!" << "\n";
  }

  CLEVER_METHOD(Hello::sayMyName)
  {
    if (!clever_check_args("s")) {
      return;
    }

    ::std::cout << "Your name is: " << args[0]->getStr()->c_str() << "\n";

  }

  // Type initialization
  // This will be called when you declare the import for this module in your
  // clever file
  //
  // import std.hello.*;
  CLEVER_TYPE_INIT(Hello::init)
  {

    // Here we define the Hello::ctor method as the constructor for our class
    setConstructor((MethodPtr) &Hello::ctor);

    // Here we add the methods to our class
    addMethod(new Function("sayHello",  (MethodPtr) &Hello::sayHello));
    addMethod(new Function("sayMyName",  (MethodPtr) &Hello::sayMyName));

  }

  }}} // clever::modules::std


At this point the code is complete. All that is left to be done is adding
some references to our module in the top level module headers and adding
some compiler directives to the compilation scripts.

In the **modules/std/hello/** directory create a CMakeLists.txt file with
the following content

::

  add_library(modules_std_hello STATIC
    module.cc
    hello.cc
  )

In the file **modules.cmake** add the following at the begining of the file

::

  clever_add_module(std_hello      ON  "enable the hello module"      "")

And the following at the end

::

  # std.hello
  if (MOD_STD_HELLO)
    add_definitions(-DHAVE_MOD_STD_HELLO)
  endif (MOD_STD_HELLO)

  clever_module_msg(std_hello ${MOD_STD_HELLO})

Open the file **modules/std/std_pkg.cc** and add the following:

::

  #ifdef HAVE_MOD_STD_HELLO
    addModule(new std::HelloModule);
  #endif

Now open **modules/std/std_forwarder.h** and add this:

::

  #ifdef HAVE_MOD_STD_HELLO
  # include "modules/std/hello/module.h"
  #endif

And finally, to finish it, add the following to **modules/std/CMakeLists.txt**

::

  if (MOD_STD_HELLO)
    list(APPEND CLEVER_MODULES hello)
  endif (MOD_STD_HELLO)

After all this, all you have to do is compile everything!

::

  $ cmake . && make

As a test code, use the following:

::

  import std.hello.*;
  import std.io.*;
  import std.reflection.*;

  var x = HelloWorld.new();
  println(type(x));
  println(x);

  x.sayHello();
  x.sayMyName("Clever");
  x.sayMyName(x);


Adding the function
-------------------

.. :todo:: Needs to be done


Adding the constant
-------------------

.. :todo:: Needs to be done

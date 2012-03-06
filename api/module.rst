Creating a new module
================================

.. highlight:: cpp

Currently a module in Clever can contains: Classes, functions and
constants. To implement a module, you need to create a class which
inherits from Module class. Just like a package, the Module class also
has a pure virtual method ``init()`` what performs similar task.

See an example:

::

  #include "compiler/module.h"

  namespace clever { namespace packages { namespace std {

  class IOModule : public Module {
  public:
	  IOModule()
  		: Module("io") { }

	  ~IOModule() { }

	  void init();
  private:
	  DISALLOW_COPY_AND_ASSIGN(IOModule);
  };

  }}} // clever::packages::std


In the ``init()`` process we must have to add the class/functions
specifications. See below:

::

  namespace clever { namespace packages { namespace std {

  namespace io {
  /**
   * println(object a, [ ...])
   * Prints the object values without trailing newline
   */
  static CLEVER_FUNCTION(print) {
	  for (size_t i = 0, size = CLEVER_NUM_ARGS(); i < size; ++i) {
  		::std::cout << CLEVER_ARG_AS_STR(i);
	  }
  }

  } // namespace io

  /**
   * Initializes Standard module
   */
  void IOModule::init() {
	  addFunction(new Function("print", &CLEVER_NS_FNAME(io, print), CLEVER_VOID))
		  ->setVariadic()
		  ->setMinNumArgs(1);
  }

  } // clever::packages::std

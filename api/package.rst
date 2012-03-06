Creating a new package
==============================

.. highlight:: cpp

A package in Clever is just like a namespace, but it only can contains
modules. To implement a new package, you must implement a pure virtual
``init()`` method which import its modules, and specify the inheritance
from *Package* class, see below:

::

  #include "compiler/module.h"

  namespace clever { namespace packages {

  class Std : public Package {
  public:
	  Std()
		  : Package("std") { }

  	  ~Std() { }

	  void init();

	  const char* getVersion() const { return NULL; }
  private:
	  DISALLOW_COPY_AND_ASSIGN(Std);
  };

  }} // clever::std_pkg


In the ``init()`` process the package just have to add the modules, such
task is done by the method ``addModule()`` from **Package** class. See
an example below:

::

  namespace clever { namespace packages {

  /**
   * Initializes Std package
   */
  void Std::init() {
	addModule(new std::IOModule);
  }

  }} // clever::packages

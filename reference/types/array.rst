Array
==========================

.. highlight:: javascript

**Methods**

* void append(arg [, ...])
	Adds new entrie(s) to the array.

::

	var arr = [1, 2];
	arr.append(3);

	println(arr); // [1, 2, 3]

* void each(Function callback):
	Executes a supplied function on each array element.

::

	var arr = [1, 2, 3];

	arr.each(function(x) { println(x); });


* mixed at(Int position):
	Returns the specified element in the array if it exists.

::

	var arr = [3];

	println(arr.at(0)); // 3


* Int size():
	Returns the array size.

::

	println([1, 2, 3].size()); // 3

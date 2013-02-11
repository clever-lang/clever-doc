Clever VM
=========

.. highlight:: cpp

Instructions
------------

Clever VM works using TAC (thread-address code [#f1]_) to describe its intermediate
code. Currently it's represented as:

::

	struct IR {
		Opcode opcode;
		Operand op1, op2, result;
		location loc;
	};

See `core/ir.h` for futher details.

Where the operand is represented as:

::

	struct Operand {
		OperandType op_type;
		ValueOffset voffset;
		size_t jmp_addr;
	};

The operand can work with four diferent informations, see below:

+--------------+-------------------------------------------------------+
| Operand type | Description                                           |
+==============+=======================================================+
| FETCH_VAR    | For fetching a variable from the environment          |
+--------------+-------------------------------------------------------+
| FETCH_CONST  | For fetching a constant from the environment          |
+--------------+-------------------------------------------------------+
| FETCH_TMP    | For fetching a temporary storage from the environment |
+--------------+-------------------------------------------------------+
| JMP_ADDR     | For jumping over the code                             |
+--------------+-------------------------------------------------------+

For futher details on Environment, see our wiki [#f2]_.

Basically, each function and class has its environment. Global scope and its
local scope uses the same environment. In compiler-time we assign an identifier
for each variable name to its respective index relative to the environment which
it is found. For threating the relative location of the variable, we assign a
identifier to the relative environment, beyond the index for the variable.

Hence 'FETCH_VAR` means that a variable must be fetched according to its relative
environment location.

For constant values (literal ones like `true`, `1`, etc), we do use a constant
environment which is global for the execution, thus using `FETCH_CONST`
to fetch the literal value based on the index associated to it on compile-time.

The `FETCH_TMP` works under the temporary environment which is located in the
current environment. This is, each environment can have its temporary environment
to store computed result to future access. (e.g. 1 + 1 / 2)

And about `JMP_ADDR` we can describe it as an address to the instrunction pointer
to be jumped over, like a `goto` operation.

Opcodes
-------

A list of our available opcodes can be seen in the file `core/opcode.h`.

When building Clever in debug mode, the option `-d` is available to provide us
a way to see the generated code. See below:

::

	$ ./clever -d -qr 'println(1);'
	[000] OP_SEND_VAL  |  0:  3  (FETCH_CONST) |          (UNUSED     ) |          (UNUSED     ) |
	[001] OP_FCALL     |  0: 41  (FETCH_VAR  ) |          (UNUSED     ) |   0:  0  (FETCH_TMP  ) |
	[002] OP_HALT      |         (UNUSED     ) |          (UNUSED     ) |          (UNUSED     ) |





.. [#f1] http://en.wikipedia.org/wiki/Three_address_code
.. [#f2] https://github.com/clever-lang/clever/wiki/RFC-%230002----Lexical-scopes,-environments,-stack-frames-and-the-call-stack

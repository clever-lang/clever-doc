Clever grammar
==============

Clever uses Bison [#f1]_ and re2c [#f2]_ tools to generate your parser and lexer.

Our grammar is described as follows below:

::

	top_statements:
			statement_list
	;

	statement_list:
			/* empty */
		|	non_empty_statement_list
	;

	non_empty_statement_list:
			var_declaration ';'
	;

	var_declaration:
			VAR IDENT
	;



.. [#f1] http://www.gnu.org/software/bison/
.. [#f2] http://re2c.org/

COMP 520: Compilers
===================
PA2 - Abstract Syntax Trees
===========================

The second milestone in the compiler project is to create an abstract syntax 
tree (AST) for any miniJava program that is syntactically valid according to 
our miniJava grammar. This assignment requires you to incorporate Java 
operator precedence rules and to build a correct AST using a set of AST 
classes outlined in this document and available as a package on our course 
website.

miniJava syntax changes
-----------------------

The grammar for this assignment is the miniJava grammar from the first 
assignment. However, you should no loger allow `--` to be parsed as two 
subtraction operators. In full Java `--` is a prefix and postfix operator 
applied to a variable to predecrement or postdecrement the value of a 
variable referenced in an expression, respectively. Since we will not 
implement this operator in miniJava, any expression involving `--` should 
be disallowed in miniJava. Here are some examples.

Valid miniJava expressions:
`-b    -(-b)    - -b     a-(-b) !b !!b`

Invalid miniJava
`--b a - --b`

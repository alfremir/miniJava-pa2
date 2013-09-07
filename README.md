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
    -b    
    -(-b)    
    - -b     
    a-(-b)
    !b 
    !!b

Invalid miniJava
    --b 
    a - --b
    a---b
    a--+--b

Operator precedence in expressions
----------------------------------

In Java the evaluation order of expressions is controlled by parentheses and 
by standard operator precedence rules from arithmetic and predicate logic. The 
following table lists the precedence order of the miniJava operators from 
lowest to highest.

class          operator(s)
disjunction    ||
conjunction    &&
equality       ==, !=
relational     <=, <, >, >=
additive       +, -
multiplicative *, /
unary          -, !

Binary operators are left associative, so that `1-2+3` means `(1-2)+3`, and 
`1+3*4/2` means `1+((3*4)/2)`. Unary operators are right associative. The 
challenge in this part of the assignment is to construct a stratified grammar 
reflecting the precedence shown above that also accomodates explicit 
precedence using parentheses. The correct AST can be constructed in the 
course of parsing such a grammar.

Abstract syntax tree classes
----------------------------

The set of classes needed to build miniJava ASTs are provided in the 
`AbstractSyntaxTrees` package. Components of the AST "grammar" are organizsed 
by the class hierarchy shown at the end. Abstract classes (shown with an "A" 
superscript next to the class icon) represent nonterminals of the AST grammar, 
such as `Statement`. The rule for `Statement` below shows the particular kinds 
of statements that may be created in an AST; each corresponds to a concrete 
class in the hierarchy. For example, a `WhileStmt` is a specific kind of 
`Statement`, and consists of an `Expression` (for the condition controlling 
execution of the loop) and a `Statement` (for the body of the loop).

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

*Valid* miniJava expressions:

    -b    
    -(-b)    
    - -b     
    a-(-b)
    !b 
    !!b

*Invalid* miniJava

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
`AbstractSyntaxTrees` package. Components of the AST "grammar" are organized 
by the class hierarchy shown at the end. Abstract classes (shown with an "A" 
superscript next to the class icon) represent nonterminals of the AST grammar, 
such as *Statement*. The rule for *Statement* below shows the particular kinds 
of statements that may be created in an AST; each corresponds to a concrete 
class in the hierarchy. For example, a `WhileStmt` is a specific kind of 
*Statement*, and consists of an *Expression* (for the condition controlling 
execution of the loop) and a *Statement* (for the body of the loop).

    Statement ::=
                Reference Expression
              | Statement*
              | Reference Expression*
              | Expression Statement Statement?
              | VarDecl Expression
              | Expression Statement

If we look inside the concrete class `WhileStmt` we find the following:

    public class WhileStmt extends Statement {

        public WhileStmt(Expression e, Statement s, SourcePosition posn) {
            super(posn);
            cond = e;
            body = s;
        }

        public Expression cond;
        public Statement body;
    }

The constructor creates a `WhileStmt` node, and its two fields provide access 
to the AST subtrees of the node (the expression `cond` controlling the loop 
repetition and the statement `body` to be executed in each repetition). Note 
the nomenclature, each kind of `Statement` has a particular name suggesting 
its kind (e.g. `While`) joined to `Stmt` to show the nonterminal from which it 
derives.

Consult the documentation, source files, and AST constructed for the sample 
program to make sure you understand the contents and structure of the AST 
classes. Note the classes make use of Java 1.5 features (enums, generics, and 
the extended `for` statement). Some auxiliary classes are included to provide 
a convenient way to create lists of Nonterminals such as the `StatementList` 
in the `BlockStmt`. The "start symbol" of the AST grammar is `Package`. A 
legal miniJava program should correspond to an AST with a `Package` root node 
that will contain a list of children, each of which is a `ClassDecl`.

The AST Visitor
---------------

The `AbstractSyntaxTrees` package includes an *interface* describing a visitor
to traverse an AST and an `ASTDisplay` *implementation* of the visitor to 
display an AST (or any AST subtree) in text form. Use this facility to inspect 
the ASTs you generate. The AST display will also list the source positions for 
each AST node if you enable the capability in `ASTDisplay` and provide an 
appropiate `toString()` method for `SourcePosition`. For these values to be 
meaningful, you need to set the source position correctly in the parser. It is 
useful for every AST node to have an associated source position that can be 
used for error reporting in later stages, but at this stage it is not required 
and will, by default, not be displayed in the AST. However to create an AST 
you will have to provide a `SourcePosition` for each node (which can be 
`null`).

Programming Assignment
----------------------

For PA2 you will make modifications in the `miniJava.SyntacticAnalyzer` 
package to construct a correct AST using the supplied 
`miniJava.AbstractSyntaxTrees` package. Your Compiler mainclass should 
determine if the input file constitutes a syntactically valid miniJava program 
as defined by PA1 and definitions above. If so, it should display the AST 
constructed (using the `showTree` method supplied in the `ASTDisplay` class), 
and then `System.exit(0)`. (Note: as distributed, `ASTDisplay` does not 
display source position, but it is an option that can be enabled - do not 
enable it in your submission!). If the input file is syntactically valid, you 
should write a diagnostic error message and terminate via `System.exit(4)`. 
You may output any additional information you wish from your compiler.

For valid miniJava programs the testing will check that you return exit code 0 
and that the AST you display matches the expected AST, and for invalid 
programs it will check that you return exit code 4.

    AST
        Declaration
            ClassDecl
            LocalDecl
                ParameterDecl
                VarDecl
            MemberDecl
                FieldDecl
                MethodDecl
        Expression
            BinaryExpr
            CallExpr
            LiteralExpr
            NewExpr
            RefExpr
            UnaryExpr
        Package
        Reference
            IndexedRef
            QualifiedRef
        Statement
            AssignStmt
            BlockStmt
            CallStmt
            IfStmt
            VarDeclStmt
            WhileStmt
        Terminal
            Identifier
            Literal
                BooleanLiteral
                IntLiteral
            Operator
        Type
            ArrayType
            BaseType
            ClassType

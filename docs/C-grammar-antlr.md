# Appendix C – Grammar (ANTLR v4 Draft)

This appendix provides a **draft ANTLR v4 grammar** for the ClearSharp language.

The grammar is:
- intentionally conservative
- designed for readability
- suitable as a starting point for a reference compiler

This grammar is **not final** and may evolve as the specification matures.

---

## 1. Grammar Conventions

- Keywords are case-insensitive (handled by lexer)
- Whitespace and newlines are insignificant except as separators
- Explicit block terminators are required
- Indentation has no semantic meaning

---

## 2. Lexer Grammar (ClearSharpLexer.g4)

```antlr
lexer grammar ClearSharpLexer;

WHITESPACE : [ \t\r\n]+ -> skip ;
COMMENT    : '//' ~[\r\n]* -> skip ;

IDENTIFIER : [a-zA-Z_][a-zA-Z0-9_]* ;

INTEGER    : [0-9]+ ;
DECIMAL    : [0-9]+ '.' [0-9]+ ;
STRING     : '"' (~["\\] | '\\' .)* '"' ;

LBRACE     : '{' ;
RBRACE     : '}' ;
LPAREN     : '(' ;
RPAREN     : ')' ;
LBRACK     : '[' ;
RBRACK     : ']' ;

PLUS       : '+' ;
MINUS      : '-' ;
STAR       : '*' ;
SLASH      : '/' ;
EQUAL      : '=' ;
NOTEQUAL   : '<>' ;
LT         : '<' ;
LTE        : '<=' ;
GT         : '>' ;
GTE        : '>=' ;
DOT        : '.' ;
COMMA      : ',' ;

```

---

## 3. Parser Grammar (ClearSharpParser.g4)

```antlr
parser grammar ClearSharpParser;

options { tokenVocab=ClearSharpLexer; }

program
    : declaration* EOF
    ;

declaration
    : serviceDecl
    | actionDecl
    | rulesDecl
    | resourceDecl
    ;

serviceDecl
    : 'service' IDENTIFIER serviceBody 'end' 'service'
    ;

serviceBody
    : (needsDecl | actionDecl)*
    ;

needsDecl
    : 'needs' IDENTIFIER 'as' IDENTIFIER
    ;

actionDecl
    : 'action' IDENTIFIER paramList? block 'end' 'action'
    ;

paramList
    : LPAREN param (COMMA param)* RPAREN
    ;

param
    : IDENTIFIER 'as' IDENTIFIER ('=' expression)? ('required')?
    ;

rulesDecl
    : 'rules' IDENTIFIER 'for' IDENTIFIER block 'end' 'rules'
    ;

resourceDecl
    : datasourceDecl
    | apiDecl
    | messageDecl
    ;

datasourceDecl
    : 'datasource' IDENTIFIER block 'end' 'datasource'
    ;

apiDecl
    : 'api' IDENTIFIER block 'end' 'api'
    ;

messageDecl
    : 'message' IDENTIFIER messageField* 'end' 'message'
    ;

messageField
    : IDENTIFIER 'as' IDENTIFIER ('required')?
    ;

block
    : statement*
    ;

statement
    : declarationStmt
    | assignmentStmt
    | ifStmt
    | forEachStmt
    | tryStmt
    | expressionStmt
    ;

ifStmt
    : 'if' expression 'then' block ('else' block)? 'end' 'if'
    ;

forEachStmt
    : 'for' 'each' IDENTIFIER 'in' expression 'do' block 'end' 'for'
    ;

tryStmt
    : 'try' block ('catch' IDENTIFIER block)? ('finally' block)? 'end' 'try'
    ;

assignmentStmt
    : expression '=' expression
    ;

expressionStmt
    : expression
    ;

expression
    : primary (operator primary)*
    ;

primary
    : IDENTIFIER
    | INTEGER
    | DECIMAL
    | STRING
    | 'Nothing'
    | LPAREN expression RPAREN
    ;

operator
    : PLUS | MINUS | STAR | SLASH
    | EQUAL | NOTEQUAL | LT | LTE | GT | GTE
    | 'and' | 'or' | 'not'
    ;
```

---

## 4. Notes and Limitations

This grammar:
- omits precedence handling (to be added via rules)
- simplifies expression parsing
- does not include full resource syntax
- does not include generators or concurrency blocks

These omissions are intentional for a first-pass grammar.

---

## 5. Next Steps

Recommended next steps:

1. Split lexer and parser grammars
2. Add keyword channels for case-insensitivity
3. Implement precedence climbing for expressions
4. Extend grammar incrementally with:
   - resources
   - concurrency
   - generators

---

### End of Appendix C

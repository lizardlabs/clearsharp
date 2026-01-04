# Part II – Lexical Structure

## Table of Contents

1. Source Text  
2. Character Set and Encoding  
3. Line Terminators and Whitespace  
4. Comments  
5. Case Insensitivity and Identifier Normalization  
6. Tokens  
   6.1 Identifiers  
   6.2 Keywords  
   6.3 Literals  
   6.4 Operators and Delimiters  
7. Keywords  
   7.1 Reserved Keywords  
   7.2 Contextual Keywords  
   7.3 Future-Reserved Keywords  

---

## 1. Source Text

A ClearSharp program consists of one or more **source files**.

Each source file is a sequence of Unicode characters that is processed as input to the ClearSharp compiler.

The compilation unit is the file; higher-level composition (projects, folders) is defined in Part III.

---

## 2. Character Set and Encoding

ClearSharp source files:
- are Unicode text
- are expected to be UTF-8 encoded
- may contain Unicode characters in identifiers, strings, and comments

---

## 3. Line Terminators and Whitespace

### 3.1 Line Terminators

The following sequences are recognized as line terminators:
- \r
- \n
- \r\n

Line terminators have no semantic meaning except where they terminate comments or string literals.

### 3.2 Whitespace

Whitespace consists of spaces, tabs, and line terminators.

Indentation has **no semantic meaning** in ClearSharp.

---

## 4. Comments

### 4.1 Line Comments

Line comments begin with `//` and continue to the end of the line.

### 4.2 Block Comments

Block comments begin with `/*` and end with `*/`.  
Block comments may span multiple lines but must not be nested.

---

## 5. Case Insensitivity and Identifier Normalization

ClearSharp is **case-insensitive everywhere**.

Identifiers are normalized using Unicode case-folding rules for comparison.  
Original casing may be preserved for diagnostics and code generation.

---

## 6. Tokens

Tokens are classified into identifiers, keywords, literals, operators, and delimiters.  
The lexer applies a maximal munch strategy.

---

## 6.1 Identifiers

Identifiers name program entities such as variables, actions, services, and types.

Rules:
- must start with a letter or underscore
- followed by letters, digits, or underscores
- must not be keywords
- are case-insensitive

---

## 6.2 Keywords

Keywords are reserved identifiers with predefined meaning.  
They cannot be used as identifiers.

---

## 6.3 Literals

### 6.3.1 String Literals

- Double-quoted strings: `"text"`
- Multiline strings using triple quotes:
```
"""
multiline
text
"""
```

### 6.3.2 Numeric Literals

- Decimal notation only
- No type suffixes (e.g., no `D`, `M`, `F`)
- Type inferred from context

### 6.3.3 Boolean Literals

```
true
false
```

### 6.3.4 Nothing Literal

```
Nothing
```

Represents absence of a value.

---

## 6.4 Operators and Delimiters

ClearSharp prefers word-based operators:

- and, or, not
- is, isNot
- isLike, isMatch

Symbolic operators are limited to arithmetic, comparison, indexing, and assignment.

---

## 7. Keywords

### 7.1 Reserved Keywords

service, module, action, rules, require, requires, ensures,  
if, then, else, select, case, for, each, in, do, return, parallel,  
try, catch, finally, throw,  
datasource, query, api, request, messagebus, message, handler,  
publish, send, to, uses, needs,  
with, lock, mutex, semaphore,  
generate,  
let, var, as, of, to,  
Nothing, Anything,  
and, or, not,  
is, isNot, isLike, isNotLike,  
isMatch, isNotMatch,  
isEmpty, isNotEmpty,  
isIn, isNotIn,  
contains, doesNotContain

---

### 7.2 Contextual Keywords

returns, input, output, base

---

### 7.3 Future-Reserved Keywords

async, await, yield, record, struct, enum

---

### End of Part II

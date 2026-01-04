# Part V – Expressions and Operators

## Table of Contents

1. Expressions Overview  
2. Primary Expressions  
3. Member Access and Invocation  
4. Indexing Expressions  
5. Operators Overview  
6. Logical Operators  
7. Comparison Operators  
8. Pattern and Matching Operators  
9. Collection Operators  
10. Arithmetic Operators  
11. Operator Precedence and Associativity  

---

## 1. Expressions Overview

An **expression** is a construct that produces a value or represents an operation.

Expressions may appear in:
- assignments
- conditions
- return statements
- arguments
- guards and rules

All expressions in ClearSharp are **side-effect aware** and evaluated eagerly unless otherwise specified.

---

## 2. Primary Expressions

Primary expressions include:

- literals
- identifiers
- parenthesized expressions
- object literals
- `Nothing`

Examples:

```
10
"Text"
Nothing
(args.Amount)
```

---

## 3. Member Access and Invocation

### 3.1 Member Access

Members are accessed using the dot operator:

```
customer.Name
order.Total
```

If the left-hand side is `Nothing`, member access evaluates to `Nothing` and does not throw.

---

### 3.2 Invocation Expressions

Actions, queries, requests, and handlers are invoked using standard call syntax:

```
Transfer("A", "B", 100)
GetCustomer({ Id = id })
```

All invocations are normalized through the Args model (see Part VII).

---

## 4. Indexing Expressions

Indexing uses square brackets:

```
items[0]
map["key"]
```

Rules:
- indexing out of range returns `Nothing`
- indexing into `Nothing` returns `Nothing`
- assignment through indexing is permitted where applicable

---

## 5. Operators Overview

ClearSharp favors **word-based operators**.

Operators are case-insensitive.

Every affirmative operator has a corresponding negative form.

Example:

```
is / isNot
contains / doesNotContain
isIn / isNotIn
```

---

## 6. Logical Operators

Logical operators operate on boolean values or boolean-coercible expressions.

### 6.1 `and`

```
a and b
```

Evaluates to true if both operands evaluate to true.

---

### 6.2 `or`

```
a or b
```

Behavior:
- if operands are boolean: logical OR
- otherwise: returns the first non-`Nothing` operand (coalesce behavior)

---

### 6.3 `not`

```
not a
```

Negates the boolean value of the operand.

---

## 7. Comparison Operators

Comparison operators include:

```
=, <>, <, <=, >, >=
```

Rules:
- comparisons with `Nothing` evaluate to false
- equality comparison is value-based
- reference equality semantics are implementation-defined

---

## 8. Pattern and Matching Operators

### 8.1 `isLike` / `isNotLike`

Performs SQL-style pattern matching.

```
name isLike "A%"
```

---

### 8.2 `isMatch` / `isNotMatch`

Performs regular expression matching.

```
email isMatch ".+@.+"
```

---

## 9. Collection Operators

### 9.1 `contains` / `doesNotContain`

```
items contains value
```

Checks for presence in a collection.

---

### 9.2 `isIn` / `isNotIn`

```
value isIn items
```

---

### 9.3 Positional Helpers

Collections expose:

- `first`
- `last`
- `fromEnd n`

Example:

```
items.fromEnd 1
```

---

## 10. Arithmetic Operators

Arithmetic operators are symbolic:

```
+, -, *, /
```

Rules:
- numeric promotion follows standard widening rules
- division by zero produces a runtime error

---

## 11. Operator Precedence and Associativity

Operator precedence (highest to lowest):

1. Member access, indexing  
2. Unary `not`  
3. Arithmetic (`*`, `/`)  
4. Arithmetic (`+`, `-`)  
5. Comparison  
6. Pattern and collection operators  
7. Logical `and`  
8. Logical `or`

Operators of equal precedence associate left-to-right unless otherwise specified.

---

### End of Part V

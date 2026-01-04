# Part IV – Types and Values

## Table of Contents

1. Type System Overview  
2. Built-in Types  
3. The `Nothing` and `Anything` Types  
4. Type Declarations and Annotations  
5. Type Inference  
6. Default Values  
7. Collections  
   7.1 Arrays  
   7.2 Maps  
8. Readable Generics  
9. Type Compatibility and Conversion  

---

## 1. Type System Overview

ClearSharp uses a **strongly-typed, statically-checked type system**.

Type checking is performed at compile time.  
Type information is preserved through lowering to C#.

Design goals:
- readability over brevity
- predictable behavior
- safe defaults
- minimal symbolic syntax

---

## 2. Built-in Types

ClearSharp provides a small set of built-in types that map directly to .NET types.

Common built-in types include:

- `Integer`
- `Decimal`
- `Boolean`
- `String`
- `Date`
- `DateTime`
- `Guid`

The exact mapping to CLR types is implementation-defined but must be consistent and documented.

---

## 3. The `Nothing` and `Anything` Types

### 3.1 `Nothing`

`Nothing` represents the absence of a value.

Rules:
- `Nothing` is a valid value for all reference types
- `Nothing` evaluates as `false` in conditions
- assigning `Nothing` never throws
- returning `Nothing` is always permitted

`Nothing` replaces `null`, `undefined`, and similar concepts.

---

### 3.2 `Anything`

`Anything` represents an unconstrained value.

Rules:
- `Anything` can hold any value
- `Anything` maps to `object` in C#
- use of `Anything` should be explicit and intentional

`Anything` exists to support dynamic or integration-heavy scenarios without symbolic syntax.

---

## 4. Type Declarations and Annotations

Types are declared using the `as` keyword.

```
let count as Integer
let name as String
```

Type annotations are optional where inference is possible.

---

## 5. Type Inference

ClearSharp supports local type inference.

```
let total = price * quantity
```

Rules:
- the inferred type must be unambiguous
- inference never changes runtime semantics
- inference failures produce diagnostics

---

## 6. Default Values

If a variable is declared without an explicit initializer:

- value types receive their default CLR value
- reference types are initialized to `Nothing`

Example:

```
let customer as Customer
// customer == Nothing
```

---

## 7. Collections

ClearSharp provides readable collection types.

---

### 7.1 Arrays

Arrays are declared using readable syntax:

```
Array of String
Array of Integer
```

Arrays behave like lists and support:

- indexing using `[ ]`
- dynamic resizing
- insertion and removal

Example:

```
let names as Array of String
names.add("Alice")
names.add("Bob")
```

Helper properties and operations:

- `isEmpty`, `isNotEmpty`
- `contains(value)`
- `indexOf(value)`
- `first`, `last`
- `fromEnd n`

Out-of-range access returns `Nothing` by default.

---

### 7.2 Maps

Maps represent key-value collections.

```
Map of String to Integer
```

Rules:
- keys are unique
- lookup by missing key returns `Nothing`
- insertion replaces existing values

Example:

```
let ages as Map of String to Integer
ages["Alice"] = 30
```

---

## 8. Readable Generics

ClearSharp uses **word-based generics**.

Instead of:

```
List<Customer>
```

ClearSharp uses:

```
Array of Customer
```

For two parameters:

```
Map of String to Customer
```

This avoids angle brackets and symbolic noise.

---

## 9. Type Compatibility and Conversion

ClearSharp allows limited implicit conversions where they are safe and readable.

Examples:
- numeric widening conversions
- `Nothing` to reference types

Unsafe or ambiguous conversions require explicit handling.

All conversions must lower to valid C# conversions.

---

### End of Part IV

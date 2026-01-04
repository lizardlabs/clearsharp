# Part VII – Actions, Args, and Execution Model

## Table of Contents

1. Actions Overview  
2. Action Declarations  
3. Parameters and Defaults  
4. The Args Model  
   4.1 Implicit Args Type  
   4.2 The `args` Variable  
   4.3 Call Normalization  
5. Required Parameters  
6. Return Semantics  
7. Return Type Inference  
8. Execution Pipeline  
9. Design Rationale  

---

## 1. Actions Overview

An **action** is the only executable routine construct in ClearSharp.

ClearSharp does not distinguish between:
- functions
- methods
- procedures
- subs

All executable logic is expressed using `action`.

This design reduces conceptual load and enforces a uniform execution model.

---

## 2. Action Declarations

### 2.1 Syntax

```
action ActionName ( ParameterList )
    statements
end action
```

The parameter list may be omitted.

### 2.2 Naming

- Action names are identifiers
- Names are case-insensitive
- Naming conventions are recommended but not enforced

---

## 3. Parameters and Defaults

Parameters are declared using:

```
Name as Type [ = DefaultValue ] [ required ]
```

Examples:

```
action Transfer(
    From as String required,
    To as String required,
    Amount as Decimal = 0
)
```

Rules:
- parameters marked `required` must be supplied
- parameters with defaults are optional
- default values are applied during call normalization

---

## 4. The Args Model

### 4.1 Implicit Args Type

For every action `X`, the compiler implicitly generates a type:

```
XArgs
```

This type contains one property per parameter.

Example:

```
action Add(a as Integer, b as Integer)
```

Generates:

```
AddArgs
```

Properties:
- are public
- are case-insensitive
- are immutable after construction

---

### 4.2 The `args` Variable

Inside every action body, an implicit readonly variable named `args` exists.

Properties:
- type: `<ActionName>Args`
- scope: entire action body
- reassignment is not allowed

Example:

```
log args.Amount
```

---

### 4.3 Call Normalization

All action calls are normalized to an Args object **before execution**.

The following calls are equivalent:

```
Transfer("A", "B", 100)
Transfer(From="A", To="B", Amount=100)
Transfer({ From="A", To="B", Amount=100 })
```

Normalization steps:
1. Map positional arguments by position
2. Map named arguments by name
3. Map object-literal properties by name
4. Apply default values
5. Validate required parameters

---

## 5. Required Parameters

Parameters marked `required`:
- must be present after normalization
- must not be `Nothing` unless explicitly allowed

Failure to satisfy required parameters results in a compile-time or runtime error (implementation-defined).

---

## 6. Return Semantics

ClearSharp does not use a `void` return type.

Rules:
- `return value` returns a value
- `return Nothing` returns no meaningful value
- absence of `return` is equivalent to `return Nothing`

---

## 7. Return Type Inference

The return type of an action is inferred:

1. Collect all return expressions
2. If none exist, return type is `Nothing`
3. If all expressions share a type, that type is used
4. Otherwise, a common supertype is inferred or an error is produced

Lowering to C# may map `Nothing` returns to `void` or `object?`.

---

## 8. Execution Pipeline

The execution of an action follows this conceptual pipeline:

1. Argument normalization (Args creation)
2. Validation (`rules`, `requires`)
3. Action body execution
4. Postconditions (`ensures`)
5. Result propagation

Cross-cutting concerns (logging, diagnostics) may be injected by the compiler or runtime.

---

## 9. Design Rationale

The Args model exists to:

- eliminate fragile positional APIs
- support safe API evolution
- unify invocation across actions, queries, requests, and handlers
- improve readability for beginners
- enable validation and tooling

This model is a core differentiator of ClearSharp.

---

### End of Part VII

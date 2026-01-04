# Part IX – Validation, Guards, and Contracts

## Table of Contents

1. Overview  
2. Rules Declarations  
3. Rule Execution  
4. `require` Statements  
5. `requires` Guards  
6. `ensures` Postconditions  
7. Diagnostics and Failure Semantics  
8. Integration with Actions, Services, and Resources  
9. Design Rationale  

---

## 1. Overview

Validation and contracts are **language-level** features in ClearSharp.

ClearSharp distinguishes:

- **rules**: reusable validation sets for Args or data types
- **requires**: preconditions for actions and operations
- **ensures**: postconditions for actions and operations

The goal is:
- readable guard clauses
- minimal boilerplate
- predictable failure behavior

---

## 2. Rules Declarations

Rules are declared using the `rules` keyword.

### 2.1 Syntax

```
rules RuleName for TargetType
    rule statements
end rules
```

Where `TargetType` is typically:
- `ActionArgs` type
- `ServiceArgs` type
- a message type
- a data DTO type

### 2.2 Example

```
rules Transfer for TransferArgs
    require From isNotEmpty else "From is required"
    require To isNotEmpty else "To is required"
    require Amount > 0 else "Amount must be positive"
end rules
```

Rules names are identifiers and are case-insensitive.

---

## 3. Rule Execution

Rules execute as part of the invocation pipeline.

Default pipeline:
1. Args normalization
2. rules evaluation (if requested)
3. requires guards
4. action body
5. ensures guards

Rules may be:
- explicitly applied
- implicitly attached by convention (future)

In v1, explicit application is required.

---

## 4. `require` Statements

A `require` statement validates a condition.

### 4.1 Syntax

```
require <condition> [ else <message> ]
```

If the condition evaluates to false, validation fails.

`Nothing` evaluates as false.

---

## 5. `requires` Guards

`requires` declares a **precondition** for an action, query, request, or handler.

### 5.1 Syntax

```
requires <condition> [ else <message> ]
```

### 5.2 Applying Rules as Preconditions

Rules may be invoked through `requires rules`.

```
requires rules Transfer
```

This executes the specified rules against the current `args`.

---

## 6. `ensures` Postconditions

`ensures` declares a **postcondition**.

### 6.1 Syntax

```
ensures <condition> [ else <message> ]
```

Postconditions are checked after action execution.

---

## 7. Diagnostics and Failure Semantics

Validation failures produce a structured diagnostic result.

Default behavior:
- in-process actions: throw a validation exception
- requests/queries: return `Nothing` or throw depending on configuration (TBD)

Diagnostics include:
- rule name
- field name (if applicable)
- message
- location

---

## 8. Integration with Actions, Services, and Resources

Validation features apply uniformly to:

- actions (using `args`)
- services (using `classArgs`)
- queries (using query args)
- requests (using request args)
- handlers (using message args)

Example:

```
action Transfer(...)
    requires rules Transfer
    ...
end action
```

---

## 9. Design Rationale

Validation is a language feature because:

- every serious system needs it
- libraries introduce ceremony
- guard clauses should be readable
- Args objects make validation natural and uniform

ClearSharp aims to make correct code easy to write and maintain.

---

### End of Part IX

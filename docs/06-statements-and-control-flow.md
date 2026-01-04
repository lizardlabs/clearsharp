# Part VI – Statements and Control Flow

## Table of Contents

1. Statements Overview  
2. Declaration Statements  
3. Assignment Statements  
4. Expression Statements  
5. Selection Statements  
   5.1 `if` Statement  
   5.2 `select` / `case when`  
6. Iteration Statements  
   6.1 `for each`  
   6.2 `parallel for each`  
7. Flow Control Statements  
8. Exception Handling  
   8.1 `try` / `catch` / `finally`  
   8.2 `throw`  
9. Statement Termination and Blocks  

---

## 1. Statements Overview

A **statement** represents an executable instruction.

Statements appear inside:
- actions
- handlers
- rules
- generators

Statements are executed sequentially unless control flow alters execution.

---

## 2. Declaration Statements

Declaration statements introduce variables.

### 2.1 `let` Declarations

```
let total as Decimal
let name = "Alice"
```

Rules:
- type annotation using `as` is optional if inferable
- uninitialized reference variables are set to `Nothing`
- declarations are block-scoped

---

## 3. Assignment Statements

Assignment uses the `=` operator.

```
total = 100
customer.Name = "Bob"
```

Rules:
- assigning to `Nothing` targets is ignored and does not throw
- compound assignments (`+=`, etc.) are not supported in v1

---

## 4. Expression Statements

Any expression may be used as a statement if its value is ignored.

```
Log("Started")
Transfer(args)
```

Expression statements are typically used for invocations.

---

## 5. Selection Statements

### 5.1 `if` Statement

The `if` statement conditionally executes statements.

#### Syntax

```
if condition then
    statements
else if otherCondition then
    statements
else
    statements
end if
```

Rules:
- `condition` is evaluated as boolean
- `Nothing` evaluates as `false`
- `then` keyword improves readability but may be optional

---

### 5.2 `select` / `case when`

The `select` statement provides multi-branch selection.

```
select status
    case when "New"
        ...
    case when "Closed"
        ...
    case else
        ...
end select
```

Rules:
- comparison is performed using equality semantics
- `case else` is optional

---

## 6. Iteration Statements

### 6.1 `for each`

Iterates over a collection.

```
for each item in items do
    process(item)
end for
```

Rules:
- iteration over `Nothing` performs zero iterations
- the iteration variable is read-only

---

### 6.2 `parallel for each`

Executes iterations concurrently.

```
parallel for each item in items max 8 do
    process(item)
end parallel
```

Rules:
- `max` limits concurrency
- errors are aggregated and surfaced after completion
- execution order is unspecified

---

## 7. Flow Control Statements

### 7.1 `return`

```
return value
```

Rules:
- terminates the current action
- `return Nothing` is permitted
- `return` without value is equivalent to `return Nothing`

---

### 7.2 Early Exit

Loop termination keywords such as `break` or `continue` are intentionally omitted in v1.

Early exit is achieved using:
- conditional logic
- returning from actions

---

## 8. Exception Handling

### 8.1 `try` / `catch` / `finally`

Exception handling uses explicit blocks.

```
try
    riskyOperation()
catch ex
    log ex.Message
finally
    cleanup()
end try
```

Rules:
- `catch` variable is optional
- multiple `catch` clauses may be supported in future versions
- `finally` is optional

---

### 8.2 `throw`

```
throw
throw ex
```

Rules:
- `throw` without argument rethrows the current exception
- `throw ex` throws the specified exception

---

## 9. Statement Termination and Blocks

Statements do not require semicolons.

Blocks are defined by:
- an opening keyword
- a corresponding `end <keyword>`

Indentation has no semantic meaning but is recommended for readability.

---

### End of Part VI

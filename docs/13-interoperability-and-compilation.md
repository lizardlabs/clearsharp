# Part XIII – Interoperability and Compilation

## Table of Contents

1. Overview  
2. Interoperability with C#  
   2.1 Calling C# from ClearSharp  
   2.2 Calling ClearSharp from C#  
   2.3 Visibility and Accessibility  
   2.4 Attributes and Metadata  
3. Type Mapping and Null Semantics  
4. Lowering to C#  
   4.1 Lowering Pipeline  
   4.2 Mapping Rules  
   4.3 Generated Code Shape  
5. Diagnostics and Errors  
6. Build Integration  
7. Versioning and Compatibility  
8. Design Rationale  

---

## 1. Overview

ClearSharp is designed for **first-class interoperability with C# and the .NET ecosystem**.

This part defines:
- how ClearSharp code interacts with C# code
- how ClearSharp is compiled and lowered
- how diagnostics and builds are handled

Interoperability is a **core guarantee**, not an optional feature.

---

## 2. Interoperability with C#

### 2.1 Calling C# from ClearSharp

ClearSharp code may call:
- C# methods
- constructors
- properties
- static members

Example:

```
use System.Guid

let id = Guid.NewGuid()
```

Rules:
- overload resolution follows C# rules
- method names are case-insensitive in ClearSharp
- arguments are passed via the Args model when applicable

---

### 2.2 Calling ClearSharp from C#

ClearSharp actions and services are exposed to C# as:

- methods
- classes
- standard .NET types

Example generated C#:

```csharp
public sealed class TransferArgs
{
    public string? From { get; init; }
    public string? To { get; init; }
    public decimal Amount { get; init; }
}
```

Actions are callable as:

```csharp
service.Transfer(new TransferArgs { From = "A", To = "B", Amount = 100 });
```

---

### 2.3 Visibility and Accessibility

Default rules:
- declarations are public by default
- visibility modifiers may be introduced in future versions

Interop respects standard .NET visibility semantics.

---

### 2.4 Attributes and Metadata

ClearSharp supports attaching attributes to declarations.

Example (syntax illustrative):

```
@Obsolete("Use NewMethod")
action OldMethod()
end action
```

Attributes are preserved during lowering and emitted into generated C#.

---

## 3. Type Mapping and Null Semantics

ClearSharp types map directly to CLR types.

Rules:
- `Nothing` maps to `null`
- reference types are nullable by default
- `Anything` maps to `object`

Null safety guarantees:
- ClearSharp prevents common null-reference failures
- C# consumers must respect nullable annotations

---

## 4. Lowering to C#

### 4.1 Lowering Pipeline

The ClearSharp compiler performs the following stages:

1. Lexical analysis  
2. Syntax parsing  
3. Semantic analysis  
4. Args generation  
5. Validation wiring  
6. Resource lowering  
7. C# code generation  

Each stage is deterministic.

---

### 4.2 Mapping Rules

General mapping rules:

| ClearSharp | C# |
|-----------|----|
| action | method |
| service | class |
| needs | constructor injection |
| rules | validation methods |
| datasource/query | data-access methods |
| api/request | HTTP client wrappers |
| generate | build-time code generation |

---

### 4.3 Generated Code Shape

Generated C# code:
- is readable
- avoids reflection-heavy constructs
- uses idiomatic .NET patterns
- is suitable for debugging

The compiler favors clarity over micro-optimizations.

---

## 5. Diagnostics and Errors

Diagnostics include:
- syntax errors
- semantic errors
- validation errors
- generator failures

Rules:
- diagnostics include source location
- messages are human-readable
- diagnostic codes are stable

---

## 6. Build Integration

ClearSharp integrates with standard .NET tooling:

- `dotnet build`
- `dotnet run`
- IDEs and CI pipelines

ClearSharp compilation may be:
- a pre-build step
- an integrated MSBuild task
- a standalone compiler invocation

---

## 7. Versioning and Compatibility

ClearSharp follows semantic versioning.

Compatibility rules:
- minor versions are backward compatible
- breaking changes require major version increments
- reserved keywords protect future evolution

---

## 8. Design Rationale

Interop and compilation are explicit because:

- ClearSharp must coexist with existing codebases
- generated code must be understandable
- debugging must be straightforward
- tooling must integrate seamlessly

ClearSharp enhances .NET without fragmenting it.

---

### End of Part XIII

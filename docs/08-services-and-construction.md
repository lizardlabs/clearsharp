# Part VIII – Services and Construction

## Table of Contents

1. Services Overview  
2. Service Declarations  
3. Dependency Declaration (`needs`)  
4. Constructors  
5. The `classArgs` Model  
6. Accessing `classArgs`  
7. Service Lifetime and Scope  
8. Inheritance and Base Construction  
9. Design Rationale  

---

## 1. Services Overview

A **service** represents a long-lived, dependency-aware component.

Services are the primary mechanism for:
- dependency injection
- encapsulating business logic
- coordinating resources

Services are designed to be:
- explicit
- testable
- free of framework ceremony

---

## 2. Service Declarations

### 2.1 Syntax

```
service ServiceName
    service body
end service
```

A service body may contain:
- dependency declarations
- constructors
- actions
- rules

### 2.2 Naming Rules

- service names are identifiers
- names are case-insensitive
- naming conventions are recommended but not enforced

---

## 3. Dependency Declaration (`needs`)

Dependencies are declared using the `needs` keyword.

```
service PaymentService
    needs repo as IPaymentRepository
    needs log as ILogger
end service
```

Rules:
- each `needs` declaration defines a required dependency
- dependencies are resolved by the runtime or host
- missing dependencies result in construction failure

---

## 4. Constructors

### 4.1 Constructor Declaration

Constructors are declared using an action named `New`.

```
action New(Environment as String = "PROD")
```

Rules:
- only one constructor may be declared per service (v1)
- constructor parameters follow the same rules as action parameters
- constructors may contain executable logic

---

## 5. The `classArgs` Model

For every service, the compiler generates an implicit **class argument object**.

```
<ServiceName>Args
```

This type contains:
- all `needs` dependencies
- all constructor parameters

An implicit readonly variable named `classArgs` is available:
- in the constructor
- in all actions of the service

---

## 6. Accessing `classArgs`

Example:

```
service PaymentService
    needs repo as IPaymentRepository
    needs log as ILogger

    action New(Environment as String)
        log.Info("Environment: " + classArgs.Environment)
    end action

    action Process()
        repo.Save(...)
    end action
end service
```

Rules:
- `classArgs` is immutable
- reassignment is not allowed
- access is always safe

---

## 7. Service Lifetime and Scope

Service lifetime is implementation-defined.

Default assumptions:
- services are singleton by default
- dependencies may be singleton or scoped
- lifetime may be overridden by configuration

ClearSharp does not expose lifetime keywords in v1.

---

## 8. Inheritance and Base Construction

### 8.1 Service Inheritance

Services may inherit from a base service.

```
service AdvancedPaymentService inherits PaymentService
```

Rules:
- base service must be resolvable
- inherited actions are available unless overridden

---

### 8.2 Base Constructor Invocation

Base constructor arguments are forwarded automatically via `classArgs`.

```
service ChildService inherits BaseService
    action New(Name as String)
        // base constructor receives matching args
    end action
end service
```

Rules:
- matching parameters are forwarded by name
- unmatched base parameters must have defaults
- explicit base calls are not required in v1

---

## 9. Design Rationale

The service and `classArgs` model exists to:

- eliminate constructor boilerplate
- simplify dependency injection
- make object construction readable
- support novice developers
- align with the Args model used everywhere else

Construction is declarative; wiring is not the user’s concern.

---

### End of Part VIII

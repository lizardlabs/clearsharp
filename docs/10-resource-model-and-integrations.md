# Part X – Resource Model and Integrations

## Table of Contents

1. Overview  
2. Resource Declarations  
3. Datasources and Queries  
   3.1 `datasource`  
   3.2 `query`  
4. HTTP APIs and Requests  
   4.1 `api`  
   4.2 `request`  
5. Messaging  
   5.1 `messagebus`  
   5.2 `message`  
   5.3 `publish` and `send`  
   5.4 `handler`  
6. Args Integration  
7. Failure Semantics  
8. Resource Lifetimes  
9. Design Rationale  

---

## 1. Overview

ClearSharp defines a **unified resource model** for interacting with external systems.

All integrations follow the same conceptual pattern:

- declare a resource
- declare operations bound to the resource
- invoke operations using the Args model

This unifies databases, HTTP APIs, and messaging under one mental model.

---

## 2. Resource Declarations

Resources are declared at file scope.

General syntax:

```
<resource-kind> ResourceName
    configuration
end <resource-kind>
```

Rules:
- resource names are identifiers
- names are case-insensitive
- resources are configuration-only

---

## 3. Datasources and Queries

### 3.1 `datasource`

A datasource defines a database connection.

Example:

```
datasource MainDb
    provider = "SqlServer"
    connection = config("MainDb.ConnectionString")
end datasource
```

---

### 3.2 `query`

A query defines a database operation.

```
query GetCustomer uses MainDb
    input Id as Guid required
    returns Customer
    sql "select * from Customers where Id = @Id"
end query
```

Rules:
- queries define implicit Args types
- parameters bind by name
- missing rows return `Nothing`

---

## 4. HTTP APIs and Requests

### 4.1 `api`

An API defines an HTTP endpoint.

```
api PaymentsApi
    base url = config("PaymentsApi.BaseUrl")
    auth = bearer token config("PaymentsApi.Token")
end api
```

---

### 4.2 `request`

A request defines an HTTP operation.

```
request GetPayment uses PaymentsApi
    method = GET
    path = "/payments/{Id}"
    input Id as Guid required
    returns PaymentDto
end request
```

Requests:
- define Args types
- bind path/query/body parameters
- hide HTTP client implementation

---

## 5. Messaging

### 5.1 `messagebus`

```
messagebus Events
    provider = "AzureServiceBus"
    connection = config("Events.Connection")
end messagebus
```

---

### 5.2 `message`

```
message PaymentRequested
    PaymentId as Guid required
    Amount as Decimal required
end message
```

Messages define schemas and implicit Args.

---

### 5.3 `publish` and `send`

```
publish PaymentRequested { PaymentId = id } uses Events
send PaymentRequested { ... } uses Events to "queue"
```

---

### 5.4 `handler`

```
handler PaymentRequested uses Events
    action Handle()
        requires rules PaymentRequested
        ...
    end action
end handler
```

Handlers receive message Args automatically.

---

## 6. Args Integration

All resource operations:
- generate Args types
- normalize calls
- support validation and guards

The Args model is uniform across:
- actions
- queries
- requests
- handlers

---

## 7. Failure Semantics

Default behavior:
- query: no rows → `Nothing`
- request: non-success → `Nothing`
- message handler failure → retry or dead-letter (provider-defined)

Failure behavior may be configured.

---

## 8. Resource Lifetimes

Resources:
- are singleton by default
- are lazily initialized
- are managed by the runtime

Explicit lifetime control is deferred to future versions.

---

## 9. Design Rationale

The resource model exists to:
- remove framework-specific APIs
- make integrations declarative
- unify external interactions
- support tooling and code generation

---

### End of Part X

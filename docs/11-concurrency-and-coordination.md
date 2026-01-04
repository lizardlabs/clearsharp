# Part XI – Concurrency and Coordination

## Table of Contents

1. Overview  
2. Concurrency Model  
3. Parallel Execution  
   3.1 `parallel for each`  
   3.2 Error Aggregation  
   3.3 Cancellation  
4. Coordination Primitives  
   4.1 `with lock`  
   4.2 `with mutex`  
   4.3 `with semaphore`  
5. Structured Concurrency Rules  
6. Interaction with Actions and Resources  
7. Failure Semantics  
8. Design Rationale  

---

## 1. Overview

ClearSharp provides **structured, readable concurrency** without exposing low-level primitives such as `Task`, `Thread`, or `async/await` in the surface language.

Concurrency is expressed declaratively and is bounded by scope.

---

## 2. Concurrency Model

ClearSharp follows a **structured concurrency model**:

- concurrent work is created explicitly
- concurrent work is bounded by a lexical scope
- completion is awaited implicitly at scope exit

This model avoids:
- runaway background tasks
- hidden async flows
- unobserved exceptions

---

## 3. Parallel Execution

### 3.1 `parallel for each`

Executes iterations concurrently.

```
parallel for each item in items max 8 do
    process(item)
end parallel
```

Rules:
- `max` specifies the maximum degree of parallelism
- if `max` is omitted, a runtime-defined default is used
- iteration order is unspecified

---

### 3.2 Error Aggregation

If one or more iterations fail:
- all iterations are awaited
- errors are aggregated
- a single composite error is raised after completion

Partial results may be preserved if explicitly handled.

---

### 3.3 Cancellation

Parallel blocks support cooperative cancellation.

Rules:
- cancellation propagates to all child executions
- cancellation behavior is implementation-defined in v1
- explicit cancellation tokens are not exposed

---

## 4. Coordination Primitives

ClearSharp exposes coordination primitives via the `with` keyword.

These primitives define **scoped critical sections**.

---

### 4.1 `with lock`

Provides a mutual exclusion lock.

```
with lock OrdersLock
    updateOrder()
end with
```

Rules:
- the lock is acquired at block entry
- the lock is released at block exit
- reentrancy is implementation-defined

---

### 4.2 `with mutex`

Provides a named, cross-scope mutual exclusion mechanism.

```
with mutex GlobalImportMutex
    runImport()
end with
```

Rules:
- mutex identity is runtime-defined
- mutex scope may be process-wide or system-wide

---

### 4.3 `with semaphore`

Limits concurrent access.

```
with semaphore UploadSlots max 4
    uploadFile()
end with
```

Rules:
- `max` specifies the concurrency limit
- permits are acquired and released automatically

---

## 5. Structured Concurrency Rules

ClearSharp enforces the following rules:

- concurrent work must be lexically scoped
- child executions cannot outlive their parent scope
- resource cleanup occurs deterministically

These rules ensure predictability and safety.

---

## 6. Interaction with Actions and Resources

Concurrency constructs may appear inside:
- actions
- handlers
- resource operations

Concurrency does not change:
- Args normalization
- validation behavior
- resource lifetimes

---

## 7. Failure Semantics

Failure inside a concurrency block:

- aborts further scheduling
- waits for running operations
- raises a composite failure

Failure handling may be customized in future versions.

---

## 8. Design Rationale

The concurrency model exists to:

- make parallelism readable
- avoid callback-based designs
- prevent accidental background work
- align with enterprise safety requirements

ClearSharp favors **explicit, bounded concurrency** over unstructured async execution.

---

### End of Part XI

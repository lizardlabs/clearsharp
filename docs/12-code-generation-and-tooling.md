# Part XII – Code Generation and Tooling

## Table of Contents

1. Overview  
2. The `generate` Construct  
3. Generator Discovery and Registration  
4. Built-in Generators  
   4.1 Scriban Templates  
   4.2 OpenAPI / Refitter  
   4.3 Database Scaffolding  
5. NuGet-Provided Generators  
6. Generator Inputs and Outputs  
7. Execution Semantics  
8. Tooling Integration  
9. Design Rationale  

---

## 1. Overview

ClearSharp treats **code generation as a first-class language feature**.

Instead of external scripts or build steps, ClearSharp provides a unified `generate` construct
that allows declarative, repeatable, and inspectable code generation.

Generators execute at **build time** and do not affect runtime behavior.

---

## 2. The `generate` Construct

### 2.1 Syntax

```
generate <generator-name>
    generator configuration
end generate
```

The generator name identifies a generator implementation.

---

### 2.2 Example

```
generate scriban
    template = "Templates/Crud.sbn"
    output = "Generated/CustomerCrud.cs"
    model = { Entity = "Customer" }
end generate
```

---

## 3. Generator Discovery and Registration

Generators are discovered through:

- built-in compiler generators
- NuGet packages referenced via `#:package`
- project-level configuration

Each generator exposes:
- a name
- supported configuration parameters
- expected inputs and outputs

---

## 4. Built-in Generators

### 4.1 Scriban Templates

The `scriban` generator executes Scriban templates.

Capabilities:
- template-based code generation
- structured model input
- deterministic output

---

### 4.2 OpenAPI / Refitter

The `refitter` generator generates HTTP clients from OpenAPI specifications.

Example:

```
generate refitter
    openapi = config("PaymentsApi.OpenApiUrl")
    output = "Generated/PaymentsApiClient.cs"
end generate
```

The generated code integrates automatically with `api` and `request` declarations.

---

### 4.3 Database Scaffolding

Database scaffolding generators generate:

- data models
- queries
- DTOs

Example:

```
generate db scaffold
    datasource = MainDb
    schema = "dbo"
    output = "Generated/Db"
end generate
```

---

## 5. NuGet-Provided Generators

Generators may be provided by NuGet packages.

Example:

```
#:package MyCompany.ClearSharp.Generators
```

Once referenced, the generator becomes available to the `generate` construct.

---

## 6. Generator Inputs and Outputs

Generators may consume:

- configuration values
- Args objects
- files
- schemas

Generators produce:
- source files
- metadata
- diagnostics

Generated files are treated as **compiler outputs**.

---

## 7. Execution Semantics

Rules:
- generators execute before semantic compilation
- generators are deterministic
- generators may overwrite previously generated files
- generator failures abort compilation

---

## 8. Tooling Integration

The `generate` model enables:

- IDE previews
- incremental regeneration
- dependency tracking
- clean rebuilds

Tooling may:
- visualize generator graphs
- show generated outputs
- allow regeneration on demand

---

## 9. Design Rationale

Code generation is a language feature because:

- modern systems depend on generated code
- external scripts are opaque and fragile
- generation should be repeatable and visible
- tooling should understand generation

ClearSharp makes generation explicit, declarative, and safe.

---

### End of Part XII

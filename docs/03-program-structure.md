# Part III – Program Structure

## Table of Contents

1. Compilation Units  
2. Source Files  
3. File-Based Namespaces  
4. Imports (`use`)  
5. Global and Directory Configuration  
   5.1 `global.lang`  
   5.2 `dir.lang`  
6. Directives  
   6.1 Directive Syntax  
   6.2 Package Directives  
7. Declaration Order and Visibility  

---

## 1. Compilation Units

A ClearSharp **compilation unit** is a single source file.

Compilation units are combined into a program according to project-level rules defined by the build system.

Each compilation unit is processed independently for lexical and syntactic analysis, and then combined during semantic analysis.

---

## 2. Source Files

ClearSharp source files:

- are plain text files
- typically use the `.csx` or `.clear` extension (exact extension is implementation-defined)
- contain zero or more declarations

A source file may contain:
- service declarations
- module declarations
- resource declarations
- action declarations
- rules and generators

There is no requirement that a source file contain exactly one declaration.

---

## 3. File-Based Namespaces

ClearSharp uses **file- and directory-based namespaces**.

Namespaces are derived from the file’s location in the project structure.

Example:

```
/Payments
    /Services
        PaymentService.clear
```

Produces the namespace:

```
Payments.Services
```

### 3.1 Default Namespace

If a file is located at the project root, it belongs to the **default namespace**.

The default namespace may be configured via `global.lang`.

---

## 4. Imports (`use`)

Imports are declared using the `use` keyword.

```
use Payments.Services
use System.Text
```

Rules:
- Imports apply to the entire file
- Imports are case-insensitive
- Duplicate imports are ignored

Imports allow access to types and members defined in other namespaces.

---

## 5. Global and Directory Configuration

ClearSharp allows configuration files that apply to multiple source files.

These files contain **only configuration and imports**, not executable logic.

---

### 5.1 `global.lang`

A `global.lang` file applies to the entire project.

It may define:
- default namespace
- common imports
- shared directives
- package references

Example:

```
namespace MyCompany.App

use System
use System.Collections
```

---

### 5.2 `dir.lang`

A `dir.lang` file applies to its directory and all subdirectories.

It may define:
- namespace prefixes
- common imports
- resource defaults

Example:

```
use MyCompany.App.Infrastructure
```

---

## 6. Directives

Directives provide compiler instructions.

They are evaluated before semantic analysis.

---

### 6.1 Directive Syntax

Directives begin with `#:` and appear at file scope.

Example:

```
#:package Refit@*
#:package Scriban@*
```

Directives:
- are case-insensitive
- must appear before declarations
- do not affect runtime behavior directly

---

### 6.2 Package Directives

Package directives declare NuGet dependencies.

```
#:package Dapper@*
```

Rules:
- packages are restored before compilation
- versions may be exact or wildcard
- packages enable language features or generators

---

## 7. Declaration Order and Visibility

Declaration order within a file is not significant.

Visibility rules:
- declarations are public by default
- visibility modifiers may be added later (TBD)
- resources and services are visible within their namespace

Forward references are allowed.

---

### End of Part III

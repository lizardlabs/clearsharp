# Part I – Introduction and Philosophy

## Table of Contents

1. Purpose of This Specification  
2. What ClearSharp Is  
3. Relationship to C# and .NET  
   3.1 Compilation Model  
   3.2 Interoperability  
4. Design Motivation  
   4.1 Problems Addressed  
   4.2 Design Response  
5. Target Audience  
6. Core Language Principles  
   6.1 Readability First  
   6.2 Explicit Structure  
   6.3 Case Insensitivity  
   6.4 One Concept, One Mechanism  
   6.5 Safe Defaults  
7. Null and Absence (`Nothing`)  
8. Uniform Parameter Model (Overview)  
9. Language-Level Enterprise Concepts  
10. Non-Goals  
11. Specification Structure  
12. Conformance  

---

## 1. Purpose of This Specification

This document defines the **ClearSharp programming language**.

The purpose of this specification is to:

- define the **syntax**, **semantics**, and **behavior** of ClearSharp programs
- provide a **precise and unambiguous reference** for implementers (compiler, tools)
- provide a **coherent mental model** for users of the language
- document the **design rationale** behind major language decisions

This specification is written to be:
- **normative** where behavior must be defined
- **descriptive** where intent and rationale improve understanding

Unless otherwise stated, the word *must* indicates a mandatory requirement.

---

## 2. What ClearSharp Is

ClearSharp is a **high-level, general-purpose programming language** designed for the **.NET ecosystem**.

ClearSharp:
- compiles to **idiomatic C# source code**
- targets the **Common Language Runtime (CLR)**
- interoperates fully with existing .NET libraries
- may coexist with C# within the same solution

ClearSharp does **not** define:
- a new runtime
- a virtual machine
- a proprietary execution environment

ClearSharp is a **source-to-source language**.  
ClearSharp programs are translated into C# programs, which are then compiled using standard .NET toolchains.

---

## 3. Relationship to C# and .NET

ClearSharp has a **dependent relationship** with C# and .NET.

### 3.1 Compilation Model

The compilation pipeline is:

```
ClearSharp source
    → ClearSharp compiler
        → C# source
            → C# compiler
                → .NET IL
```

The generated C# code:
- is readable and debuggable
- uses conventional .NET constructs
- does not rely on hidden or proprietary runtime behavior

### 3.2 Interoperability

ClearSharp guarantees:

- ClearSharp types can be consumed from C#
- C# types can be consumed from ClearSharp
- public surface area follows standard .NET visibility rules
- attributes and metadata may flow through lowering (subject to specification)

Interop is not optional; it is a **core language guarantee**.

---

## 4. Design Motivation

ClearSharp was designed in response to recurring problems observed in long-lived .NET systems.

### 4.1 Problems Addressed

- Symbol-heavy syntax discourages beginners and occasional developers
- Framework-driven patterns introduce hidden complexity
- Common enterprise concepts are not language-level
- APIs are fragile due to positional parameters
- Readability degrades rapidly in nested logic

### 4.2 Design Response

ClearSharp addresses these problems by:

- prioritizing **readability over brevity**
- preferring **words over symbols**
- making **structure explicit**
- promoting **uniform patterns** for common tasks
- elevating recurring architectural patterns into the language

---

## 5. Target Audience

ClearSharp is designed for:

- beginner and occasional developers
- business-domain developers
- enterprise application teams
- regulated and long-lived systems
- experienced .NET developers seeking reduced ceremony

ClearSharp is **not** primarily designed for:

- low-level systems programming
- performance-critical numeric computation
- graphics or game engines
- frontend scripting

---

## 6. Core Language Principles

### 6.1 Readability First

The primary optimization target of ClearSharp is **human understanding**.

Code should be understandable:
- without memorizing language tricks
- without deep framework knowledge
- without reliance on formatting conventions

### 6.2 Explicit Structure

ClearSharp uses **explicit block termination**.

Examples:

```
end if
end action
end service
```

Rules:
- block boundaries are never inferred
- indentation has no semantic meaning
- nesting is explicit and verifiable

### 6.3 Case Insensitivity

ClearSharp is **case-insensitive everywhere**.

This includes:
- keywords
- identifiers
- type names
- action names

Case is treated as a **presentation concern**, not a semantic one.

### 6.4 One Concept, One Mechanism

ClearSharp avoids overlapping or redundant constructs.

Examples:
- one executable construct (`action`)
- one null literal (`Nothing`)
- one parameter model (Args)
- one integration model (Resources)

### 6.5 Safe Defaults

ClearSharp is designed to reduce:
- accidental null dereferencing
- fragile APIs
- implicit failure modes

Where a choice exists, the language defaults to:
- explicit behavior
- predictable outcomes
- readable intent

---

## 7. Null and Absence (`Nothing`)

ClearSharp defines a single literal to represent the absence of a value:

```
Nothing
```

Design rules:
- `Nothing` replaces `null`, `undefined`, and similar concepts
- reference types are nullable by default
- `Nothing` is allowed in assignments and returns
- `Nothing` in conditions evaluates as `false`

The language does not attempt to eliminate nulls; it aims to **make them explicit and manageable**.

---

## 8. Uniform Parameter Model (Overview)

ClearSharp adopts a **universal argument object model**.

All callable entities:
- actions
- constructors
- queries
- requests
- message handlers

receive their inputs via an **implicit Args object**.

This model:
- unifies positional, named, and object-literal calls
- enables safe API evolution
- supports validation and introspection
- improves readability for novice users

The Args model is specified in detail in Part VII.

---

## 9. Language-Level Enterprise Concepts

ClearSharp explicitly recognizes the following as **language concepts**, not libraries:

- dependency injection (`service`, `needs`)
- validation (`rules`, `requires`)
- external systems (`datasource`, `api`, `messagebus`)
- code generation (`generate`)
- concurrency control (`parallel`, `with lock`)

These concepts:
- are declarative
- are inspectable by tooling
- lower to standard .NET implementations

---

## 10. Non-Goals

ClearSharp explicitly does **not** aim to:

- replace C#
- hide .NET concepts from advanced users
- introduce a proprietary runtime
- optimize for terseness
- compete with scripting languages

ClearSharp optimizes for **clarity, stability, and longevity**, not novelty.

---

## 11. Specification Structure

This specification is organized into the following parts:

- Part I – Introduction and Philosophy
- Part II – Lexical Structure
- Part III – Program Structure
- Part IV – Types and Values
- Part V – Expressions and Operators
- Part VI – Statements and Control Flow
- Part VII – Actions, Args, and Execution Model
- Part VIII – Services and Construction
- Part IX – Validation, Guards, and Contracts
- Part X – Resource Model and Integrations
- Part XI – Concurrency and Coordination
- Part XII – Code Generation and Tooling
- Part XIII – Interoperability and Compilation
- Part XIV – Compiler Architecture
- Appendices

---

## 12. Conformance

An implementation conforms to this specification if it:

- accepts all syntactically valid ClearSharp programs
- enforces the semantics defined herein
- produces diagnostics for invalid programs
- preserves the observable behavior described in this document

---

### End of Part I

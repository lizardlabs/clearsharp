# ClearSharp
ClearSharp is a high-level, general-purpose programming language designed for the .NET ecosystem. ClearSharp programs are translated into C# programs, which are then compiled using standard .NET toolchains. The primary optimization target of ClearSharp is human understanding. Code should be understandable without memorizing language tricks.

*** Note: some ideas are still under development ***


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

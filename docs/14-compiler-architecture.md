# Part XIV – Compiler Architecture

## Table of Contents

1. Overview  
2. Compiler Goals  
3. High-Level Architecture  
4. Lexical Analysis  
5. Syntax Parsing  
6. Abstract Syntax Tree (AST)  
7. Semantic Analysis  
8. Args and ClassArgs Generation  
9. Validation and Contract Wiring  
10. Resource Lowering  
11. Concurrency Lowering  
12. Code Generation (C# Emission)  
13. Incremental Compilation  
14. Error Handling and Diagnostics  
15. Extensibility Points  
16. Design Rationale  

---

## 1. Overview

This part describes the **reference compiler architecture** for ClearSharp.

The ClearSharp compiler is a **source-to-source compiler** that translates ClearSharp code
into readable, idiomatic C# code suitable for compilation by standard .NET toolchains.

This section is informative for:
- compiler implementers
- tooling authors
- advanced users

---

## 2. Compiler Goals

The ClearSharp compiler is designed to:

- preserve ClearSharp semantics exactly
- generate readable and debuggable C# code
- provide high-quality diagnostics
- support incremental and IDE-driven compilation
- enable tooling and generators

Performance is important, but **clarity and correctness** are prioritized.

---

## 3. High-Level Architecture

The compiler pipeline consists of the following stages:

1. Source loading  
2. Lexical analysis  
3. Syntax parsing  
4. AST construction  
5. Semantic analysis  
6. Model generation (Args, classArgs, resources)  
7. Lowering  
8. C# code generation  
9. Diagnostic emission  

Each stage consumes immutable data from the previous stage.

---

## 4. Lexical Analysis

The lexer:
- processes Unicode source text
- applies case-insensitive tokenization
- produces a stream of tokens

Responsibilities:
- recognize keywords, identifiers, literals
- handle comments and whitespace
- report lexical errors

ANTLR v4 is the recommended lexer technology.

---

## 5. Syntax Parsing

The parser:
- consumes tokens
- validates grammatical structure
- produces a parse tree

Parsing rules:
- explicit block terminators must match
- nesting must be structurally valid
- ambiguous constructs are rejected

ANTLR v4 or equivalent parser generators may be used.

---

## 6. Abstract Syntax Tree (AST)

The AST is a simplified, language-specific representation.

AST nodes represent:
- declarations
- statements
- expressions
- types
- resources

The AST:
- is immutable
- preserves source locations
- is independent of C# syntax

---

## 7. Semantic Analysis

Semantic analysis validates program meaning.

Responsibilities include:
- symbol resolution
- type checking
- scope validation
- rule binding
- inheritance validation

Semantic errors are reported with precise diagnostics.

---

## 8. Args and ClassArgs Generation

During semantic analysis, the compiler generates:

- `<ActionName>Args` types
- `<ServiceName>Args` types

Rules:
- Args properties match parameter declarations
- defaults are encoded
- required parameters are marked

These generated models drive validation and invocation semantics.

---

## 9. Validation and Contract Wiring

Validation constructs are lowered into:

- generated validation methods
- guard checks in generated code
- structured diagnostic results

The compiler wires:
- `rules`
- `requires`
- `ensures`

into the execution pipeline.

---

## 10. Resource Lowering

Resource declarations are lowered into:

- database access classes
- HTTP client wrappers
- messaging adapters

Lowering:
- preserves the Args model
- hides framework-specific APIs
- supports provider substitution

---

## 11. Concurrency Lowering

Concurrency constructs are lowered into:

- task groups
- structured awaits
- scoped synchronization primitives

The compiler ensures:
- no unbounded background execution
- deterministic cleanup
- aggregated error handling

---

## 12. Code Generation (C# Emission)

The final stage emits C# source code.

Generated code:
- is formatted and readable
- uses standard .NET constructs
- includes source mapping where possible
- avoids reflection-heavy designs

The compiler may emit multiple files per input file.

---

## 13. Incremental Compilation

The compiler supports incremental compilation.

Capabilities:
- file-level invalidation
- generator dependency tracking
- fast IDE feedback

Incremental correctness is a core requirement.

---

## 14. Error Handling and Diagnostics

Diagnostics:
- are categorized (error, warning, info)
- include stable diagnostic codes
- include source spans
- provide actionable messages

The compiler never fails silently.

---

## 15. Extensibility Points

The compiler exposes extension points for:

- generators
- resource providers
- analyzers
- tooling integrations

Extensions must not compromise language semantics.

---

## 16. Design Rationale

The architecture is designed to:

- keep ClearSharp conceptually small
- allow independent evolution of features
- support rich tooling
- maintain debuggability and trust

ClearSharp’s compiler is intentionally conservative and explicit.

---

### End of Part XIV

# ClearSharp Programming Language for .NET

ClearSharp is a high-level, general-purpose programming language designed for the .NET ecosystem. ClearSharp programs are translated into C# programs, which are then compiled using standard .NET toolchains. The primary optimization target of ClearSharp is human understanding. Code should be understandable without memorizing language tricks.

ClearSharp is a **readability-first programming language for .NET**.  
It compiles to **idiomatic C#**, runs on the **CLR**, and is fully interoperable with existing .NET code.

ClearSharp is designed for developers who want the **power of .NET** without symbolic noise, ceremony, or framework-heavy patterns.
It emphasizes explicit structure, safe defaults, and language-level support for common enterprise concepts.

ClearSharp is to C# what TypeScript is to JavaScript: a readable, safer, source-to-source language that transpiles to C#, combining the clarity once offered by VB.NET with the power, performance, and modern ecosystem of C#.

*** Note: many ideas are still under development ***

---

## 1. Introduction

ClearSharp is inspired by the strengths of:
- VB.NET (readability, explicit blocks)
- modern C# (.NET ecosystem, performance, tooling)
- declarative systems (validation, resources, messaging)

ClearSharp is **not a replacement for C#**.  
It is a **source-to-source language**, similar in spirit to how TypeScript relates to JavaScript.

---

## 2. Motivation and Goals

The language was designed to address common problems observed in long-lived enterprise systems:

- Too many symbols and syntax rules
- Framework-driven architecture instead of language support
- Fragile APIs due to positional parameters
- Accidental complexity in async, DI, validation, and integration

### Core goals

- Human-readable code
- Beginner and occasional-developer friendly
- Explicit structure (no indentation-based semantics)
- One uniform execution model
- First-class support for enterprise patterns
- Zero lock-in to proprietary runtimes

---

## 3. Key Characteristics

- Case-insensitive language
- Explicit block endings (`end if`, `end action`, …)
- Single executable construct (`action`)
- Universal Args model for calls and constructors
- Safe null handling via `Nothing`
- No `Task`, `async`, or `await` in surface syntax
- Declarative resources (database, API, messaging)
- Structured concurrency
- Built-in validation and contracts
- First-class code generation

---

## 4. Language Specification

The full language specification is located in the **docs** folder.

### Core Specification

- [Part I – Introduction and Philosophy](docs/01-introduction-and-philosophy.md)
- [Part II – Lexical Structure](docs/02-lexical-structure.md)
- [Part III – Program Structure](docs/03-program-structure.md)
- [Part IV – Types and Values](docs/04-types-and-values.md)
- [Part V – Expressions and Operators](docs/05-expressions-and-operators.md)
- [Part VI – Statements and Control Flow](docs/06-statements-and-control-flow.md)
- [Part VII – Actions, Args, and Execution Model](docs/07-actions-args-and-execution.md)
- [Part VIII – Services and Construction](docs/08-services-and-construction.md)
- [Part IX – Validation, Guards, and Contracts](docs/09-validation-guards-contracts.md)
- [Part X – Resource Model and Integrations](docs/10-resource-model-and-integrations.md)
- [Part XI – Concurrency and Coordination](docs/11-concurrency-and-coordination.md)
- [Part XII – Code Generation and Tooling](docs/12-code-generation-and-tooling.md)
- [Part XIII – Interoperability and Compilation](docs/13-interoperability-and-compilation.md)
- [Part XIV – Compiler Architecture](docs/14-compiler-architecture.md)

### Appendices

- [Appendix A – Keyword List](docs/A-keywords.md)
- [Appendix B – Operator List](docs/B-operators.md)
- [Appendix C – Grammar (ANTLR Draft)](docs/C-grammar-antlr.md)
- [Appendix D – ClearSharp → C# Translation Examples](docs/D-translation-examples.md)

---

## 5. Examples – Service, Namespace, and Actions

This example demonstrates a realistic ClearSharp service with:
- file-based namespace
- dependency injection
- constructor (`New`)
- actions with Args
- validation rules
- readable, explicit structure

---

### ClearSharp Source

**File:** `Payments/Services/PaymentService.clear`

```text
use System.Guid

service PaymentService
    needs repo as IPaymentRepository
    needs log  as ILogger

    action New(Environment as String = "PROD")
        log.Info("Starting PaymentService in " + classArgs.Environment)
    end action

    action CreatePayment(
        CustomerId as Guid required,
        Amount as Decimal required,
        Currency as String = "EUR"
    )
        requires Amount > 0 else "Amount must be greater than zero"
        requires Currency isNotEmpty else "Currency is required"

        let paymentId = Guid.NewGuid()

        repo.Save({
            Id = paymentId,
            CustomerId = CustomerId,
            Amount = Amount,
            Currency = Currency
        })

        log.Info("Payment created: " + paymentId)

        return paymentId
    end action
end service
```

Examples are also included throughout the specification, especially in:

- Args normalization
- Services and DI
- Validation rules
- Resource usage
- Translation appendix

Additional runnable examples may be added later.

---

## 6. Project Structure

Recommended repository layout:

```
/docs
    01-introduction-and-philosophy.md
    02-lexical-structure.md
    ...
/src
    Compiler
    Tooling
README.md
```

---

## 7. Action Plan and Roadmap

### Phase 1 – Foundation
- [x] Define language philosophy and goals
- [ ] Define complete language specification
- [ ] Define keyword and operator sets
- [ ] Define Args and classArgs model
- [ ] Define resource and validation models

### Phase 2 – Reference Compiler
- [ ] Create ANTLR v4 grammar (lexer + parser)
- [ ] Build AST and semantic model
- [ ] Implement C# code generation
- [ ] Implement diagnostics
- [ ] Implement Args and classArgs lowering

### Phase 3 – Tooling
- [ ] CLI compiler
- [ ] MSBuild integration
- [ ] IDE syntax highlighting
- [ ] Incremental compilation
- [ ] Generator infrastructure

### Phase 4 – Ecosystem
- [ ] Reference standard library
- [ ] Built-in generators (Scriban, OpenAPI, DB)
- [ ] Documentation site
- [ ] Sample projects

---

## 8. Status

This project is currently in the **language design and specification phase**.

No guarantees are made regarding:
- timelines
- production readiness
- long-term support

The primary goal at this stage is **clarity and correctness of the language design**.

---

## 9. Final Note

This project is **not sponsored, endorsed, or promoted** by any company.

If you are interested in related software or tools by the author, you may optionally check:
- https://www.lizard-labs.com  
- https://log-parser.com  

This reference is purely informational.

---

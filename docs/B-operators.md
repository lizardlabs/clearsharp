# Appendix B – Complete Operator List

This appendix defines the **authoritative list of operators** in ClearSharp.

Operators are:
- case-insensitive
- word-based where possible
- paired with explicit opposites
- designed for readability

Symbolic operators are limited to universally understood arithmetic and indexing.

---

## 1. Assignment Operators

| Operator | Meaning |
|--------|--------|
| `=` | Assign value |

Notes:
- assignment to `Nothing` targets is ignored
- compound assignments are intentionally omitted in v1

---

## 2. Arithmetic Operators

| Operator | Meaning |
|--------|--------|
| `+` | Addition / concatenation |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Division |

Rules:
- numeric widening allowed
- division by zero throws

---

## 3. Logical Operators

| Operator | Meaning |
|--------|--------|
| `and` | Logical AND |
| `or` | Logical OR / coalesce |
| `not` | Logical negation |

Notes:
- `or` returns first non-`Nothing` operand if operands are not boolean

---

## 4. Comparison Operators

| Operator | Meaning |
|--------|--------|
| `=` | Equality |
| `<>` | Inequality |
| `<` | Less than |
| `<=` | Less than or equal |
| `>` | Greater than |
| `>=` | Greater than or equal |

Rules:
- comparisons with `Nothing` evaluate to false
- equality is value-based

---

## 5. Identity and Type Operators

| Operator | Meaning |
|--------|--------|
| `is` | Identity / type match |
| `isNot` | Negated identity |

Examples:

```
value is Nothing
value isNot Nothing
```

---

## 6. Collection Membership Operators

| Operator | Meaning |
|--------|--------|
| `contains` | Collection contains value |
| `doesNotContain` | Negated contains |
| `isIn` | Value in collection |
| `isNotIn` | Negated isIn |

---

## 7. Pattern Matching Operators

| Operator | Meaning |
|--------|--------|
| `isLike` | SQL-style pattern match |
| `isNotLike` | Negated pattern match |
| `isMatch` | Regex match |
| `isNotMatch` | Negated regex match |

---

## 8. Positional and Index Operators

| Operator | Meaning |
|--------|--------|
| `[index]` | Index access |
| `first` | First element |
| `last` | Last element |
| `fromEnd n` | Element from end |

Rules:
- out-of-range access returns `Nothing`

---

## 9. Invocation and Member Access

| Operator | Meaning |
|--------|--------|
| `.` | Member access |
| `()` | Invocation |
| `{}` | Object literal |

---

## 10. Concurrency and Coordination Operators

| Operator | Meaning |
|--------|--------|
| `with lock` | Exclusive access |
| `with mutex` | Cross-scope exclusion |
| `with semaphore` | Limited concurrency |

---

## 11. Operator Precedence (Summary)

From highest to lowest:

1. Member access, indexing  
2. Invocation  
3. Unary `not`  
4. Arithmetic (`*`, `/`)  
5. Arithmetic (`+`, `-`)  
6. Comparison  
7. Pattern / membership  
8. Logical `and`  
9. Logical `or`

---

## 12. Reserved Operator Forms

The following operator forms are reserved:

- `??`
- `?.`
- `=>`
- `|`
- `&`

These are intentionally excluded to avoid symbolic overload.

---

### End of Appendix B

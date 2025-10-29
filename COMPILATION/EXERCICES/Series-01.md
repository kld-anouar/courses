# TD01 — Solutions

**Course:** Compilation / Lexical Analysis

**Instructor:** Dr. Anouar Khaldi

---

## Exercise 01 — Fundamental Regular Expressions

Let $\Sigma = {a,b}$.

1. Regular expressions:

   * (a) Strings containing at least one $a$:
     $(a|b)^* a (a|b)^*$
   * (b) Strings that end with $b$:
     $(a|b)^* b$
   * (c) Strings that contain exactly one $a$: $(b)^* a (b)^*$.

2. Examples

* (a) Contain at least one $a$:

  * In: `a`, `ba`, `baba`
  * Not in: `bbb`

* (b) End with $b$:

  * In: `b`, `ab`, `aab`
  * Not in: `aa`

* (c) Exactly one $a$:

  * In: `a`, `bab`, `bbabbb`
  * Not in: `aa` (contains two `a`'s)

---

## Exercise 02 — Describing Languages Formally

Given:

$R_1 = a^*(b ;|; a b a^*)$
$R_2 = (a | b)^* a (a | b)^*$
$R_3 = (a b)^* ;|; (b a)^*$

**Descriptions**:

* $R_1$: Strings consisting of zero or more $a$'s followed either by a single $b$ or by $a b$ followed by zero or more $a$'s. Informally: strings of $a$'s with **one occurrence of $b$ possibly preceded by an $a$**, i.e., strings that contain at least one $b$ where the $b$ may be in contexts `...b` or `...aba...`.

* $R_2$: All strings over ${a,b}$ that contain **at least one $a$**.

* $R_3$: Strings that are entirely repetitions of the block `ab` (possibly empty) or entirely repetitions of the block `ba` (possibly empty). That is, either $(ab)(ab)(ab)...$ or $(ba)(ba)...$.

**Equivalence of $R_1$ and $R_2$**:

They are **not equivalent**. Counterexample: the string `a` belongs to $R_2$ (it contains an `a`) but does not belong to $R_1$ because $R_1$ requires a `b` in the suffix.

---

## Exercise 03 — Regular Definitions and Token Classification

Given definitions:

$Letter = A | B | \dots | Z | a | b | \dots | z$
$Digit = 0 | 1 | \dots | 9$
$Identifier = Letter (Letter | Digit)^*$
$Number = Digit^+ (. Digit^+)? (E (+ | -)? Digit^+)?$

1. **Rewriting `Number` using only union, concatenation, Kleene star**

Replace `+` by `Digit Digit^*` and `?` by union with $\varepsilon$.

$Digit^+ = Digit ; Digit^*$
$(. Digit^+)? = \varepsilon ;|; . ; Digit ; Digit^*$
$(E(+|-) ? Digit^+)? = \varepsilon ;|; E ( ( + | - ) | \varepsilon ) Digit ; Digit^*$

Thus:

$Number = Digit ; Digit^* ; (\varepsilon ; | ; . ; Digit ; Digit^*) ; (\varepsilon ; | ; E ( + ; | ; - ; | ; \varepsilon ) Digit ; Digit^*)$

2. **Keyword `if`**

$Keyword = i; f$

This token matches the exact two-character sequence `if`.

3. **How the lexer differentiates `if` as Keyword from Identifier**

The lexer prioritizes keyword rules. When a lexeme like `if` matches both `Keyword` and `Identifier`, the lexer emits `Keyword`. Otherwise, it emits `Identifier`.

---

## Exercise 04 — Understanding Automaton M1

```mermaid
graph LR
  q0((q0))
  q1((q1))
  q2((q2))
  q0 -- "a" --> q0
  q0 -- "b" --> q1
  q1 -- "a" --> q2
  q2 -- "a,b" --> q2
  classDef accept fill:#e6ffe6,stroke:#2b8f2b,stroke-width:2px;
  class q2 accept;
```

**Language:** M1 accepts all strings over ${a,b}$ that contain the substring `ba`.
Equivalent regular expression: $(a|b)^* b a (a|b)^*$.

---

## Exercise 05 — DFA M2

```mermaid
graph LR
  p0((q0))
  p1((q1))
  p0 -- "1" --> p1
  p0 -- "0" --> p0
  p1 -- "1" --> p1
  p1 -- "0" --> p0
  classDef accept fill:#e6ffe6,stroke:#2b8f2b,stroke-width:2px;
  class p1 accept;
```

Language: all binary strings ending in `1`.
Regular expression: $(0|1)^* 1$.

---

## Exercise 06 — DFA for Exactly Two $a$

```mermaid
graph LR
  s0((q0))
  s1((q1))
  s2((q2))
  s3((q3))
  s0 -- "a" --> s1
  s0 -- "b" --> s0
  s1 -- "a" --> s2
  s1 -- "b" --> s1
  s2 -- "a" --> s3
  s2 -- "b" --> s2
  s3 -- "a" --> s3
  s3 -- "b" --> s3
  classDef accept fill:#e6ffe6,stroke:#2b8f2b,stroke-width:2px;
  class s2 accept;
```

Regular expression: $(b)^* a (b)^* a (b)^*$.

---

## Exercise 07 — Automaton M3

```mermaid
graph LR
  r0((q0))
  r1((q1))
  r2((q2))
  r3((q3))
  r0 -- "a" --> r1
  r1 -- "b" --> r2
  r1 -- "a" --> r3
  r2 -- "a,b" --> r2
  classDef accept fill:#e6ffe6,stroke:#2b8f2b,stroke-width:2px;
  class r2 accept;
```

Language: strings starting with `a`, containing the substring `ab`.
Regular expression: $a (a|b)^* b (a|b)^*$.

---

## Exercise 08 — DFA for Strings Ending with `01`

```mermaid
graph LR
  c0((q0))
  c1((q1))
  c2((q2))
  c0 -- "0" --> c1
  c0 -- "1" --> c0
  c1 -- "0" --> c1
  c1 -- "1" --> c2
  c2 -- "0" --> c1
  c2 -- "1" --> c0
  classDef accept fill:#e6ffe6,stroke:#2b8f2b,stroke-width:2px;
  class c2 accept;
```

Language: binary strings ending with `01`.
Regular expression: $(0|1)^* 0 1$.

---

**End of Solutions**

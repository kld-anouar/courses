<style>
/* Remove the white background from the initial polygon */
polygon[fill="white"] {
  fill: none !important;
}

/* Style for all path and ellipse elements (lines, arrows, circles) */
.graph path, .graph ellipse {
  stroke: white !important;
  fill: none !important; /* Ensures state fills are transparent/none */
}

/* Style for all text (state names, transition symbols) */
.graph text {
  fill: white !important;
}

/* Specific fix for final state rings and arrow heads */
.graph polygon[fill="black"] {
  fill: white !important;
  stroke: white !important;
}
</style>

# Chapter 2: Lexical Analysis
**Course:** Compilation / Lexical Analysis
**Instructor:** Dr. Anouar Khaldi

---

## Lexical Analysis

### Overview • Tokens • Formal Languages • Regular Expressions • Examples

---

## Overview of Lexical Analysis

Lexical analysis is the **first phase** of the compiler front‑end.

It:

* Converts **characters → tokens**
* Acts as a bridge between raw code and parser
* Simplifies parsing
* Detects early lexical errors

---

## Definition

Lexical Analysis (Tokenization) transforms a **sequence of characters** into a **sequence of tokens**.

The **Lexer** (scanner/tokenizer):

* Provides the parser with structured input
* Recognizes token patterns via regex / automata
* Removes whitespace & comments
* Handles lexical errors
* Manages symbol table entries

---

## Key Concepts

### Pattern

A **pattern** is a rule (often a regular expression) defining valid words in source code.

Examples:

* Identifiers: start with a letter + letters/digits → `count1`, `totalSum`
* Numbers: digits only → `123`, `45`
* Operators: `+`, `-`, `*`, `/`

---

### Lexeme

A **lexeme** is the actual characters in source code that match a pattern.

Example:

```c
x = 10;
```

Lexemes:

* `x` → identifier
* `=` → operator
* `10` → number

---

### Token

A **token** is the category/class assigned to each lexeme.

| Token      | Lexeme | Pattern                |
| ---------- | ------ | ---------------------- |
| KEYWORD    | `if`   | "if"                   |
| IDENTIFIER | `x`    | Letter (letter/digit)* |
| ARITH_OP   | `+`    | +, -, *, /             |

---

## Most Common Token Types

* **Keywords** – `if`, `while`, `class`
* **Identifiers** – names of variables/functions
* **Literals** – numbers, strings, booleans
* **Operators** – `=`, `+`, `-`, `*`, `/`, `<`, `>`, `==`, `!=`, `&&`, `||`, `!`
* **Delimiters** – `;`, `,`, `()`, `{}`
* **Comments** – ignored by lexer

---

## Token Attributes

Each token has:

* Token Name (type)
* Value (actual text or symbol table reference)
* Position (line/column)

Example:

```
result := 3.14 * radius ^ 2
```

Tokens:

```
IDENTIFIER, ASSIGN_OP, NUMBER, MULTIPLY_OP, IDENTIFIER, EXPONENT_OP, NUMBER
```

---

## Lexical Error Handling

Lexical errors include:

* Illegal characters
* Invalid numbers
* Unclosed strings
* Misspelled keywords

Recovery techniques:

* **Panic mode** – skip symbols
* **Insertion** – add missing char
* **Replacement** – correct wrong char
* **Ignore** – drop invalid char

---

## Formal Languages

A **formal language** is a set of strings over an alphabet Σ.

Key terms:

* Alphabet Σ: finite set of symbols (e.g., `{a, b, c}`)
* String: sequence of symbols (e.g., `"ab"`)
* Language: set of strings
* Grammar: rules for forming strings

Symbols:

* `ε` = empty string
* `∅` = empty language

---

## Regular Expressions (Regex)

A **regular expression (RE)** defines patterns to match text.

Used for:

* Token specification
* Pattern matching
* Token creation

Example: `[0-9]+` matches `123`, `42`

---

## Formal Definition of Regular Expressions

Given alphabet Σ:

* `∅` → empty language
* `ε` → {ε}
* `a ∈ Σ` → {"a"}

---

## Regular Expression Operations

* Union: `R|S`
* Concatenation: `RS`
* Kleene star: `R*` (0+ repetitions)
* Plus: `R+` (1+ repetitions)
* Optional: `R?` (0 or 1)

---

## Algebraic Properties

| Property       | Rule                 | Example    |
| -------------- | -------------------- | ---------- |
| Associativity  | `(R+S)+T = R+(S+T)`  | `{a,b,c}`  |
| Commutativity  | `R+S = S+R`          | `{a,b}`    |
| Distributivity | `R(S+T)=RS+RT`       | `ab or ac` |
| Identity       | `R+∅ = R` ; `Rε = R` | `aε = a`   |
| Annihilator    | `R∅ = ∅`             | `a∅ = ∅`   |
| Idempotence    | `R+R = R`            | `a`        |

---

## Examples of Regular Expressions

| Language                       | Regular Expression              |
| ------------------------------ | ------------------------------- |
| Strings starting with `0`      | `0(0+1)*`                       |
| Start with `0` and end in `1`  | `0(0+1)*1`                      |
| Start `0` or end `1`           | `0(0+1)* + (0+1)*1`             |
| At least one `0`               | `(0+1)*0(0+1)*`                 |
| Length 2 or 3 over `{a,b,c,d}` | `(a+b+c+d)(a+b+c+d)(a+b+c+d+ε)` |
| No repeated `1`                | `(10+0)*(ε+1)`                  |

---

## More Regex Examples

| RE       | Meaning                   |
| -------- | ------------------------- |
| `a*b*`   | All `a`s before `b`s      |
| `(ab)*`  | Alternating `a` and `b`   |
| `a*ba*`  | Exactly one `b`           |
| `a?b*`   | At most one `a`, then b's |
| `a*b⁺a*` | At least one b in middle  |
| `a*b*c*` | a's, then b's, then c's   |

---

## Finite Automata in Lexical Analysis

Regular expressions define token patterns, but we need a machine to **recognize** them.
That machine is the **Finite Automaton (FA)**.

### Finite Automaton (FA)

* Finite number of states
* Reads input symbol by symbol
* Accepts input if ending in an accepting state

### Types

* **DFA** — Deterministic Finite Automaton (1 transition per input)
* **NFA** — Nondeterministic Finite Automaton (multiple transitions / ε-moves)

### DFA Formal Definition

```
M = (Q, Σ, δ, q₀, F)
```

* `Q`: set of states
* `Σ`: input alphabet
* `δ`: transition function `Q × Σ → Q`
* `q₀`: start state
* `F`: accepting states

### Example DFA: Strings ending in `1`

```
State | 0   | 1
----- | --- | ---
→q1   | q1  | q2
*q2   | q1  | q2
```

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="233pt" height="92pt" viewBox="0.00 0.00 233.00 92.00">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 88.19)">
<title>DFA</title>
<polygon fill="white" stroke="none" points="-4,4 -4,-88.19 228.77,-88.19 228.77,4 -4,4"/>
<!-- q1 -->
<g id="node1" class="node">
<title>q1</title>
<ellipse fill="none" stroke="black" cx="111.69" cy="-24.69" rx="20.69" ry="20.69"/>
<text xml:space="preserve" text-anchor="middle" x="111.69" y="-20.49" font-family="Times,serif" font-size="14.00">q1</text>
</g>
<!-- q1&#45;&gt;q1 -->
<g id="edge2" class="edge">
<title>q1-&gt;q1</title>
<path fill="none" stroke="black" d="M104.56,-44.6C103.57,-54.53 105.95,-63.39 111.69,-63.39 115.01,-63.39 117.21,-60.43 118.28,-56.01"/>
<polygon fill="black" stroke="black" points="121.77,-56.27 118.75,-46.11 114.78,-55.93 121.77,-56.27"/>
<text xml:space="preserve" text-anchor="middle" x="111.69" y="-67.59" font-family="Times,serif" font-size="14.00">0</text>
</g>
<!-- q2 -->
<g id="node2" class="node">
<title>q2</title>
<ellipse fill="none" stroke="black" cx="200.08" cy="-24.69" rx="20.69" ry="20.69"/>
<ellipse fill="none" stroke="black" cx="200.08" cy="-24.69" rx="24.69" ry="24.69"/>
<text xml:space="preserve" text-anchor="middle" x="200.08" y="-20.49" font-family="Times,serif" font-size="14.00">q2</text>
</g>
<!-- q1&#45;&gt;q2 -->
<g id="edge3" class="edge">
<title>q1-&gt;q2</title>
<path fill="none" stroke="black" d="M132.73,-24.69C141.93,-24.69 153.12,-24.69 163.66,-24.69"/>
<polygon fill="black" stroke="black" points="163.65,-28.19 173.65,-24.69 163.65,-21.19 163.65,-28.19"/>
<text xml:space="preserve" text-anchor="middle" x="153.89" y="-28.89" font-family="Times,serif" font-size="14.00">1</text>
</g>
<!-- q2&#45;&gt;q1 -->
<g id="edge4" class="edge">
<title>q2-&gt;q1</title>
<path fill="none" stroke="black" d="M180.12,-10.18C171.3,-5.12 160.54,-1.28 150.39,-3.89 146.58,-4.87 142.75,-6.33 139.06,-8.02"/>
<polygon fill="black" stroke="black" points="137.74,-4.76 130.53,-12.53 141,-10.95 137.74,-4.76"/>
<text xml:space="preserve" text-anchor="middle" x="153.89" y="-8.09" font-family="Times,serif" font-size="14.00">0</text>
</g>
<!-- q2&#45;&gt;q2 -->
<g id="edge5" class="edge">
<title>q2-&gt;q2</title>
<path fill="none" stroke="black" d="M191.55,-47.97C190.74,-58.38 193.58,-67.39 200.08,-67.39 204.04,-67.39 206.64,-64.04 207.89,-59.07"/>
<polygon fill="black" stroke="black" points="211.35,-59.68 208.51,-49.48 204.37,-59.23 211.35,-59.68"/>
<text xml:space="preserve" text-anchor="middle" x="200.08" y="-71.59" font-family="Times,serif" font-size="14.00">1</text>
</g>
<!-- start -->
<g id="node3" class="node">
<title>start</title>
</g>
<!-- start&#45;&gt;q1 -->
<g id="edge1" class="edge">
<title>start-&gt;q1</title>
<path fill="none" stroke="black" d="M53.71,-24.69C61.77,-24.69 70.75,-24.69 79.16,-24.69"/>
<polygon fill="black" stroke="black" points="79,-28.19 89,-24.69 79,-21.19 79,-28.19"/>
</g>
</g>
</svg>

### Example DFA: Contains at least one `1` and even number of `0`s after last `1`

```
State | 0   | 1
----- | --- | ---
→q1   | q1  | q2
*q2   | q3  | q2
q3    | q2  | q2
```

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="331pt" height="92pt" viewBox="0.00 0.00 331.00 92.00">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 88.19)">
<title>DFA2</title>
<polygon fill="white" stroke="none" points="-4,4 -4,-88.19 327.16,-88.19 327.16,4 -4,4"/>
<!-- q1 -->
<g id="node1" class="node">
<title>q1</title>
<ellipse fill="none" stroke="black" cx="111.69" cy="-24.69" rx="20.69" ry="20.69"/>
<text xml:space="preserve" text-anchor="middle" x="111.69" y="-20.49" font-family="Times,serif" font-size="14.00">q1</text>
</g>
<!-- q1&#45;&gt;q1 -->
<g id="edge2" class="edge">
<title>q1-&gt;q1</title>
<path fill="none" stroke="black" d="M104.56,-44.6C103.57,-54.53 105.95,-63.39 111.69,-63.39 115.01,-63.39 117.21,-60.43 118.28,-56.01"/>
<polygon fill="black" stroke="black" points="121.77,-56.27 118.75,-46.11 114.78,-55.93 121.77,-56.27"/>
<text xml:space="preserve" text-anchor="middle" x="111.69" y="-67.59" font-family="Times,serif" font-size="14.00">0</text>
</g>
<!-- q2 -->
<g id="node2" class="node">
<title>q2</title>
<ellipse fill="none" stroke="black" cx="200.08" cy="-24.69" rx="20.69" ry="20.69"/>
<ellipse fill="none" stroke="black" cx="200.08" cy="-24.69" rx="24.69" ry="24.69"/>
<text xml:space="preserve" text-anchor="middle" x="200.08" y="-20.49" font-family="Times,serif" font-size="14.00">q2</text>
</g>
<!-- q1&#45;&gt;q2 -->
<g id="edge3" class="edge">
<title>q1-&gt;q2</title>
<path fill="none" stroke="black" d="M132.73,-24.69C141.93,-24.69 153.12,-24.69 163.66,-24.69"/>
<polygon fill="black" stroke="black" points="163.65,-28.19 173.65,-24.69 163.65,-21.19 163.65,-28.19"/>
<text xml:space="preserve" text-anchor="middle" x="153.89" y="-28.89" font-family="Times,serif" font-size="14.00">1</text>
</g>
<!-- q2&#45;&gt;q2 -->
<g id="edge5" class="edge">
<title>q2-&gt;q2</title>
<path fill="none" stroke="black" d="M191.55,-47.97C190.74,-58.38 193.58,-67.39 200.08,-67.39 204.04,-67.39 206.64,-64.04 207.89,-59.07"/>
<polygon fill="black" stroke="black" points="211.35,-59.68 208.51,-49.48 204.37,-59.23 211.35,-59.68"/>
<text xml:space="preserve" text-anchor="middle" x="200.08" y="-71.59" font-family="Times,serif" font-size="14.00">1</text>
</g>
<!-- q3 -->
<g id="node3" class="node">
<title>q3</title>
<ellipse fill="none" stroke="black" cx="302.47" cy="-24.69" rx="20.69" ry="20.69"/>
<text xml:space="preserve" text-anchor="middle" x="302.47" y="-20.49" font-family="Times,serif" font-size="14.00">q3</text>
</g>
<!-- q2&#45;&gt;q3 -->
<g id="edge4" class="edge">
<title>q2-&gt;q3</title>
<path fill="none" stroke="black" d="M225.13,-24.69C238.54,-24.69 255.41,-24.69 269.88,-24.69"/>
<polygon fill="black" stroke="black" points="269.82,-28.19 279.82,-24.69 269.82,-21.19 269.82,-28.19"/>
<text xml:space="preserve" text-anchor="middle" x="253.27" y="-28.89" font-family="Times,serif" font-size="14.00">0</text>
</g>
<!-- q3&#45;&gt;q2 -->
<g id="edge6" class="edge">
<title>q3-&gt;q2</title>
<path fill="none" stroke="black" d="M284.97,-13.23C278.64,-9.48 271.16,-5.8 263.77,-3.89 253.25,-1.18 241.93,-3.33 231.85,-7.1"/>
<polygon fill="black" stroke="black" points="230.6,-3.83 222.87,-11.08 233.43,-10.23 230.6,-3.83"/>
<text xml:space="preserve" text-anchor="middle" x="253.27" y="-8.09" font-family="Times,serif" font-size="14.00">0, 1</text>
</g>
<!-- start -->
<g id="node4" class="node">
<title>start</title>
</g>
<!-- start&#45;&gt;q1 -->
<g id="edge1" class="edge">
<title>start-&gt;q1</title>
<path fill="none" stroke="black" d="M53.71,-24.69C61.77,-24.69 70.75,-24.69 79.16,-24.69"/>
<polygon fill="black" stroke="black" points="79,-28.19 89,-24.69 79,-21.19 79,-28.19"/>
</g>
</g>
</svg>

### NFA Formal Definition

```
M = (Q, Σ, δ, q₀, F)
```

Where `δ: Q × (Σ ∪ {ε}) → P(Q)` (can move without consuming input)

### Example NFA

Transitions:

```
δ(q1,0)={q1}
δ(q1,1)={q1,q2}
δ(q2,0)={q3}
δ(q2,ε)={q3}
δ(q3,1)={q4}
δ(q4,0)={q4}
δ(q4,1)={q4}
```

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="416pt" height="92pt" viewBox="0.00 0.00 416.00 92.00">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 88.19)">
<title>NFA</title>
<polygon fill="white" stroke="none" points="-4,4 -4,-88.19 411.55,-88.19 411.55,4 -4,4"/>
<!-- q1 -->
<g id="node1" class="node">
<title>q1</title>
<ellipse fill="none" stroke="black" cx="111.69" cy="-24.69" rx="20.69" ry="20.69"/>
<text xml:space="preserve" text-anchor="middle" x="111.69" y="-20.49" font-family="Times,serif" font-size="14.00">q1</text>
</g>
<!-- q1&#45;&gt;q1 -->
<g id="edge2" class="edge">
<title>q1-&gt;q1</title>
<path fill="none" stroke="black" d="M104.56,-44.6C103.57,-54.53 105.95,-63.39 111.69,-63.39 115.01,-63.39 117.21,-60.43 118.28,-56.01"/>
<polygon fill="black" stroke="black" points="121.77,-56.27 118.75,-46.11 114.78,-55.93 121.77,-56.27"/>
<text xml:space="preserve" text-anchor="middle" x="111.69" y="-67.59" font-family="Times,serif" font-size="14.00">0, 1</text>
</g>
<!-- q2 -->
<g id="node2" class="node">
<title>q2</title>
<ellipse fill="none" stroke="black" cx="196.08" cy="-24.69" rx="20.69" ry="20.69"/>
<text xml:space="preserve" text-anchor="middle" x="196.08" y="-20.49" font-family="Times,serif" font-size="14.00">q2</text>
</g>
<!-- q1&#45;&gt;q2 -->
<g id="edge3" class="edge">
<title>q1-&gt;q2</title>
<path fill="none" stroke="black" d="M132.64,-24.69C141.98,-24.69 153.33,-24.69 163.78,-24.69"/>
<polygon fill="black" stroke="black" points="163.52,-28.19 173.52,-24.69 163.52,-21.19 163.52,-28.19"/>
<text xml:space="preserve" text-anchor="middle" x="153.89" y="-28.89" font-family="Times,serif" font-size="14.00">1</text>
</g>
<!-- q3 -->
<g id="node3" class="node">
<title>q3</title>
<ellipse fill="none" stroke="black" cx="294.47" cy="-24.69" rx="20.69" ry="20.69"/>
<text xml:space="preserve" text-anchor="middle" x="294.47" y="-20.49" font-family="Times,serif" font-size="14.00">q3</text>
</g>
<!-- q2&#45;&gt;q3 -->
<g id="edge4" class="edge">
<title>q2-&gt;q3</title>
<path fill="none" stroke="black" d="M217.05,-24.69C230.1,-24.69 247.42,-24.69 262.27,-24.69"/>
<polygon fill="black" stroke="black" points="262.01,-28.19 272.01,-24.69 262.01,-21.19 262.01,-28.19"/>
<text xml:space="preserve" text-anchor="middle" x="245.27" y="-28.89" font-family="Times,serif" font-size="14.00">0, ε</text>
</g>
<!-- q4 -->
<g id="node4" class="node">
<title>q4</title>
<ellipse fill="none" stroke="black" cx="382.85" cy="-24.69" rx="20.69" ry="20.69"/>
<ellipse fill="none" stroke="black" cx="382.85" cy="-24.69" rx="24.69" ry="24.69"/>
<text xml:space="preserve" text-anchor="middle" x="382.85" y="-20.49" font-family="Times,serif" font-size="14.00">q4</text>
</g>
<!-- q3&#45;&gt;q4 -->
<g id="edge5" class="edge">
<title>q3-&gt;q4</title>
<path fill="none" stroke="black" d="M315.5,-24.69C324.7,-24.69 335.89,-24.69 346.44,-24.69"/>
<polygon fill="black" stroke="black" points="346.42,-28.19 356.42,-24.69 346.42,-21.19 346.42,-28.19"/>
<text xml:space="preserve" text-anchor="middle" x="336.66" y="-28.89" font-family="Times,serif" font-size="14.00">1</text>
</g>
<!-- q4&#45;&gt;q4 -->
<g id="edge6" class="edge">
<title>q4-&gt;q4</title>
<path fill="none" stroke="black" d="M374.33,-47.97C373.52,-58.38 376.36,-67.39 382.85,-67.39 386.81,-67.39 389.41,-64.04 390.66,-59.07"/>
<polygon fill="black" stroke="black" points="394.13,-59.68 391.28,-49.48 387.14,-59.23 394.13,-59.68"/>
<text xml:space="preserve" text-anchor="middle" x="382.85" y="-71.59" font-family="Times,serif" font-size="14.00">0,1</text>
</g>
<!-- start -->
<g id="node5" class="node">
<title>start</title>
</g>
<!-- start&#45;&gt;q1 -->
<g id="edge1" class="edge">
<title>start-&gt;q1</title>
<path fill="none" stroke="black" d="M53.71,-24.69C61.77,-24.69 70.75,-24.69 79.16,-24.69"/>
<polygon fill="black" stroke="black" points="79,-28.19 89,-24.69 79,-21.19 79,-28.19"/>
</g>
</g>
</svg>

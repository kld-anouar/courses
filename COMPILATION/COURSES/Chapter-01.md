# Compilation – Introduction
**Course:** Compilation / Lexical Analysis

**Instructor:** Dr. Anouar Khaldi

---

**Democratic and Popular Republic of Algeria**
**Ministry of Higher Education and Scientific Research**
**University of Kasdi Merbah, Ouargla**
*Computer Security Engineer*

**October 2025**

## Introduction

### Definition • Pipeline • Purpose • Example


## Introduction to Compilation

Programming languages like **C, Java, Python** are human‑friendly.
Computers only understand **machine code (binary)**.
A **compiler** translates between them.

### What is Compilation?

| High‑level language | → | Low‑level language |
| ------------------- | - | ------------------ |

A **compiler** is software that translates high‑level code into machine code.

**Main Functions:**

* **Translation** – Source → machine code
* **Error Detection** – Finds syntax & semantic errors
* **Optimization** – Improves performance

## Compiler vs Interpreter

| Feature        | Compiler            | Interpreter          |
| -------------- | ------------------- | -------------------- |
| Translation    | Entire program      | Line‑by‑line         |
| Execution      | Produces executable | Executes directly    |
| Speed          | Faster              | Slower               |
| Error Handling | Reports all errors  | Stops at first error |
| Examples       | C, C++, Go          | Python, JavaScript   |


## Compilation Pipeline

### Front‑End

* Lexical Analysis
* Syntax Analysis
* Semantic Analysis

> Ensures correctness

### Back‑End

* Optimization
* Code Generation
* Linking & Loading

> Produces efficient executable

## Example: Compilation Stages

### Source Code

```
position = initial + rate * 60
```

### 1️⃣ Lexical Analysis

| Lexeme     | Token          |
| ---------- | -------------- |
| `position` | Identifier     |
| `=`        | Assignment     |
| `initial`  | Identifier     |
| `+`        | Addition       |
| `rate`     | Identifier     |
| `*`        | Multiplication |
| `60`       | Number         |

Token stream:

```
<id, E1> <=> <id, E2> <+> <id, E3> <*> <num, 60>
```

### 2️⃣ Syntax Analysis

Parse tree created

```
=
├── id1
└── +
    ├── id2
    └── *
        ├── id3
        └── 60
```

### 3️⃣ Semantic Analysis

Adds type info / checks types

### 4️⃣ Intermediate Code

```
t1 = inttofloat(60)
t2 = id3 * t1
t3 = id2 + t2
id1 = t3
```

### 5️⃣ Optimization

```
t1 = id3 * 60.0
id1 = id2 + t1
```

### 6️⃣ Code Generation (Assembly‑like)

```
IDF R2, id3
MULF R2, R2, #60.0
LDF R1, id2
ADDF R1, R1, R2
STF id1, R1
```

### 7️⃣ Linking & Loading

* Links object code with libraries
* Loads executable into memory
* Program runs

## Applications of Compiler Design

* Programming language development
* IDEs, debuggers, analyzers
* Performance optimization
* Natural Language Processing
* Database query engines
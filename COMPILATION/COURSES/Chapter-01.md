# Compilation – Introduction

**Course:** Compilation / Introduction
**Instructor:** Dr. Anouar Khaldi

## 1 Introduction

### 1.1 Definition • Pipeline • Purpose • Example

## 2 Introduction to Compilation

Programming languages like **C, Java, Python** are human‑friendly.
Computers only understand **machine code (binary)**.
A **compiler** translates between them.

### 2.1 What is Compilation?

| High‑level language | → | Low‑level language |
| ------------------- | - | ------------------ |

A **compiler** is software that translates high‑level code into machine code.

**Main Functions:**

* **Translation** – Source → machine code
* **Error Detection** – Finds syntax & semantic errors
* **Optimization** – Improves performance

## 3 Compiler vs Interpreter

| Feature        | Compiler            | Interpreter          |
| -------------- | ------------------- | -------------------- |
| Translation    | Entire program      | Line‑by‑line         |
| Execution      | Produces executable | Executes directly    |
| Speed          | Faster              | Slower               |
| Error Handling | Reports all errors  | Stops at first error |
| Examples       | C, C++, Go          | Python, JavaScript   |

## 4 Compilation Pipeline

### 4.1 Front‑End

* Lexical Analysis
* Syntax Analysis
* Semantic Analysis

Ensures correctness

### 4.2 Back‑End

* Optimization
* Code Generation
* Linking & Loading

Produces efficient executable

## 5 Example: Compilation Stages

### 5.1 Source Code

```
position = initial + rate * 60
```

### 5.2 Lexical Analysis

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

### 5.3 Syntax Analysis

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

### 5.4 Semantic Analysis

Adds type info / checks types

### 5.5 Intermediate Code

```
t1 = inttofloat(60)
t2 = id3 * t1
t3 = id2 + t2
id1 = t3
```

### 5.6 Optimization

```
t1 = id3 * 60.0
id1 = id2 + t1
```

### 5.7 Code Generation (Assembly‑like)

```
IDF R2, id3
MULF R2, R2, #60.0
LDF R1, id2
ADDF R1, R1, R2
STF id1, R1
```

### 5.8 Linking & Loading

* Links object code with libraries
* Loads executable into memory
* Program runs

## 6 Applications of Compiler Design

* Programming language development
* IDEs, debuggers, analyzers
* Performance optimization
* Natural Language Processing
* Database query engines

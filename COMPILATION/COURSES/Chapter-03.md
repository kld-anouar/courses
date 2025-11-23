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

## Chapter 3: Syntactic Analysis
**Course:** Compilation / Syntactic Analysis</br>
**Instructor:** Dr. Anouar Khaldi

---
### 3.1 Introduction to Syntax Analysis

Syntax analysis (parsing) checks if the token sequence conforms to the grammar rules and builds a parse tree.

**Input:** Token stream from lexical analyzer  
**Output:** Parse tree or syntax tree

### 3.2 Context-Free Grammars (CFG)

A grammar $G$ is defined as: $G = (N, T, P, S)$

- $N$: Set of non-terminals
- $T$: Set of terminals (tokens)
- $P$: Set of production rules
- $S$: Start symbol

**Example Grammar:**
```
E → E + T | T
T → T * F | F
F → (E) | id
```


**$ N $ (Non-terminals)**:  
  $ \{ E, T, F \} $  
  → These are variables that can be expanded.

**$ T $ (Terminals)**:  
  $ \{ +, *, (, ), \text{id} \} $  
  → These are literal symbols in the language (operators, parentheses, identifiers).

**$ P $ (Production Rules)**:  
  $$
  \begin{align*}
  E &\to E + T \\
  E &\to T \\
  T &\to T * F \\
  T &\to F \\
  F &\to (E) \\
  F &\to \text{id}
  \end{align*}
  $$

**$ S $ (Start Symbol)**:  
  $ E $  
  → The grammar starts with `E`.

---


### 3.3 Derivations and Parse Trees

**Leftmost Derivation:** Always expand the leftmost non-terminal first  
**Rightmost Derivation:** Always expand the rightmost non-terminal first

**Example for `id + id * id`:**

Leftmost derivation:
```python
E
⇒ E + T              (expand **E → E + T**)
⇒ T + T              (expand **leftmost E → T**)
⇒ F + T              (expand **leftmost T → F**)
⇒ id + T             (expand **F → id**)
⇒ id + T * F         (expand **T → T * F**)
⇒ id + F * F         (expand **leftmost T → F**)
⇒ id + id * F        (expand **F → id**)
⇒ id + id * id       (expand **F → id**)
```
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="250pt" height="386pt" viewBox="0.00 0.00 250.00 386.00">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 382)">
<title>ParseTree</title>
<!-- E1 -->
<g id="node1" class="node">
<title>E1</title>
<ellipse fill="none" stroke="white" cx="88.4" cy="-354.6" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="88.4" y="-348.6" font-family="Arial" font-size="20.00" fill="white">E</text>
</g>
<!-- E2 -->
<g id="node2" class="node">
<title>E2</title>
<ellipse fill="none" stroke="white" cx="23.4" cy="-271.8" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="23.4" y="-265.8" font-family="Arial" font-size="20.00" fill="white">E</text>
</g>
<!-- E1&#45;&gt;E2 -->
<g id="edge1" class="edge">
<title>E1-&gt;E2</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M73.97,-335.67C65.47,-325.1 54.56,-311.53 45.04,-299.7"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="47.92,-297.7 38.92,-292.1 42.46,-302.08 47.92,-297.7"/>
</g>
<!-- plus -->
<g id="node3" class="node">
<title>plus</title>
<ellipse fill="none" stroke="white" cx="88.4" cy="-271.8" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="88.4" y="-265.8" font-family="Arial" font-size="20.00" fill="white">+</text>
</g>
<!-- E1&#45;&gt;plus -->
<g id="edge2" class="edge">
<title>E1-&gt;plus</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M88.4,-331.09C88.4,-323.71 88.4,-315.33 88.4,-307.28"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="91.9,-307.28 88.4,-297.28 84.9,-307.28 91.9,-307.28"/>
</g>
<!-- T1 -->
<g id="node4" class="node">
<title>T1</title>
<ellipse fill="none" stroke="white" cx="153.4" cy="-271.8" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="153.4" y="-265.8" font-family="Arial" font-size="20.00" fill="white">T</text>
</g>
<!-- E1&#45;&gt;T1 -->
<g id="edge3" class="edge">
<title>E1-&gt;T1</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M102.83,-335.67C111.33,-325.1 122.24,-311.53 131.76,-299.7"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="134.34,-302.08 137.88,-292.1 128.88,-297.7 134.34,-302.08"/>
</g>
<!-- T2 -->
<g id="node5" class="node">
<title>T2</title>
<ellipse fill="none" stroke="white" cx="23.4" cy="-189" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="23.4" y="-183" font-family="Arial" font-size="20.00" fill="white">T</text>
</g>
<!-- E2&#45;&gt;T2 -->
<g id="edge4" class="edge">
<title>E2-&gt;T2</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M23.4,-248.29C23.4,-240.91 23.4,-232.53 23.4,-224.48"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="26.9,-224.48 23.4,-214.48 19.9,-224.48 26.9,-224.48"/>
</g>
<!-- T3 -->
<g id="node6" class="node">
<title>T3</title>
<ellipse fill="none" stroke="white" cx="88.4" cy="-189" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="88.4" y="-183" font-family="Arial" font-size="20.00" fill="white">T</text>
</g>
<!-- T1&#45;&gt;T3 -->
<g id="edge7" class="edge">
<title>T1-&gt;T3</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M138.97,-252.87C130.47,-242.3 119.56,-228.73 110.04,-216.9"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="112.92,-214.9 103.92,-209.3 107.46,-219.28 112.92,-214.9"/>
</g>
<!-- star -->
<g id="node7" class="node">
<title>star</title>
<ellipse fill="none" stroke="white" cx="153.4" cy="-189" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="153.4" y="-183" font-family="Arial" font-size="20.00" fill="white">*</text>
</g>
<!-- T1&#45;&gt;star -->
<g id="edge8" class="edge">
<title>T1-&gt;star</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M153.4,-248.29C153.4,-240.91 153.4,-232.53 153.4,-224.48"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="156.9,-224.48 153.4,-214.48 149.9,-224.48 156.9,-224.48"/>
</g>
<!-- F1 -->
<g id="node8" class="node">
<title>F1</title>
<ellipse fill="none" stroke="white" cx="218.4" cy="-189" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="218.4" y="-183" font-family="Arial" font-size="20.00" fill="white">F</text>
</g>
<!-- T1&#45;&gt;F1 -->
<g id="edge9" class="edge">
<title>T1-&gt;F1</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M167.83,-252.87C176.33,-242.3 187.24,-228.73 196.76,-216.9"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="199.34,-219.28 202.88,-209.3 193.88,-214.9 199.34,-219.28"/>
</g>
<!-- F2 -->
<g id="node9" class="node">
<title>F2</title>
<ellipse fill="none" stroke="white" cx="23.4" cy="-106.2" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="23.4" y="-100.2" font-family="Arial" font-size="20.00" fill="white">F</text>
</g>
<!-- T2&#45;&gt;F2 -->
<g id="edge5" class="edge">
<title>T2-&gt;F2</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M23.4,-165.49C23.4,-158.11 23.4,-149.73 23.4,-141.68"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="26.9,-141.68 23.4,-131.68 19.9,-141.68 26.9,-141.68"/>
</g>
<!-- F3 -->
<g id="node10" class="node">
<title>F3</title>
<ellipse fill="none" stroke="white" cx="88.4" cy="-106.2" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="88.4" y="-100.2" font-family="Arial" font-size="20.00" fill="white">F</text>
</g>
<!-- T3&#45;&gt;F3 -->
<g id="edge10" class="edge">
<title>T3-&gt;F3</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M88.4,-165.49C88.4,-158.11 88.4,-149.73 88.4,-141.68"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="91.9,-141.68 88.4,-131.68 84.9,-141.68 91.9,-141.68"/>
</g>
<!-- id1 -->
<g id="node11" class="node">
<title>id1</title>
<ellipse fill="none" stroke="white" cx="218.4" cy="-106.2" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="218.4" y="-100.2" font-family="Arial" font-size="20.00" fill="white">id</text>
</g>
<!-- F1&#45;&gt;id1 -->
<g id="edge12" class="edge">
<title>F1-&gt;id1</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M218.4,-165.49C218.4,-158.11 218.4,-149.73 218.4,-141.68"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="221.9,-141.68 218.4,-131.68 214.9,-141.68 221.9,-141.68"/>
</g>
<!-- id2 -->
<g id="node12" class="node">
<title>id2</title>
<ellipse fill="none" stroke="white" cx="23.4" cy="-23.4" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="23.4" y="-17.4" font-family="Arial" font-size="20.00" fill="white">id</text>
</g>
<!-- F2&#45;&gt;id2 -->
<g id="edge6" class="edge">
<title>F2-&gt;id2</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M23.4,-82.69C23.4,-75.31 23.4,-66.93 23.4,-58.88"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="26.9,-58.88 23.4,-48.88 19.9,-58.88 26.9,-58.88"/>
</g>
<!-- id3 -->
<g id="node13" class="node">
<title>id3</title>
<ellipse fill="none" stroke="white" cx="88.4" cy="-23.4" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="88.4" y="-17.4" font-family="Arial" font-size="20.00" fill="white">id</text>
</g>
<!-- F3&#45;&gt;id3 -->
<g id="edge11" class="edge">
<title>F3-&gt;id3</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M88.4,-82.69C88.4,-75.31 88.4,-66.93 88.4,-58.88"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="91.9,-58.88 88.4,-48.88 84.9,-58.88 91.9,-58.88"/>
</g>
</g>
</svg>

Rightmost derivation:
```python
E
⇒ E + T             (use E → E + T)
⇒ E + T * F         (expand rightmost T → T * F)
⇒ E + T * id        (expand F → id)
⇒ E + F * id        (expand rightmost T → F)
⇒ E + id * id       (expand F → id)
⇒ T + id * id       (expand E → T)
⇒ F + id * id       (expand T → F)
⇒ id + id * id      (expand F → id)
```
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="250pt" height="386pt" viewBox="0.00 0.00 250.00 386.00">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 382)">
<title>ParseTree</title>
<!-- E1 -->
<g id="node1" class="node">
<title>E1</title>
<ellipse fill="none" stroke="white" cx="153.4" cy="-354.6" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="153.4" y="-348" font-family="Arial" font-size="22.00" fill="white">E</text>
</g>
<!-- T1 -->
<g id="node2" class="node">
<title>T1</title>
<ellipse fill="none" stroke="white" cx="88.4" cy="-271.8" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="88.4" y="-265.2" font-family="Arial" font-size="22.00" fill="white">T</text>
</g>
<!-- E1&#45;&gt;T1 -->
<g id="edge1" class="edge">
<title>E1-&gt;T1</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M138.97,-335.67C130.47,-325.1 119.56,-311.53 110.04,-299.7"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="112.92,-297.7 103.92,-292.1 107.46,-302.08 112.92,-297.7"/>
</g>
<!-- plus -->
<g id="node3" class="node">
<title>plus</title>
<ellipse fill="none" stroke="white" cx="153.4" cy="-271.8" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="153.4" y="-265.2" font-family="Arial" font-size="22.00" fill="white">+</text>
</g>
<!-- E1&#45;&gt;plus -->
<g id="edge2" class="edge">
<title>E1-&gt;plus</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M153.4,-331.09C153.4,-323.71 153.4,-315.33 153.4,-307.28"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="156.9,-307.28 153.4,-297.28 149.9,-307.28 156.9,-307.28"/>
</g>
<!-- E2 -->
<g id="node4" class="node">
<title>E2</title>
<ellipse fill="none" stroke="white" cx="218.4" cy="-271.8" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="218.4" y="-265.2" font-family="Arial" font-size="22.00" fill="white">E</text>
</g>
<!-- E1&#45;&gt;E2 -->
<g id="edge3" class="edge">
<title>E1-&gt;E2</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M167.83,-335.67C176.33,-325.1 187.24,-311.53 196.76,-299.7"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="199.34,-302.08 202.88,-292.1 193.88,-297.7 199.34,-302.08"/>
</g>
<!-- T2 -->
<g id="node5" class="node">
<title>T2</title>
<ellipse fill="none" stroke="white" cx="23.4" cy="-189" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="23.4" y="-182.4" font-family="Arial" font-size="22.00" fill="white">T</text>
</g>
<!-- T1&#45;&gt;T2 -->
<g id="edge4" class="edge">
<title>T1-&gt;T2</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M73.97,-252.87C65.47,-242.3 54.56,-228.73 45.04,-216.9"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="47.92,-214.9 38.92,-209.3 42.46,-219.28 47.92,-214.9"/>
</g>
<!-- star -->
<g id="node6" class="node">
<title>star</title>
<ellipse fill="none" stroke="white" cx="88.4" cy="-189" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="88.4" y="-182.4" font-family="Arial" font-size="22.00" fill="white">*</text>
</g>
<!-- T1&#45;&gt;star -->
<g id="edge5" class="edge">
<title>T1-&gt;star</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M88.4,-248.29C88.4,-240.91 88.4,-232.53 88.4,-224.48"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="91.9,-224.48 88.4,-214.48 84.9,-224.48 91.9,-224.48"/>
</g>
<!-- F1 -->
<g id="node7" class="node">
<title>F1</title>
<ellipse fill="none" stroke="white" cx="153.4" cy="-189" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="153.4" y="-182.4" font-family="Arial" font-size="22.00" fill="white">F</text>
</g>
<!-- T1&#45;&gt;F1 -->
<g id="edge6" class="edge">
<title>T1-&gt;F1</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M102.83,-252.87C111.33,-242.3 122.24,-228.73 131.76,-216.9"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="134.34,-219.28 137.88,-209.3 128.88,-214.9 134.34,-219.28"/>
</g>
<!-- T3 -->
<g id="node8" class="node">
<title>T3</title>
<ellipse fill="none" stroke="white" cx="218.4" cy="-189" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="218.4" y="-182.4" font-family="Arial" font-size="22.00" fill="white">T</text>
</g>
<!-- E2&#45;&gt;T3 -->
<g id="edge7" class="edge">
<title>E2-&gt;T3</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M218.4,-248.29C218.4,-240.91 218.4,-232.53 218.4,-224.48"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="221.9,-224.48 218.4,-214.48 214.9,-224.48 221.9,-224.48"/>
</g>
<!-- F2 -->
<g id="node9" class="node">
<title>F2</title>
<ellipse fill="none" stroke="white" cx="23.4" cy="-106.2" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="23.4" y="-99.6" font-family="Arial" font-size="22.00" fill="white">F</text>
</g>
<!-- T2&#45;&gt;F2 -->
<g id="edge8" class="edge">
<title>T2-&gt;F2</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M23.4,-165.49C23.4,-158.11 23.4,-149.73 23.4,-141.68"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="26.9,-141.68 23.4,-131.68 19.9,-141.68 26.9,-141.68"/>
</g>
<!-- id1 -->
<g id="node10" class="node">
<title>id1</title>
<ellipse fill="none" stroke="white" cx="153.4" cy="-106.2" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="153.4" y="-99.6" font-family="Arial" font-size="22.00" fill="white">id</text>
</g>
<!-- F1&#45;&gt;id1 -->
<g id="edge10" class="edge">
<title>F1-&gt;id1</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M153.4,-165.49C153.4,-158.11 153.4,-149.73 153.4,-141.68"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="156.9,-141.68 153.4,-131.68 149.9,-141.68 156.9,-141.68"/>
</g>
<!-- F3 -->
<g id="node11" class="node">
<title>F3</title>
<ellipse fill="none" stroke="white" cx="218.4" cy="-106.2" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="218.4" y="-99.6" font-family="Arial" font-size="22.00" fill="white">F</text>
</g>
<!-- T3&#45;&gt;F3 -->
<g id="edge11" class="edge">
<title>T3-&gt;F3</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M218.4,-165.49C218.4,-158.11 218.4,-149.73 218.4,-141.68"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="221.9,-141.68 218.4,-131.68 214.9,-141.68 221.9,-141.68"/>
</g>
<!-- id2 -->
<g id="node12" class="node">
<title>id2</title>
<ellipse fill="none" stroke="white" cx="23.4" cy="-23.4" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="23.4" y="-16.8" font-family="Arial" font-size="22.00" fill="white">id</text>
</g>
<!-- F2&#45;&gt;id2 -->
<g id="edge9" class="edge">
<title>F2-&gt;id2</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M23.4,-82.69C23.4,-75.31 23.4,-66.93 23.4,-58.88"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="26.9,-58.88 23.4,-48.88 19.9,-58.88 26.9,-58.88"/>
</g>
<!-- id3 -->
<g id="node13" class="node">
<title>id3</title>
<ellipse fill="none" stroke="white" cx="218.4" cy="-23.4" rx="23.4" ry="23.4"/>
<text xml:space="preserve" text-anchor="middle" x="218.4" y="-16.8" font-family="Arial" font-size="22.00" fill="white">id</text>
</g>
<!-- F3&#45;&gt;id3 -->
<g id="edge12" class="edge">
<title>F3-&gt;id3</title>
<path fill="none" stroke="white" stroke-width="1.2" d="M218.4,-82.69C218.4,-75.31 218.4,-66.93 218.4,-58.88"/>
<polygon fill="white" stroke="white" stroke-width="1.2" points="221.9,-58.88 218.4,-48.88 214.9,-58.88 221.9,-58.88"/>
</g>
</g>
</svg>

### 3.4 Problems in Grammar Design

#### 3.4.1 Ambiguity

A grammar is ambiguous if there exists a string with two or more distinct parse trees.

**Ambiguous Grammar:**
```python
E → E + E
E → E * E
E → id
```

For the string `id + id * id`, we can have:

`(id + id) * id`  
`id + (id * id)`

**Solution:** Use precedence and associativity rules.

**Fix (Unambiguous Grammar):**
```
E → E + T | T
T → T * F | F
F → id
```

#### 3.4.2 Left Recursion

A grammar is **left-recursive** if a nonterminal `A` can derive a string that begins with itself, i.e. `A ⇒⁺ Aα`.

**Problem:**
Top-down parsers can fall into **infinite recursion** when handling left-recursive rules.

**Example:**

```
E → E + T | T
```

Here, `E` is left-recursive because the production `E → E + T` starts with `E`.

**Elimination Algorithm:**
For a rule of the form:

```c
A → Aα | β //   βαααα..α | β
```

We rewrite it as:

```
A  → βA'
A' → αA' | ε
```

**After elimination:**

```
E  → TE'
E' → +TE' | ε
```

Now the grammar is **right-recursive**, avoiding infinite loops in top-down parsing.


#### 3.4.3 Left Factorization

When two productions start with the same prefix, parser cannot decide which to use.

**Example:**
```
S → if E then S else S | if E then S
```

**Factorization:**
```
S → if E then S S'
S' → else S | ε
```

### 3.5 First and Follow Sets

#### 3.5.1 First Set

**FIRST(α):** Set of terminals that can appear as the first symbol in strings derived from α.

**Rules:**

1. If X is terminal: `FIRST(X) = {X}`
2. If X → ε add ε to `FIRST(X)`
3. If X → Y₁Y₂...Yₖ:

   * Add `FIRST(Y₁) - {ε}` to `FIRST(X)`
   * If Y₁ ⇒* ε, add `FIRST(Y₂) - {ε}`
   * Continue until a non-nullable symbol or all symbols processed

**Example:**

```
E → TE'
E' → +TE' | ε
T → FT'
T' → *FT' | ε
F → (E) | id
```

**Calculation:**

```
FIRST(F) = {(, id}
FIRST(T) = FIRST(F) = {(, id}
FIRST(E) = FIRST(T) = {(, id}
FIRST(E') = {+, ε}
FIRST(T') = {*, ε}
```

#### 3.5.2 Follow Set

**FOLLOW(A):** Set of terminals that can appear immediately after A.

**Rules:**

1. Place `$` in `FOLLOW(S)` where S is the start symbol.
2. If A → αBβ, add `FIRST(β) - {ε}` to `FOLLOW(B)`.
3. If A → αB or (A → αBβ and ε ∈ FIRST(β)), add `FOLLOW(A)` to `FOLLOW(B)`.

**Example (same grammar):**

```
FOLLOW(E)  = {), $}
FOLLOW(E') = {), $}
FOLLOW(T)  = {+, ), $}
FOLLOW(T') = {+, ), $}
FOLLOW(F)  = {*, +, ), $}
```
### 3.6 Parsing Categories

There are two major categories of parsing techniques:

| Category              | Direction     | Derivation Type                   | Examples                      |
| --------------------- | ------------- | --------------------------------- | ----------------------------- |
| **Top-Down Parsing**  | Left-to-right | Leftmost derivation               | Recursive Descent, LL(1)      |
| **Bottom-Up Parsing** | Left-to-right | Rightmost derivation (in reverse) | LR(0), SLR(1), LALR(1), LR(1) |

<!-- ####  Top‑Down (Descending) Parsing

* Starts from the **start symbol** and tries to **predict** the input
* Builds the parse tree **from root to leaves**
* Follows **leftmost derivation**
* Most intuitive method

####  Bottom‑Up Parsing

* Starts from the **input tokens** and tries to **reduce** them to the start symbol
* Builds the parse tree **from leaves to root**
* Follows **rightmost derivation in reverse**
* Used in real-world compilers (YACC, Bison) -->

### 3.6.2 Top-Down Parsing

Top-down parsers start from the start symbol and try to derive the input string.

#### Example Grammar
We use the following grammar and input:
```
E → E + T | T
T → id

Input: id + id
```
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="206pt" height="260pt" viewBox="0.00 0.00 206.00 260.00">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 256)">
<title>TopDownTree</title>
<polygon fill="white" stroke="none" points="-4,4 -4,-256 202,-256 202,4 -4,4"/>
<!-- E0 -->
<g id="node1" class="node">
<title>E0</title>
<path fill="none" stroke="black" d="M114,-252C114,-252 84,-252 84,-252 78,-252 72,-246 72,-240 72,-240 72,-228 72,-228 72,-222 78,-216 84,-216 84,-216 114,-216 114,-216 120,-216 126,-222 126,-228 126,-228 126,-240 126,-240 126,-246 120,-252 114,-252"/>
<text xml:space="preserve" text-anchor="middle" x="99" y="-229.8" font-family="Times,serif" font-size="14.00">E</text>
</g>
<!-- E1 -->
<g id="node2" class="node">
<title>E1</title>
<path fill="none" stroke="black" d="M42,-180C42,-180 12,-180 12,-180 6,-180 0,-174 0,-168 0,-168 0,-156 0,-156 0,-150 6,-144 12,-144 12,-144 42,-144 42,-144 48,-144 54,-150 54,-156 54,-156 54,-168 54,-168 54,-174 48,-180 42,-180"/>
<text xml:space="preserve" text-anchor="middle" x="27" y="-157.8" font-family="Times,serif" font-size="14.00">E</text>
</g>
<!-- E0&#45;&gt;E1 -->
<g id="edge1" class="edge">
<title>E0-&gt;E1</title>
<path fill="none" stroke="black" d="M81.2,-215.7C72.72,-207.45 62.41,-197.43 53.07,-188.35"/>
<polygon fill="black" stroke="black" points="55.54,-185.86 45.93,-181.4 50.66,-190.88 55.54,-185.86"/>
</g>
<!-- plus -->
<g id="node3" class="node">
<title>plus</title>
<path fill="none" stroke="black" d="M114,-180C114,-180 84,-180 84,-180 78,-180 72,-174 72,-168 72,-168 72,-156 72,-156 72,-150 78,-144 84,-144 84,-144 114,-144 114,-144 120,-144 126,-150 126,-156 126,-156 126,-168 126,-168 126,-174 120,-180 114,-180"/>
<text xml:space="preserve" text-anchor="middle" x="99" y="-157.8" font-family="Times,serif" font-size="14.00">+</text>
</g>
<!-- E0&#45;&gt;plus -->
<g id="edge2" class="edge">
<title>E0-&gt;plus</title>
<path fill="none" stroke="black" d="M99,-215.7C99,-208.41 99,-199.73 99,-191.54"/>
<polygon fill="black" stroke="black" points="102.5,-191.62 99,-181.62 95.5,-191.62 102.5,-191.62"/>
</g>
<!-- T2 -->
<g id="node4" class="node">
<title>T2</title>
<path fill="none" stroke="black" d="M186,-180C186,-180 156,-180 156,-180 150,-180 144,-174 144,-168 144,-168 144,-156 144,-156 144,-150 150,-144 156,-144 156,-144 186,-144 186,-144 192,-144 198,-150 198,-156 198,-156 198,-168 198,-168 198,-174 192,-180 186,-180"/>
<text xml:space="preserve" text-anchor="middle" x="171" y="-157.8" font-family="Times,serif" font-size="14.00">T</text>
</g>
<!-- E0&#45;&gt;T2 -->
<g id="edge3" class="edge">
<title>E0-&gt;T2</title>
<path fill="none" stroke="black" d="M116.8,-215.7C125.28,-207.45 135.59,-197.43 144.93,-188.35"/>
<polygon fill="black" stroke="black" points="147.34,-190.88 152.07,-181.4 142.46,-185.86 147.34,-190.88"/>
</g>
<!-- T3 -->
<g id="node5" class="node">
<title>T3</title>
<path fill="none" stroke="black" d="M42,-108C42,-108 12,-108 12,-108 6,-108 0,-102 0,-96 0,-96 0,-84 0,-84 0,-78 6,-72 12,-72 12,-72 42,-72 42,-72 48,-72 54,-78 54,-84 54,-84 54,-96 54,-96 54,-102 48,-108 42,-108"/>
<text xml:space="preserve" text-anchor="middle" x="27" y="-85.8" font-family="Times,serif" font-size="14.00">T</text>
</g>
<!-- E1&#45;&gt;T3 -->
<g id="edge4" class="edge">
<title>E1-&gt;T3</title>
<path fill="none" stroke="black" d="M27,-143.7C27,-136.41 27,-127.73 27,-119.54"/>
<polygon fill="black" stroke="black" points="30.5,-119.62 27,-109.62 23.5,-119.62 30.5,-119.62"/>
</g>
<!-- id2 -->
<g id="node7" class="node">
<title>id2</title>
<path fill="none" stroke="black" d="M186,-108C186,-108 156,-108 156,-108 150,-108 144,-102 144,-96 144,-96 144,-84 144,-84 144,-78 150,-72 156,-72 156,-72 186,-72 186,-72 192,-72 198,-78 198,-84 198,-84 198,-96 198,-96 198,-102 192,-108 186,-108"/>
<text xml:space="preserve" text-anchor="middle" x="171" y="-85.8" font-family="Times,serif" font-size="14.00">id</text>
</g>
<!-- T2&#45;&gt;id2 -->
<g id="edge6" class="edge">
<title>T2-&gt;id2</title>
<path fill="none" stroke="black" d="M171,-143.7C171,-136.41 171,-127.73 171,-119.54"/>
<polygon fill="black" stroke="black" points="174.5,-119.62 171,-109.62 167.5,-119.62 174.5,-119.62"/>
</g>
<!-- id1 -->
<g id="node6" class="node">
<title>id1</title>
<path fill="none" stroke="black" d="M42,-36C42,-36 12,-36 12,-36 6,-36 0,-30 0,-24 0,-24 0,-12 0,-12 0,-6 6,0 12,0 12,0 42,0 42,0 48,0 54,-6 54,-12 54,-12 54,-24 54,-24 54,-30 48,-36 42,-36"/>
<text xml:space="preserve" text-anchor="middle" x="27" y="-13.8" font-family="Times,serif" font-size="14.00">id</text>
</g>
<!-- T3&#45;&gt;id1 -->
<g id="edge5" class="edge">
<title>T3-&gt;id1</title>
<path fill="none" stroke="black" d="M27,-71.7C27,-64.41 27,-55.73 27,-47.54"/>
<polygon fill="black" stroke="black" points="30.5,-47.62 27,-37.62 23.5,-47.62 30.5,-47.62"/>
</g>
</g>
</svg>

#### Recursive Descent Parsing

**Idea:** For each non-terminal, write a C function that parses it.

Simple, intuitive, close to hand-parsing.

**Grammar:**

```
E → TE'
E' → +TE' | ε
T → FT'
F → (E) | id
T' → *FT' | ε
```

**Pseudo-code in C style:**

```C
void E() {
    T();         // parse T
    E_prime();   // parse E'
}

void E_prime() {
    if (lookahead == '+') {
        match('+');
        T();
        E_prime();
    }
    // else epsilon
}

void T() {
    F();
    T_prime();
}
```

**Note:** Works only for grammars without left recursion.

#### LL(1) Parsing

**LL(1)** = **L**eft-to-right scan, **L**eftmost derivation, **1** lookahead token.

LL(1) parsers use a parsing **table** and a **stack**. The parsing table has rows for non-terminals and columns for terminals, which will guid the parser which production to apply.

**To fill an LL(1) table:**

1. For each production `A → α`, look at the terminals in `FIRST(α)`. For each terminal `t` in `FIRST(α)`, put `A → α` in the table cell at row `A` and column `t`.
2. If `ε` is in `FIRST(α)`, then for each terminal `t` in `FOLLOW(A)`, put `A → ε` in the table cell at row `A` and column `t`.
#### LL(1) Parsing

**LL(1)** = **L**eft-to-right scan, **L**eftmost derivation, **1** lookahead token.

LL(1) parsers use a **parsing table** and a **stack**. The parsing table has **rows for non-terminals** and **columns for terminals**, guiding the parser which production to apply.

**To fill an LL(1) table:**

1. For each production `A → α`, look at the terminals in `FIRST(α)`. For each terminal `t` in `FIRST(α)` (except ε), put `A → α` in the table cell at row `A` and column `t`.
2. If `ε ∈ FIRST(α)`, then for each terminal `t` in `FOLLOW(A)`, put `A → ε` in the table cell at row `A` and column `t`.

**Grammar (short, but with some FIRST & FOLLOW):**

```
S → A B
A → 0 | ε
B → 1
```

**Explanation:**

* `S` is the start symbol
* `A` can be `0` or empty (ε)
* `B` is `1`
* Accepts strings: `01` and `1`

### FIRST & FOLLOW sets

| NT | FIRST    | FOLLOW |
| -- | -------- | ------ |
| S  | { 0, 1 } | { $ }  |
| A  | { 0, ε } | { 1 }  |
| B  | { 1 }    | { $ }  |

### LL(1) Table

| NT | 0     | 1     | $ |
| -- | ----- | ----- | - |
| S  | S→A B | S→A B |   |
| A  | A→0   | A→ε   |   |
| B  |       | B→1   |   |

**Condition:** Each entry must contain **at most one production**.

#### LL(1) Parsing Algorithm

1. Initialize stack = `S $`

2. Initialize input = `string $`

3. Repeat:

   * Let `X` be the top of the stack
   * Let `a` be the current input token

   **Case 1:** `X` is a terminal

   * If `X == a` → pop stack, advance input
   * Else → **error** (mismatch)

   **Case 2:** `X` is a non‑terminal

   * Look up parsing table entry `M[X, a]`
   * If empty → **error** (no rule)
   * Else → pop `X` and **push RHS of rule** (in reverse order)

   **Case 3:** `X = $` and `a = $`

   * **Accept** (string belongs to language)

4. If input ends but stack still not matched → **reject**

##### Example: Parse `01`

Input: `01 $`
Stack: `S $`

#### LL(1) Parsing Trace for `01`

| Step | Stack   | Input  | Action     |
| ---- | ------- | ------ | ---------- |
| 1    | `S $`   | `01 $` | `S → A B`  |
| 2    | `A B $` | `01 $` | `A → 0`    |
| 3    | `0 B $` | `01 $` | match `0`  |
| 4    | `B $`   | `1 $`  | `B → 1`    |
| 5    | `1 $`   | `1 $`  | match `1`  |
| 6    | `$`     | `$`    | **ACCEPT** |



### 3.6.3 Bottom-Up Parsing

Bottom-up parsers work in the **opposite direction** of top-down parsers. They start from the input tokens and **build upward** to reach the start symbol.

#### Example Grammar
We will use this grammar throughout our examples:
```
E → E + T | T
T → id

Input: id + id
```

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="206pt" height="260pt" viewBox="0.00 0.00 206.00 260.00">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 256)">
<title>BottomUpTree</title>
<polygon fill="white" stroke="none" points="-4,4 -4,-256 202,-256 202,4 -4,4"/>
<!-- id1 -->
<g id="node1" class="node">
<title>id1</title>
<path fill="none" stroke="black" d="M42,-36C42,-36 12,-36 12,-36 6,-36 0,-30 0,-24 0,-24 0,-12 0,-12 0,-6 6,0 12,0 12,0 42,0 42,0 48,0 54,-6 54,-12 54,-12 54,-24 54,-24 54,-30 48,-36 42,-36"/>
<text xml:space="preserve" text-anchor="middle" x="27" y="-13.8" font-family="Times,serif" font-size="14.00">id</text>
</g>
<!-- T1 -->
<g id="node4" class="node">
<title>T1</title>
<path fill="none" stroke="black" d="M42,-108C42,-108 12,-108 12,-108 6,-108 0,-102 0,-96 0,-96 0,-84 0,-84 0,-78 6,-72 12,-72 12,-72 42,-72 42,-72 48,-72 54,-78 54,-84 54,-84 54,-96 54,-96 54,-102 48,-108 42,-108"/>
<text xml:space="preserve" text-anchor="middle" x="27" y="-85.8" font-family="Times,serif" font-size="14.00">T</text>
</g>
<!-- id1&#45;&gt;T1 -->
<g id="edge1" class="edge">
<title>id1-&gt;T1</title>
<path fill="none" stroke="black" d="M27,-36.3C27,-43.59 27,-52.27 27,-60.46"/>
<polygon fill="black" stroke="black" points="23.5,-60.38 27,-70.38 30.5,-60.38 23.5,-60.38"/>
</g>
<!-- id2 -->
<g id="node2" class="node">
<title>id2</title>
<path fill="none" stroke="black" d="M186,-108C186,-108 156,-108 156,-108 150,-108 144,-102 144,-96 144,-96 144,-84 144,-84 144,-78 150,-72 156,-72 156,-72 186,-72 186,-72 192,-72 198,-78 198,-84 198,-84 198,-96 198,-96 198,-102 192,-108 186,-108"/>
<text xml:space="preserve" text-anchor="middle" x="171" y="-85.8" font-family="Times,serif" font-size="14.00">id</text>
</g>
<!-- T2 -->
<g id="node5" class="node">
<title>T2</title>
<path fill="none" stroke="black" d="M186,-180C186,-180 156,-180 156,-180 150,-180 144,-174 144,-168 144,-168 144,-156 144,-156 144,-150 150,-144 156,-144 156,-144 186,-144 186,-144 192,-144 198,-150 198,-156 198,-156 198,-168 198,-168 198,-174 192,-180 186,-180"/>
<text xml:space="preserve" text-anchor="middle" x="171" y="-157.8" font-family="Times,serif" font-size="14.00">T</text>
</g>
<!-- id2&#45;&gt;T2 -->
<g id="edge2" class="edge">
<title>id2-&gt;T2</title>
<path fill="none" stroke="black" d="M171,-108.3C171,-115.59 171,-124.27 171,-132.46"/>
<polygon fill="black" stroke="black" points="167.5,-132.38 171,-142.38 174.5,-132.38 167.5,-132.38"/>
</g>
<!-- plus -->
<g id="node3" class="node">
<title>plus</title>
<path fill="none" stroke="black" d="M114,-180C114,-180 84,-180 84,-180 78,-180 72,-174 72,-168 72,-168 72,-156 72,-156 72,-150 78,-144 84,-144 84,-144 114,-144 114,-144 120,-144 126,-150 126,-156 126,-156 126,-168 126,-168 126,-174 120,-180 114,-180"/>
<text xml:space="preserve" text-anchor="middle" x="99" y="-157.8" font-family="Times,serif" font-size="14.00">+</text>
</g>
<!-- E2 -->
<g id="node7" class="node">
<title>E2</title>
<path fill="none" stroke="black" d="M114,-252C114,-252 84,-252 84,-252 78,-252 72,-246 72,-240 72,-240 72,-228 72,-228 72,-222 78,-216 84,-216 84,-216 114,-216 114,-216 120,-216 126,-222 126,-228 126,-228 126,-240 126,-240 126,-246 120,-252 114,-252"/>
<text xml:space="preserve" text-anchor="middle" x="99" y="-229.8" font-family="Times,serif" font-size="14.00">E</text>
</g>
<!-- plus&#45;&gt;E2 -->
<g id="edge5" class="edge">
<title>plus-&gt;E2</title>
<path fill="none" stroke="black" d="M99,-180.3C99,-187.59 99,-196.27 99,-204.46"/>
<polygon fill="black" stroke="black" points="95.5,-204.38 99,-214.38 102.5,-204.38 95.5,-204.38"/>
</g>
<!-- E1 -->
<g id="node6" class="node">
<title>E1</title>
<path fill="none" stroke="black" d="M42,-180C42,-180 12,-180 12,-180 6,-180 0,-174 0,-168 0,-168 0,-156 0,-156 0,-150 6,-144 12,-144 12,-144 42,-144 42,-144 48,-144 54,-150 54,-156 54,-156 54,-168 54,-168 54,-174 48,-180 42,-180"/>
<text xml:space="preserve" text-anchor="middle" x="27" y="-157.8" font-family="Times,serif" font-size="14.00">E</text>
</g>
<!-- T1&#45;&gt;E1 -->
<g id="edge3" class="edge">
<title>T1-&gt;E1</title>
<path fill="none" stroke="black" d="M27,-108.3C27,-115.59 27,-124.27 27,-132.46"/>
<polygon fill="black" stroke="black" points="23.5,-132.38 27,-142.38 30.5,-132.38 23.5,-132.38"/>
</g>
<!-- T2&#45;&gt;E2 -->
<g id="edge6" class="edge">
<title>T2-&gt;E2</title>
<path fill="none" stroke="black" d="M153.2,-180.3C144.72,-188.55 134.41,-198.57 125.07,-207.65"/>
<polygon fill="black" stroke="black" points="122.66,-205.12 117.93,-214.6 127.54,-210.14 122.66,-205.12"/>
</g>
<!-- E1&#45;&gt;E2 -->
<g id="edge4" class="edge">
<title>E1-&gt;E2</title>
<path fill="none" stroke="black" d="M44.8,-180.3C53.28,-188.55 63.59,-198.57 72.93,-207.65"/>
<polygon fill="black" stroke="black" points="70.46,-210.14 80.07,-214.6 75.34,-205.12 70.46,-210.14"/>
</g>
</g>
</svg>


#### 3.6.3.1 What is LR Parsing?

**LR** stands for:
- **L** = Read input from **L**eft to right
- **R** = Build **R**ightmost derivation (but in reverse)

LR parsing is the main method for **bottom-up** parsing. It reads your code from left to right and builds the parse tree starting from the leaves (tokens) going up to the root (start symbol).

**Why do we use LR parsing?**
- It is **more powerful** than LL parsing
- It can handle **left-recursive grammars**, while LL cannot
- It is used in **real compilers** like GCC, Java compiler, and Python compiler


##### The Four Basic Operations

Every LR parser uses these **4 operations**:

1. **SHIFT** → Take one token from input and push it onto the stack
2. **REDUCE** → Replace symbols on top of stack with a non-terminal (using a grammar rule)
3. **ACCEPT** → Success! The input is correct — input string is valid and fully parsed
4. **ERROR** → The input has a syntax error — no valid action exists.

##### Simple Example with Steps

**Grammar:**
```
E → E + T
E → T
T → id
```

**Input:** `id + id`

Let's parse this step by step:

| Step | Stack | Input | Action | Explanation |
|------|-------|-------|--------|-------------|
| 1 | $ | id + id $ | SHIFT | Move "id" to stack |
| 2 | $ id | + id $ | REDUCE T→id | Replace "id" with T using rule T→id |
| 3 | $ T | + id $ | REDUCE E→T | Replace "T" with E using rule E→T |
| 4 | $ E | + id $ | SHIFT | Move "+" to stack |
| 5 | $ E + | id $ | SHIFT | Move "id" to stack |
| 6 | $ E + id | $ | REDUCE T→id | Replace "id" with T |
| 7 | $ E + T | $ | REDUCE E→E+T | Replace "E + T" with E |
| 8 | $ E | $ | ACCEPT | Done! Input is correct  |

**Important:** The symbol `$` marks the end of input. It helps the parser know when to stop.

#### 3.6.3.2 How Does an LR Parser Work?

An LR parser uses **two important tables** to make decisions:

##### 1. ACTION Table
This table tells the parser **what to do** at each step.
- **Input:** (current state, current input token)
- **Output:** SHIFT, REDUCE, ACCEPT, or ERROR

##### 2. GOTO Table
This table tells the parser **which state to go to** after a REDUCE.
- **Input:** (current state, non-terminal symbol)
- **Output:** next state number


##### LR Parsing Algorithm (Step by Step)

Here is how the parser works:

```
Algorithm: LR Parsing

Step 1: Initialize
   - Stack starts with: [0]        (state 0 is the starting state)
   - Input is: your tokens + $     ($ marks the end)

Step 2: Loop (repeat until ACCEPT or ERROR)
   
   a) Let s = top state on stack
   b) Let a = current input token
   
   c) Look up ACTION[s, a] in the ACTION table
   
   d) If ACTION says "SHIFT to state t":
       - Push token a onto stack
       - Push state t onto stack
       - Move to next input token
   
   e) If ACTION says "REDUCE using rule A → β":
       - Remove 2 × |β| items from stack  
         (we remove both symbols and states)
       - Let s' = new top state on stack
       - Push non-terminal A onto stack
       - Push GOTO[s', A] onto stack
   
   f) If ACTION says "ACCEPT":
       - Print "Input is valid and correct!"
       - Stop successfully
   
   g) If ACTION says "ERROR":
       - Print "Syntax Error"
       - Stop with error

Step 3: End
```

**Note:** When we REDUCE using rule `A → β`, we pop `2 × |β|` items because each grammar symbol on the stack has a state with it.

##### Three Types of LR Parsers

There are **three main types** of LR parsers. They differ in power and table size:

| Type | Power | Table Size | When to Use |
|------|-------|------------|-------------|
| **SLR(1)** | Basic | Small | For learning, simple languages |
| **LALR(1)** | Medium | Medium | For real compilers  |
| **LR(1)** | Highest | Very Large | For theory and research |

Let's study each one in detail.

## 1. SLR(1) Parser (Simple LR)

**Main Idea:** SLR(1) uses **FOLLOW sets** to decide when to reduce.

**SLR** stands for **Simple LR**. It is the easiest LR parser to understand.

### Complete Example

Let's work through a full example from start to finish.

**Grammar (Augmented):**

```
0. S' → S
1. S  → C C
2. C  → c C
3. C  → d
```

This grammar generates strings like: `c d`, `c c d`, `d d`, `c c c d d`, etc.


#### FIRST and FOLLOW Sets

```
FIRST(S) = {c, d}
FIRST(C) = {c, d}

FOLLOW(S) = {$}
FOLLOW(C) = {c, d, $}
```

#### Build LR(0) Items (States)
An **item** is a production rule with a dot (•) showing how much we have seen so far.

**Example:** `C → c•C` means "we have seen c, and we expect to see C next"


##### I₀ (Start State)

```
S' → •S
S  → •C C
C → •c C
C → •d
```

##### I₁ = GOTO(I₀, C)

```
S  → C•C
C → •c C
C → •d
```

##### I₂ = GOTO(I₀, c)

```
C → c•C
C → •c C
C → •d
```

##### I₃ = GOTO(I₀, d)

```
C → d•
```

##### I₄ = GOTO(I₁, C)

```
C → c C•
```

##### I₅ = GOTO(I₃, C)

```
S  → C C•
```

##### I₆ = GOTO(I₀, S)

```
S' → S•
```


#### Build Complete SLR(1) Table
Now we fill the ACTION and GOTO tables using these rules:

**For ACTION[i, a]:**
- If state i has item `X → α•aβ`, add "SHIFT j" where j is the state after reading a
- If state i has item `A → α•` (dot at end), add "REDUCE A→α" for all tokens in FOLLOW(A)
- If state i has item `S → S'•` (start symbol complete), add "ACCEPT" for $

**For GOTO[i, A]:**
- If we can go from state i to state j by reading non-terminal A, put j in GOTO[i, A]

| State | c  | d  | $   | S | C |
| ----- | -- | -- | --- | - | - |
| 0     | s2 | s3 | -   | 6 | 1 |
| 1     | s2 | s3 | -   | - | 4 |
| 2     | s2 | s3 | -   | - | 4 |
| 3     | r3 | r3 | r3  | - | - |
| 4     | r2 | r2 | r2  | - | - |
| 5     | -  | -  | r1  | - | - |
| 6     | -  | -  | acc | - | - |

**Legend:**

* **sN** = SHIFT and go to state N
* **rN** = REDUCE using rule N
* **acc** = ACCEPT
* **-** = ERROR


#### Parse the String `c d d $`

| Step | Stack     | Input   | Action  | Explanation                        |
| ---- | --------- | ------- | ------- | ---------------------------------- |
| 1    | 0         | c d d $ | **s2**  | Shift c and go to state 2          |
| 2    | 0 c 2     | d d $   | **s3**  | Shift d and go to state 3          |
| 3    | 0 c 2 d 3 | d $     | **r3**  | Reduce using `C → d`               |
| 4    | 0 c 2 C 4 | d $     | **r2**  | Reduce using `C → c C`             |
| 5    | 0 C 1     | d $     | **s3**  | Shift d and go to state 3          |
| 6    | 0 C 1 d 3 | $       | **r3**  | Reduce using `C → d`               |
| 7    | 0 C 1 C 4 | $       | **r1**  | Reduce using `S → C C`; goto S → 6 |
| 8    | 0 S 6     | $       | **acc** | Accept                             |

**Result:** The string `c d d` is **ACCEPTED**.


### Problems with SLR(1)

SLR(1) is simple, but sometimes it creates **conflicts** (situations where the parser doesn't know what to do).

**Example of a problematic grammar:**
```
S → a A d
S → b B d
A → c
B → c
```

Here, both A and B can be followed by 'd' (it's in both FOLLOW sets). So when we see "c" followed by "d", the parser doesn't know whether to reduce using A→c or B→c.

This is called a **reduce-reduce conflict**.

**Solution:** Use more powerful parsers like **LALR(1)** or **LR(1)**.


## 2. LR(1) Parser (Canonical LR)

**Main Idea:** LR(1) tracks the **exact lookahead** for every item, not just FOLLOW sets.

**LR(1)** is more powerful than SLR(1) because it is more precise.

### LR(1) Item Format

In LR(1), each item has **two parts**:

```
[A → α•β, a]
```

**Meaning:**

* `A → α•β` is the production with the dot showing how much we have recognized.
* `a` is the **lookahead token** (what we expect to follow A when we reduce).

**Example:** `[C → c•C, $]`
Means: "We are parsing `C → cC`, and after reducing to C, the next token should be `$`."

---

### Grammar Used

```
1. S → C C
2. C → c C
3. C → d
```


### LR(1) States (Canonical)

##### I₀ = Start State

```
[S → •C C, $]
[C → •c C, c|d]
[C → •d, c|d]
```

##### I₁ = GOTO(I₀, c)

```
[C → c•C, c|d]
[C → •c C, c|d]
[C → •d, c|d]
```

##### I₂ = GOTO(I₀, d)

```
[C → d•, c|d]
```

##### I₃ = GOTO(I₀, C)

```
[S → C•C, $]
[C → •c C, $]
[C → •d, $]
```

##### I₄ = GOTO(I₃, c)

```
[C → c•C, $]
[C → •c C, $]
[C → •d, $]
```

##### I₅ = GOTO(I₃, d)

```
[C → d•, $]
```

##### I₆ = GOTO(I₁, C)

```
[C → c C•, c|d]
```

##### I₇ = GOTO(I₃, C)

```
[S → C C•, $]
```

##### I₈ = GOTO(I₄, C)

```
[C → c C•, $]
```

### Key Observation

* States **I2** and **I5** both contain `C → d•`, **but with different lookaheads**.
* I2 reduces when next token ∈ {c, d}
* I5 reduces only when next token = $

This precision avoids shift/reduce conflicts.


### LR(1) Parsing Table

| State | c  | d  | $   | C (GOTO) |
| ----- | -- | -- | --- | -------- |
| 0     | s1 | s2 | -   | 3        |
| 1     | s1 | s2 | -   | 6        |
| 2     | r3 | r3 | -   | -        |
| 3     | s4 | s5 | -   | 7        |
| 4     | s4 | s5 | -   | 8        |
| 5     | -  | -  | r3  | -        |
| 6     | r2 | r2 | -   | -        |
| 7     | -  | -  | acc | -        |
| 8     | -  | -  | r2  | -        |

---

**Advantages of LR(1):**
- More states, but **no conflicts**
- More accurate than SLR(1)
- Can parse more grammars

**Disadvantage:**
- Tables can become **very large** for complex grammars
- Takes more memory

---

## 3. LALR(1) Parser (Lookahead LR)

**Main Idea:** LALR(1) **merges** LR(1) states that have the same core items.

**LALR** stands for **Lookahead LR**. It is a compromise between SLR(1) and LR(1).

### How LALR(1) Works

1. First, build the complete LR(1) automaton (all states)
2. Find states that have the **same core** (same items, ignoring lookaheads)
3. **Merge** those states together
4. **Combine** their lookahead sets

### Example

In our previous LR(1) example:
- State I1 has core items: `{C → c•C, C → •c C, C → •d}` with lookaheads {c, d}
- State I4 has the **same core items** but with lookahead {$}
- **LALR(1) merges them** into one state with combined lookaheads: {c, d, $}

---

**Result:**
- **Fewer states** than LR(1) (smaller tables)
- **More power** than SLR(1) (fewer conflicts)
- **Perfect balance** for real compilers

---

**LALR(1) Table for our grammar:**

For this particular grammar, the LALR(1) table looks similar to SLR(1):

| State | c | d | $ | C (GOTO) |
|-------|---|---|---|----------|
| 0 | s1 | s2 | - | 3 |
| 1 | s1 | s2 | - | 4 |
| 2 | r3 | r3 | r3 | - |
| 3 | s1 | s2 | - | 5 |
| 4 | r2 | r2 | r2 | - |
| 5 | - | - | acc | - |

**Used in:** YACC (Yet Another Compiler Compiler) and Bison (GNU parser generator) - these are real tools used to build compilers!

---

## Summary and Comparison

### Quick Comparison Table

```
Characteristic    SLR(1)        LALR(1)       LR(1)
─────────────────────────────────────────────────────
Parsing Power     Basic         Good          Best
Table Size        Small         Medium        Large
Conflicts         More          Fewer         Fewest
Real-World Use    Learning      Production    Theory
Examples          Study         YACC, Bison   Research
```

---

### When to Use Each Parser

**Use SLR(1) when:**
- You are learning about compilers
- Your grammar is simple
- You are working on a small project
- Table size is very important

**Use LALR(1) when:**
- You are building a real compiler
- You want to use tools like YACC or Bison
- You need the best balance of power and efficiency
- This is the **most common choice** in practice

**Use LR(1) when:**
- Your grammar has complex conflicts that LALR(1) cannot handle
- You are doing research or theoretical work
- Table size is not a concern
- You need maximum parsing power

---

### Key Differences in Detail

| Feature | SLR(1) | LALR(1) | LR(1) |
|---------|--------|---------|-------|
| Uses FOLLOW sets | Yes | No | No |
| Tracks exact lookahead | No | Yes | Yes |
| Number of states | Fewest | Medium | Most |
| Merge states | No | Yes | No |
| Reduce-reduce conflicts | More likely | Less likely | Least likely |
| Memory usage | Low | Medium | High |

---

## Important Points to Remember

1. **LR parsing is bottom-up** — it builds the parse tree from leaves (tokens) up to the root (start symbol)

2. **Four operations** — Every LR parser uses: SHIFT, REDUCE, ACCEPT, ERROR

3. **SLR(1)** uses FOLLOW sets to decide when to reduce. Simple but may have conflicts.

4. **LR(1)** tracks exact lookahead for each item. Powerful but creates large tables.

5. **LALR(1)** merges LR(1) states with same core. Best choice for real compilers.

6. **Real compilers use LALR(1)** — Tools like YACC and Bison generate LALR(1) parsers.

7. **Stack is crucial** — The parser stack holds both states and symbols.

8. **Tables guide decisions** — ACTION table says what to do, GOTO table says where to go.

---

## Practice Exercise

Try this yourself to test your understanding!

**Given Grammar:**
```
E → E + T | T
T → id
```

**Task:** Parse the input `id + id` using shift-reduce operations. Fill in the table.

**Answer:**

| Step | Stack | Input | Action | Rule Used |
|------|-------|-------|--------|-----------|
| 1 | $ | id + id $ | SHIFT id | - |
| 2 | $ id | + id $ | REDUCE | T → id |
| 3 | $ T | + id $ | REDUCE | E → T |
| 4 | $ E | + id $ | SHIFT + | - |
| 5 | $ E + | id $ | SHIFT id | - |
| 6 | $ E + id | $ | REDUCE | T → id |
| 7 | $ E + T | $ | REDUCE | E → E + T |
| 8 | $ E | $ | ACCEPT | Success!  |

**Congratulations!** You have successfully parsed `id + id` and built the parse tree from bottom to top.

---
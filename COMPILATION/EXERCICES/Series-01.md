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

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="232pt" height="94pt" viewBox="0.00 0.00 232.00 94.00">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 90.36)">
<title>M1</title>
<polygon fill="white" stroke="none" points="-4,4 -4,-90.36 227.88,-90.36 227.88,4 -4,4"/>
<!-- q0 -->
<g id="node1" class="node">
<title>q0</title>
<ellipse fill="none" stroke="black" cx="21.78" cy="-25.78" rx="21.78" ry="21.78"/>
<text xml:space="preserve" text-anchor="middle" x="21.78" y="-21.58" font-family="Helvetica,sans-Serif" font-size="14.00">q0</text>
</g>
<!-- q0&#45;&gt;q0 -->
<g id="edge1" class="edge">
<title>q0-&gt;q0</title>
<path fill="none" stroke="black" d="M13.9,-46.24C12.81,-56.45 15.44,-65.56 21.78,-65.56 25.54,-65.56 28,-62.35 29.14,-57.6"/>
<polygon fill="black" stroke="black" points="32.63,-57.9 29.58,-47.76 25.64,-57.59 32.63,-57.9"/>
<text xml:space="preserve" text-anchor="middle" x="21.78" y="-69.76" font-family="Times,serif" font-size="14.00">a</text>
</g>
<!-- q1 -->
<g id="node2" class="node">
<title>q1</title>
<ellipse fill="none" stroke="black" cx="108.33" cy="-25.78" rx="21.78" ry="21.78"/>
<text xml:space="preserve" text-anchor="middle" x="108.33" y="-21.58" font-family="Helvetica,sans-Serif" font-size="14.00">q1</text>
</g>
<!-- q0&#45;&gt;q1 -->
<g id="edge2" class="edge">
<title>q0-&gt;q1</title>
<path fill="none" stroke="black" d="M43.68,-25.78C53.06,-25.78 64.35,-25.78 74.79,-25.78"/>
<polygon fill="black" stroke="black" points="74.57,-29.28 84.57,-25.78 74.57,-22.28 74.57,-29.28"/>
<text xml:space="preserve" text-anchor="middle" x="65.06" y="-29.98" font-family="Times,serif" font-size="14.00">b</text>
</g>
<!-- q2 -->
<g id="node3" class="node">
<title>q2</title>
<ellipse fill="none" stroke="black" cx="198.1" cy="-25.78" rx="21.78" ry="21.78"/>
<ellipse fill="none" stroke="black" cx="198.1" cy="-25.78" rx="25.78" ry="25.78"/>
<text xml:space="preserve" text-anchor="middle" x="198.1" y="-21.58" font-family="Helvetica,sans-Serif" font-size="14.00">q2</text>
</g>
<!-- q1&#45;&gt;q2 -->
<g id="edge3" class="edge">
<title>q1-&gt;q2</title>
<path fill="none" stroke="black" d="M130.58,-25.78C139.65,-25.78 150.49,-25.78 160.77,-25.78"/>
<polygon fill="black" stroke="black" points="160.49,-29.28 170.49,-25.78 160.49,-22.28 160.49,-29.28"/>
<text xml:space="preserve" text-anchor="middle" x="151.22" y="-29.98" font-family="Times,serif" font-size="14.00">a</text>
</g>
<!-- q2&#45;&gt;q2 -->
<g id="edge4" class="edge">
<title>q2-&gt;q2</title>
<path fill="none" stroke="black" d="M189.39,-50.53C188.79,-60.83 191.69,-69.56 198.1,-69.56 201.91,-69.56 204.48,-66.48 205.81,-61.82"/>
<polygon fill="black" stroke="black" points="209.28,-62.31 206.68,-52.04 202.31,-61.69 209.28,-62.31"/>
<text xml:space="preserve" text-anchor="middle" x="198.1" y="-73.76" font-family="Times,serif" font-size="14.00">a,b</text>
</g>
</g>
</svg>

**Language:** M1 accepts all strings over ${a,b}$ that contain the substring `ba`.
Equivalent regular expression: $(a|b)^* b a (a|b)^*$.

---

## Exercise 05 — DFA M2

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="146pt" height="94pt" viewBox="0.00 0.00 146.00 94.00">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 90.36)">
<title>M2</title>
<polygon fill="white" stroke="none" points="-4,4 -4,-90.36 142.11,-90.36 142.11,4 -4,4"/>
<!-- q0 -->
<g id="node1" class="node">
<title>q0</title>
<ellipse fill="none" stroke="black" cx="21.78" cy="-25.78" rx="21.78" ry="21.78"/>
<text xml:space="preserve" text-anchor="middle" x="21.78" y="-21.58" font-family="Helvetica,sans-Serif" font-size="14.00">q0</text>
</g>
<!-- q0&#45;&gt;q0 -->
<g id="edge2" class="edge">
<title>q0-&gt;q0</title>
<path fill="none" stroke="black" d="M13.9,-46.24C12.81,-56.45 15.44,-65.56 21.78,-65.56 25.54,-65.56 28,-62.35 29.14,-57.6"/>
<polygon fill="black" stroke="black" points="32.63,-57.9 29.58,-47.76 25.64,-57.59 32.63,-57.9"/>
<text xml:space="preserve" text-anchor="middle" x="21.78" y="-69.76" font-family="Times,serif" font-size="14.00">0</text>
</g>
<!-- q1 -->
<g id="node2" class="node">
<title>q1</title>
<ellipse fill="none" stroke="black" cx="112.33" cy="-25.78" rx="21.78" ry="21.78"/>
<ellipse fill="none" stroke="black" cx="112.33" cy="-25.78" rx="25.78" ry="25.78"/>
<text xml:space="preserve" text-anchor="middle" x="112.33" y="-21.58" font-family="Helvetica,sans-Serif" font-size="14.00">q1</text>
</g>
<!-- q0&#45;&gt;q1 -->
<g id="edge1" class="edge">
<title>q0-&gt;q1</title>
<path fill="none" stroke="black" d="M43.76,-25.78C52.98,-25.78 64.08,-25.78 74.59,-25.78"/>
<polygon fill="black" stroke="black" points="74.57,-29.28 84.57,-25.78 74.57,-22.28 74.57,-29.28"/>
<text xml:space="preserve" text-anchor="middle" x="65.06" y="-29.98" font-family="Times,serif" font-size="14.00">1</text>
</g>
<!-- q1&#45;&gt;q0 -->
<g id="edge4" class="edge">
<title>q1-&gt;q0</title>
<path fill="none" stroke="black" d="M90.98,-10.75C82.14,-5.95 71.55,-2.47 61.56,-4.98 57.93,-5.89 54.26,-7.22 50.72,-8.76"/>
<polygon fill="black" stroke="black" points="49.27,-5.57 41.91,-13.19 52.42,-11.82 49.27,-5.57"/>
<text xml:space="preserve" text-anchor="middle" x="65.06" y="-9.18" font-family="Times,serif" font-size="14.00">0</text>
</g>
<!-- q1&#45;&gt;q1 -->
<g id="edge3" class="edge">
<title>q1-&gt;q1</title>
<path fill="none" stroke="black" d="M103.55,-50.53C102.94,-60.83 105.87,-69.56 112.33,-69.56 116.17,-69.56 118.76,-66.48 120.11,-61.82"/>
<polygon fill="black" stroke="black" points="123.58,-62.31 120.99,-52.04 116.61,-61.69 123.58,-62.31"/>
<text xml:space="preserve" text-anchor="middle" x="112.33" y="-73.76" font-family="Times,serif" font-size="14.00">1</text>
</g>
</g>
</svg>

Language: all binary strings ending in `1`.
Regular expression: $(0|1)^* 1$.

---

## Exercise 06 — DFA for Exactly Two $a$

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="231pt" height="94pt" viewBox="0.00 0.00 231.00 94.00">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 90.36)">
<title>M2</title>
<polygon fill="white" stroke="none" points="-4,4 -4,-90.36 227.1,-90.36 227.1,4 -4,4"/>
<!-- q0 -->
<g id="node1" class="node">
<title>q0</title>
<ellipse fill="none" stroke="black" cx="21.78" cy="-25.78" rx="21.78" ry="21.78"/>
<text xml:space="preserve" text-anchor="middle" x="21.78" y="-21.58" font-family="Helvetica,sans-Serif" font-size="14.00">q0</text>
</g>
<!-- q0&#45;&gt;q0 -->
<g id="edge1" class="edge">
<title>q0-&gt;q0</title>
<path fill="none" stroke="black" d="M13.97,-46.24C12.89,-56.45 15.5,-65.56 21.78,-65.56 25.51,-65.56 27.94,-62.35 29.07,-57.6"/>
<polygon fill="black" stroke="black" points="32.56,-57.9 29.51,-47.76 25.57,-57.59 32.56,-57.9"/>
<text xml:space="preserve" text-anchor="middle" x="21.78" y="-69.76" font-family="Times,serif" font-size="14.00">b</text>
</g>
<!-- q1 -->
<g id="node2" class="node">
<title>q1</title>
<ellipse fill="none" stroke="black" cx="107.55" cy="-25.78" rx="21.78" ry="21.78"/>
<text xml:space="preserve" text-anchor="middle" x="107.55" y="-21.58" font-family="Helvetica,sans-Serif" font-size="14.00">q1</text>
</g>
<!-- q0&#45;&gt;q1 -->
<g id="edge2" class="edge">
<title>q0-&gt;q1</title>
<path fill="none" stroke="black" d="M43.92,-25.78C53.04,-25.78 63.93,-25.78 74.05,-25.78"/>
<polygon fill="black" stroke="black" points="73.9,-29.28 83.9,-25.78 73.9,-22.28 73.9,-29.28"/>
<text xml:space="preserve" text-anchor="middle" x="64.66" y="-29.98" font-family="Times,serif" font-size="14.00">a</text>
</g>
<!-- q1&#45;&gt;q1 -->
<g id="edge3" class="edge">
<title>q1-&gt;q1</title>
<path fill="none" stroke="black" d="M99.74,-46.24C98.66,-56.45 101.27,-65.56 107.55,-65.56 111.28,-65.56 113.71,-62.35 114.84,-57.6"/>
<polygon fill="black" stroke="black" points="118.33,-57.9 115.28,-47.76 111.34,-57.59 118.33,-57.9"/>
<text xml:space="preserve" text-anchor="middle" x="107.55" y="-69.76" font-family="Times,serif" font-size="14.00">b</text>
</g>
<!-- q2 -->
<g id="node3" class="node">
<title>q2</title>
<ellipse fill="none" stroke="black" cx="197.32" cy="-25.78" rx="21.78" ry="21.78"/>
<ellipse fill="none" stroke="black" cx="197.32" cy="-25.78" rx="25.78" ry="25.78"/>
<text xml:space="preserve" text-anchor="middle" x="197.32" y="-21.58" font-family="Helvetica,sans-Serif" font-size="14.00">q2</text>
</g>
<!-- q1&#45;&gt;q2 -->
<g id="edge4" class="edge">
<title>q1-&gt;q2</title>
<path fill="none" stroke="black" d="M129.79,-25.78C138.86,-25.78 149.71,-25.78 159.98,-25.78"/>
<polygon fill="black" stroke="black" points="159.7,-29.28 169.7,-25.78 159.7,-22.28 159.7,-29.28"/>
<text xml:space="preserve" text-anchor="middle" x="150.43" y="-29.98" font-family="Times,serif" font-size="14.00">a</text>
</g>
<!-- q2&#45;&gt;q2 -->
<g id="edge5" class="edge">
<title>q2-&gt;q2</title>
<path fill="none" stroke="black" d="M188.6,-50.53C188,-60.83 190.91,-69.56 197.32,-69.56 201.12,-69.56 203.69,-66.48 205.03,-61.82"/>
<polygon fill="black" stroke="black" points="208.5,-62.31 205.9,-52.04 201.53,-61.69 208.5,-62.31"/>
<text xml:space="preserve" text-anchor="middle" x="197.32" y="-73.76" font-family="Times,serif" font-size="14.00">b</text>
</g>
</g>
</svg>

Regular expression: $(b)^* a (b)^* a (b)^*$.

---

## Exercise 07 — Automaton M3

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="232pt" height="156pt" viewBox="0.00 0.00 232.00 156.00">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 152.36)">
<title>M3</title>
<polygon fill="white" stroke="none" points="-4,4 -4,-152.36 227.88,-152.36 227.88,4 -4,4"/>
<!-- q0 -->
<g id="node1" class="node">
<title>q0</title>
<ellipse fill="none" stroke="black" cx="21.78" cy="-41.78" rx="21.78" ry="21.78"/>
<text xml:space="preserve" text-anchor="middle" x="21.78" y="-37.58" font-family="Helvetica,sans-Serif" font-size="14.00">q0</text>
</g>
<!-- q1 -->
<g id="node2" class="node">
<title>q1</title>
<ellipse fill="none" stroke="black" cx="107.55" cy="-41.78" rx="21.78" ry="21.78"/>
<text xml:space="preserve" text-anchor="middle" x="107.55" y="-37.58" font-family="Helvetica,sans-Serif" font-size="14.00">q1</text>
</g>
<!-- q0&#45;&gt;q1 -->
<g id="edge1" class="edge">
<title>q0-&gt;q1</title>
<path fill="none" stroke="black" d="M43.92,-41.78C53.04,-41.78 63.93,-41.78 74.05,-41.78"/>
<polygon fill="black" stroke="black" points="73.9,-45.28 83.9,-41.78 73.9,-38.28 73.9,-45.28"/>
<text xml:space="preserve" text-anchor="middle" x="64.66" y="-45.98" font-family="Times,serif" font-size="14.00">a</text>
</g>
<!-- q2 -->
<g id="node3" class="node">
<title>q2</title>
<ellipse fill="none" stroke="black" cx="198.1" cy="-87.78" rx="21.78" ry="21.78"/>
<ellipse fill="none" stroke="black" cx="198.1" cy="-87.78" rx="25.78" ry="25.78"/>
<text xml:space="preserve" text-anchor="middle" x="198.1" y="-83.58" font-family="Helvetica,sans-Serif" font-size="14.00">q2</text>
</g>
<!-- q1&#45;&gt;q2 -->
<g id="edge2" class="edge">
<title>q1-&gt;q2</title>
<path fill="none" stroke="black" d="M127.33,-51.53C138.12,-57.14 151.97,-64.33 164.48,-70.83"/>
<polygon fill="black" stroke="black" points="162.75,-73.88 173.24,-75.38 165.98,-67.67 162.75,-73.88"/>
<text xml:space="preserve" text-anchor="middle" x="150.83" y="-68.98" font-family="Times,serif" font-size="14.00">b</text>
</g>
<!-- q3 -->
<g id="node4" class="node">
<title>q3</title>
<ellipse fill="none" stroke="black" cx="198.1" cy="-21.78" rx="21.78" ry="21.78"/>
<text xml:space="preserve" text-anchor="middle" x="198.1" y="-17.58" font-family="Helvetica,sans-Serif" font-size="14.00">q3</text>
</g>
<!-- q1&#45;&gt;q3 -->
<g id="edge3" class="edge">
<title>q1-&gt;q3</title>
<path fill="none" stroke="black" d="M129.09,-37.14C139.85,-34.71 153.27,-31.68 165.34,-28.95"/>
<polygon fill="black" stroke="black" points="165.91,-32.41 174.89,-26.79 164.37,-25.58 165.91,-32.41"/>
<text xml:space="preserve" text-anchor="middle" x="150.83" y="-37.18" font-family="Times,serif" font-size="14.00">a</text>
</g>
<!-- q2&#45;&gt;q2 -->
<g id="edge4" class="edge">
<title>q2-&gt;q2</title>
<path fill="none" stroke="black" d="M189.32,-112.53C188.71,-122.83 191.64,-131.56 198.1,-131.56 201.94,-131.56 204.53,-128.48 205.88,-123.82"/>
<polygon fill="black" stroke="black" points="209.35,-124.31 206.76,-114.04 202.38,-123.69 209.35,-124.31"/>
<text xml:space="preserve" text-anchor="middle" x="198.1" y="-135.76" font-family="Times,serif" font-size="14.00">a,b</text>
</g>
</g>
</svg>

Language: strings starting with `a`, containing the substring `ab`.
Regular expression: $a (a|b)^* b (a|b)^*$.

---

## Exercise 08 — DFA for Strings Ending with `01`

<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="233pt" height="127pt" viewBox="0.00 0.00 233.00 127.00">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 123.36)">
<title>EndsWith01</title>
<polygon fill="white" stroke="none" points="-4,4 -4,-123.36 228.67,-123.36 228.67,4 -4,4"/>
<!-- q0 -->
<g id="node1" class="node">
<title>q0</title>
<ellipse fill="none" stroke="black" cx="21.78" cy="-21.78" rx="21.78" ry="21.78"/>
<text xml:space="preserve" text-anchor="middle" x="21.78" y="-17.58" font-family="Helvetica,sans-Serif" font-size="14.00">q0</text>
</g>
<!-- q0&#45;&gt;q0 -->
<g id="edge2" class="edge">
<title>q0-&gt;q0</title>
<path fill="none" stroke="black" d="M13.9,-42.24C12.81,-52.45 15.44,-61.56 21.78,-61.56 25.54,-61.56 28,-58.35 29.14,-53.6"/>
<polygon fill="black" stroke="black" points="32.63,-53.9 29.58,-43.76 25.64,-53.59 32.63,-53.9"/>
<text xml:space="preserve" text-anchor="middle" x="21.78" y="-65.76" font-family="Times,serif" font-size="14.00">1</text>
</g>
<!-- q1 -->
<g id="node2" class="node">
<title>q1</title>
<ellipse fill="none" stroke="black" cx="108.33" cy="-62.78" rx="21.78" ry="21.78"/>
<text xml:space="preserve" text-anchor="middle" x="108.33" y="-58.58" font-family="Helvetica,sans-Serif" font-size="14.00">q1</text>
</g>
<!-- q0&#45;&gt;q1 -->
<g id="edge1" class="edge">
<title>q0-&gt;q1</title>
<path fill="none" stroke="black" d="M41.97,-31.08C52.67,-36.27 66.22,-42.84 78.2,-48.65"/>
<polygon fill="black" stroke="black" points="76.34,-51.64 86.86,-52.85 79.39,-45.34 76.34,-51.64"/>
<text xml:space="preserve" text-anchor="middle" x="65.06" y="-47.71" font-family="Times,serif" font-size="14.00">0</text>
</g>
<!-- q1&#45;&gt;q1 -->
<g id="edge3" class="edge">
<title>q1-&gt;q1</title>
<path fill="none" stroke="black" d="M100.46,-83.24C99.37,-93.45 101.99,-102.56 108.33,-102.56 112.1,-102.56 114.55,-99.35 115.7,-94.6"/>
<polygon fill="black" stroke="black" points="119.19,-94.9 116.14,-84.76 112.19,-94.59 119.19,-94.9"/>
<text xml:space="preserve" text-anchor="middle" x="108.33" y="-106.76" font-family="Times,serif" font-size="14.00">0</text>
</g>
<!-- q2 -->
<g id="node3" class="node">
<title>q2</title>
<ellipse fill="none" stroke="black" cx="198.89" cy="-38.78" rx="21.78" ry="21.78"/>
<ellipse fill="none" stroke="black" cx="198.89" cy="-38.78" rx="25.78" ry="25.78"/>
<text xml:space="preserve" text-anchor="middle" x="198.89" y="-34.58" font-family="Helvetica,sans-Serif" font-size="14.00">q2</text>
</g>
<!-- q1&#45;&gt;q2 -->
<g id="edge4" class="edge">
<title>q1-&gt;q2</title>
<path fill="none" stroke="black" d="M130.53,-61.2C138.35,-60.3 147.22,-58.91 155.11,-56.78 158.25,-55.93 161.46,-54.9 164.64,-53.77"/>
<polygon fill="black" stroke="black" points="165.74,-57.1 173.79,-50.2 163.2,-50.58 165.74,-57.1"/>
<text xml:space="preserve" text-anchor="middle" x="151.61" y="-62.45" font-family="Times,serif" font-size="14.00">1</text>
</g>
<!-- q2&#45;&gt;q0 -->
<g id="edge6" class="edge">
<title>q2-&gt;q0</title>
<path fill="none" stroke="black" d="M174.88,-28.52C162,-23.33 145.48,-17.6 130.11,-14.98 104.95,-10.69 76.06,-12.92 54.59,-15.93"/>
<polygon fill="black" stroke="black" points="54.35,-12.43 45,-17.41 55.42,-19.34 54.35,-12.43"/>
<text xml:space="preserve" text-anchor="middle" x="108.33" y="-19.18" font-family="Times,serif" font-size="14.00">1</text>
</g>
<!-- q2&#45;&gt;q1 -->
<g id="edge5" class="edge">
<title>q2-&gt;q1</title>
<path fill="none" stroke="black" d="M173.16,-33.96C165.03,-33.28 156.04,-33.5 148.11,-35.98 143.45,-37.43 138.86,-39.66 134.56,-42.23"/>
<polygon fill="black" stroke="black" points="132.7,-39.26 126.4,-47.77 136.64,-45.05 132.7,-39.26"/>
<text xml:space="preserve" text-anchor="middle" x="151.61" y="-40.18" font-family="Times,serif" font-size="14.00">0</text>
</g>
</g>
</svg>

Language: binary strings ending with `01`.
Regular expression: $(0|1)^* 0 1$.
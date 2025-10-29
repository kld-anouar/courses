<style>
/* Dark-theme Graphviz fix */
polygon[fill="white"] { fill: none !important; }
.graph path, .graph ellipse { stroke: white !important; fill: none !important; }
.graph text { fill: white !important; }
.graph polygon[fill="black"] { fill: white !important; stroke: white !important; }
</style>

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

**Language:** M1 accepts all strings over {a,b} that contain the substring `ba`.
Equivalent regular expression: $(a|b)^* b a (a|b)^*$.

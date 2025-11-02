### Lexical Analysis Exercise

#### Objective
In this exercise, students will implement a simple lexical analyzer for a miniature programming language. The analyzer must divide source code into lexemes and identify their corresponding token types using deterministic finite automata (DFA) based on predefined regular expressions.

#### Step 1 – Lexeme Extraction
You are provided with the following Python function that splits code into lexemes. Study it carefully, as it will be the first stage of your lexical analyzer.

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1w6dWW9baJgzh1MxN8iPeL5mFFsP_EkcS?usp=sharing)



```python
def split_to_lexemes(code):
    lexemes = []
    current = ""

    separators = [' ', '\t', '\n']
    symbols = ['+', '-', '*', '/', '=', '(', ')', '{', '}', ';', ',', '<', '>']

    for ch in code:
        if ch in separators:
            if current:
                lexemes.append(current)
                current = ""
        elif ch in symbols:
            if current:
                lexemes.append(current)
                current = ""
            lexemes.append(ch)
        else:
            current += ch

    if current:
        lexemes.append(current)

    return lexemes
```

Example:
```python
code = 'void main() { int x = 15 * 30; string msg = "Hello"; }'
print(split_to_lexemes(code))
```
Expected output:
```
['void', 'main', '(', ')', '{', 'int', 'x', '=', '15', '*', '30', ';', 'string', 'msg', '=', '"Hello"', ';', '}']
```

#### Step 2 – Token Recognition
For each lexeme returned by the previous function, you will implement a function that checks which token type it belongs to. Each token type must be recognized by an automaton you will design. Use the following regular expressions as a reference for your automata.

#### Regular Expressions
- **Keyword**: `(int|float|void|if|else|while|return)`
- **Identifier**: `[a-zA-Z_][a-zA-Z0-9_]*`
- **Number**: `[0-9]+`
- **String literal**: `"([^"\\]|\\.)*"`
- **Operator**: `[+-*/==<>]`
- **Separator**: `[(){};,]`

#### Step 3 – Implementation
Implement a Python program with the following structure:

```python
def is_keyword(token):
    # DFA for keywords
    pass

def is_identifier(token):
    # DFA for identifiers
    pass

def is_number(token):
    # DFA for numbers
    pass

def is_string_literal(token):
    # DFA for string literals
    pass

def is_operator(token):
    # DFA for operators
    pass

def is_separator(token):
    # DFA for separators
    pass

def classify_token(token):
    if is_keyword(token):
        return "KEYWORD"
    elif is_identifier(token):
        return "IDENTIFIER"
    elif is_number(token):
        return "NUMBER"
    elif is_string_literal(token):
        return "STRING_LITERAL"
    elif is_operator(token):
        return "OPERATOR"
    elif is_separator(token):
        return "SEPARATOR"
    else:
        return "UNKNOWN"
```

#### Step 5 – Example Input and Output
**Input:**
```
void main() { int x = 15 * 30; string msg = "Hello"; }
```

**Output:**
```python
[
    ('void', 'KEYWORD'),
    ('main', 'IDENTIFIER'),
    ('(', 'SEPARATOR'),
    (')', 'SEPARATOR'),
    ('{', 'SEPARATOR'),
    ('int', 'KEYWORD'),
    ('x', 'IDENTIFIER'),
    ('=', 'OPERATOR'),
    ('15', 'NUMBER'),
    ('*', 'OPERATOR'),
    ('30', 'NUMBER'),
    (';', 'SEPARATOR'),
    ('string', 'KEYWORD'),
    ('msg', 'IDENTIFIER'),
    ('=', 'OPERATOR'),
    ('"Hello"', 'STRING_LITERAL'),
    (';', 'SEPARATOR'),
    ('}', 'SEPARATOR')
]
```

#### Step 6 – Notes
The `is_identifier(token)` function must be implemented using a graph-based deterministic finite automaton (DFA).
Hint: use a `dictionary` to represent the DFA transitions.


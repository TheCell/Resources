---
tags:
  - javascript
  - software
  - utility
  - processing
created: 24.04.2025
up: "[[Code]]"
---
[https://github.com/Experience-Monks/math-as-code](https://github.com/Experience-Monks/math-as-code)

This is a reference to ease developers into mathematical notation by showing comparisons with JavaScript code.

Motivation: Academic papers can be intimidating for self-taught game and graphics programmers. :)

This guide is not yet finished. If you see errors or want to contribute, please [open a ticket](https://github.com/Jam3/math-as-code/issues) or send a PR.

> **Note**: For brevity, some code examples make use of [npm packages](https://www.npmjs.com/). You can refer to their GitHub repos for implementation details.

# foreword
Mathematical symbols can mean different things depending on the author, context and the field of study (linear algebra, set theory, etc). This guide may not cover _all_ uses of a symbol. In some cases, real-world references (blog posts, publications, etc) will be cited to demonstrate how a symbol might appear in the wild.

For a more complete list, refer to [Wikipedia - List of Mathematical Symbols](https://en.wikipedia.org/wiki/List_of_mathematical_symbols).

For simplicity, many of the code examples here operate on floating point values and are not numerically robust. For more details on why this may be a problem, see [Robust Arithmetic Notes](https://github.com/mikolalysenko/robust-arithmetic-notes) by Mikola Lysenko.

# contents
- [variable name conventions](https://github.com/Experience-Monks/math-as-code#variable-name-conventions)
- [equals `=` `≈` `≠` `:=`](https://github.com/Experience-Monks/math-as-code#equals-symbols)
- [square root and complex numbers `√` _`i`_](https://github.com/Experience-Monks/math-as-code#square-root-and-complex-numbers)
- [dot & cross `·` `×` `∘`](https://github.com/Experience-Monks/math-as-code#dot--cross)
    - [scalar multiplication](https://github.com/Experience-Monks/math-as-code#scalar-multiplication)
    - [vector multiplication](https://github.com/Experience-Monks/math-as-code#vector-multiplication)
    - [dot product](https://github.com/Experience-Monks/math-as-code#dot-product)
    - [cross product](https://github.com/Experience-Monks/math-as-code#cross-product)
- [sigma `Σ`](https://github.com/Experience-Monks/math-as-code#sigma) - _summation_
- [capital Pi `Π`](https://github.com/Experience-Monks/math-as-code#capital-pi) - _products of sequences_
- [pipes `||`](https://github.com/Experience-Monks/math-as-code#pipes)
    - [absolute value](https://github.com/Experience-Monks/math-as-code#absolute-value)
    - [Euclidean norm](https://github.com/Experience-Monks/math-as-code#euclidean-norm)
    - [determinant](https://github.com/Experience-Monks/math-as-code#determinant)
- [hat **`â`**](https://github.com/Experience-Monks/math-as-code#hat) - _unit vector_
- ["element of" `∈` `∉`](https://github.com/Experience-Monks/math-as-code#element)
- [common number sets `ℝ` `ℤ` `ℚ` `ℕ`](https://github.com/Experience-Monks/math-as-code#common-number-sets)
- [function `ƒ`](https://github.com/Experience-Monks/math-as-code#function)
    - [piecewise function](https://github.com/Experience-Monks/math-as-code#piecewise-function)
    - [common functions](https://github.com/Experience-Monks/math-as-code#common-functions)
    - [function notation `↦` `→`](https://github.com/Experience-Monks/math-as-code#function-notation)
- [prime `′`](https://github.com/Experience-Monks/math-as-code#prime)
- [floor & ceiling `⌊` `⌉`](https://github.com/Experience-Monks/math-as-code#floor--ceiling)
- [arrows](https://github.com/Experience-Monks/math-as-code#arrows)
    - [material implication `⇒` `→`](https://github.com/Experience-Monks/math-as-code#material-implication)
    - [equality `<` `≥` `≫`](https://github.com/Experience-Monks/math-as-code#equality)
    - [conjunction & disjunction `∧` `∨`](https://github.com/Experience-Monks/math-as-code#conjunction--disjunction)
- [logical negation `¬` `~` `!`](https://github.com/Experience-Monks/math-as-code#logical-negation)
- [intervals](https://github.com/Experience-Monks/math-as-code#intervals)
- [more...](https://github.com/Experience-Monks/math-as-code#more)
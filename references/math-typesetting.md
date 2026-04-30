# Math Typesetting in Typst

## Inline and Display Math

Typst uses single dollar sign `$` for both inline and display math (unlike `$$` in LaTeX). Whitespace determines display vs inline:

```typst
// Inline math (no spaces)
$x^2 + y^2 = z^2$

// Display math (note the spaces inside $)
$ sum_(i=1)^n i = (n(n+1))/2 $
```

## Variables

In math, single letters are always displayed as is. Multiple letters, however, are interpreted as variables and functions.

To display multiple letters verbatim, place them into quotes. To access single letter variables, use the hash syntax.

```typst
$ Δ = b^2 - 4 a c $  // Space required between `a` and `c`
$ A_(i j) $          // Space required between `i` and `j`
$ A = pi r^2 $
$ "area" = pi dot "radius"^2 $
$ "area" = π ⋅ "radius"^2 $  // Unicode symbols work too
$ cal(A) :=
    { x in RR | x "is natural" } $
#let x = 5
$ #x < 17 $  // Renders as `5 < 17`
```

## Common Notation

```typst
$alpha, beta, gamma$  // Greek letters
$α, β, γ$             // Unicode Greek letters
$RR, ZZ, NN$          // Number sets (blackboard bold)
$arrow(v), hat(x)$    // Vectors and accents
$mat(1, 2; 3, 4)$     // Matrices
$ mat(
  1, 2, ..., 10;
  2, 2, ..., 10;
  dots.v, dots.v, dots.down, dots.v;
  10, 10, ..., 10;
) $  // Large matrix with ellipses
$cases(x "if" x > 0, -x "otherwise")$  // Piecewise
```

## Alignment

Use `&` to align equations and `\` for line breaks:

```typst
$ x &= a + b \
    &= c + d $
```

## Equation Numbering

```typst
// Numbered equation (with label)
$ E = m c^2 $ <eq-einstein>

// Reference it
See @eq-einstein.

// Suppress numbering for a single equation
#set math.equation(numbering: none)
```

## Common Patterns

### Fractions and Roots

```typst
$ (a + b) / c $        // Fraction
$ sqrt(x) $            // Square root
$ root(3, x) $         // Cube root
```

### Subscripts and Superscripts

```typst
$ x_1, x_2, ..., x_n $
$ e^(i pi) + 1 = 0 $
```

### Operators

```typst
$ sum_(i=0)^n a_i $
$ integral_0^1 f(x) dif x $    // Use `dif` for differential d
$ product_(k=1)^n k $
$ lim_(n -> infinity) a_n $
```

### Brackets and Parentheses

Typst auto-sizes paired parentheses and brackets.

```typst
// Auto-sized parentheses and brackets:
$ ( sum_(i=1)^n i^2 ) $
$ [ sum_(i=1)^n i^2 ] $
$ { sum_(i=1)^n i^2 } $

// Pre-defined functions with auto-sized delimiters:
$ abs(x) $  // Absolute value
$ norm(x) $ // Norm (double bars)
$ floor(x/2), ceil(x/2), round(x/2) $

// Use `lr()` for explicit auto-sizing:
$ lr(angle.l sum_(i=1)^n i^2 angle.r) $  // like `\langle ...\rangle` in LaTeX
$ lr(⟨ sum_(i=1)^n i^2 ⟩) $              // Unicode angle brackets
// `mid()` scales delimiters to the nearest surrounding lr() group:
$ { x mid(|) sum_(i=1)^n w_i|f_i (x)| < 1 } $
```

No need for `\left` and `\right` like in LaTeX.

### Mathematical Spacing

Use spacing constants between elements in formulas:

| Typst | Size | LaTeX equivalent | Example |
|---|---|---|---|
| `thin` | 1/6 em | `\,` | `$ a thin b $` |
| `med` | 2/9 em | `\:` | `$ a med b $` |
| `thick` | 5/18 em | `\;` | `$ a thick b $` |
| `quad` | 1 em | `\quad` | `$ a quad b $` |
| `wide` | 2 em | `\qquad` | `$ a wide b $` |

Alternatively, use `#h(size)` for custom spacing:

```typst
$ a #h(0.5em) b $
```

## Typst Does Not Use Braces `{}`

Unlike LaTeX, Typst never uses braces `{}` in math mode for grouping, subscripts, superscripts, or function arguments. Use `()` instead:

| LaTeX (Don't use in Typst) | Typst | Context |
|---|---|---|
| `x^{2n}` | `x^(2n) $` | Multi-character superscript |
| `x_{ij}` | `x_(i j) $` | Multi-character subscript |
| `\frac{a+b}{c}` | `$ (a+b) / c $` | Fractions use `/` |
| `\sqrt[3]{x}` | `$ root(3, x) $` | Roots use function syntax |
| `\left\lbrace ... \right\rbrace` | `$ { ... } $` | Auto-sized braces |

## Anti-Patterns

| Avoid | Prefer | Reason |
|-------|--------|--------|
| No spaces in display math `$x^2$` | Spaces: `$ x^2 $` | Display mode requires spaces |
| `d x` for differential | `dif x` | Semantic markup |

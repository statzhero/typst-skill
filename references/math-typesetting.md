# Math Typesetting in Typst

## Inline and Display Math

```typst
// Inline math
$x^2 + y^2 = z^2$

// Display math (note the spaces inside $)
$ sum_(i=1)^n i = (n(n+1))/2 $
```

## Common Notation

```typst
$alpha, beta, gamma$           // Greek letters
$RR, ZZ, NN$                   // Number sets (blackboard bold)
$arrow(v), hat(x)$             // Vectors and accents
$mat(1, 2; 3, 4)$              // Matrices
$cases(x "if" x > 0, -x "otherwise")$  // Piecewise
```

## Alignment

Use `&` to align equations and `\` for line breaks:

```typst
$ x &= a + b \
    &= c + d $
```

## Equation Numbering

Equations are unnumbered by default, and referencing an unnumbered equation is a compile error. Turn numbering on once at the top of the document.

```typst
#set math.equation(numbering: "(1)")

// Numbered equation (with label)
$ E = m c^2 $ <eq-einstein>

// Reference it
See @eq-einstein.

// Suppress numbering for a single equation
#math.equation(block: true, numbering: none, $ a + b = c $)
```

A `#set math.equation(numbering: none)` rule turns numbering off for every equation after it, so use it only to switch numbering off for the rest of a document or inside a `#[...]` scope.

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

### Brackets and Delimiters

Typst auto-sizes delimiters:

```typst
$ lr(( sum_(i=1)^n i^2 )) $     // Explicit auto-sizing with lr()
```

## Anti-Patterns

| Avoid | Prefer | Reason |
|-------|--------|--------|
| No spaces in display math `$x^2$` | Spaces: `$ x^2 $` | Display mode requires spaces |
| `d x` for differential | `dif x` | Semantic markup |
| Manual bracket sizing | `lr()` | Automatic and consistent |

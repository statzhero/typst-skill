# Quarto Integration with Typst

## Document Setup

### Basic Quarto YAML

```yaml
---
title: "My Document"
author: "Author Name"
format:
  typst:
    fontsize: 11pt
    margin:
      x: 2.5cm
      y: 3cm
---
```

### Custom Template

```yaml
---
format:
  typst:
    template-partials:
      - typst-template.typ
      - typst-show.typ
---
```

## Raw Typst Blocks

Inject raw Typst code inside a Quarto `.qmd`:

````markdown
```{=typst}
#set text(fill: blue)
This is raw Typst content in a Quarto document.
```
````

## Accessing Quarto Variables in Custom Templates

Quarto uses two template partials to bridge metadata into Typst:

- **`typst-template.typ`** defines the core template function with parameters for title, author, etc.
- **`typst-show.typ`** maps Pandoc metadata to those function arguments.

Create a custom format with `quarto create extension format` (select Typst) to generate these partials.

```typst
// Example typst-template.typ
#let article(title: none, authors: (), abstract: [], body) = {
  set document(title: title)
  align(center)[
    #text(size: 18pt, weight: "bold")[#title]
    #v(0.5em)
    #authors.map(a => a.name).join(", ")  // Quarto passes each author as a dictionary
  ]
  pad(x: 2em, abstract)
  body
}
```

In raw Typst blocks inside `.qmd` files, you do not have direct access to YAML metadata. Use Quarto's own cross-referencing and variable substitution instead.

## Math in `.qmd` Files

Write LaTeX math in the Markdown body. Pandoc parses it and converts it to Typst math, so `$\frac{\alpha}{\beta}$` becomes `$alpha / beta$`. Pandoc does not recognize Typst math in the body: `$alpha/beta$` comes out as `$a l p h a\/b e t a$`, and `$sum_(i=1)^n x_i$` fails to compile. Typst math syntax works only inside `{=typst}` raw blocks.

## Slides with Typst via Quarto

### Using Touying through Quarto

For presentations, the recommended approach is using Touying (see `/typst-touying` skill). In a Quarto context, use raw Typst blocks. Touying 0.8 requires Typst 0.15, which Quarto 1.11 bundles:

````markdown
---
format: typst
---

```{=typst}
#import "@preview/touying:0.8.0": *
#import themes.simple: *

#show: simple-theme.with(aspect-ratio: "16-9")

= Section Title

== Slide Title

- First point
- Second point
```
````

## Cross-References

Quarto cross-references work with Typst output:

```markdown
See @fig-plot for the results.

![Caption](plot.png){#fig-plot}
```

## Bibliography

Set the bibliography in YAML. By default, Quarto 1.11 passes it to Typst's own citation engine: `@key` becomes `#cite(<key>)` and Quarto emits `#bibliography()` at the end. `csl:` takes a Typst built-in style name such as `apa` or a path to a `.csl` file.

```yaml
---
bibliography: references.bib
csl: apa
format: typst
---
```

To have Pandoc's citeproc format the citations instead, for example to match HTML output exactly, add `citeproc: true` under `format: typst:`.

## Typst-Specific Options

```yaml
---
format:
  typst:
    font-paths: ["fonts/"]       # Custom font directories
    section-numbering: "1.1.a"   # Section number format
    columns: 2                   # Two-column layout
---
```

## Anti-Patterns

| Avoid | Prefer | Reason |
|-------|--------|--------|
| Typst math in the `.qmd` body | LaTeX math (`$\frac{a}{b}$`) | Pandoc converts LaTeX math to Typst and garbles Typst math |
| `#bibliography()` in Quarto docs | YAML `bibliography:` key | Let Quarto handle it |
| Mixing Quarto and Typst cross-refs | Pick one system per doc | Avoid conflicts |

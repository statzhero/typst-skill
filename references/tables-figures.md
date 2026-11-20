# Tables and Figures in Typst

## Basic Table

```typst
#table(
  columns: (auto, 1fr, 1fr),
  align: (left, center, right),
  stroke: 0.5pt,
  inset: 8pt,

  // Header row
  table.header(
    [*Name*], [*Value*], [*Unit*]
  ),

  // Data rows
  [Alpha], [1.0], [m/s],
  [Beta], [2.5], [kg],
)
```

## Table Styling

### Conditional Fill and Stroke

```typst
#table(
  columns: 3,
  fill: (x, y) => if y == 0 { luma(230) },
  stroke: (x, y) => if y == 0 { (bottom: 1pt) },
  table.header([*A*], [*B*], [*C*]),
  [1], [2], [3],
)
```

### Alternating Row Colors

```typst
#table(
  columns: 3,
  fill: (x, y) => if calc.odd(y) { luma(240) },
  // ...
)
```

### Spanning Columns and Rows

```typst
#table(
  columns: 3,
  table.cell(colspan: 3)[*Header spanning all columns*],
  [A], [B], [C],
  table.cell(rowspan: 2)[Merged], [1], [2],
  [3], [4],
)
```

### Booktabs-Style (Academic Tables)

```typst
#table(
  columns: 3,
  stroke: none,
  inset: (x: 10pt, y: 5pt),
  table.hline(stroke: 1.5pt),
  table.header([*Variable*], [*Mean*], [*SD*]),
  table.hline(stroke: 0.5pt),
  [Age], [34.2], [12.1],
  [Income], [52,100], [18,400],
  table.hline(stroke: 1.5pt),
)
```

## Figures

### Basic Figure

```typst
#figure(
  image("diagram.svg", width: 80%, alt: "Bar chart of audit fees by firm size"),
  caption: [A descriptive caption.],
) <fig-diagram>

// Reference
See @fig-diagram for details.
```

Since 0.14, Typst writes tagged, accessible PDFs by default. Give every image an `alt` description; screen readers read it in place of the image.

### Side-by-Side Figures

```typst
#grid(
  columns: 2,
  gutter: 1em,
  figure(image("a.png"), caption: [First]),
  figure(image("b.png"), caption: [Second]),
)
```

### Figure with Table

```typst
#figure(
  table(
    columns: 2,
    [*Key*], [*Value*],
    [A], [1],
    [B], [2],
  ),
  caption: [Summary statistics.],
  kind: table,
) <tbl-summary>
```

## Image Formats

Typst supports SVG, PNG, JPG, GIF, WebP, and PDF (the last two since 0.14). Prefer SVG or PDF for diagrams and plots, since both stay vector.

```typst
#image("photo.jpg", width: 50%)
#image("diagram.svg", width: 100%)
#image("plot.pdf", width: 80%)
```

## Anti-Patterns

| Avoid | Prefer | Reason |
|-------|--------|--------|
| Hard-coded column widths | `1fr`, `auto`, `%` | Responsive layout |
| Manual figure numbering | `<label>` + `@label` | Automatic cross-refs |
| Inline table styling | Consistent `fill`/`stroke` functions | Maintainable |
| `kind: image` for table figures | `kind: table` | Correct numbering sequence |

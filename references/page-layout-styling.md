# Page Layout and Styling in Typst

## Page Setup

```typst
#set page(
  paper: "us-letter",      // or "a4", "a5", etc.
  margin: (x: 2.5cm, y: 3cm),
  header: context [         // context is required for counter(...).display()
    #set text(8pt)
    Document Title
    #h(1fr)
    #counter(page).display()
  ],
  footer: context [
    Page #counter(page).display() of #counter(page).final().first()
  ],
)
```

## Document Title

Since 0.14, `title()` renders the title set in `document`, and the PDF metadata picks it up as well.

```typst
#set document(title: [Firm Size and Audit Fees], author: "Author Name")
#show title: set text(size: 18pt)

#title()
```

## Text Styling

### Set Rules (Global Defaults)

```typst
#set text(font: "New Computer Modern", size: 11pt)
#set par(justify: true, leading: 0.65em)
#set heading(numbering: "1.1")
```

### Show Rules (Transform Content)

```typst
// Custom heading styling
#show heading.where(level: 1): it => {
  pagebreak(weak: true)
  text(size: 18pt, weight: "bold", it.body)
}

// Style all links
#show link: set text(fill: blue)

// Transform raw code blocks
#show raw.where(block: true): it => {
  block(fill: luma(240), inset: 8pt, radius: 4pt, it)
}
```

## Custom Functions

### Callout Boxes

```typst
#let callout(title: none, icon: none, body) = {
  block(
    width: 100%,
    fill: rgb("#f0f0f0"),
    stroke: (left: 3pt + blue),
    inset: (left: 12pt, rest: 8pt),
    radius: (right: 4pt),
  )[
    #if title != none [
      *#title* \
    ]
    #body
  ]
}
```

### Theorem Environments

```typst
#let theorem(body, name: none) = {
  block(
    width: 100%,
    inset: 8pt,
    stroke: (left: 2pt + black),
  )[
    *Theorem#if name != none [ (#name)]*.
    #emph(body)
  ]
}

#theorem(name: "Pythagoras")[
  $a^2 + b^2 = c^2$
]
```

## Layout Primitives

### Two-Column Layout

```typst
#grid(
  columns: (1fr, 1fr),
  gutter: 2em,
  [Left column content],
  [Right column content],
)
```

### Horizontal and Vertical Spacing

```typst
#h(1fr)          // Horizontal fill (push content apart)
#h(2em)          // Fixed horizontal space
#v(1cm)          // Vertical space
#parbreak()      // Paragraph break
#pagebreak()     // Page break
```

### Boxes and Blocks

```typst
// Inline box
#box(fill: yellow, inset: 2pt)[highlighted]

// Block-level container
#block(
  fill: luma(230),
  inset: 8pt,
  radius: 4pt,
  width: 100%,
)[Block content]
```

## Colors

```typst
// Named colors
#text(fill: red)[Red text]

// RGB
#text(fill: rgb("#2196F3"))[Blue text]
#text(fill: rgb(33, 150, 243))[Blue text]

// Grayscale
#text(fill: luma(128))[Gray text]

// With opacity
#text(fill: red.transparentize(50%))[Semi-transparent]
```

## Variables and Logic

```typst
#let doc-name = "My Document"  // not `title`, which would shadow title()
#let draft = true

#if draft [
  #text(fill: red)[DRAFT]
]

#for item in ("A", "B", "C") [
  Item: #item \
]
```

## Debugging

1. **Visualize bounds**: `#box(stroke: red, [content])` to see element boundaries
2. **Check types**: `#type(variable)` to inspect a value's type
3. **Compilation errors**: Usually unmatched brackets `[]`, `()`, `{}`
4. **Missing content**: Ensure content blocks `[]` are properly closed

## Anti-Patterns

| Avoid | Prefer | Reason |
|-------|--------|--------|
| Inline styles everywhere | `#set` rules | Consistent, maintainable |
| Hard-coded sizes | Relative units (`1fr`, `%`) | Responsive layout |
| Manual numbering | `#counter()` | Automatic updates |
| `\n` for line breaks | Blank line or `\` | More readable |
| Repeating style in every heading | `#show heading` rule | Single source of truth |

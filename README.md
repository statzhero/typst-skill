# typst-skill

An LLM skill for modern [Typst](https://github.com/typst/typst) typesetting (0.15+), so you no longer need to wait for LaTeX to compile.

## What does it do?

When you ask an LLM to write Typst markup, it can fall back on LaTeX habits or outdated syntax. This skill gives the model a structured reference for idiomatic Typst patterns so it produces clean, correct code with less back-and-forth.

The skill covers: set and show rules, math typesetting, tables and figures, citations and bibliography, page layout and styling, custom functions, and Quarto integration. Related skills handle slides ([typst-touying-skill](https://github.com/statzhero/typst-touying-skill)) and diagrams ([typst-cetz-skill](https://github.com/statzhero/typst-cetz-skill)) if installed.

## Installation

### Claude Code (one command)

Clone directly into your skills directory:

```bash
git clone https://github.com/statzhero/typst-skill.git ~/.claude/skills/typst
```

That's it. The skill is available immediately as `/typst` in any Claude Code session.

If you prefer not to use the terminal, you can add skills from the Claude desktop app:

1. [Download this repository as a ZIP](https://github.com/statzhero/typst-skill/archive/refs/heads/main.zip) from GitHub.
2. Open the Claude desktop app and switch to the **Code** tab.
3. Click **Customize** in the left sidebar, then select **Skills**.
4. Click the **+** button, choose **Upload a skill**, and select the ZIP file.

### Codex

Clone into your user skills directory (available across all projects):

```bash
git clone https://github.com/statzhero/typst-skill.git ~/.agents/skills/typst
```

If you prefer not to use the terminal, [download the ZIP](https://github.com/statzhero/typst-skill/archive/refs/heads/main.zip), unzip it, and move the folder to `~/.agents/skills/typst/` or `.agents/skills/typst/` inside your project.

### Other LLMs

Paste the contents of `SKILL.md` into your system prompt or attach it as context. The reference files in `references/` can be appended when you need coverage of a specific topic.

## Test the skill

After installing, try this prompt in Claude Code:

```
/typst Make an appendix table of variable definitions with columns for variable, definition, and source. Use booktabs-style rules and a caption I can reference.
```

## See also

[econ-working-paper](https://typst.app/universe/package/econ-working-paper) — a Typst template for working papers in economics, finance, and accounting.

## License

[CC-BY-4.0](LICENSE)

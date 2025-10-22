# Working Paper Skills

Claude skills for conducting mathematical research with LaTeX working documents.

## What Are Skills?

Skills are folders of instructions, scripts, and resources that Claude loads dynamically to improve performance on specialized tasks. This repository provides a skill designed specifically for mathematical research collaboration, helping maintain clean LaTeX documents while tracking mathematical insights through version control.

## What This Skill Provides

The **working-paper** skill enables collaborative mathematical research by:

- **Maintaining clean LaTeX** with semantic macros that are easy to refactor
- **Documenting mathematical insights** in git commits, not just code changes
- **Organizing mathematical content** (definitions, theorems, proofs, proof sketches)
- **Tracking failed approaches** so you don't revisit dead ends
- **Managing bibliographies** with proper arXiv citation formatting

## Installation

### Claude Code

Add this marketplace to Claude Code:

```
/plugin marketplace add droy/working-paper-skills
```

Then install the skill:

```
/plugin install working-paper
```

### Manual Installation

Clone or download this repository and copy the `working-paper/` directory to your Claude Code skills folder (`~/.claude/skills/`).

## What's Included

### Templates

- **working-paper-template.tex**: LaTeX starter template with:
  - Standard mathematical packages (amsmath, amsthm, amssymb)
  - Biblatex configuration
  - Theorem environments
  - Semantic macro examples
  - Suggested document structure

- **bibliography-template.bib**: Example bibliography with proper arXiv citation format

### Reference Documentation

- **latex-best-practices.md**: Detailed guidelines for:
  - Semantic macro design (compact, prefix-free naming)
  - LaTeX hygiene principles
  - Working with existing documents

### Core Workflows

The skill provides guidance on:

- **Initial setup**: Choosing between new templates or working with existing documents
- **LaTeX hygiene**: Using semantic macros (`\measa`, `\vb`, `\setx`) instead of raw characters
- **Git commit practices**: Writing commits that explain mathematical insights
- **Document organization**: Structuring definitions, theorems, and proofs
- **Documenting failures**: Recording why certain approaches don't work
- **Bibliography management**: Building references incrementally with biblatex

## Example Use Cases

**Starting a new research project:**
```
"I want to start a new working paper on measure theory.
Use the working-paper skill to set up the project."
```

**Working on existing research:**
```
"Help me add a new theorem to my working paper.
The theorem states that..."
```

**Documenting a failed approach:**
```
"The direct construction approach didn't work because of
non-commutativity. Help me document this properly."
```

## Key Principles

### Semantic Macros

Instead of using raw characters that are hard to refactor:

```latex
% Bad - hard to change later
Let a and b be two measures...
```

Use semantic macros:

```latex
% Good - easy to refactor
\newcommand{\measa}{\mu}
\newcommand{\measb}{\nu}
Let \measa and \measb be two measures...
```

### Mathematical Git Commits

Write commit messages that explain the **mathematical insight**, not just the change:

```
✗ Bad: "Deleted section 3"

✓ Good: "Remove flawed approach based on direct construction

The direct construction in Section 3 fails because it doesn't
account for the non-commutativity of the operators."
```

## Philosophy

This skill treats your working document as both a research log and a foundation for future publications. The LaTeX document reflects your current understanding, while git history documents your mathematical journey—including dead ends and failed approaches that might benefit future readers.

## Contributing

Issues and pull requests welcome! If you develop improvements or find issues, please contribute back to help other mathematical researchers.

## License

This skill is provided as-is for educational and research purposes. Feel free to modify and adapt it for your own use.

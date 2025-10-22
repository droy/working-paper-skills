---
name: working-paper
description: Use when conducting mathematical research with LaTeX working documents - maintains clean LaTeX with semantic macros, documents mathematical insights in git commits, organizes definitions/theorems/proofs, and tracks both successful results and failed approaches
---

# Working Paper

## Overview

This skill enables collaborative mathematical research by maintaining a LaTeX working document that serves as both a research log and a foundation for future publications. The working document reflects current understanding through definitions, theorems, proofs, proof sketches, and connecting text, while git commits document the mathematical insights underlying changes.

## Initial Setup

Before starting work, determine the project structure:

1. **New vs. Existing Document**
   - New project: Use `assets/working-paper-template.tex` as starting point
   - Existing document: Follow established conventions and style

2. **File Organization** (ask user for preference)
   - Single `.tex` file (recommended for most projects)
   - Multiple files (for very large projects)

3. **Git Repository**
   - Initialize if starting fresh
   - Ensure existing repository is clean before beginning work

## LaTeX Hygiene

### Semantic Macros

Use semantic macros instead of raw characters to make notation refactorable and meaningful.

**Core principles**:
- **Compact**: Keep macros short (token-efficient for both humans and LLMs)
- **Prefix-free**: Enable clean search-and-replace without false matches
- **Semantic**: Convey meaning, not just appearance

**Naming patterns** (from `references/latex-best-practices.md`):
- Measures: `\measa`, `\measb`, `\measmu`
- Vectors: `\va`, `\vb`, `\vx`
- Sets: `\seta`, `\setb`, `\setx`
- Functions: `\fa`, `\fb`, `\fmap`
- Operators: `\opa`, `\opb`, `\opt`

**Example**:

Bad (hard to refactor):
```latex
Let a and b be two measures...
```

Good (easy to refactor):
```latex
\newcommand{\measa}{\mu}
\newcommand{\measb}{\nu}
Let \measa and \measb be two measures...
```

**For existing documents**: Use existing macro patterns and definitions. Don't introduce new conventions unless there's clear benefit.

**When introducing new macros**: Check what's already defined and maintain consistency with existing patterns.

### Code Quality

Follow these practices for maintainable LaTeX:

1. **One sentence per line**: Makes git diffs readable
2. **Consistent indentation**: Aids in understanding structure
3. **Structural comments**: Use `% ============` dividers for major sections
4. **Descriptive labels**: `\label{thm:main}` not `\label{t1}`

### Equation Formatting

Format long equations using `align` environment to prevent overfull hbox issues:

**Alignment conventions**:
- Place `&` before relations (`=`, `\le`, `\ge`, etc.)
- When breaking at binary operators (`+`, `-`), place operator on new line
- Indent continued lines with `&\quad` or `&\qquad` to align with content (not relation)

**Example**:
```latex
\begin{align}
f(x) &= a + b + c + d + e + f + g \\
     &\quad + h + i + j + k \\
     &= (a + b + c) + (d + e + f) \\
     &\le M
\end{align}
```

### LaTeX Warnings

**Ignore unless user explicitly requests fixes**:
- Overfull `\hbox` warnings (often benign)
- Underfull `\hbox` warnings
- Minor spacing issues

**Address proactively**:
- Undefined references or citations
- Missing labels
- Package conflicts or errors
- Multiply defined labels

Use good formatting practices (like `align` for long equations) to naturally avoid warnings, but don't chase down every overfull hbox.

## Document Organization

Organize content to aid both human and LLM readers:

**Standard sections**:
- Introduction and motivation
- Definitions and preliminaries
- Main results (theorems, lemmas, propositions)
- Proofs (or proof sketches with TODOs for gaps)
- Open questions
- Failed approaches (when informative)

**Theorem environments**: Use standard names consistently
- `theorem`, `lemma`, `proposition`, `corollary`
- `definition`, `example`
- `remark`, `note`

**Incomplete proofs**: Mark clearly
```latex
\begin{proof}[Proof sketch]
The main idea is...
% TODO: Fill in details for the case when...
\end{proof}
```

## Git Commit Practices

Git commits serve two purposes:
1. Track literal changes to document
2. Document mathematical insights and reasoning

### When to Commit

Commit frequently at natural breakpoints:
- After adding/completing a definition
- After stating a theorem or lemma
- After completing/updating a proof
- After documenting a failed approach
- After significant reorganization
- Before exploring speculative directions

### Commit Messages

Write commit messages that explain the **mathematical insight**, not just the literal change.

**Bad** (describes only the change):
```
Deleted section 3
```

**Good** (explains the mathematical reasoning):
```
Remove flawed approach based on direct construction

The direct construction in Section 3 fails because it doesn't
account for the non-commutativity of the operators. This approach
cannot be salvaged without additional assumptions.
```

**Good** (documents progress):
```
Add proof sketch for main theorem using duality argument

Proof uses duality to reduce to the bounded case. Details remain
for handling the convergence in Step 3, but the overall strategy
is sound.
```

### Commit Workflow

Follow standard git workflow:
1. Review changes: `git diff` to see what changed
2. Stage relevant files: `git add <files>`
3. Write meaningful commit message documenting the mathematical insight
4. Commit: `git commit`

**Never use** `--no-verify` or skip commit messages to save time.

## Documenting Failed Approaches

Failed approaches are valuable. Document them when:
- The approach seems natural/obvious (prevents others from trying)
- The failure reveals important structural properties
- The approach partially works (may inspire future work)

**Where to document**:
1. **Always**: In git commit message when removing/changing the approach
2. **Often**: In working document section "Failed Approaches" or "Open Questions"

**What to include**:
- What was attempted
- Why it seemed promising
- Why it fails
- Whether partial results can be salvaged

Example:
```latex
\subsection{Failed Approach: Direct Construction}

We initially attempted to construct \measa directly using...
This fails because the construction doesn't preserve the
required commutativity property. The underlying issue is that...
```

## Bibliography Management

Use biblatex for reference management:

**Setup** (in preamble):
```latex
\usepackage[backend=biber,style=alphabetic]{biblatex}
\addbibresource{references.bib}
```

**arXiv citations**: Use `@misc` with `eprint` and `archivePrefix`:
```bibtex
@misc{author2024example,
    author = {First Author and Second Coauthor},
    title = {An Example Paper Title},
    year = {2024},
    eprint = {2401.12345},
    archivePrefix = {arXiv},
    primaryClass = {math.PR}
}
```

**Building bibliography**: Add references gradually as related work is identified.

## Working Session Workflow

Typical workflow for a research session:

1. **Start**: Review current state of working document and recent commits
2. **Work**: Make changes (add definitions, theorems, proofs, etc.)
3. **Maintain hygiene**: Use semantic macros, keep organization clean
4. **Commit frequently**: Document insights as you develop them
5. **Document dead ends**: When approaches fail, explain why
6. **End session**: Ensure document is in clean state with recent work committed

## Resources

### references/latex-best-practices.md
Detailed guidelines for semantic macros, LaTeX hygiene, and working with existing documents.

### assets/working-paper-template.tex
Starting template for new projects with:
- Standard mathematical packages (amsmath, amsthm, amssymb)
- Biblatex setup
- Theorem environments
- Semantic macro examples
- Suggested document structure

### assets/bibliography-template.bib
Example bibliography entries showing proper formatting for arXiv preprints and published works, with names in natural order.

## Common Pitfalls

**Avoid**:
- Using raw characters (a, b, c) for mathematical objects that appear multiple times
- Vague commit messages that don't explain mathematical reasoning
- Deleting failed approaches without documenting why they failed
- Inconsistent macro naming within a document
- Unnecessary refactoring that clutters git history
- Skipping commits to "save time" (commits are the research log)

**Remember**:
- The working document is both research log and publication foundation
- Git history documents your mathematical journey
- Clean, semantic LaTeX helps both humans and LLMs understand the work
- Failed approaches are valuable contributions to mathematical knowledge

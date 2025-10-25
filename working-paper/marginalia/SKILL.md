---
name: marginalia-collaboration
description: Use when collaborating on LaTeX documents and need to add comments, questions, or review markers that can be easily toggled off for publication - provides margin notes and inline highlighting for localized discussion
---

# Marginalia: Collaborative LaTeX Commenting

## Overview

**Marginalia is a collaboration tool for having conversations directly in your LaTeX document.** Use margin notes to ask questions, raise concerns, and mark text for review—all localized to specific places in the text.

**Core principle**: Keep the document readable while enabling rich collaboration. Comments live in margins, not inline.

## When to Use

Use marginalia when:
- Working with co-authors who need to comment on specific parts
- Claude needs to ask questions or raise concerns about text
- You're adding new text that needs review
- Flagging errors or issues that need attention

Don't use for:
- Final camera-ready documents (use `hide` option first)
- Documents where margin space is critical

## Setup

Add to your LaTeX preamble:

```latex
\usepackage{marginalia}
```

For wider margins to accommodate notes:

```latex
\usepackage[setmargin=true,marginparwidth=0.75in]{marginalia}
```

## Quick Reference: Command Selection

| Situation | Command | Result |
|-----------|---------|--------|
| Ask question or add comment | `\fTBD{comment}` | Margin note with superscript number |
| Mark text needing attention | `\NA{text}` | Inline highlighted text |
| Flag serious error | `\fPROBLEM{comment}` | Margin note (like fTBD but for errors) |
| Placeholder for missing text | `\TBD{placeholder}` | Inline highlighted placeholder |
| Inline error marker | `\PROBLEM{text}` | Inline highlighted error |

**Key distinction**:
- **f-prefix** (`\fTBD`, `\fPROBLEM`) = **margin notes** (don't interrupt flow)
- **no prefix** (`\TBD`, `\PROBLEM`, `\NA`) = **inline** (interrupt flow)

**Prefer margin notes** to keep document readable.

## Core Pattern: Localized Conversation

### Using \fTBD for Questions and Comments

`\fTBD` creates margin notes for **conversation**—questions, suggestions, concerns localized to specific text.

**Example: Claude asking about a claim**
```latex
The measure $\mu$ is finite\fTBD{Is this always true, or only under
the compactness assumption from Theorem 2?} on the space $X$.
```

**Example: User requesting clarification**
```latex
We apply the dominated convergence theorem\fTBD{Claude: please add
the conditions that make DCT applicable here} to obtain...
```

**Why this matters**: The conversation happens *at the location* where it's relevant. No disconnected comments in separate files or general notes.

### Using \NA for Attention Markers

`\NA` highlights text that **needs attention**—new drafts, uncertain statements, anything requiring review.

**Example: Rough draft text**
```latex
\NA{This paragraph attempts to connect our framework to prior
work on martingale theory, but I'm not confident the connection
is correct.}\fTBD{Please verify this reasoning}
```

The text stays inline (it's meant for the document) but highlighted to signal it needs checking.

### Using \fPROBLEM for Error Flags

`\fPROBLEM` is the severe version of `\fTBD`—use when something is **seriously wrong**.

**Example: Mathematical error**
```latex
\begin{equation}
\int_X f \, d\mu = \sum_{i=1}^n f(x_i)\fPROBLEM{WRONG: This assumes
X is finite, but we stated X is uncountable in Section 2}
\end{equation}
```

## Critical Rule: Placement and Spacing

**ALWAYS explain this when showing marginalia commands**:

### Rule 1: No Spaces Around Commands

❌ **Wrong**:
```latex
This theorem \fTBD{cite source} shows the result.
```

✓ **Correct**:
```latex
This theorem\fTBD{cite source} shows the result.
```

**Why**: When marginalia is disabled for publication (using `hide` option), extra spaces remain in the document. The wrong example becomes:

```
This theorem  shows the result.
```

Note the double space. The correct example has proper spacing.

### Rule 2: Prefer End-of-Line Placement

✓ **Best**:
```latex
This theorem shows the result\fTBD{cite source}
and provides a bound on the error.
```

**Why**:
- Easier to spot when scanning code
- Facilitates automated removal with shell scripts
- Prevents spacing issues entirely

**When to use inline**: When the comment is about a specific word or phrase, place it immediately after that word (no spaces).

## Camera-Ready Preparation

### Disabling Marginalia for Publication

Change your usepackage line:

```latex
\usepackage[hide=true]{marginalia}
```

or simply:

```latex
\usepackage[hide]{marginalia}
```

This will:
- Remove all margin notes (`\fTBD`, `\fPROBLEM`)
- Remove highlighting from `\NA{text}` (text remains, highlight disappears)
- Remove superscript numbers from main text

### IMPORTANT: Check Margins

**When disabling marginalia, always verify that margins look correct.**

If margins appear too wide after removing margin notes, adjust:

```latex
\usepackage[hide=true,setmargin=true,marginparwidth=1in]{marginalia}
```

Or handle margin adjustment separately in your document class/geometry package.

**Why this matters**: Documents formatted with wide margins for notes may look odd when notes disappear. Always preview the camera-ready version.

## Common Mistakes

| Mistake | Problem | Fix |
|---------|---------|-----|
| Adding spaces around commands | Creates double spaces when hidden | Place command adjacent to word: `word\fTBD{}` |
| Not explaining spacing rules | User copies format but doesn't understand principle | Always explain why placement matters |
| Using inline commands unnecessarily | Makes document hard to read | Prefer `\fTBD` over `\TBD` to keep margins |
| Forgetting to check margins | Camera-ready version has weird spacing | Always preview with hide option before submission |
| Using TBD/PROBLEM for conversation | Interrupts document flow | Use margin versions (fTBD/fPROBLEM) for discussion |

## Collaboration Workflow

### When Claude Adds Marginalia

**Always explain**:
1. Why you chose that command type
2. The spacing rules (no spaces around commands)
3. That it can be toggled off with `hide` option

**Example**:
"I'm adding a margin note with `\fTBD{question}` to ask about this assumption. Note that I'm placing it directly after the word with no spaces—this prevents spacing issues when you disable marginalia later using the `hide=true` option."

### When User Requests Marginalia

**Interpret requests as**:
- "Add a comment" → `\fTBD{}`
- "Mark this for review" → `\NA{}`
- "This is wrong" → `\fPROBLEM{}`

**Always ask if uncertain** which command type is appropriate.

### When User Requests Inline Commands

If user asks for inline commands (`\TBD`, `\PROBLEM`) instead of margin versions:

**Scenario A: User casually requests inline without context**

Gently suggest margin version: "I can do that. Just to note—margin notes (`\fTBD`) are typically preferred because they keep the document readable. But if you have a specific reason for inline, I'm happy to show you how."

**Scenario B: User explicitly states reason for preferring inline**

Example: "I find margin notes hard to read" or "I need comments in the text itself"

Respect their stated preference immediately. Explain inline commands without re-suggesting margins—they've already considered and chosen.

**Balance**: Teach best practice without being tone-deaf to stated preferences.

## Real-World Example

```latex
\section{Main Result}\fTBD{Should this be "Main Theorem" instead?}

\begin{theorem}
  Let $\mu$ be a \NA{finite} measure on a measurable space $(X, \mathcal{F})$.
  Then\fTBD{Claude: I think we need a compactness assumption here.
  Can you verify?} for any integrable function $f$,
  \begin{equation}
    \int_X f \, d\mu < \infty\fPROBLEM{This is trivial from
    integrability. Did we mean to say something stronger?}
  \end{equation}
\end{theorem}
```

**This shows**:
- Section title question: `\fTBD`
- Uncertain word needing verification: `\NA`
- Missing assumption: `\fTBD`
- Serious error in statement: `\fPROBLEM`

All localized to where they matter, keeping the document readable while enabling rich collaboration.

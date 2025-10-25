# LaTeX Best Practices for Working Papers

## Semantic vs Syntactic Commands

**Core principle**: Always prefer semantic macros that specify **meaning** over syntactic macros that specify **appearance**.

**Why?** Semantic macros make notation refactorable. When you change your mind about notation (which happens constantly in research), you can update one macro definition instead of hunting through the document.

**Examples of semantic vs syntactic built-ins**:
- Use `\to` (semantic: "goes to") not `\rightarrow` (syntactic: "right arrow")
- Use `\Nats` (semantic: naturals) not `\mathbb{N}` (syntactic: blackboard N)

**Key heuristic**: **If you're repeating a chunk of LaTeX code multiple times, there's probably a macro that needs to be defined.**

## Semantic Macro Design

### Core Principles

1. **Compact**: Keep macros short to minimize token usage and typing burden
2. **Prefix-free**: Design macros so search-and-replace works cleanly without false matches
3. **Semantic**: Macros should convey meaning, not just appearance

### Macro Naming Patterns

Use type-based prefixes combined with simple identifiers:

**Measures**: `\meas` + identifier
- `\measa`, `\measb`, `\measmu`, `\measnu`

**Vectors**: `\v` + identifier
- `\va`, `\vb`, `\vx`, `\vy`

**Sets**: `\set` + identifier
- `\seta`, `\setb`, `\setx`, `\sety`

**Functions**: `\f` + identifier
- `\fa`, `\fb`, `\fmap`, `\fproj`

**Operators**: `\op` + identifier
- `\opa`, `\opb`, `\opt`, `\opproj`

**Spaces**: `\sp` + identifier
- `\spa`, `\spb`, `\sph` (for Hilbert space)

### When to Introduce New Macros

**Do introduce macros when**:
- A mathematical object will be referenced multiple times
- The notation is complex or context-dependent
- You want flexibility to change notation later

**Don't introduce macros for**:
- One-off expressions that appear only once
- Standard notation that won't change (e.g., `\mathbb{R}`, `\inf`, `\sup`)

### Simple Variable Macros

**Bad**: Using raw lowercase Roman characters for variables
```latex
Let a and b be two measures...
```
Problem: Later changing `a` to `\alpha` requires careful search-and-replace to avoid changing every occurrence of the letter 'a'

**Good**: Using semantic macros
```latex
\newcommand{\measa}{\mu}
\newcommand{\measb}{\nu}
Let \measa and \measb be two measures...
```
Benefit: Can change `\measa` from `\mu` to `\alpha` with a single edit to the macro definition

### Composite Notation Macros

For repeated composite notation, define macros with arguments.

**Example**: If you're using `L_{\mathcal{D}}(h)` repeatedly for "risk of classifier h under distribution D":

```latex
\newcommand{\Dist}{\mathcal{D}}  % semantic macro for distribution
\newcommand{\risk}[2]{L_{#1}(#2)}  % semantic macro for risk

% Usage:
\risk{\Dist}{h}
```

This makes both the notation and the code easier to read and change. You can easily modify how `\risk` is typeset without hunting through the document.

### Avoiding Bare Roman Characters

**Problem**: Using bare letters like `x`, `y`, `r`, `s` for mathematical variables makes them nearly impossible to refactor later. The letter `r` appears everywhere in surrounding text, so find-and-replace is painful and error-prone.

**Three solutions**:

1. **Always use macros** (recommended for longer papers):
   ```latex
   \newcommand{\radius}{r}
   \newcommand{\dist}{d}
   % Later, easy to change r → \rho without affecting text
   ```

2. **Bracket convention** for easy search-replace:
   ```latex
   % Use {x} and {r} throughout
   C^{x}  % easy to replace {x} with {x^*}
   ```
   Warning: Be careful with expressions like `C^{x}` when replacing `{x}` with `x^*` (produces `C^x^*`, an error).

3. **Use Greek letters** whose macros are easily swapped:
   ```latex
   \alpha, \beta, \gamma  % easy to search-replace \alpha → \beta
   ```

**When to use which approach**:
- Very short assignments: Bare letters are fine
- Longer papers where notation might change: Use macros or Greek letters
- Following established conventions: If mimicking another paper's notation wholesale, bare letters may be acceptable
- Generic placeholders: Define `\genvar`, `\genfn` macros for generic variables

**Note**: Even with these conventions, **always use semantic macros for composite notation** like `\risk` above—they're easier to type and read regardless of symbol choice.

## Working with Existing Documents

When working with an existing paper:
- **Follow existing conventions**: Use the paper's established macro patterns
- **Reuse existing macros**: Check what's already defined before creating new ones
- **Match the style**: Maintain consistency with existing mathematical notation
- **Don't refactor unnecessarily**: Only change notation when there's a clear benefit

## LaTeX Hygiene

### General Guidelines

1. **One sentence per line**: Makes git diffs more readable
2. **Consistent indentation**: Helps identify structure
3. **Comments for structure**: Use `% ============` style dividers for sections
4. **Explicit labels**: Use descriptive labels (`\label{thm:main}` not `\label{t1}`)

### Theorem Environments

Use standard theorem environments consistently:
- `theorem`, `lemma`, `proposition`, `corollary`
- `definition`, `example`
- `remark`, `note`

### Proof Organization

For incomplete proofs:
- Mark clearly what's done vs. what needs work
- Use proof sketches with explicit TODOs for gaps
- Document assumptions or unproven claims clearly

Example:
```latex
\begin{proof}[Proof sketch]
The main idea is...
% TODO: Fill in details for the case when...
\end{proof}
```

### Code Quality and Whitespace

**Line breaks and logical chunks**:
- **One sentence per line**: Makes git diffs readable
- When sentence is very long, break it up into logical chunks
- Different lines for sequence of mathematical objects
- Helps version control: changes affect only small chunks of text

**Whitespace in math mode**: Ignored by LaTeX, but massively improves readability
```latex
% Stack long \frac arguments vertically:
\frac{
  a + b + c + d + e
}{
  x + y + z + w
}
```

**Comments for collaboration**:
- Hide code you might want later (long derivations that are "straightforward")
- Explain why choices were made for future collaborators
- Mark TODOs and gaps in proofs
- Comments run until end of line (use `%`)

### Labels and References

Use `cleveref` package for automatic reference formatting:
- Format labels as `type:description` (e.g., `defn:mixability`, `thm:main-result`, `lem:continuity`)
- Compact but informative: convey what the object is about
- Use `\cref{defn:mixability}` to reference (automatically adds "Definition")
- Never use generic labels like `\label{t1}` or `\label{eq1}`
- For custom environments not covered by default, add `\crefname` commands in preamble
- Example: `\crefname{assumption}{Assumption}{Assumptions}` for assumption environment

## Equation Formatting

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

## Recommended Packages

These packages make writing complex mathematical documents much easier:

**Essential**:
- **amsmath, amssymb, amsthm**: Core packages for mathematical typesetting
  - Use `align`, `gather`, `split` environments instead of deprecated `eqnarray`
  - Better spacing and more powerful equation formatting
- **cleveref**: Automatic reference formatting with `\cref{label}`
  - Figures out correct word ("Theorem", "Definition", "Equation")
  - Add `\crefname` commands for custom environments
- **biblatex**: Modern bibliography management (replacing natbib)
  - Use with biber backend: `\usepackage[backend=biber,style=alphabetic]{biblatex}`
  - Better control over citation formatting

**Very useful**:
- **autonum**: Automatically numbers only equations that are referenced
  - Lets you use numbered equations everywhere
  - Only actually numbered in output if cited with `\ref` or `\cref`
  - Cleaner output without manual `equation*` vs `equation` decisions

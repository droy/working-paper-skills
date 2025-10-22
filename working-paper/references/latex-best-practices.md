# LaTeX Best Practices for Working Papers

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

### Avoiding Common Pitfalls

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

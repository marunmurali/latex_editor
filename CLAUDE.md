# Working on LaTeX papers in this workspace

Each subfolder here is a separate paper, usually an Overleaf project with its
own git repo. Co-authors edit the same files in the Overleaf web editor.

## Ground rules

- Never invent citations, references, numbers, or results. If a claim needs a
  source that isn't in the `.bib`, say so and leave a `% TODO: cite` comment.
- Don't rename or remove `\label`, `\ref`, `\cite`, `\eqref` keys or bib
  entries unless asked. Other files and co-authors depend on them.
- Keep edits local to what was asked. Don't reflow or rewrap paragraphs you
  didn't change: co-authors review diffs on Overleaf, and whitespace churn
  hides the real change.
- Preserve the author's voice and terminology. Suggest, don't rewrite, unless
  asked for a rewrite.
- Don't add packages or change the preamble/document class without asking;
  venue templates are strict.

## Checking your work

- After editing, compile with `latexmk -pdf -interaction=nonstopmode <root>.tex`
  from the paper folder and fix any new errors, undefined references, or
  overfull boxes you introduced.
- The root file is the one with `\documentclass`; if unsure, look for
  `% !TEX root` comments or ask.

## Git

- Never `git push` to the paper remote unless explicitly asked: it publishes
  straight to Overleaf for all co-authors.
- Never force-push. Overleaf has a single branch.

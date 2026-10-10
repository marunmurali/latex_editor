# latex_editor

**Write your Overleaf papers locally in VS Code, with a full TeX Live and an AI writing assistant in the same window.**

[![Open in Dev Containers](https://img.shields.io/static/v1?label=Dev%20Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/marunmurali/latex_editor)
![TeX Live](https://img.shields.io/badge/TeX%20Live-full-008080?logo=latex)
![Claude Code](https://img.shields.io/badge/Claude%20Code-included-d97757)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Overleaf is great for collaborating, but not for long editing sessions: no
local search across files, no real editor, no way to let an AI assistant read
the whole paper and compile it. This repository is a ready-made
[dev container](https://containers.dev) that fixes that. Clone your Overleaf
project into it, open it in VS Code, and you get:

- **Full TeX Live** (`texlive-full` + `latexmk`), so every package your
  co-authors use on Overleaf also compiles locally.
- **LaTeX Workshop**: build on save, PDF preview in a tab, SyncTeX jumps
  between source and PDF, error/warning panel.
- **Claude Code** (CLI and VS Code extension): ask for edits, tightening,
  consistency checks, or reviewer-style feedback across the whole paper. It
  can run `latexmk` itself to check that its changes still compile.
- **Two-way sync with Overleaf** through Overleaf's Git integration: your
  co-authors keep using the web editor, you `git pull` / `git push`.

Nothing about your paper is stored in this repository. The container is the
only thing tracked here; your Overleaf project lives in its own nested Git
repo and is ignored (see [How it works](#how-it-works)).

## Quick start

**You need:** Docker, VS Code with the
[Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
extension, and an Overleaf plan with
[Git integration](https://www.overleaf.com/learn/how-to/Git_integration).

1. **Clone this repo and open it in the container**

   ```sh
   git clone https://github.com/marunmurali/latex_editor.git
   code latex_editor
   ```

   When VS Code asks, choose **Reopen in Container** (or run
   *Dev Containers: Reopen in Container* from the command palette). The first
   build downloads TeX Live and takes a while; later starts are instant.

2. **Clone your Overleaf project inside it**

   In Overleaf, open your project, then *Menu → Integrations → Git* to get
   the clone URL, and create a token under *Account Settings → Git
   integration*. In the container terminal:

   ```sh
   git clone https://git.overleaf.com/<project-id> my-paper
   ```

   Username is `git`, password is the token. To avoid retyping it:
   `git config --global credential.helper 'cache --timeout=86400'`.

3. **Write**

   Open `my-paper/main.tex` (or whatever your root file is) and save: LaTeX
   Workshop builds it and opens the PDF in a tab. Open the Claude Code panel
   (or run `claude` in the terminal) and start asking.

4. **Sync with your co-authors**

   ```sh
   cd my-paper
   git pull            # before you start, always
   # ... edit ...
   git add -A && git commit -m "Tighten related work"
   git push            # shows up on Overleaf within seconds
   ```

## Working with an AI assistant on a paper

Claude Code reads [`CLAUDE.md`](CLAUDE.md) at the root of this repo
automatically, for every project you clone under it. It holds house rules for
paper editing: never invent citations, keep `\label`/`\ref`/`\cite` keys
intact, make small diffs your co-authors can review on Overleaf, and compile
after every change. Edit it to match your field and style.

Prompts that work well:

- *"Read the whole paper and list inconsistencies in notation between
  sections 2 and 4."*
- *"Tighten the abstract to 150 words without dropping any claim."*
- *"Act as reviewer 2 for the experiments section. Be specific."*
- *"Fix every overfull hbox in the last build log."*
- *"Check that every figure and table is referenced in the text."*

Put paper-specific notes (venue, page limit, target audience, notation
conventions) in a `CLAUDE.md` inside your paper folder. It is loaded on top of
the root one. If you don't want it on Overleaf, add it to the paper's
`.gitignore`.

## How it works

```
latex_editor/              ← this repo (tracks only the container + docs)
├── .devcontainer/         ← Ubuntu 24.04 + texlive-full + Node 22 + Claude Code
├── CLAUDE.md              ← writing rules for the AI assistant
├── my-paper/              ← your Overleaf project, its own git repo (ignored here)
└── another-paper/         ← as many as you like
```

The [`.gitignore`](.gitignore) ignores everything by default and only
whitelists the container files and docs. So you can clone any number of
Overleaf projects next to each other without them ever being committed to
this repository, and pushing here never leaks a draft.

## Tips

- **Keep build files off Overleaf.** LaTeX Workshop writes `.aux`, `.log`,
  `.synctex.gz`, etc. next to your sources. Add a `.gitignore` to your
  paper repo so they don't get pushed:

  ```gitignore
  *.aux
  *.bbl
  *.blg
  *.fdb_latexmk
  *.fls
  *.log
  *.out
  *.synctex.gz
  *.toc
  *.pdf
  ```

  (Drop the `*.pdf` line if your project contains PDF figures.)
- **Overleaf has a single branch.** Pull before editing and push often;
  merge conflicts with someone typing on the web are no fun.
- **Root file.** If LaTeX Workshop builds the wrong file, add
  `% !TEX root = main.tex` at the top of the included files.
- **Not on Overleaf?** Any Git remote works the same way (GitHub, GitLab,
  a plain local folder).

## License

[MIT](LICENSE)

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Personal homebrewing logbook ("Quaderno di birrificazione", brewery "Bagna Naso") written in Italian as a LaTeX document. There is no application code. `docs/` holds reference material (BJCP guidelines, yeast charts, recipe sources); `quaderno/` holds the book source.

## Build

No build script, Makefile, or CI is committed. LaTeX is not installed on the machine where this was written, so the commands below are the standard ones for this document, not verified against the repo:

- Compile from `quaderno/` (the `\input` paths are relative to it): `latexmk -pdf main.tex`. `quaderno/.latexmkrc` sets `$out_dir = 'build'` and `$jobname = 'quaderno'`, so the output is `quaderno/build/quaderno.pdf`; only that PDF is tracked, the other files in `build/` are git-ignored. Without latexmk, run `pdflatex -output-directory=build -jobname=quaderno main.tex` twice so the table of contents resolves.
- Packages required: babel `italian`, siunitx (with `DeclareSIUnit` for `\pt`, `\vol`, `\cell`, `\IBU`), booktabs, tabularx, caption, float, amsmath.

## Structure of the book

`quaderno/main.tex` is the only entry point and pulls in everything with `\input`:

- `formulario.tex`: reference formulas and conventions (density, gravity points, malt, hops, yeast, water, priming). Its "Convenzioni" block defines units and the density-correction convention; new calculations should follow it.
- `impianto.tex`: equipment and current brewhouse parameters, plus a "Storico" table that is meant to be updated after every brew.
- `cotte/cottaNN.tex`: one brew per file. `main.tex` has an `\input` line per cotta, and each file starts with `\section{Cotta #N --- Style}` followed by target/result tables, recipe (malt, hops, water, yeast), and notes.

Adding a brew means copying `quaderno/template/cotta.tex` (the brew template, not compiled by `main.tex`) to `cotte/cottaNN.tex`, filling the `<...>` placeholders and the red "Da definire" fields, and adding a `\clearpage` plus `\input{cotte/cottaNN}` in `main.tex`. Also update the parameter history in `impianto.tex` when the equipment or measurements change.

## Conventions to keep

- Content is in Italian. Keep new prose, headings, and table labels in Italian.
- Density values are relative and dimensionless (`1,048`), with the decimal comma (`siunitx` is set to `output-decimal-marker = {,}`). Write numbers with commas in prose too.
- Efficiencies and acid alpha fractions are stored as fractions (`0,78`, not `78%`), per `formulario.tex`.
- Measured values and targets sit side by side in each brew's "Obiettivi e risultati" table. Keep that layout.
- Brews are written as `.tex` only. Don't reintroduce the old Markdown brew notes.

## Git workflow

- Work directly on `main`. Don't create feature branches unless the user asks for one.
- Commit and push only when the user asks.

# LaTeX report template

Based on template by Erik Hjelmås.

[example.pdf](example.pdf) shows a report produced with this template.

## Setup

A LaTeX installation that is able to compile this report:

```bash
apt install texlive-latex-extra
apt install python-pygments
```

The script `./c.sh` produces the pdf. I use the following mapping to invoke the script from inside Vim:

```vim
nnoremap ø :!./c.sh<cr>
```

This template is fully compatible with [Overleaf](https://www.overleaf.com/). Just remember to set the main tex file to `report.tex`.

## Usage

* The main document is `report.tex`. This is where the actual content is written.
* If you want to include additional packages you should put them in `preamble.tex`.
* Put images in `images/`. Use the picture in the template as a reference for how to use images.
* Put references in `report.bib`. Currently the template is set up to print all references, not just the ones cited with `\cite{...}`. To change this behavior remove `\nocite{*}` from `report.tex`.
* The `.gitignore` excludes all build artifacts from version control.

# TODO

- [ ] Add other types of citations.
- [ ] Write Makefile for repo.
- [ ] Add example of code being included from a file.

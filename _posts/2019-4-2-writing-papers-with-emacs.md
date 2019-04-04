---
layout: post
title: "Writing Papers with Emacs"
---
## The Search for Better Paper Writing Software

I recently experimented by writing some papers with Emacs. I usually
use Google Docs but it lacks a few features that I wanted from my
writing software. I use Vim keybindings all the time and Google Docs
doesn't have any good Vim emulation. Additionally, I always disliked
having to input formulas in Google Docs and wanted Latex support.

Enter Emacs. After doing a bit of research and installing several
packages I have found Emacs to be a great tool for writing papers
quickly and efficiently. Here are the modes I use.

### Auto-Fill Mode

This mode allows you to wrap text in paragraphs automatically. When
you type past the end of the line it will place the next word on a new
line just like a standard word processor. In combination with M-q for
reformatting paragraphs, this mode makes formatting a breeze.

### Flyspell Mode

Flyspell mode setups up a spell checker for Emacs. It will underline
incorrect spellings for you and allows you to easily select correct
spellings. My only complaint is that the grammar check does not seem
as comprehensive as the one in Google Docs, but it gets the job done.

### Wc Mode

This mode is super simple, it just gives you the current word count
for the file.

## Writer Mode

Since I was enabling these pretty frequently I decided to create a
minor mode for Emacs that would enable all three of them
simultaneously. Here is the Elisp code:

```
;; create minor mode for writing
(define-minor-mode writer-mode
  "Enables flyspell spellchecker, and autofill-mode for text wrapping"
  :lighter "write mode"
  (auto-fill-mode)
  (flyspell-mode)
  (wc-mode))
```

## Org Ref

The most impressive package in the writing setup is org-ref. Org-ref
is a system for managing your references and it is super
useful. Essentially you create a bibliography file that holds all the
Bibtex citations for your references. Org-ref can automatically
download these from CrossRef if you provide a DOI (digital object
identifier). Most academic papers will have one of these, making
citations a breeze. Org-ref also supports hotkeys for inserting
citations and exports to a nicely formatted PDF file. This package has
saved me a ton of time compared to my old workflow with references.

Here is the Elisp code for setting up Org-ref.

```
(require 'org-ref)

(setq org-latex-pdf-process (list "latexmk -shell-escape -bibtex -f -pdf %f"))

(setq reftex-default-bibliography '("~/Documents/papers/references.bib"))

;; see org-ref for use of these variables
(setq org-ref-bibliography-notes "~/Documents/papers/notes.org"
      org-ref-default-bibliography '("~/Documents/papers/references.bib")
      org-ref-pdf-directory "~/Documents/papers/bibtex-pdfs/")
```

## Workflow

Perhaps my favorite part about using Emacs for writing is the
improved workflow. I keep two buffers open, one with the main text and
one with an Org-mode outline. The outline allows me to take notes as I
type and think of new ideas. I frequently create TODO items at the
bottom of the file so that I can keep track of changes I'd like to
make. Switching between the buffers or pulling up my bibliography file
to open a link is super fast as well. I can also check my papers into
version control and keep them in private remote repos so I have
backups.

Ultimately, if you're looking for some software to replace the usual
Google Docs workflow I'd take a good look at Emacs!

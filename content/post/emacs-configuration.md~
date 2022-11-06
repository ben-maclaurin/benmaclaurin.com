+++
title = "Emacs config"
date = 2022-11-04T20:20:00+00:00
draft = false
+++

<div class="ox-hugo-toc toc">

<div class="heading">Table of Contents</div>

- [Setup Processes](#setup-processes)
    - [Archives](#archives)
    - [use-package](#use-package)
- [Core Modifications](#core-modifications)
    - [Display](#display)
    - [Meta key](#meta-key)
    - [Font](#font)
    - [Open this file](#open-this-file)
    - [Org](#org)
    - [Movement mnemonics](#movement-mnemonics)
    - [Visual line mode](#visual-line-mode)
- [Packages](#packages)
    - [tree-sitter](#tree-sitter)
    - [rust-mode](#rust-mode)
    - [ef-themes](#ef-themes)
    - [ox-hugo](#ox-hugo)
    - [magit](#magit)
    - [avy](#avy)
    - [ivy](#ivy)
    - [elfeed](#elfeed)
    - [embark](#embark)
    - [which-key](#which-key)
    - [org-roam](#org-roam)
    - [eglot](#eglot)
    - [org-bullets](#org-bullets)

</div>
<!--endtoc-->

This is a [literate programming](https://en.wikipedia.org/wiki/Literate_programming) page, documenting my GNU Emacs
configuration. `org-babel-tangle` is used to output the resulting
`init.el` file.


## Setup Processes {#setup-processes}

Tasks and operations that are required for the rest of the expressions
to run.


### Archives {#archives}

The following lines initialise the package archives.

```lisp
(require 'package)

(setq package-archives '(("melpa" . "https://melpa.org/packages/")
			 ("org" . "https://orgmode.org/elpa/")
			 ("elpa" . "https://elpa.gnu.org/packages/")))

(package-initialize)
(unless package-archive-contents
  (package-refresh-contents))
```


### use-package {#use-package}

Less verbose package definitions and management...

```lisp
(unless (package-installed-p 'use-package)
  (package-install 'use-package))

(require 'use-package)
(setq use-package-always-ensure t)

(use-package command-log-mode)
```


## Core Modifications {#core-modifications}

A series of QOL changes I have made to the default distribution.


### Display {#display}

Reduce clutter and noise in the default Emacs GUI (e.g. hides scroll
bar and menu bar).

```lisp
(setq inhibit-startup-message t)
(scroll-bar-mode -1)
(tool-bar-mode -1)
(menu-bar-mode -1)
```


### Meta key {#meta-key}

Remap the Emacs meta (M) modifier to the macOS command key for
improved ergonomics and comfort.

```lisp
(setq mac-command-modifier 'meta)
```


### Font {#font}

Set the default font size and face. I use Prot Stavrou's Iosevka Comfy.

```lisp
(set-face-attribute 'default nil :font "Iosevka Comfy" :height 195)
```


### Open this file {#open-this-file}

Keybinding to enable swift modification of this file.

```lisp
(global-set-key (kbd "C-x .") (lambda () (interactive) (find-file "~/Developer/ben-maclaurin.github.io/content-org/all-posts.org")))
```

Once edits have been made, `org-babel-tangle` can be executed with
`C-c C-v t`, followed by `C-x r .` to reload `init.el`:

```lisp
(global-set-key (kbd "C-x r .") (lambda () (interactive) (load-file "~/.emacs.d/init.el")))
```


### Org {#org}

A set of configurations extending the [org major mode](https://orgmode.org/).


#### org-capture-templates {#org-capture-templates}

`org-capture` is a helpful utility which enables the quick collation
of thoughts/ideas/tasks (and their contexts).

I have specified the following templates and keybindings:


#### org-agenda {#org-agenda}

Keybinding for org-agenda mode:

```lisp
(global-set-key (kbd "C-c a") (lambda () (interactive) (org-agenda)))
```


### Movement mnemonics {#movement-mnemonics}

Two motion mnemonics inspired by `C-n` and `C-p` which jump eight
lines (plus or minus depending on direction):

```lisp
(global-set-key (kbd "M-n") (lambda () (interactive) (next-line 8)))
(global-set-key (kbd "M-p") (lambda () (interactive) (previous-line 8)))
```


### Visual line mode {#visual-line-mode}

Keybinding to toggle visual-line-mode for buffer wrapping:

```lisp
(global-set-key (kbd "C-x v l") (lambda () (interactive) (visual-line-mode 'toggle)))
```


## Packages {#packages}

External packages I have installed.


### tree-sitter {#tree-sitter}

An incremental tree parsing package that provides syntax
highlighting. The lines below install `tree-sitter` and enable the
mode globally.

```lisp
(global-tree-sitter-mode)
```


### rust-mode {#rust-mode}

Major mode support for the Rust programming language.

```lisp
(use-package rust-mode
    :config
  (require 'rust-mode))
```


### ef-themes {#ef-themes}

A beautiful and accessible collection of themes by Prot Stavrou.

```lisp
(use-package ef-themes
    :config
  (load-theme 'ef-frost))
```


### ox-hugo {#ox-hugo}

`ox-hugo` provides org export support for Hugo-compatible markdown (it
powers this blog).

```lisp
(use-package ox-hugo
    :config
  (with-eval-after-load 'ox
    (require 'ox-hugo)))
```


### magit {#magit}

`magit` is an interface for Git. `C-x m` is bound to `magit-status`
for ease-of-access:

```lisp
(use-package magit
    :config
  (global-set-key (kbd "C-x m") 'magit-status))
```


### avy {#avy}

This package uses char-based decision trees for optimal buffer
navigation. `C-;` is bound to `avy-goto-line`:

```lisp
(use-package avy
    :config
  (global-set-key (kbd "C-;") 'avy-goto-char))
```


### ivy {#ivy}

An advanced completion mechanism. Includes helpful prompts for
commands, dired, swiper and more...

```lisp
(use-package counsel)

(use-package ivy
    :config
  (ivy-mode)
  (setq ivy-use-virtual-buffers t)
  (setq enable-recursive-minibuffers t)
  ;; enable this if you want `swiper' to use it
  ;; (setq search-default-mode #'char-fold-to-regexp)
  (global-set-key "\C-s" 'swiper)
  (global-set-key (kbd "C-c C-r") 'ivy-resume)
  (global-set-key (kbd "<f6>") 'ivy-resume)
  (global-set-key (kbd "M-x") 'counsel-M-x)
  (global-set-key (kbd "C-x C-f") 'counsel-find-file)
  (global-set-key (kbd "<f1> f") 'counsel-describe-function)
  (global-set-key (kbd "<f1> v") 'counsel-describe-variable)
  (global-set-key (kbd "<f1> o") 'counsel-describe-symbol)
  (global-set-key (kbd "<f1> l") 'counsel-find-library)
  (global-set-key (kbd "<f2> i") 'counsel-info-lookup-symbol)
  (global-set-key (kbd "<f2> u") 'counsel-unicode-char)
  (global-set-key (kbd "C-c g") 'counsel-git)
  (global-set-key (kbd "C-c j") 'counsel-git-grep)
  (global-set-key (kbd "C-c k") 'counsel-ag)
  (global-set-key (kbd "C-x l") 'counsel-locate)
  (global-set-key (kbd "C-S-o") 'counsel-rhythmbox)
  (define-key minibuffer-local-map (kbd "C-r") 'counsel-minibuffer-history))
```


### elfeed {#elfeed}

Serves RSS feeds. The following lines define my subscription list:

```lisp
(use-package elfeed
    :config
  (setq elfeed-feeds
	'("https://ben-maclaurin.github.io/index.xml"
	  "https://ciechanow.ski/atom.xml"
	  "https://fasterthanli.me/index.xml"
	  "https://hnrss.org/frontpage")))
```

`C-x w` launches elfeed:

```lisp
(global-set-key (kbd "C-x w") 'elfeed)
```


### embark {#embark}

Enables a set of quick access commands around the point:

```lisp
(use-package embark
    :config
  (global-set-key (kbd "C-.") (lambda () (interactive) (embark-act))))
```


### which-key {#which-key}

A minor mode that provides prompts and tips around an incomplete key
sequence:

```lisp
(use-package which-key
    :config
  (require 'which-key)
  (which-key-mode))
```


### org-roam {#org-roam}

Org-based knowledge management system.

```lisp
(use-package org-roam
    :ensure t
    :custom
    (org-roam-directory (file-truename "~/org/roam"))
    :bind (("C-c n l" . org-roam-buffer-toggle)
	   ("C-c n f" . org-roam-node-find)
	   ("C-c n g" . org-roam-graph)
	   ("C-c n i" . org-roam-node-insert)
	   ("C-c n c" . org-roam-capture)
	   ("C-c n j" . org-roam-dailies-capture-today))
    :config
    (org-roam-setup))
```


### eglot {#eglot}

An LSP client... tries to match a locally-installed LSP with the current buffer:

```lisp
(use-package eglot)
```


### org-bullets {#org-bullets}

Renders nice bullet point UTF characters to replace org headline stars:

```lisp
(use-package org-bullets
    :config
  (require 'org-bullets)
  (add-hook 'org-mode-hook (lambda () (org-bullets-mode 1))))
```

+++
title = "Emacs configuration"
date = 2022-10-29T18:07:00+01:00
draft = false
+++

This page documents the contents of my init.el file and associated packages for [GNU Emacs](https://www.gnu.org/software/emacs/).


## Vanilla mods {#vanilla-mods}

This section documents the modifications I have made to the vanilla GNU Emacs distribution.


### Archives {#archives}

These lines initialise the package archives.

```lisp
(setq package-archives '(("melpa" . "https://melpa.org/packages/")
			 ("org" . "https://orgmode.org/elpa/")
			 ("elpa" . "https://elpa.gnu.org/packages/")))

(package-initialize)
(unless package-archive-contents
 (package-refresh-contents))

(unless (package-installed-p 'use-package)
   (package-install 'use-package))
```


### Interface {#interface}

The following lines make several modifications to the default Emacs interface. These are designed to make the display less cluttered with more room for the buffer.

```lisp
(setq inhibit-startup-message t)
(scroll-bar-mode -1)
(tool-bar-mode -1)
(menu-bar-mode -1)
```


### Typeface {#typeface}

This line sets the editor font size and face. I use [Iosevka Comfy](https://gitlab.com/protesilaos/iosevka-comfy) by Protesilaos Stavrou.

```lisp
(set-face-attribute 'default nil :font "Iosevka Comfy" :height 195)
```


### Meta key {#meta-key}

The following line remaps the Emacs meta `M` modifier to the slightly more erognomic macOS command key.

```lisp
(setq mac-command-modifier 'meta)
```


### Quickly open Emacs config {#quickly-open-emacs-config}

This line opens the `emacs.d` directory with `C-x .`

```lisp
(global-set-key (kbd "C-x .") (lambda () (interactive) (dired "~/.emacs.d/")))
```


### Cursor type {#cursor-type}

Change the cursor type to bar, as I prefer it in non-modal editors.

```lisp
(setq-default cursor-type 'bar)
```


## Packages {#packages}

External packages which I have installed and customised.


### evil {#evil}

Enable evil mode, which provides Vim keybinding support for Emacs:

```lisp
(require 'evil)
(evil-mode 1)
```

This line maps `C-u` to `PageUp` in evil mode:

```nil
(setq evil-want-C-u-scroll t)
```

Remap `C-j` and `C-k` to `PageUp` and `PageDn` respectively (via evil):

```lisp
(global-set-key (kbd "C-j") (lambda () (interactive) (evil-scroll-down 0)))
(global-set-key (kbd "C-k") (lambda () (interactive) (evil-scroll-up 0)))
```

I use evil exclusively for text editing. For any other arbitrary buffer I use the default Emacs keybindings. To quickly toggle between the modes I use `C-z`:

```lisp
(global-set-key (kbd "C-z" (lambda () (interactive) (evil-mode))))
```


### key-chord {#key-chord}

I use the key-chord package to remap `jk` key presses in quick succession to escape:

```lisp
(setq key-chord-two-keys-delay 0.3)
(key-chord-define evil-insert-state-map "jk" 'evil-normal-state)
(key-chord-mode 1)
```


### ivy {#ivy}

Ivy is an advance and extensive completion mechanism. Out of the box it provides helpful completions for commands, dired, swiper, buffers and more...

```lisp
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
(define-key minibuffer-local-map (kbd "C-r") 'counsel-minibuffer-history)
```


### avy {#avy}

avy allows you to jump around text. When a single char is entered, avy highlights candidates.

`C-;` is bound to `avy-goto-line` to enable a shortcut for this functionality:

```lisp
(global-set-key (kbd "C-;") 'avy-goto-char)
```

A convenient key binding for line jumping in avy...

```lisp
(global-set-key (kbd "C-'") 'avy-goto-line)
```


### magit {#magit}

The [magit](https://magit.vc/) package is an interface for Git inside Emacs. I use it for all Git-related operations.

I have bound `C-x m` to `magit-status` for quicker access to Magit:

```lisp
(global-set-key (kbd "C-x m") 'magit-status)
```


### eglot {#eglot}

[eglot](https://github.com/joaotavora/eglot) is an Emacs client for LSP (Language Server Protocol) servers. When `M-x eglot` is executed inside a file, Eglot attempts to find the associated LSP and run it.


### tree-sitter-mode {#tree-sitter-mode}

Enables `tree-sitter-mode` globally. Treesitter is an incremental parsing library.

```lisp
(global-tree-sitter-mode)
```


### rust-mode {#rust-mode}

Instantiates a major mode for the [Rust programming language](https://www.rust-lang.org/).

```lisp
(require 'rust-mode)
```


### ef-themes {#ef-themes}

I use the accessible `ef-themes` collection by [Protesilaos Stavrou](https://protesilaos.com/).

```lisp
(load-theme 'ef-summer)
```


### ox-hugo {#ox-hugo}

`ox-hugo` provides a convenient way to export Org files to Hugo-compatible markdown. It is used in the generation of [my personal blog](https://ben-maclaurin.github.io/).

```lisp
(with-eval-after-load 'ox
(require 'ox-hugo))
```


### linum-relative {#linum-relative}

This package provides relative line numbers globally and plays well with evil.

```lisp
(require 'linum-relative)
(linum-on)
```

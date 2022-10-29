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


## Packages {#packages}

External packages which I have installed and customised.


### magit {#magit}

The [magit](https://magit.vc/) package is an interface for Git inside Emacs. I use it for all Git-related operations.


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
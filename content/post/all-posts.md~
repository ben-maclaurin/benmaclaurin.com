+++
draft = false
+++

## <span class="org-todo done DONE">DONE</span> Emacs config {#emacs-configuration}

This is a [literate programming](https://en.wikipedia.org/wiki/Literate_programming) page, documenting my GNU Emacs
configuration. `org-babel-tangle` is used to output the resulting
`init.el` file.


### Setup Processes {#setup-processes}

Tasks and operations that are required for the rest of the expressions
to run.


#### Archives {#archives}

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


#### use-package {#use-package}

Less verbose package definitions and management...

```lisp
(unless (package-installed-p 'use-package)
  (package-install 'use-package))

(require 'use-package)
(setq use-package-always-ensure t)

(use-package command-log-mode)
```


### Core Modifications {#core-modifications}

A series of QOL changes I have made to the default distribution.


#### Display {#display}

Reduce clutter and noise in the default Emacs GUI (e.g. hides scroll
bar and menu bar).

```lisp
(setq inhibit-startup-message t)
(scroll-bar-mode -1)
(tool-bar-mode -1)
(menu-bar-mode -1)
```


#### Meta key {#meta-key}

Remap the Emacs meta (M) modifier to the macOS command key for
improved ergonomics and comfort.

```lisp
(setq mac-command-modifier 'meta)
```


#### Font {#font}

Set the default font size and face. I use Prot Stavrou's Iosevka Comfy.

```lisp
(set-face-attribute 'default nil :font "Iosevka Comfy" :height 195)
```


#### Open this file {#open-this-file}

Keybinding to enable swift modification of this file.

```lisp
(global-set-key (kbd "C-x .") (lambda () (interactive) (find-file "~/Developer/ben-maclaurin.github.io/content-org/all-posts.org")))
```


#### Org {#org}

A set of configurations extending the [org major mode](https://orgmode.org/).

<!--list-separator-->

-  org-capture-templates

    `org-capture` is a helpful utility which enables the quick collation of thoughts/ideas/tasks (and their contexts).

    I have specified the following templates and keybindings:

<!--list-separator-->

-  org-agenda

    Keybinding for org-agenda mode:

    ```lisp
    (global-set-key (kbd "C-c a") (lambda () (interactive) (org-agenda)))
    ```


#### Movement mnemonics {#movement-mnemonics}

Two motion mnemonics inspired by `C-n` and `C-p` which jump eight lines (plus or minus depending on direction):

```lisp
(global-set-key (kbd "M-n") (lambda () (interactive) (next-line 8)))
(global-set-key (kbd "M-p") (lambda () (interactive) (previous-line 8)))
```


#### Visual line mode {#visual-line-mode}

Keybinding to toggle visual-line-mode for buffer wrapping:

```lisp
(global-set-key (kbd "C-x v l") (lambda () (interactive) (visual-line-mode 'toggle)))
```


#### Quit Emacs {#quit-emacs}

Quit Emacs in macOS style:

```lisp
(global-set-key (kbd "M-q") (lambda () (interactive) (save-buffers-kill-emacs)))
```


### Packages {#packages}

External packages I have installed.


#### tree-sitter {#tree-sitter}

An incremental tree parsing package that provides syntax
highlighting. The lines below install `tree-sitter` and enable the
mode globally.

```lisp
(global-tree-sitter-mode)
```


#### rust-mode {#rust-mode}

Major mode support for the Rust programming language.

```lisp
(use-package rust-mode
    :config
  (require 'rust-mode))
```


#### ef-themes {#ef-themes}

A beautiful and accessible collection of themes by Prot Stavrou.

```lisp
(use-package ef-themes
    :config
  (load-theme 'ef-frost))
```


#### ox-hugo {#ox-hugo}

`ox-hugo` provides org export support for Hugo-compatible markdown (it powers this blog).

```lisp
(use-package ox-hugo
    :config
  (with-eval-after-load 'ox
    (require 'ox-hugo)))
```


#### magit {#magit}

`magit` is an interface for Git. `C-x m` is bound to `magit-status` for ease-of-access:

```lisp
(use-package magit
    :config
  (global-set-key (kbd "C-x m") 'magit-status))
```


#### avy {#avy}

This package uses char-based decision trees for optimal buffer navigation. `C-;` is bound to `avy-goto-line`:

```lisp
(use-package avy
    :config
  (global-set-key (kbd "C-;") 'avy-goto-char))
```


#### ivy {#ivy}

An advanced completion mechanism. Includes helpful prompts for commands, dired, swiper and more...

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


#### elfeed {#elfeed}

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


#### embark {#embark}

Enables a set of quick access commands around the point:

```lisp
(use-package embark
    :config
  (global-set-key (kbd "C-.") (lambda () (interactive) (embark-act))))
```


#### which-key {#which-key}

A minor mode that provides prompts and tips around an incomplete key sequence:

```lisp
(use-package which-key
    :config
  (require 'which-key)
  (which-key-mode))
```


## <span class="org-todo done DONE">DONE</span> Fast motions with avy and evil {#fast-motions-with-avy-and-evil}

I have discovered a beautifully versatile motion technique via avy
with evil mode. Both packages are admirable in their own right. avy
brings [char-based decision tree jumping](https://github.com/abo-abo/avy) to the buffer, and evil is [an
extensible vi layer](https://github.com/emacs-evil/evil) for Emacs. Combining them, however, feels _magic_.


### How it works {#how-it-works}

Below is some elisp from my init file:

```lisp
(global-set-key (kbd "C-x .") (lambda () (interactive) (dired "~/.emacs.d/")))
```

What if I want to delete from the beginning of the line to the end of
`(interactive)`?

Maybe `df)`? No. That would give me:

```lisp
(lambda () (interactive) (dired "~/.emacs.d/")))
```

Instead, I could count the occurences of `)` up until the termination
point (there are 3 in this case). `d3f)` would give me want I want:

```lisp
(dired "~/.emacs.d/")))
```

With avy, this becomes delightfully simple. We no longer need to count
the occurences of a character. Instead, the following key combination
will work:

1.  `d` for delete
2.  `C-;` which is my custom keybinding for `avy-goto-char`
3.  `<char>` where `<char>` is the target char
4.  `<avy char>` which is the character in the visual decision tree
    which represents our target

Here is a screenshot of step 3:

{{< figure src="/ox-hugo/avy-demo.png" >}}

In the example screenshot I would press `d` as this is the character
that corresponds to the termination point.

So, in _approximately_ the same number of actions we achieved
comparable behaviour without the additional cognitive load of having
to count character occurences.

Before I switched to Emacs, I tried to set this up with [leap](https://github.com/ggandor/leap.nvim) in Neovim
but couldn't get it to work properly.


### Watch the video {#watch-the-video}

<iframe width="100%" height="315" src="https://www.youtube.com/embed/FiLgoZgaqYo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


## Rust: Trait Objects and the Newtype Pattern {#rust-trait-objects-and-the-newtype-pattern}


### Introduction {#introduction}

I built a simple Hacker News wrapper in Rust to learn about API design
principles. Along the way I also discovered the [Newtype pattern](https://rust-unofficial.github.io/patterns/patterns/behavioural/newtype.html) which
provides type safety and encapsulation. The following post is a
summary of learnings:

I set a small challenge of building a wrapper around the Hacker News
API. To keep the project simple and focus on the goal of studying
idiomatic Rust, I only aggregated the top stories (i.e. frontpage
matter) and ignored all other site categories (polls, jobs, etc.).


### Understanding the data structure {#understanding-the-data-structure}

-   Stories can have _many_ children
-   Comments can have _many_ children
-   Comments can have _only one_ parent

Both sotries and comments are "items". You may be able to spot a
simple rule here:

-   Stories are _always_ parents. Comments can be either parents _or_ children.


### Defining the item type {#defining-the-item-type}

First, I defined the item type.

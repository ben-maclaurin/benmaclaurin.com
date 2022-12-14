+++
draft = false
+++

## <span class="org-todo done DONE">DONE</span> About {#index}


### About {#about}

This is a test 2


## <span class="org-todo done DONE">DONE</span> Emacs config {#emacs-configuration}

This is a [literate programming](https://en.wikipedia.org/wiki/Literate_programming) page, documenting my GNU 'Emacs
configuration. `org-babel-tangle` is used to output the resulting
`init.el` file. The source of this file can be viewed [here](https://github.com/ben-maclaurin/ben-maclaurin.github.io/blob/main/content-org/all-posts.org#emacs-config).


### Setup Processes {#setup-processes}

Tasks and operations that are required for the rest of the expressions
to run.


#### Archives {#archives}

The following lines initialise the package archives.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; archives

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
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; use-package

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
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; display

(setq inhibit-startup-message t)
(scroll-bar-mode -1)
(tool-bar-mode -1)
(menu-bar-mode -1)
```


#### Meta key {#meta-key}

Remap the Emacs meta (M) modifier to the macOS command key for
improved ergonomics and comfort.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; meta key

(setq mac-command-modifier 'meta)
```


#### Font {#font}

Set the default font size and face.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; font

(set-face-attribute 'default nil :font "IBM Plex Mono" :height 165)
```


#### Open this file {#open-this-file}

Keybinding to enable swift modification of this file.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; open this file

(global-set-key (kbd "C-x .") (lambda () (interactive) (find-file "~/Developer/ben-maclaurin.github.io/content-org/all-posts.org")))
```

Once edits have been made, `org-babel-tangle` can be executed with
`C-c C-v t`, followed by `C-x r .` to reload `init.el`:

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; reload init file

(global-set-key (kbd "C-x r .") (lambda () (interactive) (load-file "~/.emacs.d/init.el")))
```


#### Org {#org}

A set of configurations extending the [org major mode](https://orgmode.org/).

<!--list-separator-->

-  Custom org mode fonts

    ```lisp
    (use-package mixed-pitch
        :hook
      ;; If you want it in all text modes:
      (text-mode . mixed-pitch-mode))

    (custom-theme-set-faces
     'user
     '(variable-pitch ((t (:family "Atkinson Hyperlegible" :height 180 :weight normal))))
     '(fixed-pitch ((t ( :family "IBM Plex Mono" :height 160)))))

    (add-hook 'org-mode-hook 'variable-pitch-mode)
    ```

<!--list-separator-->

-  Hide emphasis markers

    ```lisp
    (setq org-hide-emphasis-markers t)
    ```

<!--list-separator-->

-  org-capture-templates

    `org-capture` is a helpful utility which enables the quick collation
    of thoughts/ideas/tasks (and their contexts).

    I have specified the following templates and keybindings:

<!--list-separator-->

-  todo-keywords

    Modify the default to-do keywords

    ```lisp
    ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
    ;; org todo keywords

    (setq org-todo-keywords
        '((sequence "TODO" "IN PROGRESS" "|" "DONE" "DELEGATED")))
    ```

<!--list-separator-->

-  org-agenda

    Keybinding for org-agenda mode:

    ```lisp
    ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
    ;; org agenda mode

    (global-set-key (kbd "C-c a") (lambda () (interactive) (org-agenda)))
    ```


#### Movement mnemonics {#movement-mnemonics}

Two motion mnemonics inspired by `C-n` and `C-p` which jump eight
lines (plus or minus depending on direction):

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; movement mnemonics

(global-set-key (kbd "M-n") (lambda () (interactive) (next-line 8)))
(global-set-key (kbd "M-p") (lambda () (interactive) (previous-line 8)))
```


#### Visual line mode {#visual-line-mode}

Keybinding to toggle visual-line-mode for buffer wrapping:

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; visual line mode

(global-set-key (kbd "C-x v l") (lambda () (interactive) (visual-line-mode 'toggle)))
```


#### Org agenda files location {#org-agenda-files-location}

Set the location for agenda files:

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; org agenda files location

(setq org-agenda-files '("~/org/task.org"))
```


#### Line numbers {#line-numbers}

Enable relative line numbers in editors.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; line numbers

;;(global-display-line-numbers-mode)
```


#### Save place {#save-place}

Persist cursor locations across sessions.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; save place

(save-place-mode 1)
```


#### Allow hash key entry on macOS {#allow-hash-key-entry-on-macos}

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; allow hash key entry on macOS

(global-set-key (kbd "M-3") '(lambda () (interactive) (insert "#")))
```


### Packages {#packages}

External packages I have installed.


#### tree-sitter {#tree-sitter}

An incremental tree parsing package that provides syntax
highlighting. The lines below install `tree-sitter` and enable the
mode globally.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; tree-sitter

(global-tree-sitter-mode)
(add-hook 'tree-sitter-after-on-hook #'tree-sitter-hl-mode)
```


#### rust-mode {#rust-mode}

Major mode support for the Rust programming language.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; rust-mode

(use-package rust-mode
    :config
  (require 'rust-mode))
```


#### ef-themes {#ef-themes}

A beautiful and accessible collection of themes by Prot Stavrou.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; ef-themes

(use-package ef-themes :config (load-theme 'ef-spring))
```


#### ox-hugo {#ox-hugo}

`ox-hugo` provides org export support for Hugo-compatible markdown (it
powers this blog).

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; ox-hugo

(use-package ox-hugo
    :config
  (with-eval-after-load 'ox
    (require 'ox-hugo)))
```


#### magit {#magit}

`magit` is an interface for Git. `C-x m` is bound to `magit-status`
for ease-of-access:

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; magit

(use-package magit
    :config
  (global-set-key (kbd "C-x m") (lambda () (interactive) (split-window-right) (other-window-prefix) (magit-status))))
```


#### avy {#avy}

This package uses char-based decision trees for optimal buffer
navigation. `C-;` is bound to `avy-goto-char`:

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; avy

(use-package avy
    :config
  (global-set-key (kbd "C-;") 'avy-goto-char)
  (global-set-key (kbd "C-l") 'avy-goto-line))
```


#### swiper {#swiper}

Better search:

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; swiper

(use-package swiper)
(global-set-key "\C-s" 'swiper)
```


#### vertico {#vertico}

Vertico is a performant and minimalistic completion tool which extends the default Emacs UI. I use it as an Ivy replacement.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; vertico

(use-package vertico
    :init
  (vertico-mode)
  (setq vertico-count 20))

(use-package vertico-directory
    :after vertico
    :ensure nil
    :bind (:map vertico-map
		("RET" . vertico-directory-enter)
		("DEL" . vertico-directory-delete-char)
		("M-DEL" . vertico-directory-delete-word))
    :hook (rfn-eshadow-update-overlay . vertico-directory-tidy))

```


#### marginalia {#marginalia}

Provides rich descriptions next to minibuffer completions.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; marginalia

(use-package marginalia
    :init
  (marginalia-mode))
```


#### counsel {#counsel}

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; counsel

(use-package counsel)
```

```lisp
(global-set-key (kbd "C-q") nil)
(global-set-key (kbd "C-c g") 'counsel-git-grep)
(global-set-key (kbd "C-SPC") 'counsel-git)
(global-set-key (kbd "M-SPC") 'switch-to-buffer)
(global-set-key (kbd "C-x b") nil)
```


#### elfeed {#elfeed}

Serves RSS feeds. The following lines define my subscription list:

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; elfeed

(use-package elfeed
    :config
  (setq elfeed-feeds
	'("https://ben-maclaurin.github.io/index.xml"
	  "https://ciechanow.ski/atom.xml"
	  "https://fasterthanli.me/index.xml"
	  "https://hnrss.org/frontpage"
	  "https://nitter.net/hlissner/rss"
	  "https://nitter.net/karpathy/rss"
	  "https://nitter.net/aratramba/rss"
	  "https://nitter.net/ohhdanm/rss"
	  "https://lexfridman.com/feed/podcast/"
	  "https://nitter.net/ukutaht/rss"
	  "https://nitter.net/chris_mccord/rss"
	  "https://nitter.net/josevalim/rss"
	  "https://nitter.net/jonhoo/rss"
	  "https://nitter.net/rich_harris/rss")))
```

`C-x w` launches elfeed:

```lisp
(global-set-key (kbd "C-x w") 'elfeed)
```

Keybinding to update the feeds:

```lisp
(global-set-key (kbd "C-x u") 'elfeed-update)
```


#### which-key {#which-key}

A minor mode that provides prompts and tips around an incomplete key
sequence:

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; which-key

(use-package which-key
    :config
  (require 'which-key)
  (which-key-mode))
```


#### org-roam {#org-roam}

Org-based knowledge management system.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; org-roam

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


#### eglot {#eglot}

An LSP client... tries to match a locally-installed LSP with the current buffer:

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; eglot

(use-package eglot)
```


#### expand-region {#expand-region}

Increase a selection by a set of semantic units.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; expand-region

;; (use-package expand-region
;;     :bind ("C-" . 'er/expand-region))
```


#### company-mode {#company-mode}

A completions helper. Improves on the existing eglot completion mechanism:

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; company-mode

(use-package company
    :config
  (add-hook 'after-init-hook 'global-company-mode))
```


#### evil-mode {#evil-mode}

Vim emulation for Emacs.

```lisp
(unless (package-installed-p 'evil)
  (package-install 'evil))
(setq evil-want-C-u-scroll t)
(require 'evil)
(define-key evil-normal-state-map (kbd "C-.") nil)
(evil-mode 1)
```


#### key-chord {#key-chord}

Switch to normal mode by pressing `j` and `k` in quick succession.

```lisp
(use-package key-chord
    :config
  (setq key-chord-two-keys-delay 0.3)
  (key-chord-define evil-insert-state-map "jk" 'evil-normal-state)
  (key-chord-mode 1))
```


#### meow {#meow}

A modal editor.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; meow

(defun meow-setup ()
(setq meow-cheatsheet-layout meow-cheatsheet-layout-qwerty)
(meow-motion-overwrite-define-key
 '("j" . meow-next)
 '("k" . meow-prev)
 '("<escape>" . ignore))
(meow-leader-define-key
 ;; SPC j/k will run the original command in MOTION state.
 '("j" . "H-j")
 '("k" . "H-k")
 ;; Use SPC (0-9) for digit arguments.
 '("1" . meow-digit-argument)
 '("2" . meow-digit-argument)
 '("3" . meow-digit-argument)
 '("4" . meow-digit-argument)
 '("5" . meow-digit-argument)
 '("6" . meow-digit-argument)
 '("7" . meow-digit-argument)
 '("8" . meow-digit-argument)
 '("9" . meow-digit-argument)
 '("0" . meow-digit-argument)
 '("/" . meow-keypad-describe-key)
 '("?" . meow-cheatsheet))
(meow-normal-define-key
 '("0" . meow-expand-0)
 '("9" . meow-expand-9)
 '("8" . meow-expand-8)
 '("7" . meow-expand-7)
 '("6" . meow-expand-6)
 '("5" . meow-expand-5)
 '("4" . meow-expand-4)
 '("3" . meow-expand-3)
 '("2" . meow-expand-2)
 '("1" . meow-expand-1)
 '("-" . negative-argument)
 '(";" . meow-reverse)
 '("," . meow-inner-of-thing)
 ;;'("." . meow-bounds-of-thing)
 '("." . er/expand-region)
 ;;'("[" . meow-beginning-of-thing)
 ;;'("]" . meow-end-of-thing)
 '("a" . meow-append)
 '("A" . meow-open-below)
 '("b" . meow-back-word)
 '("B" . meow-back-symbol)
 '("c" . meow-change)
 '("x" . meow-delete)
 '("X" . meow-backward-delete)
 '("w" . meow-next-word)
 '("W" . meow-next-symbol)
 '("f" . meow-find)
 '("g" . meow-cancel-selection)
 '("G" . meow-grab)
 '("h" . meow-left)
 '("H" . meow-left-expand)
 '("i" . meow-insert)
 '("I" . meow-open-above)
 '("j" . meow-next)
 '("J" . meow-next-expand)
 '("k" . meow-prev)
 '("K" . meow-prev-expand)
 '("l" . meow-right)
 '("L" . meow-right-expand)
 '("m" . meow-join)
 '("n" . meow-search)
 '("o" . meow-open-below)
 '("O" . meow-open-above)
 '("p" . meow-yank)
 '("q" . meow-quit)
 '("Q" . meow-goto-line)
 '("r" . meow-replace)
 '("R" . meow-swap-grab)
 '("d" . meow-kill)
 '("t" . meow-till)
 '("u" . meow-undo)
 '("U" . meow-undo-in-selection)
 '("/" . meow-bounds-of-thing)
 '("e" . meow-mark-word)
 '("E" . meow-mark-symbol)
 '("v" . meow-line)
 '("X" . meow-goto-line)
 '("y" . meow-save)
 '("Y" . meow-sync-grab)
 '("z" . meow-pop-selection)
 '("'" . repeat)
 '("s" . meow-hyper-mode)
 '("\"" . meow-hyper-string)
 '("(" . meow-hyper-paren)
 '(")" . meow-hyper-paren)
 '("'" . meow-hyper-quote)
 '("{" . meow-hyper-curly)
 '("}" . meow-hyper-curly)
 '("[" . meow-hyper-bracket)
 '("]" . meow-hyper-bracket)
 '("<escape>" . ignore)))
```

```lisp
(use-package meow
    :config
  (require 'meow)
  (meow-setup)
  (meow-global-mode 1)
  (setq meow-expand-hint-remove-delay 2.0)
  (meow-setup-indicator))
```

Register a new inner bound for &lt;&gt; tags:

```lisp
(meow-thing-register 'tag '(pair ("<") (">")) '(pair ("<") (">")))

(add-to-list 'meow-char-thing-table '(?\( . round))
(add-to-list 'meow-char-thing-table '(?\) . round))
(add-to-list 'meow-char-thing-table '(?\" . string))

(add-to-list 'meow-char-thing-table '(?\{ . curly))
(add-to-list 'meow-char-thing-table '(?\} . curly))

(add-to-list 'meow-char-thing-table '(?\[ . square))
(add-to-list 'meow-char-thing-table '(?\] . square))
```

<!--list-separator-->

-  meow-hyper

    Create a _hyper_ mode for meow.

    ```lisp
    ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
    ;; meow-hyper

    (setq meow-hyper-keymap (make-keymap))
    (meow-define-state hyper
      "a hyper mode for meow insertions"
      :lighter " [H]"
      :keymap meow-hyper-keymap)

    (setq meow-cursor-type-hyper 'hollow)

    (meow-define-keys 'hyper
      '("<escape>" . meow-normal-mode)
      '("h" . meow-hyperhtml-mode))
    ```

    ```lisp
    (defun meow-hyper-string () (interactive)
           (if (and transient-mark-mode mark-active)
    	   (progn
    	     (goto-char (region-end))
    	     (insert "\"")
    	     (goto-char (region-beginning))
    	     (insert "\""))
    	   (insert "\"\"")
    	   (meow-normal-mode)))

    (defun meow-hyper-quote () (interactive)
           (if (and transient-mark-mode mark-active)
    	   (progn
    	     (goto-char (region-end))
    	     (insert "'")
    	     (goto-char (region-beginning))
    	     (insert "'"))
    	   (insert "'")
    	   (meow-normal-mode)))

    (defun meow-hyper-paren () (interactive)
           (if (and transient-mark-mode mark-active)
    	   (progn
    	     (goto-char (region-end))
    	     (insert ")")
    	     (goto-char (region-beginning))
    	     (insert "("))
    	   (insert "()")
    	   (meow-normal-mode)))

    (defun meow-hyper-curly () (interactive)
           (if (and transient-mark-mode mark-active)
    	   (progn
    	     (goto-char (region-end))
    	     (insert "}")
    	     (goto-char (region-beginning))
    	     (insert "{"))
    	   (insert "{}")
    	   (meow-normal-mode)))

    (defun meow-hyper-bracket () (interactive)
           (if (and transient-mark-mode mark-active)
    	   (progn
    	     (goto-char (region-end))
    	     (insert "]")
    	     (goto-char (region-beginning))
    	     (insert "["))
    	   (insert "[]")
    	   (meow-normal-mode)))

      (defun meow-hyper-tag () (interactive)
           (if (and transient-mark-mode mark-active)
    	   (progn
    	     (goto-char (region-end))
    	     (insert ">")
    	     (goto-char (region-beginning))
    	     (insert "<"))
    	   (insert "<>")
    	   (meow-normal-mode)))
    ```

    <!--list-separator-->

    -  meow-hyper-html

        A meow hyper mode specifically designed around HTML entry.

        ```lisp
        ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
        ;; meow-hyper-html

        (setq meow-hyperhtml-keymap (make-keymap))
        (meow-define-state hyperhtml
          "a hyper mode for meow insertions"
          :lighter " [HH]"
          :keymap meow-hyperhtml-keymap)

        (setq meow-cursor-type-hyperhtml 'hbar)

        (meow-define-keys 'hyperhtml
          '("<escape>" . meow-normal-mode)
          '("d" . meow-hyper-html-div-class)
          '("D" . meow-hyper-html-div)
          '("p" . meow-hyper-html-p-class)
          '("P" . meow-hyper-html-p))

        ```

        ```lisp
        (defun meow-hyper-html-div () (interactive)
               (if (and transient-mark-mode mark-active)
        	   (progn
        	     (goto-char (region-end))
        	     (insert "</div>")
        	     (goto-char (region-beginning))
        	     (insert "<div>") (meow-normal-mode))
        	   (insert "<div></div>")
        	   (meow-normal-mode)))

        (defun meow-hyper-html-div-class () (interactive)
               (if (and transient-mark-mode mark-active)
        	   (progn
        	     (goto-char (region-end))
        	     (insert "</div>")
        	     (goto-char (region-beginning))
        	     (insert "<div className=\"\">") (meow-normal-mode))
        	   (insert "<div className=\"\"></div>")
        	   (meow-normal-mode)))

        (defun meow-hyper-html-p () (interactive)
               (if (and transient-mark-mode mark-active)
        	   (progn
        	     (goto-char (region-end))
        	     (insert "</p>")
        	     (goto-char (region-beginning))
        	     (insert "<p>") (meow-normal-mode))
        	   (insert "<p></p>")
        	   (meow-normal-mode)))

        (defun meow-hyper-html-p-class () (interactive)
               (if (and transient-mark-mode mark-active)
        	   (progn
        	     (goto-char (region-end))
        	     (insert "</p>")
        	     (goto-char (region-beginning))
        	     (insert "<p className=\"\">") (meow-normal-mode))
        	   (insert "<p className=\"\"></p>")
        	   (meow-normal-mode)))
        ```


#### typescript-mode {#typescript-mode}

Adds Typescript support to Emacs.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; typescript-mode

(use-package typescript-mode
    :after tree-sitter
    :config
    (define-derived-mode typescriptreact-mode typescript-mode
      "TypeScript TSX")

    (add-to-list 'auto-mode-alist '("\\.tsx?\\'" . typescriptreact-mode))
    (add-to-list 'tree-sitter-major-mode-language-alist '(typescriptreact-mode . tsx)))

```


#### aphelia {#aphelia}

Auto formatting for TS documents.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; aphelia

(use-package apheleia
    :ensure t
    :config
    (apheleia-global-mode +1))

```


#### go-mode {#go-mode}

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; go-mode

(use-package go-mode)
```


#### savehist {#savehist}

Persist minibuffer history across Emacs sessions:

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; savehist

(use-package savehist
    :init
  (savehist-mode))
```


#### elixir-mode {#elixir-mode}

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; elixir-mode

(use-package elixir-mode)
```


#### elixir-ls {#elixir-ls}

Setup the Elixir Language Server:

```lisp
(require 'eglot)

(add-to-list 'eglot-server-programs '(elixir-mode "~/elixir-ls/language_server.sh"))
```


#### pulsar {#pulsar}

Don't lose the cursor:

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; pulsar

(use-package pulsar
    :init
  (setq pulsar-pulse t)
  (setq pulsar-delay 0.055)
  (setq pulsar-iterations 10)
  (setq pulsar-face 'pulsar-magenta)
  (setq pulsar-highlight-face 'pulsar-yellow)

  (pulsar-global-mode 1))
```


#### move-text {#move-text}

Easily move lines up and down in the editor.

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; move-text

(use-package move-text
    :init
  (global-set-key (kbd "M-j") 'move-text-line-down)
  (global-set-key (kbd "M-k") 'move-text-line-up)
  (global-set-key (kbd "C-S-j") 'move-text-down)
  (global-set-key (kbd "C-S-k") 'move-text-up)

  (move-text-default-bindings))
```


#### devdocs {#devdocs}

Provides devdocs support with syntax highlighting:

```lisp
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; devdocs

(use-package devdocs
    :config
  (global-set-key (kbd "C-h D") 'devdocs-lookup))
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

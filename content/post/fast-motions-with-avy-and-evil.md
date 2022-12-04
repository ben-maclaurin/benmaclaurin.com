+++
title = "Fast motions with avy and evil"
date = 2022-10-30T00:49:00+01:00
draft = false
+++

<div class="ox-hugo-toc toc">

<div class="heading">Table of Contents</div>

- [How it works](#how-it-works)
- [Watch the video](#watch-the-video)

</div>
<!--endtoc-->

I have discovered a beautifully versatile motion technique via avy
with evil mode. Both packages are admirable in their own right. avy
brings [char-based decision tree jumping](https://github.com/abo-abo/avy) to the buffer, and evil is [an
extensible vi layer](https://github.com/emacs-evil/evil) for Emacs. Combining them, however, feels _magic_.


## How it works {#how-it-works}

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


## Watch the video {#watch-the-video}

<iframe width="100%" height="315" src="https://www.youtube.com/embed/FiLgoZgaqYo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

> _There is a good chance parts of this post are incorrect. I'm always keen to update my ideas, so if you disagree with anything, I'd be grateful if you could [reach out via email](mailto:contact@benmaclaurin.com)._

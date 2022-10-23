---
title: "Improving Vim Form"
date: 2022-10-20T21:39:40+01:00
draft: false
---

## Introduction

There is no _wrong_ way to use Vim (unless of course you navigate via the inferior `←↑↓→`) ;). Jokes aside, Vim is an incredibly versatile editor and users should traverse code by any means comfortable. However, after 2 years of usage I felt over-reliant on the `hjkl` and [visual mode](https://learnbyexample.github.io/vim_reference/Visual-mode.html) selection.

## Avoiding repetition

Assume we want to copy the following code block, a simple test:

```rust
#[test]
fn convert_to_colemak() {
    assert_eq!('r', 's'.as_colemak());
    assert_eq!("rrr", String::from("sss").as_colemak());
}
```

A few weeks ago I would approach this by hitting `<C-v>` to enter visual mode, then `jjjj` (one `j` per line which is equivalent to pressing `↓` four times), followed by `y` to "yank" the selection. This _works_, but you could see how the same operation would quickly become arduous for longer blocks. There is, of course, a better way to do this and in half the number of keystrokes. Provided you start at the top of the code block, `y4j` will yield the same result. It is essentially verbatim for:

> repeat the _yank_ operation (`y`), `4x` or `4` lines downwards (`j`). 

Now, I wasn't completely obvlivious to such methods and regularly used the likes of `ciw` (clear in word) or `dit` (delete in tag), but they weren't gettting the full attention they deserved. By leaning on visual mode, I tended to overlook the powerful `yiw` (yank in word) and its contemporaries. 

> The "Zen" of **vi** is that you're speaking a language. The initial `y` is a verb. The statement `yy` is a synonym for `y_`. The `y` is doubled up to make it easier to type, since it is such a common operation. – Jim Dennis

I will never be able to explain this as well as Jim Dennis, so you should visit his [StackOverflow comment](https://stackoverflow.com/questions/1218390/what-is-your-most-productive-shortcut-with-vim/1220118#1220118) for further reading.

## A tip for (vi)mproving

If, like me, you have a tendency to form the occasional bad habit, I highly recommend applying a temporary measure to your `.vimrc` or `init.lua`:

```
nnoremap v <nop>
nnoremap <S-v> <nop>
```

This disables `v` and `<S-v>` to in [normal mode](https://learnbyexample.github.io/vim_reference/Normal-mode.html). Whilst uncomfortable at first, this helped retrain my muscle memory for both actions. If you are feeling particularly adventurous (or nonchalant), you could try replicating the above for `j` and `k` to train in [line jumping](https://vim.fandom.com/wiki/Go_to_line) and [other efficient](https://github.com/ggandor/leap.nvim) navigation methods.

## Closing note

After a few months of [Neovim](https://neovim.io/), I've recently dived back into the world of JetBrain editors. I love the fact that you can combine the ingenuity of a 30-year-old [modal editor](https://en.wikipedia.org/wiki/Vim_(text_editor)) with the fully-featured and professional [JetBrains suite](https://www.jetbrains.com/). I will likely write a follow up with my thoughts on this, as it is worthy of its own post.




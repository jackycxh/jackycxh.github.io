---
title: Emacs Daemon and Font
tags:
  - Emacs
  - Linux
categories:
  - Emacs
  - Linux
date: 2009-07-22 23:09:09
---

For Emacs CVS version (V23).

Font setting in `.emacs`

```lisp
(create-fontset-from-fontset-spec "-*-bitstream vera sans mono-medium-r-*-*-13-*-*-*-*-*-fontset-global, han: WenQuanYi Zen Hei-8")
(setq window-system-default-frame-alist
    '(
        ;; if frame created on x display
        (x
            (menu-bar-lines . nil) (tool-bar-lines . nil)
            ;; mouse
            (mouse-wheel-mode . 1)
            (mouse-wheel-follow-mouse . t)
            (mouse-avoidance-mode . 'exile)
            ;; face
            (font . "fontset-global")
        )
        ;; if on term
        (nil
            (menu-bar-lines . 0) (tool-bar-lines . 0)
            (background-color . "black")
            (foreground-color . "white")
        )
    )
)
```

alias for running daemon and client.

```bash
alias et='emacsclient -t "$@" -a ""'
alias ee='emacsclient -nc "$@" -a ""'
```

`et` will run the emacsclient in terminal

`ee` will run it in X

Then when the first time running `et` or `ee`, the daemon will start and a new client will attached to the daemon. The daemon will resist in the memory. Next time you running `et` or `ee` again, a new client will attached to the existing daemon.

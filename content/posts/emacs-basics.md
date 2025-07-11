---
author: amrithmmh
category:
  - uncategorized
date: "2022-10-31T11:42:19+00:00"
guid: https://armphibian.wordpress.com/?p=460
title: Emacs basics
url: /2022/10/31/emacs-basics/

---
Initial setup

1. install emacs > open terminal > type emacs
1. edit **init.el** inside ~/.emacs.d/ directory and add basic config to make it usable or use shared emacs config **( check the below youtube video for some basic setup )**

```
;;stop startup screen
(setq inhibit-splash-screen t)
;;font
(set-face-attribute 'default nil :font "Source Code Pro 16")
;;window controls
(add-hook 'window-setup-hook 'toggle-frame-maximized t)
;; remove menu tool scroll bar
(menu-bar-mode -1)
(tool-bar-mode -1)
(scroll-bar-mode -1)

;; Display line numbers in every buffer
(global-display-line-numbers-mode 1)

;;them directory
(add-to-list 'custom-theme-load-path "~/.emacs.d/themes")
;; Load the Modus Vivendi dark theme
(load-theme 'dracula t)

;;C style
(setq c-default-style "linux"
      c-basic-offset 4)
;;eletric pair mode
(electric-pair-mode 1)

```

note: C= ctrl key , M = alt key (meta key) , example : C-x C-f means press Ctrl+x together release then press Ctrl+f together

Basic commands required /info to use emacs

1. **s** elect all buffer _C-x h_ , copy _M-w_, paste C-y
1. to open dred (file manager / browse directory) : C-x d . opens dred and promts for a directoy
1. to exit emacs C-x C-c
1. File open C-x C-f , file save C-x C-s , close file C-x k , save as C-x C-w
1. Abort a command C-g

handy commands:

**Cursor**:

- Control-A moves the cursor to the start of the current line.
- Control-E moves the cursor to the end of the current line.
- ESCAPE-F moves the cursor forward to the next word.
- ESCAPE-B moves the cursor back to the previous word.
- ESCAPE-< moves the cursor to the start of the buffer.
- ESCAPE-> moves the cursor to the end of the buffer.

**Line edit :**

- Control-D deletes forward one letter.
- Control-K deletes from the point to the end of the line.
- ESCAPE-D deletes forward one word.
- ESCAPE-delete deletes backward one word.

**Emacs has an on-line help system that can be invoked by typing Control-H. If you type the question mark (?), emacs will present a list of help topics you can choose.**

### Editing

> C-x uUndoShift-DelCutCtrl-InsCopyShift-InsPasteC-sInteractive SearchTABIndent Current LineC-M-\Indent Selection

### Buffers

> C-x bSwitch BuffersC-x C-bGet a List of BuffersC-x oSwitch to Other WindowC-x 1Close Other WindowC-x 2Split the Screen Horizontally

**useful information:** [https://fsl.fmrib.ox.ac.uk/fslcourse/unix\_intro/textedit.html#general](https://fsl.fmrib.ox.ac.uk/fslcourse/unix_intro/textedit.html#general)

**Useful link :** [http://www.cs.cornell.edu/courses/cs312/2007sp/software/quick-emacs.html](http://www.cs.cornell.edu/courses/cs312/2007sp/software/quick-emacs.html)

[http://ocean.stanford.edu/research/quick\_emacs.html](http://ocean.stanford.edu/research/quick_emacs.html)

**C programming setup for emacs :**

{{< youtube wm3px6vPTiU >}}&ab\_channel=TimothyUnkert

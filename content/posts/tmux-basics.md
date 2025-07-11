---
author: amrithmmh
category:
  - uncategorized
date: "2022-10-31T13:21:53+00:00"
guid: https://armphibian.wordpress.com/?p=471
title: TMUX basics
url: /2022/10/31/tmux-basics/

---
1. install tmux using package manager , ex: sudo apt install tmux
1. open a terminal , type tmux and hit enter , thats it youre in!

`Hierarchy in tmux: session > window > pane.Prefix for commands and help:
Ctrl-b and sth: issue a command to tmux, inside tmux.
Ctrl-b and ?: see all commands.Session commands:
tmux: creates a new session and enters it.
tmux new -s sessionname: creates a session with the specified name.
tmux ls: lists all sessions.
Ctrl-b and d: detach current session.
Ctrl-b and D: detach selected session (interactive).
tmux attach-session -t sessionNameOrNumber: attach to target session.
tmux rename-session -t sessionNameOrNumber newName: renames sess.
//to get rid of a session, close all panes.Window commands:
Ctrl-b and c: create a window.
Ctrl-b and w: choose a window from a list.
Ctrl-b and <number>: switch to window <number>.
Ctrl-b and p: switch to previous window (acc. to bottom bar).
Ctrl-b and n: switch to next window (acc. to bottom bar).
Ctrl-b , : Rename current window.Pane commands:
Ctrl-d OR exit: kills the current pane, without asking for confirmation.
Ctrl-b and x: quit. closes current pane after asking for confirmation.
Ctrl-b and %: vertically splits the window into two panes.
Ctrl-b and ": horizontally splits the window into two panes.
Ctrl-b and o: traverses panes.
Ctrl-b and <arrow keys="">: traverses panes.
Ctrl-b and ; : switches back and forth btw. the last two panes.
Ctrl-b z: make pane temporarily go full screen - go back to normal size.
Ctrl-b Ctrl-<arrowkey>: resize pane in a certain direction.`

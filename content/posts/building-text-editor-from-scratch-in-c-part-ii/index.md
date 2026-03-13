+++
title = "Building Text Editor From Scratch in C - Part II"
date = 2026-03-13T16:39:42+03:00
tags = ["Building", "Text Editor", "From Scratch", "System Design", "C"]
draft = false
+++

The part 2 started with disabling `Ctrl-C` and `Ctrl-Z` signals. Instead of terminating current process by pressing `Ctrl-C` I was able to read byte code. For `Ctrl-C` it was `3` byte and for `Ctrl-Z` it was `26`. I was able to do that by using `ISIG` flag:

```c
raw.c_lflag &= ~(ECHO | ICANON | ISIG);
```

Next step was disabling `Ctrl-S` and `Ctrl-Q` signals. It turned out that `Ctrl-S` and `Ctrl-Q` were needed for [software flow control](https://en.wikipedia.org/wiki/Software_flow_control) to transmit data to the terminal. `Ctrl-S` allows you to stop transmission and `Ctrl-Q` allows you to resume transmission.

In order to disable transmission I needed to add `IXON` flag:

```c
raw.c_iflag &= ~(IXON);
```

The next step was to disable `Ctrl-V` signal that is done by `IEXTEN` flag:

```c
raw.c_lflag &= ~(ECHO | ICANON | IEXTEN | ISIG);
```

The `IEXTEN` flag also helped me to get code for control character `Ctrl-O` on macOS.

Lastly, I was able to fix `Ctrl-M` signal. This was a tricky one because instead of having byte code `13` it had `10` that was already used by `Ctrl-J`. I also found that `Enter` also produced byte code `10`. What happened was that terminal was translating any carriage return (`13`, `\r`) into new lines (`10`, `\n`).

I used `ICRNL` flag to fix `Ctrl-M` signal but `Enter` was still producing `13`...

```c
raw.c_iflag &= ~(ICRNL | IXON);
```

That is it for now. Thank you for reading! 😊. See you next week.

#### Sources
- https://viewsourcecode.org/snaptoken/kilo/index.html
- https://austinhenley.com/blog/challengingprojects.html

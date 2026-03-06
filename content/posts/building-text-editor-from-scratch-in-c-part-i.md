+++
title = 'Building Text Editor From Scratch in C - Part I'
date = 2026-03-06T17:46:41+03:00
tags = ["Building", "Text Editor", "From Scratch", "C", "System Design"]
draft = false
+++

### Building a text editor from scratch in C 
#### What I learned so far

I learned that I need to start with a simple `read()` operation that reads user input.
```c
while (read(STDIN_FILENO, &c, 1) == 1);
```
But there was a caveat: if you use the `read()` operation, the terminal starts in **canonical mode**. This mode sends keyboard input only after the user presses `Enter`, so it was not very useful because I want to process all keypresses.

The solution was to start the terminal in **raw mode**, but there was no built-in functionality, so I needed to write it myself by turning off some flags.

To get **raw mode** to work, I needed to **disable echoing** so that I don't see each key that I typed. 
```c
raw.c_lflag &= ~(ECHO);
```

Next, after the user exits, I needed to restore all terminal original attributes to the initial state by **disabling raw mode**.
```c
tcsetattr(STDIN_FILENO, TCSAFLUSH, &orig_termios);
```

I needed to turn off **canonical mode** in order to read input byte-by-byte instead of line-by-line. It can be done by using the `ICANON` flag.
```c
raw.c_lflag &= ~(ECHO | ICANON);
```

As a result of all the steps described above and by using control characters `iscntrl`, I was able to print ASCII codes.
```c
if (iscntrl(c)) {
    printf("%d\n", c);
} else {
    printf("%d ('%c')\n", c, c);
}
```

That's it for this week. I will continue to share my learnings. 

#### Thank you for reading! 😊

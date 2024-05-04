+++
title = 'How to find memory leaks?'
date = 2023-12-20T00:00:00+03:00
tags = ["Swift", "Memory Leak"]
draft = false
+++

The common way to find memory leaks is by using Xcode Instruments.
All you need is the following:

- Open Xcode Instruments
- Choose Leaks option
![alt image](images/1.jpg#center)

- Select Simulator where you are going to test your application
![alt image](images/2.jpg#center)

- Select your installed application
![alt image](images/3.jpg#center)

When you finish preparation, you can start immediate recoding and check application for leaks. To do that, you need to open Simulator and try some cases that could cause memory leaks.

After you spend some time trying different scenarios, you can see that Instruments found Leaked Objects.
![alt image](images/4.jpg#center)

Another way to find memory leaks is by using Debug Memory Graph in Xcode Debug Area.
![alt image](images/5.jpg#center)

Inside Debug Memory Graph, you can find MemoryLeaks section.
![alt image](images/6.jpg#center)

MemoryLeaks section displays what objects have strong reference cycles between themselves.

![alt image](images/7.jpg#center)
All tools above have opportunity to find and highlight potential issues and help you resolve them.
---
published: false
title: "eBPF: A Programmer's Perspective"
cover_image: "https://raw.githubusercontent.com/waqqas/blog/master/blog-posts/react-native-applying-patches/assets/cover.jpg"
description: "This article describes eBPF from a perspective of a programmer"
tags: kernel, ebpf, programming
series:
canonical_url:
---

There are many great articles that describe eBPF in general. In this article, I would like to focus on what eBPF means for a programmer.

It won't go into the history of eBPF, not it's evolution over the years. This article would be more pragmatic to get a programmer to start writing eBPF programs as quickly as possible.

It will only go though enough theory so that a programmer can understand whats going on.

Introduction
---

You can compare eBPF to micro-controller software development. There is no OS. You have limited program and memory constraints. There is limited functionality that is at your disposal. You will be "cross-compiling" your program to run in this "micro-controller" (Virtual Machine)

Toolchain Setup
---

The first order of the business to get a toolchain that can "cross-compile" your program. 


Event Driven Programming Model
---

There would be specific events that you be generated and you will be writing eBPF program that react to those events. Each event has it's own data and you can return some specific values to control what happens next.

Data Storage
---

Every program need to store some data. You have some generic and some specific data structure available to choose from.


Program Constraints
---

There is limitation on not only how much program that you write, but also what you can do.

Debugging Interface
---

Understanding how to debug a program is very important.


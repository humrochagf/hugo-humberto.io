---
title: "How to write framework agnostic Micro-Apps"
description: "Sharing my thoughts on how to write micro-apps in an effective way while keeping it fun."
publishDate: 2024-10-01
tags:
  - microapps
  - microservices
  - python
  - self-hosting

---

Every 5 years or so there's a new kid on the block, a wild framework has appeared and we decide that in our new side project we are going to use this brand new framework that looks cooler and faster.

Usually what happens is that we spend most of the time learning the new framework. The side project start to feel like work and there's no fun anymore. Eventually that cool little fun project starves and dies, often before even starting.

But what if we go 100% lazy and delay all of those decisions for when and if we actually need to take them? This way we'll be able to drop a side project because the idea wasn't that great after all and not because it we got overwhelmed. Much more fun isn't it?

In this guide I'm proposing to explore the **Service -> Repository** architecture that is usually hidden under the [MVC]() and [MTV]() architectures as a way to quickly achieve our goes.

I'm also targeting this guide to hobbyists and self hosting enthusiasts who wants to have fun with small side projects. Performance is by far a concern here.

## Service Repository Architecture



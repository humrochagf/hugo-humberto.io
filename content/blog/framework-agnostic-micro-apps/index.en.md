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

But what if we go 100% lazy and delay all of those decisions for when it actually matters? This way we'll be able to drop a side project because the idea wasn't that great after all and not because it we got overwhelmed. Much more fun isn't it?

![gif of a panda throwing things of the table]()

Jokes aside, in this guide I'm proposing to explore the **Service -> Repository** architecture that is usually hidden under the [MVC]() and [MTV]() architectures as a way to quickly achieve our goes.

I'm also targeting this guide to hobbyists and self hosting enthusiasts who wants to have fun with small side projects. Performance is far from being a concern here.

## The Project Idea

Lets say we are preparing a seedling nursery and we want to track when our seedlings are ready to be planted.

![seedling nursery illustration]()

To keep things simple in our example, everything we want to store is represented by this data class:

```python
from dataclasses import dataclass
from datetime import date

@dataclass
class Seedling:
    name: str
    ready_at: date
    started_at: date = date.today()
```

Our seedling will have a name, a ready date for us to know that around this date we should plant it, and the date we started it.

Now it's time decide on how to implement it.

## Implementation Decision

As said in the beginning, here is where the rocket science would start with things like:

    "let's use a blazing fast (maybe rust based) framework"
    "i should make an api with docs support"
    "what if I use this new packaging solution"
    "so, I'll test it with AI"
    "and put some lasers there..."

So, instead, let's take a deep breath, go drink a glass of water and watch the nature, or something chill while we think what do we really want to accomplish with our initial idea.

![picture of a zen cat]()

It's totally fine if you are using the idea just as an excuse to learn something, but if that's not the case we can completely skip all of those decisions.

We need a framework in the first place, in fact at this point we don't even need to decide if it will be web based, let the "soil" free for ideas to grow as we do real [MVP]().

In the case of our seedling project here we want to **register** our seedlings, and we want to know **when they are ready** to be planted.

We can express this by writing a doctest file called `nursery.py`:

```python
"""
Seedling Nursery MVP
====================

A cool little side project to help me take care of the garden.

    >>> service = SeedlingService()

Register Seedling
------------------

Register a seedling in the service given a name and the time for it to be ready in days.

    >>> service.register_seedling("Sprout", ready_in_days=0)
    True

Fetch Weekly Seedlings
---------------------

    >>> len(service.fetch_weekly_seedlings())
    1
"""

if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

We are starting a `SeedlingService` that should be able to register a seedling and also to fetch seedlings should be be ready for planting this week.

For now this is our indented Service API and we can use doctest to check if our intentions were met by our implementation:

```shell
python nursery.py
```

The tests are failing for now because only declared our intention.

Let's then structure our code to accomplish that.

## Service Repository Architecture

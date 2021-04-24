---
layout: post
title: Simplest async/await example possible in Python
image: https://raw.githubusercontent.com/talkpython/async-techniques-python-course/master/readme_resources/async-python.png
description: >
  I've read many examples, blog posts, questions/answers about asyncio / async / await in Python 3.5+, many were complex, the simplest I found was probably this one.
hide_description: true
hide_image: true
comments: true
last_modified_at: 2021-04-24
categories: [stackoverflow]
tags: [python, asynchronous, async-await, python-asyncio]
---

Simplest async/await example possible in Python

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/50757497/simplest-async-await-example-possible-in-python) by [Basj](https://stackoverflow.com/users/1422096/basj)

I've read many examples, blog posts, questions/answers about `asyncio` / `async` / `await` in Python 3.5+, many were complex, the simplest I found was probably [this one](https://stackoverflow.com/questions/37278647/fire-and-forget-python-async-await/37345564#37345564).

Still it uses `ensure_future`, and for learning purposes about asynchronous programming in Python, I would like to see an even more minimal example, and what are the minimal tools necessary to do a basic async / await example.

Question: is it possible to give a simple example showing how `async` / `await` works, by using only these two keywords + code to run the async loop + other Python code but no other asyncio functions?

Example: something like this:

```py
import asyncio

async def async_foo():
    print("async_foo started")
    await asyncio.sleep(5)
    print("async_foo done")

async def main():
    asyncio.ensure_future(async_foo())  # fire and forget async_foo()
    print('Do some actions 1')
    await asyncio.sleep(5)
    print('Do some actions 2')

loop = asyncio.get_event_loop()
loop.run_until_complete(main())
```

but without `ensure_future`, and still demonstrates how await / async works.

## [Answer](https://stackoverflow.com/a/64508472/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)

Since everything is nicely explained, then let's run some examples with event loops compare synchronous code to asynchronous code.


synchronous code:
```py
import time

def count():
    time.sleep(1)
    print('1')
    time.sleep(1)
    print('2')
    time.sleep(1)
    print('3')

def main():
    for i in range(3):
        count()

if __name__ == "__main__":
    t = time.perf_counter()
    main()
    t2 = time.perf_counter()
    
    print(f'Total time elapsed: {t2:0.2f} seconds')
```

output:
```sh
1
2
3
1
2
3
1
2
3
Total time elapsed: 9.00 seconds
```

We can see that each cycle of count running to completion before the next cycle begins.


asynchronous code:
```py
import asyncio
import time

async def count():
    await asyncio.sleep(1)
    print('1')
    await asyncio.sleep(1)
    print('2')
    await asyncio.sleep(1)
    print('3')

async def main():
    await asyncio.gather(count(), count(), count())

if __name__ == "__main__":
    t = time.perf_counter()
    asyncio.run(main())
    t2 = time.perf_counter()

    print(f'Total time elapsed: {t2:0.2f} seconds')
```

output:
```sh
1
1
1
2
2
2
3
3
3
Total time elapsed: 3.00 seconds
```

The asynshonous equivalent on the other hand looks somting like this took three seconds to run as opposed to nine secounds. 
The first count cycle was started and as soon as it hit the `await`s sleep one Python was free to do other work, for instance starting the secound and subsequently the third count cycles.
This is why we have all the ones than all tubes then all three. 
In the output programing concurrently can be a very valuable tool.
Multiprocessing has the operating do all of the multitasking work and in Python it's the only option for multi-core concurrency that is  having your program executed on multiple cores of CPU.
If use threads then the operating system is still doing all of the multitasking work and in cpython the global intrepeter lock prevents multi-core concurrency in asynshonous programming.
There is no operating system intervention there's one process there's one thread so what's going on well tasks can release the CPU when there are waiting periods, so that other task can use it.

```py
import asyncio

loop = asyncio.get_event_loop()


async def greeter(name):
    print(f"Hi, {name} you're in a coroutine.")

try:
    print('starting coroutine')
    coro = greeter('LP')
    print('entering event loop')
    loop.run_until_complete(coro)
finally:
    print('closing event loop')
    loop.close()
```

output:
```sh
starting coroutine
entering event loop
Hi, LP you're in a coroutine.
closing event loop
```

Asynchronous frameworks need a scheduler usually called an event loop. This event loop keeps track of all the running tasks and when a function suspended it returns control to the event loop which then will find another function to start or resume and this is called cooperative multitasking. Async IO provides a framework an asynchronous framework that's centered on this event loop and it efficiently handles input/output events an application interacts with the event loop explicitly it registers code to be run and then it lets the event loop the scheduler make the necessary calls into application code when the resources are available. 
So, if a network server open sockets and then registers them to be told when input events occur on them the event loop will alert the server code when there's a new incoming connection or when there's data to be read.
If there's no more data to be read from a socket than the server then yields control back to the event loop. 

The mechanism from yielding control back to the event loop depends on co-routines co-routines are a language construct designed for concurrent operation. The co-routine can pause execution using the awake keyword with another co-routine and while it's paused the co-routine state is maintained allowing it to resume where it left off one co-routine can start another and then wait for the results and this makes it easier to decompose a task into reusable parts.

```py
import asyncio

loop = asyncio.get_event_loop()

async def outer():
    print('in outer')
    print('waiting for result 1')
    result1 = await phase1()
    print('waiting for result 2')
    result2 = await phase2(result1)
    return result1, result2


async def phase1():
    print('in phase1')
    return 'phase1 result'

async def phase2(arg):
    print('in phase2')
    return 'result2 derived from {}'.format(arg)

asyncio.run(outer())
```

output:
```sh
in outer
waiting for result 1
in phase1
waiting for result 2
in phase2
```

This example asks two phases that must be executed in order but that can run concurrently with other operations. The `awake` keyword is used instead of addingbthe new co-routines to the loop because control flow is already inside of a co-routine being managed by the loop. It isn't necessary to tell the loop to manage the new co-routines. 

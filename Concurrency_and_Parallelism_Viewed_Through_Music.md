[TOC]

# Concurrency, Parallelism, Synchronous, Asynchronous and all that Jazz

Terms that will be covered and differentiated in this document:
* concurrency
* parallelism
* asynchronous
* synchronous
* threading
* multiprocessing
* processes
* threads
* multithreading

## What is typically *missing* from "asynchronous vs synchronous" explanations?

I was introduced to all these terms *through the lens of* building a REST API.

My belief is that the reason I don't feel as though I have a firm understanding of these concepts is because I am trying to differentiate/"create some
space between" these terms **through that same lens**, and this is difficult to do because the structure of a RESTful API *is inherently
asynchronous*.

It would make such little sense to have an API be synchronous that it is glossed over

so lets zoom out, remove this lens, define some terms, and get a handle on what is going on...

### Problem Domain & Solution Domain

| Layer Level | Layer Description       |
| ---         | ---                     |
| 2           | Solution Implementation |
| 1           | Solution Structure      |
| 0           | Problem Definition      |

* Problems...:
    * ...have *definitions*.
    * ...have multiple solutions.
* Solutions...:
    * ...have *structure*.
    * ...have *dimensions*.
* Implementations...:
    * ...can be in different *technologies*.
    * ...are *executed* with these technologies.

Given this, a few statements can be:
* Definition precedes Structure and Structure precedes Implementation.
* Problems are not inherently concurrent, parallel, synchronous or asynchronous. These words don't *belong to that layer*. They belong in the
  Implementation layer.
*


The words *"concurrent, parallel, asynchronous and synchronous"* are *descriptors of the structure or execution of a
particular solution* to a problem. They are not terms that apply to a problem.

* 'Concurrent', 'asynchronous' and 'synchronous' are a descriptors of the *structure* of a particular solution.
* 'Parallel' is a descriptor of the *execution* of a particular solution.


An asynchronous implementation makes sense when:
* 



**General structure of an API:**
![](images/concurreny_vs_parallelism__api.png)

There are N clients requesting some service from 1 server








## What is our Guiding Question?

My hypothesis is:

A mismatch between the *structure* of a solution and the *implementation* of a solution will be slower than a match between the said structure


| Solution     | Implementation | "will be"   | Solution     | Implementation |
| ----         | ----           | ----        | ----         | ----           |
| Synchronous  | Asynchronous   | Slower than | Synchronous  | Synchronous    |
| Asynchronous | Synchronous    | Faster than | Asynchronous | Synchronous    |


 


A solution whose *structure* is asynchronous
1. A 'problem domain process' that is inherently synchronous will still be synchronous when written with asynchronous code.

2. Not only will a 'problem domain process' that is inherently synchronous still be synchronous when written with
   asynchronous code, it will be *slower* than if it had been written with synchronous code.


```python
import asyncio

async def return_x(x):
    await asyncio.sleep(1)
    return x

async def return_y(y):
    await asyncio.sleep(1)
    return y

async def main(x, y):
    x = await return_x(x)
    y = await return_y(y)
    print(x + y)
    return x + y

if __name__ == '__main__':
    asyncio.run(main(1, 2))
```

True or not?: A mismatch between the *structure* of a solution and the technology used to *implement* that solution
usually results in an inefficiency with respect to a particular *dimension* of that solution (e.g. time)....

## Examples

A problem gets broken down into tasks A, B, C and D, each of which must be completed for the problem to be completed.
1. If...

$$
\begin{align}
D & = h(C) \\
C & = g(B) \\
B & = f(A) \\
& A
\end{align}
$$

...then the *structure* of the solution is sequential/linear. This means the *execution* of the solution should be *synchronous*

![](images/concurreny_vs_parallelism__sequential.png)

![](images/async_vs_sync__example1.png)

* A note on how I'm measuring execution time:
    * I'm not measuring the time of the *entire* program (`$ time python [ tmp.py | tmp2.py ]`)
        * This would not only measure the time of interpretation, but would also measure the time it takes to import
          `asyncio`
    * I'm not measuring the time to start and stop the event loop (`asyncio.run(<whatever>)`)
        * This doesn't feel like a fair comparison

**Results**
```bash
# Synchronous technology used to implement a synchronous solution
Average runtime (nanoseconds): 1242.164
Runtime deciles (nanoseconds): [   0.    0. 1000. 1000. 1000. 1000. 1000. 1000. 2000. 2000.]

# Asynchronous technology used to implement a synchronous solution
Average runtime (nanoseconds): 3336.65
Runtime deciles (nanoseconds): [1000. 2000. 2000. 3000. 3000. 3000. 3000. 3000. 3000. 4000.]
```

2. If...
    * 


and therefore neither concurrency nor parallelism are applicable descriptors of this solution.

* Examples:
    * **Chess:** A single chess match between two players. There is no way around the fact that each player must wait on
      the other player to make their next move.
    * **Medicine:** 
    * **Programming (without I/O):**
    * **Programming (with I/O):**



The question is not:
> Should the solution be asynchronous or synchronous?

The question is:

> *Which* parts of the solution can/should be asynchronous and *which* parts *must* be synchronous?


IT IS ABOUT UNDERSTANDING THE STRUCTURE OF THE SUBSOLUTION.....

Technologies are used to implement solutions. 



## Lenses / Perspectives

These terms are best viewed through different lenses / from different perspectives. The following is a list of the the
lenses through which we will look at these concepts:

* Problem Domain
* Solution Domain
    * Hardware
    * Software
        * Layer 0
        * Layer 1
        * Layer 2
    * Programmer

### Viewed through the Hardware Lens

From the

### Viewed through the Programmer Lens

The key thing to understand/accept is *you are programming at a higher level of abstraction when you are programming
asynchronously.*

When programming synchronously, 


### Concepts 

Concurrency & Parallelism are concepts that exist outside the domain of software and computer science. A good way of
separating these two concepts is asking the question:

> Is waiting *on external events* an unavoidable part of the solution to a problem you are trying to solve?

The answer to this is **"yes" in a lot of situations**:
* Physical World Examples: 
    * Chess Tournaments:
        * Player A must wait for Player B to move before he can move again.
    * Baking/Cooking:
        * You must wait for the oven to preheat before you can use it
    * 
* Digital World Examples: 
    * You must wait for an API response after you have sent a request
    * You must wait for a DB to return data you queried



* **Concurrency**: *Progress* on two or more tasks is made *at the exact same time.*
    * Concurrency is about the *design/structure* of the solution
* **Parallelism**: *Actions* need to make progress on two or more tasks are *performed at the exact same time.*
    * Parallelism is about *action*



When I say "at the exact same time" I mean *at the exact same time*, the same way you can snap the fingers of both hands
*at the exact same time*.



The real questions is:

* Multiprocessing:
    * A means to implement parallelism.
    * The act of spreading operations over multiple cores on a computer
* Threading:
    * 


There are two perspectives/lenses through with we look at these terms:
* The "Problem Domain" Lens:
    * Definitions viewed through this lens:
        * Concurrency/Concurrent: **Progress** on two or more tasks is made *at the exact same time.*
* The "Solution Domain" Lens:
    * Definitions viewed through this lens:
        * Parallelism/Parallel: **Actions** need to make progress on two or more tasks are **performed** *at the exact same time.*


* All parallelism is concurrent, not all concurrency is parallelism.



## Programs, Processes & Threads

* Programs are what developers write...
    * Processes are executing instances of an application.
        * Threads are paths of execution *within* a process.

* Processes setup the resources need

# Concurrency & Parallelism Through the Lens of Music

Music domain (piano being the instrument of choice):
* notes
* chords
* piece(song)
* hand (singular)
* hands (both)
* fingers (hardware?)
* fingering

Computer Science domain:
* concurrency
* parallelism
* threading
* multiprocessing
* asynchronous I/O
* process/task/threads
* processor
* core
* hyperthreading

# Initial Guesses

* `Music Domain <--> Computer Science Domain`
* `fingers <--> cores`
* `hands <--> processor`

**<u>How to read the below table:</u>**

"`X` *is/are to the* **Music Domain** in the same way the `Y` *is/are to the* **Computer Science Domain**"

Further definitions:

* "People create programs to direct processes." - SICP
  * Addition: "People create programs *which resemble solutions to problems* to direct *computational* process"
  * Addition: "People create programs to direct *computational* process *whose outcome solves a problem*."

`Problem <--> Programs <--> Process`

`Real World <--> Programs <--> Computational World`



| `X`            | `Y`                  |
| -------------- | -------------------- |
| Pieces (songs) | Application Programs |
| Chords         | Parallelism          |
| Notes          | Concurrency          |
|                | Synchronous          |
|                | Asynchronous         |
| Fingers        | Cores                |
| Hands          | Processors           |

```python
import multiprocessing
import time
import random


def worker(number):
    sleep = random.randrange(1, 10)
    time.sleep(sleep)
    print("I am Worker {}, I slept for {} seconds".format(number, sleep))


for i in range(5):
    t = multiprocessing.Process(target=worker, args=(i,))
    t.start()


print("All Processes are queued, let's see when they finish!")
```

# IDK man...

You *can* call non-async code from async code

> "Conversely you absolutely can call non-async code from async-code, in fact it’s easy to do so. But if a
> method/function call might “block” (ie. take a long time before it returns) then you really shouldn’t."
- [Part 5 Here](https://bbc.github.io/cloudfit-public-docs/)

...however, if that call that might "block" is necessary for all downstream operations, *there is no avoiding the
blocking...?*

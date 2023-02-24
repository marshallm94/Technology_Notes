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
* asyncronous I/O
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

## Hypothesis

1. A 'problem domain process' that is inherently syncronous will still be syncronous when written with asyncronous code.

2. Not only will a 'problem domain process' that is inherently syncronous still be syncronous when written with
   asyncronous code, it will be *slower* than if it had been written with syncronous code.


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

# Speed Up Your Python Program With Concurrency

🐍 Source: [Real Python Tutorial](https://realpython.com/python-concurrency/)
- 2025/09/10

Concurrency refers to the ability of a program to manage multiple tasks at once, improving performance and responsiveness.

It encompasses different models like **threading**, **asynchronous tasks**, and **multiprocessing**, each offering unique benefits and trade-offs.

In Python, **threads** and **asynchronous tasks** facilitate concurrency on **a single processor**, while **multiprocessing** allows for true parallelism by utilizing **multiple CPU cores**.

Understanding concurrency is crucial for optimizing programs, especially those that are **I/O-bound** or **CPU-bound**. Efficient concurrency management can significantly enhance a program’s performance by **reducing wait times and better utilizing system resources**.

## Overview

- Understand the different forms of concurrency in Python
- Implement multi-threaded and asynchronous solutions for I/O-bound tasks
- Leverage multiprocessing for CPU-bound tasks to achieve true parallelism
- Choose the appropriate concurrency model based on your program’s needs

## Exploring Concurrency in Python

Concurrency can take different forms depending on the problem it aims to solve.

### What Is Concurrency?

The dictionary definition of concurrency is simultaneous occurrence. In Python, the things that are occurring simultaneously are called by different names, including these:
- Thread
- Task
- Process

At a high level, they all refer to **a sequence of instructions that run in order**. You can think of them as different **trains of thought**.
- Each one can be stopped at certain points, and the CPU or brain that’s processing them can switch to a different one. 
- The state of each train of thought is saved so it can be restored right where it was interrupted.
- Only **multiple system processes** can enable Python to run these trains of thought at literally the same time.
- **Threads** and **asynchronous tasks** always run on a single processor, which means they can only run one at a time. They just cleverly **find ways to take turns** to speed up the overall process.
  - **Note**: Threads in most other programming languages often run in parallel. To learn why Python threads can’t, check out [the Python Global Interpreter Lock (GIL)](../WhatIsThePythonGlobalInterpreterLock(GIL).md)

The way the threads, tasks, or processes take turns differs. 
- In a **multi-threaded approach**, the operating system actually knows about each thread and can interrupt it **at any time** to start running a different thread. This mechanism is also true for **processes**. It’s called **preemptive multitasking** since the operating system can preempt your thread or process to make the switch.
  - Preemptive multitasking is handy in that the code in the thread doesn’t need to do anything special to make the switch. 
  - It can also be difficult because of that at any time phrase. The context switch can happen in the middle of a single Python statement, even a trivial one like `x = x + 1`. This is because Python statements typically consist of several low-level bytecode instructions.
- On the other hand, asynchronous tasks use **cooperative multitasking**. 
  - The tasks must cooperate with each other by announcing when they’re ready to be switched out without the operating system’s involvement. This means that the code in the task has to change slightly to make it happen.
  - The benefit of doing this extra work upfront is that you always know where your task will be swapped out, making it easier to reason about the flow of execution. 
  - A task won’t be swapped out in the middle of a Python statement unless that statement is appropriately marked. You’ll see later how this can simplify parts of your design.

### What Is Parallelism?

A **process** can be thought of as almost a completely different program, though technically, it’s usually defined as **a collection of resources** including memory, file handles, and things like that. One way to think about it is that **each process runs in its own Python interpreter**.

Because they’re different processes, each of your trains of thought in a program leveraging **multiprocessing** can run on **a different CPU core**. Running on a different core means that they can actually run at the same time, which is fabulous. There are some complications that arise from doing this, but Python does a pretty good job of smoothing them over most of the time.

Python modules and differences:
| Python Module | CPU | Multitasking | Switching Decision |
|---------------|-----|--------------|-------------------|
| `asyncio` | One | Cooperative | The tasks decide when to give up control. |
| `threading` | One | Preemptive | The operating system decides when to switch tasks external to Python. |
| `multiprocessing` | Many | Preemptive | The processes all run at the same time on different processors. |

### When Is Concurrency Useful?



## Speeding Up an I/O-Bound Program

### Synchronous Version



## Speeding Up a CPU-Bound Program

## Deciding When to Use Concurrency

## Conclusion
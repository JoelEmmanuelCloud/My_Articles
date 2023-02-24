# Introduction to Asynchronous Programming in Python

Welcome to this introductory article on Asynchronous Programming.

What I will be covering is the Async and, coroutines functionality from the **Asyncio** module in Python.

Now it's worth noting that this article is not for beginners, you should have some basic programming knowledge to follow along, especially in Python, and if you're using any version of Python below 3.7, a lot of the stuff I will be showing you will not apply. so please make sure to use Python 3.7 version and above.

With that said, let's dive right in. Before we get into asynchronous programming, I want to start by defining what synchronous programming is.

Synchronous programming just means that every code that we write is executing sequentially.

If I define a function main(), and then return the string "HelloWorld". The print statement is not going to run until this function main() is executed.

```python
import asyncio

def main():
    return 'HelloWorld'


result = main()
print(result)
```

It doesn't matter how long this function takes. It doesn't matter if the main() function is doing anything or not. The function must finish executing before the print statement is going to run.

This is, by definition, a synchronous approach. You're doing things sequentially. And I like to think about synchronous programming as having a kind of performance based on the clock speed of our computer processor. How fast your code runs is determined by your computer's processing speed.

But with asynchronous programming, things are a little bit different. We don't necessarily need to do things sequentially. And the reason for that is the following.

Let's say this function here will wait on something that's not related to our computer. The function is waiting for a file to upload or download for some users to input some information.

```python
import asyncio

def main():
    return 'HelloWorld'


result = main()
print(result)
```

While that function is waiting, another code should be able to run.

We don't need to do things sequentially. We don't need to wait for the main() to finish running before we can print the result. While the main() is waiting for something else to happen, our processor is free to do some other operations.

This allows us to go run some other code and then when it's done, we'll come back to it later and resume the main() execution.

I understand that this may be unclear right now. But just understand that when we're talking about synchronous versus asynchronous programming, asynchronous programming means that we don't need to necessarily wait for something to be done before we run other parts of our code.

This is very beneficial, especially if you're waiting for a network operation like you're querying a database or waiting for a response from some server that's not disallowing your computer from running, you can now execute other operations.

With that said, let's dive in. The first thing I need to discuss here is something called **coroutines** and **async** keywords.

```python
import asyncio
```

As you can see here, I have imported this module called **asyncio**. This module is built in Python, you don't need to install it, and you can import it if you use a version of Python that's 3.7 and above.

The first thing that we need to do when we're creating an asynchronous program is we need to create what's known as a **coroutine**. A **coroutine** is simply kind of a wrapped version of the function that allows it to run asynchronously. [Wikipedia's definition of a coroutine](https://en.wikipedia.org/wiki/Coroutine#:~:text=Coroutines%20are%20computer%20program%20components,iterators%2C%20infinite%20lists%20and%20pipes.)

Because you want to think about it in this way when you're writing code. Traditionally, we're writing synchronous code. To be able to write asynchronous code, we have to do things a little bit differently. we can't use the same code that we use when we're writing something synchronous.

To create a coroutine, what you do is define a function. So I'm going to define a function called main(), and inside of this function, I will print the string 'greetings'. And then before this function definition, you put the keyword **async**.

```python
import asyncio

async def main():
    print('greetings')

main()
```

What **async** tells Python to do is essentially create a wrapper around this function. So when you call this function, what it's going to do is returned to you a coroutine object.

This coroutine object is just like a function, and it can be executed. And to execute a coroutine, you need to do what's known as the **await**, you need to **await** for its execution.

Let me just show you how we can come work with **coroutines** and what a **coroutine** is.

Right now, if I call this the main function, you would expect **greetings** to be printed on the screen. But when I do that.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677237311983/baf49f66-76d8-4b5f-8eae-4a3d715afcae.png align="center")

Notice that we got this thing RuntimeWarning coroutine 'main' never awaited.

Now, this is telling us that, hey, we can't run this function. Because what we've done is we haven't awaited the **coroutine**. When we call this function it returns a **coroutine**. It doesn't work like a regular vanilla Python function.

Now, let me show you what I mean. If I print the main(). You're gonna see below that it gives me what's called a coroutine object.

```python
import asyncio

async def main():
    print('greetings')


print(main())
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677238965583/3ef0c2e1-7719-43d4-bf4c-7aaa5686dfac.png align="center")

It says the coroutine object, and main() are in this memory location &lt;0x0........&gt;.

So calling this function doesn't just execute its code, and it gives me this kind of new object. This new object is asynchronous and can be used in the asynchronous program.

If you want this code to run, we need to create what's known as an **Async Event Loop**. The Async event loop is what's allowing us to run this python **asyncio** code. [Wikipedia's definition of an event loop](https://en.wikipedia.org/wiki/Event_loop)

To run our function we need to call the **asyncio** module to create an event loop and add a coroutine to the event loop as shown below.

```python

import asyncio

async def main():
    print('greetings')


asyncio.run(main())
```

This line of code is the coroutine that creates the event loop for our code to run.

```python
asyncio.run(main())
```

When we run this code we get our output as "greetings"

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677244050627/897ccccd-3896-49f3-bce5-41264f039bd3.png align="center")

---

This is the basis of asynchronous programming in python, hope you find this article useful and please like and share.
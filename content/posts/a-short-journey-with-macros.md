---
title: "A Short Journey With Macros"
date: 2021-03-09T07:53:34-07:00
---

Playing with embedded systems for quite a few years, I have gained special love for arrays, enums, structs and, macros.
These are basic data structures and language features when used properly, would offer great readability, space/time efficiency
and, pleasure. A lot of the time with these simple tools one can construct zero-cost abstraction, which still facinates me every now and then.
<!--more-->

But these would need some serious article to cover (maybe later). This quick note only tells a recent short journey of mine
with the (C) macros.

At work I implemented a mechanism for system-wide event communication. Being a core component its APIs get called
everywhere, but there are times when we would like to build without this eventing mechanism, say to save memory when
eventing is not needed.

One major API is used to send an event. For the sake of this quick note let's say it is named *send(uint32 event)*. So
the requirement here is, for all the *send(...)* references, to call the actual *send* function when eventing is enabled, and to do nothing otherwise.

The quickest way would be using #ifdef:
```c
   #ifdef EVENT_ENABLED
   send(1);
   #endif
```
Apparently the issue here is all the calls to *send()* will need to have the #ifdef binding.
It is not fun to read code with a lot of these preprocessor primitives.

Another option will be to move the *#ifdef* into the *send()* function:
```c
   int send(uint32 event)
   {
#ifdef EVENT_ENABLED
       // send the event here
#endif
       return 0;
   }
```
This is better, but the dummy function will not be completely eliminated: At least for the compiler I use (arc-zephyr-elf-gcc 9.2.0), a call to
this *send()* will still result in the assembly as below:
```asm
   mov_s   r0,0
   j_s     [blink]
```
The caller still jumps to execute this function. 0 is still saved to *r0* before return.
*blink* is one of the link registers in the ARC processori.
And yes that is with size optimization.
It is close enough but I wonder if the initial goal of doing nothing is achievable.

And then it comes to my mind:
```c
#ifdef EVENT_ENABLED
int send(uint32 event);
#else
#define send(x)
#endif
```
Putting this in the header and all calls to *send()* will turn to no-op. Mission complete!
But wait, *send()* is a function that returns a value, so with the no-op trick above
```c
int ret = send(123);
```
will turn into
```c
int ret = ;
```
And the compiler won't be happy.

How about this?
```c
#ifdef EVENT_ENABLED
int send(uint32 event);
#else
#define send(x) 0
#endif
```
That would satisfy the compiler for calls like *int ret = send(123);* but when the caller doesn't check the return
value, a call to the function becomes *0;*, which triggers a warning from the compiler: *"statement has no effect"* (`-Wunused-value`).

A common way to workaround this warning is via a type cast:
```c
(void*)0;
```
but it doesn't work here because that would break the use case of *int ret = send(123);*.

Another way out is to instruct the compiler to ignore this warning. Globally disable this warning is not advisble as it
would blind us from other potential mistakes somewhere else, so how about only ignoring the warning for this code?

```c
#pragma GCC diagnostic push
#pragma GCC diagnostic ignored "-Wunused-value"
	/* code that requires to ignored the warning */
#pragma GCC diagnostic pop
```

Note that we cannot directly use the #pragma directives here because that would look like

```c
#ifdef EVENT_ENABLED
int send(uint32 event);
#else
#define send(x)                                 \
#pragma GCC diagnostic push                     \
#pragma GCC diagnostic ignored "-Wunused-value" \
    0                                           \
#pragma GCC diagnostic pop
#endif
```
and since the preprocessor will only scan once, any preprocessor directives, *#pragma* in our case, inside a macro will
generate the `'#' is not followed by a macro parameter` error.
So what to do if we want to use pragma inside a marco?
Well some compilers support the pragma operator, so
```c
_Pragma("argument")
```
is the same as
```c
#pragma argument
```
Fortunately our compiler support this feature as well. So the below definition does the trick.
```c
#ifdef EVENT_ENABLED
int send(uint32 event);
#else
#define send(x)                                       \
_Pragma ("GCC diagnostic push")                       \
_Pragma ("GCC diagnostic ignored \"-Wunused-value\"") \
    0;                                                \
_Pragma ("GCC diagnostic pop")
#endif
```
When *EVENT_ENABLED* is not set, the call to *send()* produces zero code.

OK sounds like we have something working, but there is one side effect: if the caller misses the semicolon when calling the function, the error will not be detected by the compiler when *EVENT_ENABLED* is not set.
And what about when the compiler doesn't support the *_Pragma* operator?
Would there be a different solution that is less dependent to some compiler features, without side effect, and, to some degree, more appealing to the eye?

I have been so focused, or stuck, in a working macro solution. Now that I step back, there are other tools in the box too!

To conclude this post I ended up with the definition below:
```c
#ifdef EVENT_ENABLED
#define SEND(x) send(x)
#else
inline int dummy() {return 0;}
#define send(x) dummy()
#endif
```
So no messy pragma argument, no side effect and no dependency on non-common compiler feature.
Inline function saves the day!

Now I can sleep without dreaming about the macros and preprocessor primitives, until later :)

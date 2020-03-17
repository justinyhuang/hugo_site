---
title: The Not-So-Easy C
date: 2012-08-05T13:49:00
---

Whenever I get an embedded software engineer's resume, one thing I can guarantee is there will be definitely a line says:

Excellent in C/C++ (or something similar)

<!--more-->

Another one I am sure is someone will challenge the C++ skills of the candidate: so let's talk about templates...

I understand why the graduates think they are qualified for C/C++ programming, and I fully agree C++ is way more than inheritance and OOP.

What I don't understand is why people believe C is simple and, relatively, easy.

I've heard a humble interviewee said: I am not a C++ expert, but I know C very well.

Really?

Let's test ourselves with some quick questions:

  * Based on What C standard did you write your last C program?
    You might argue that I am showing off by asking this pedantic question.
    I'm not.
    If you are not using C99 or later, you will not be able to write inline functions because that is not supported. You will have to put all variable declarations at the top of the start of a compound statement or the compiler will complain about the "declaration may not appear after executable statement in block" error; You cannot define a variable-length array, and you will have to define your own boolean data type - these are not provided in the pre-C99 standards. There are more.
    This is why you need to understand what standard you are using before touching the keyboard. With the knowledge, you might solve the compilation errors easily by adding an compile option, -std=c99.

  * So I assume you use C99, which I believe is the most popular standard nowadays. What does the C keyword, _restrict_, mean?
    No it is not _register_, I mean _restrict_.
    If you have no idea or are not sure about your answer, check it out [here](http://en.wikipedia.org/wiki/Restrict).
    Again one would question the point of this question: it is too esoteric, even more esoteric than the word "esoteric"!
    Calm down. Think about the applications of C today. There are good chances that you write C code for some embedded
    systems, which are very sensitive to efficiency and the footprint of the executable image. Proper use of the _restrict_ keyword [could help the compiler to produce significantly faster executables](http://developers.sun.com/solaris/articles/cc_restrict.html). If you seldom consider ways to optimize your code, you might have been lucky (or unfortunate, depending on how you look at it) to work on something called embedded but doesn't care too much about memory and efficiency, or, you might not know C very well, as an embedded software engineer.

  * How would you define an integer?
    A question, instead of an answer is expected.
    One should ask: "How many bits of the integer we are talking about?" before writing on the whiteboard.
    And a statement "U32 x;" is better than "int x;", while "uint32_t x;" is even better because that is C standard, defined in stdint.h.
    "int" is evil in the embedded system industry. The type system of C has been long disputed and it is the embedded software engineers' responsibility to make it right.
    It is a must, but not virtue, to write clear, safe and portable code in C.


So, do you still think C is easy and you know it very well?

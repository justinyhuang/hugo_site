---
title: Note on a Few Defects
date: 2010-08-08T04:23:00
---


These days I have been working on some coding stuff, and was stuck in some stupid, yet interesting defects.

<!--more-->

Here are two of them, that I still remember:

{{< highlight cpp >}}

    //..
    if ( 0x2400 & 0x2000 != 0 ) printf("you know this must be true...\n");
    //..

{{< /highlight >}}

The above is an abstraction from codes of one of my projects.

At the moment of coding I believe I certainly will see the line prints.

After I see nothing I believe there must be something wrong with my eyes.

Then I went to the restroom, took some water, and return to my seat.

Aha! I suddenly realized how mind absent I was:


the operator *&* is below *!=* in the [precedence table](http://en.wikipedia.org/wiki/Operators_in_C_and_C%2B%2B#Operator_precedence), which means *0x2400 & 0x2000 != 0* is actually *0x2400 & ( 0x2000 != 0 )* !

Well, I bet this precedence problem is just a piece of cake for a college student (or even a high school boy).

But when it comes to real life and real code...

Hmmm, I believe, truely, I need reviewing some of the text books.

I can feel the disdain on your face  ;-), so here comes a little bit more interesting one: (again it is a make-up model of the real code)

{{< highlight cpp >}}
    #define E_NOTIMPL 0x01234567
    //..
    class Base
    {
       public:
          virtual int foo() = 0;
          int newfoo() { return E_NOTIMPL; }
       //..
    }

    class Derived : public virtual Base
    {
       public:
          virtual int foo() {printf("Derived foo\n"); return 0;}
          virtual int newfoo() {printf("Derived newfoo\n"); return 0;}
       //..
    }

    int main()
    {
       Derived d;
       Base* pb = &d;
       pb->foo();
       pb->newfoo();
    }
{{< /highlight >}}

The problem of the code above is, i can't get the "Derived newfoo" at all. What is even quirky is, in my debugger I see no execution point in the line *pb->newfoo();*, which means line 24 doesn't even execute!

And again I had some (more) water and returned to the code:


in line 07 the keyword *virtual* is missing, thus there is no virtual table entry in the *Base* object, thus *Derived::newfoo()* can never be invoked...


but why line 24 is not executed?


because the code for an ARM platform has been compiled with specialized optimization and line 24 was skipped!


This is the whole story about bug fixing and, water drinking.

Apparently there is still a long way for me to grow into a competent programmer...

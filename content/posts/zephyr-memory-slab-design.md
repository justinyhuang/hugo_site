---
title: "Zephyr Memory Slab Design"
date: 2021-04-13T16:28:54-06:00
---

I have been always interested in zero cost abstraction designs and data structures. Recently I just found another great example while checking the [Zephyr project](https://github.com/zephyrproject-rtos/zephyr): the memory slabs.

<!--more-->

I don't think the Zephyr memory slab is the first/only implementation of such design but since it is what I have been played with, let's just give Zephyr the credit for this beautiful implementation.

The idea of a memory slab in Zephyr is simple: a memory pool is defined ahead of time with a known number of fixed size memory chunks linked together. Users can allocate one chunk at a time from the memory pool, use it and give it back to the pool when done. Once allocated, the memory chunk is nothing but an ordinary piece of memory. It is the free chunks that are interesting: they are connected like a linked list.

OK, linked list is nothing exciting: typically you have a simple struct that has a pointer to the data object, and the also a pointer to the next struct with the same type of data object. They could be singly, or doubly or even multiply linked lists, but that is pretty much it.

So what is special to the linked list/memory used in the memory slabs? It costs zero byte to implement the link.

The first thought that occurred to me could be No-Way, but after seeing how it is done, it becomes Oh-Of-Course. Just like some of the magic tricks.

The key here is the memory slab is not maintaining a linked list of any arbitary data, the item to be linked with is *free* memory chunks/buffers. And since it is freed memory, no one is using it and nor does the data in it matters.

And you've probably already guessed: we can implement the linked list *in* the memory chunks!

This is exactly what Zephyr does: A memory slab keeps track of the address of the first memory chunk to allocate (if any), and it saves the address to the next memory chunk in the begining of the first memory chunk, and so on.

So instead of

<pre>
   node  ->  node  ->  node  ->  node -> ...
    |         |         |         |
    v         v         v         v
 _______   _______   _______   _______
|  buf  | |  buf  | |  buf  | |  buf  |
|-------| |-------| |-------| |-------|
|       | |       | |       | |       |
|_______| |_______| |_______| |_______|
</pre>

where *buf* is a memory buffer, and *node* is a struct that needs some space to store the address of the buffer and the address of the next node,

in the memory slab the linked list looks like

<pre>
 _______   _______   _______   _______
|  buf  | |  buf  | |  buf  | |  buf  |
|-------| |-------| |-------| |-------|
|  addr-+-+->addr-+-+->addr-+-+->addr-+-> ...
|_______| |_______| |_______| |_______|
</pre>

where the *addr* (of the next freed buffer) is stored inside the current buffer.

By borrowing the freed buffers, memory slabs achieves zero cost (in space) implementation of a linked list.


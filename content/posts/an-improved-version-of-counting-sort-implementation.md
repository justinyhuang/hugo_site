---
title: An Improved Version of the Counting Sort Implementation
date: 2011-11-26T02:12:00
libraries:
- katex
---

<!--more-->

The "Introduction to Algorithm" provides an pseudo-implementation of counting sort.

    COUNTING-SORT(A, B, k)

    1  for i  ← 0 to k

    2      do C[i] ← 0

    3  for j ← 1 to length[A]

    4      do  C[A[j]] ← C[A[j]] + 1

    5  ► C[i] now contains the number of elements equal to i

    6  for i  ← 1 to k

    7      do C[i] ← C[i] + C[i -1]

    8  ► C[i] now contains the number of elements less than or equal to i

    9  for j  ← length[A] downto 1

    10    do B[C[A[j]]] ← A[j]

    11          C[A[j]] ← C[A[j]] - 1


It works well, but still there is room for improvement.
Below is another version that makes counting sort a bit more faster:

    COUNTING-SORT-2(A, B, k)

    1  for i  ← 0 to k

    2      do C[i] ← 0

    3  for j ← 1 to length[A]

    4      do  C[A[j]] ← C[A[j]] + 1

    5  ► C[i] now contains the number of elements equal to i

    6  n = 0

    7 for i  ← 0 to k

    8      for j ← 1 to C[i]

    9            do B[n] ← i

    10                n ← n + 1


Start from line 6 is where the two implementations differentiate.
when no optimization is taken into account.

In COUNTING-SORT line 6 to line 11 takes:

     k +  len(A) * 2 assignments, and k + len(A) pluses

In COUNTING-SORT-2 line 6 to line 10 takes:

     len(A) * 2 assignments, and len(A) pluses

The two implementations both cost

$$\Omega(len(A))$$

but in the latter the constant *k* is eliminated.

At the end is a short python test that gives a taste of difference in real performance on my 2.8GHz Win7 laptop:

{{< highlight python >}}

    import time
    def time_elapsed(func):
        def wrapper(*arg):
            t1 = time.time()
            res = func(*arg)
            t2 = time.time()
            print("time elapsed in %s: %0.3f ms" % (func.__name__, (t2 - t1) * 1000.0))
            return res
        return wrapper

    @time_elapsed
    def counting_sort(a, k):
        c = [0] * (k + 1)
        for j in a:
            c[j] = c[j] + 1
        for i in range(1, k + 1):
            c[i] = c[i] + c[i - 1]
        b = a
        for j in a[::-1]:
        b[c[j] - 1] = j
        c[j] = c[j] - 1
        return b

    @time_elapsed
    def counting_sort_2(a, k):
        c = [0] * (k + 1)
        for j in a:
            c[j] = c[j] + 1
        b = []
        for i in range(k + 1):
            for j in range(c[i]):
                b.append(i)
        return b
{{< /highlight >}}

Sorting with counting_sort_2:

{{< highlight python >}}
    data = [3,7,2,1,6,4,5,0] * 1000
    counting_sort_2(data, 8 )
{{< /highlight >}}

time elapsed in counting_sort_2: 22.000 ms

While doing the same sort with counting_sort:

{{< highlight python >}}
    data = [3,7,2,1,6,4,5,0] * 1000
    counting_sort(data, 8 )
{{< /highlight >}}

time elapsed in counting_sort: 36.000 ms

---
title: Encryption and Decryption - Note of Fundamental Number Theory II
date: 2011-06-07T07:03:00
markup: mmark
libraries:
- mathjax
---

The first thing we need to do in the main function is to prepare two groups of keys.

<!--more-->

The function setRSAKeys() helps to generate a public/private key pairs:

{{< highlight cpp >}}

 int setRSAKeys( key* publickey, key* privatekey)
 {
     //generate two 512bit primes p and q
     int p, q;
     srand((unsigned)time(0));
     getPrime(&p);
     getPrime(&q);
     //printf("p = %d, q = %d\n", p, q);

     //calculate n and phi(n)
     int n, phi_n;
     n = p * q;
     phi_n = (p - 1) * (q - 1);
     //printf("n = %d, phi(n) = %d\n", n, phi_n);

     //calculate e, a small odd relatively prime to phi_n
     int e = 1;
     do
     {
         e += 2;
     } while (1 != gcd(e, phi_n));
     //printf("e = %d\n", e);

     //calculate d, the multiplicative inverse of e, modulo phi_n
     euclid_coefficients ec ;
     int d;
     ec = Extended_Euclid(e, phi_n);
     d = ec.x;
     if (0 > d)
     {
         d += phi_n;
     }
     //printf("d = %d\n", d);

     //return (n, e) and (n, d)
     publickey->n = n;
     publickey->_key = e;
     privatekey->n = n;
     privatekey->_key = d;

     return 0;
 }
{{< /highlight >}}

In the function above, two primes are picked to compute $$n$$, which in reality is a gigantic number because $$p$$ and $$q$$ are big primes, say 1024 bits each. (again in this note we simplified the case by using small integers)

Then $$e$$ and $$d\ =\ \phi(n)$$ are obtained to form the primes: $$e$$is a small odd integer and falls into the public key set. $$d$$, another big number calculated based on $$n$$, is not very easy to guess (or reversed-engineering), and is therefore put as part of the private key set.

Euler's phi function: $$\phi(n)=n\ \displaystyle\prod_{p|n} \left(1- \frac{1}{p}\right)$$

where $$\phi(n)$$is the size of $$\mathbb{Z}_{n}^*$$, $$p$$ runs over all the primes dividing $$n$$(including $$n$$itself, if $$n$$is prime, too)

$$\mathbb{Z}_{n}^*$$is the **multiplicative group modulo n**, or defined as

$$\mathbb{Z}_{n}^* = {[a]_n}$$in $$\mathbb{Z}_{n} : gcd(a,n) = 1$$

where $$[a]_{n}$$= $$a'$$mod $$n$$, $$a'$$in $${Z}$$

e.g. $$\mathbb{Z}_{15}^*\ =\ \{1, 2, 4, 7, 8, 11, 13, 14\}$$, $$\phi( 15 ) = 15 ( 1 - \frac{1}{3} ) ( 1 - \frac{1}{5} ) = 8$$

When we select $$p$$ and $$q$$, which are primes and $$n\ =\ pq$$, The phi function becomes $$\phi(n)\ =\ (p-1)(q-1)$$ , much easier now =)

At the end of the above function we have set two groups of keys: the public key $$\{e,\ n\}$$and the private key $$\{d,\ n\}$$

With the public and private keys, encrypting and decrypting a message becomes easy.
Given a message $$M$$, to encrypt $$M$$we do: $$M^e\ mod\ n$$

the outcome of which, $$S$$, is the encrypted message.

To decrypt the secret, $$S^d\ mod\ n$$ will give you the original message.

$$d = e^{-1}\ mod\ \phi(n) = e^{-1}\ mod\ ((p-1)(q-1))$$ guarantees for the given $$p$$, $$q$$and $$e$$there will be a unique $$d$$. Therefore for a given public key $$\{ e,\ n \}$$, there will be one and only one private key $$\{ d,\ n \}$$. This is exactly what we want!

On the other hand, $$(M^e\ mod\ n)^d\ mod\ n\ =\ M^{ed}\ mod\ n\ =\ M$$ promises the encryption and decryption are irreversible operations.

To be continued...

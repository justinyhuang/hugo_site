---
title: Encryption and Decryption - Note of Fundamental Number Theory I
date: 2011-06-06T08:51:00
---

Reading the number theory has taken me a while.
And I guess, possibly, the most useful application of number theories is information encryption/decryption and digital signatures.

<!--more-->

This note lists the essence of elementary number theory (well at least I think so), by showing how basic encryption/decryption and digital signatures is done.
Here we go...

The [public-key cryptosystem](http://en.wikipedia.org/wiki/Public-key_cryptography) is based on the dramatic difference between the ease of finding large prime numbers and the difficulty of factoring the product of two large prime numbers.

Each participant in the system has a *public key* or/and a *secret key*. There are many algorithms for the public-key method, the [RSA public-key cryptosystem](http://en.wikipedia.org/wiki/RSA) is one of them. In the rest of this note we will only focus on the RSA method.

When applied in an encryption/decryption scenario, usage of the keys is depicted as below:

  Plain Message     ---Public Key---> Encrypted Message
  Encrypted Message ---Secret Key---> Plain Message

No one will be able to get the plain message from the encrypted one except for the one who holds the secret key.
Everyone who has the public key will be able to send the encrypted message to the secret key holder.

When applied in a digital signature case:


  Plain Message  ---Secret Key---> Signed Message
  Signed Message ---Public Key---> Plain Message


No one will be able to produce a 'fake' message with the same digital signature.
Everyone will be able to read the plain message sent with digital signature, as long as they have a public key.

When combining the two above, one can send a signed and encrypted message.

But how to make the keys and how the keys work?

In reality RSA requires playing with very big numbers, something like 1024-bit integers. For the sake of simplicity we are not going to stick with those big fat digits, instead small numbers are used just to show how things work. For serious code, you may find the [GMP](http://gmplib.org/manual/) library interesting.

OK, let's get our hands dirty with some C code.
First the top main function:

{{< highlight cpp >}}

    typedef struct key
    {
       int n;
       int _key;
    } key;

    #define MESSAGE_LENGTH 5

    int main(void)
    {
       /*prepare the keys*/
       key publickey, privatekey;
       setRSAKeys(&publickey, &privatekey);

       /* use long here to hold the encrypted message */
       long message[MESSAGE_LENGTH] = {'h', 'e', 'l', 'l', 'o'};

       /* Encryption - process message with the publickey */
       processMessage(message, publickey);

       printf("the encrypted message:\n");
       printMessage(message);

       printf("the decrypted message:\n");
       processMessage(message, privatekey);

       /* prints the outcome */
       printMessage(message);

       return EXIT_SUCCESS;
    }
{{< /highlight >}}

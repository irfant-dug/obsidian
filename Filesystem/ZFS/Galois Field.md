Évariste Galois (a 19th-century mathematician) invented a system where numbers "wrap around" a maximum limit, creating a closed loop called a Finite Field. You do this by dividing your overgrown number by a specific "prime" number and keeping the remainder.

In Reed-Solomon, shifting bits will caused bit overflow, which exceed the boundaries of a byte (8 bit). Galois Finited Field is a way to represent overflow bits as equivalent 8 bits, thus circumventing bit overflow.  This is done by XOR operation between the overflow bits and an irreducible polynomial 

## Galois Field Mathematics
 $$ x^8  + x^8 =  x^8  \oplus x^8 = 0 $$
 If we look at our irreducible polynomial, we know it represents the "boundary" of our closed loop. We set it to equal 0:

$$x^8+x^4+x^3+x^2+1=0$$

Now, using basic algebra, let's move x8 to the other side of the equals sign:

$$x^4+x^3+x^2+1=−x^8$$

Because addition and subtraction are the exact same thing in XOR math, −x8 is just x8:

$$x^8=x^4+x^3+x^2+1$$
Any time you shift a bit so far to the left that it becomes an x^8 (the 9th bit), it is mathematically identical to x^4+x^3+x^2+1. So, when a `1` falls off the 8-bit cliff, the computer doesn't throw it away. It replaces that fallen 9th bit with its exact 8-bit mathematical equivalent: **`0001 1101`**

## Irreducible Polynomial

There are 30 more irreducible polynomial of degree 8 that can be used, but the most popular would be *1 0001 1101* - used in Reed-Solomon, and *1 0001 1011* - used in AES, known as Rijndael polynomial.


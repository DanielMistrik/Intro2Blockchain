# Chapter
[Cryptography](https://techterms.com/definition/cryptography) is the study of securing
information derived from complex mathematics. This is a very general definition because
cryptography is a very large topic that goes beyond just CS. Cryptography is a fascinating
field that lies in the cross paths of computer science and mathematics and is the 
subject of active research. For those at Cornell who are interested in exploring
more of cryptography we recommend [CS4830](https://cornellcswiki.gitlab.io/classes/CS4830.html).

:::{note}
When we talk about cryptography in blockchain we are mainly concerned with 
cryptography's ability to secure and validate data like blocks and the data they store.
:::

Blockchain depends on two primary cryptographic devices:
* **Hashing** - Used to validate blocks in *Proof-of-Work* consensus, act as a signature of blocks and the 
backbone of merkle trees.
* **Digital Signature** - Used to verify individual participants signing off on a piece
of data and so validate data in blocks.

The two devices work in tandem and give blockchain trust even when the participants are
untrustworthy. In the previous chapter we left this definition without explaining the 
seeming paradox of a trusted system composed of untrusted nodes. Both of the devices 
above are rooted in theoretical math and so can be proven correct and valid. Not even
dishonest participants can forge them which means their use guarantees trust and validity.

## Cryptography Basics
Before we get into the intricacies of cryptography we need to understand some basic 
ideas that cryptography uses. First we need to understand the 'language' cryptography
operates in, *number systems* and how it manipulates this language, *bitwise operations*.

### Number Systems
As you may or may not know computers don't understand human languages, they understand
binary. Luckily thanks to countless very bright programmers and engineers who 
built a *layer of abstraction* we can use computers without knowing binary. But 
cryptography works at the very 'core' of computers and so uses languages they understand.

*Binary* is at the end of the day what computers really understand, it is the 
sequential stream of 1's and 0's that you see in many movies and images. While it is
now closely associated with computers it is first and foremost a *number system*.

*Number Systems* are how we represent numbers. You are most likely accustomed to 
the base-10(*decimal*) number system that has 10 numerals. We use it because it is the most 
intuitive as we have 10 fingers, but it is not the only number system out there.
The [Babylonians used a base-60 number system](https://www.thoughtco.com/why-we-still-use-babylonian-mathematics-116679)
and cryptography primarily uses *binary* and *hexadecimal*.

We use *binary*, or base-2 composed of 2 numerals 1 and 0, because it is what basic
building blocks of computers, *logic gates*, can work with. 

We use *hexadecimal*,or base-16 composed of the standard 10 numerals and the 6 letters
from 'a' to 'f', because working with binary is incredibly complicated. Hexadecimal
is more *terse* and is used frequently in  *lower-level programming*, programming
that works in languages computers understand more. 

We will see hexadecimal re-appear throughout the course as we examine the basic building
blocks of blockchain implementations like Ethereum. 

:::{admonition} Example
:class: tip
Below is the number 42 represented in three different number systems:
* **Binary**: 101010
* **Decimal**: 42
* **Hexadecimal**: 2A
:::

### Bitwise Operations
As with all languages, we need a way to manipulate them. Bitwise operations are 
operators on binary and imitate what *logic gates*, the basic building blocks of 
computers do. While their use-case isn't immediately clear they are an integral part of
computer science with [a wide range of applications](https://stackoverflow.com/questions/2096916/real-world-use-cases-of-bitwise-operators) 
including cryptography.

Bitwise operations work by taking to binary *streams*, sequences of 1's and 0's, of
equal length and calculating a new binary stream from them. They do this by analyzing 
the bytes at every individual position and depending on what operator they are
producing either a 1 or 0.

Bitwise Operators are not an easy concept, and we do not expect you to understand 
them fully. We only want to make sure you have a rough idea of how they work so when
we explain processes like hashing you will be able to understand it a little more
deeply. 

:::{admonition} Example
:class: tip
There are many bitwise operators, but we will be looking at OR and AND. 

**OR** - Produces a 1 if either or both input bits are 1 and 0 otherwise. If we had 
input streams *1010* and *1100* the OR operator would output *1110*. This is because 
when the operator looks at the first bit of both streams it sees it has two 1's and 
so produces another 1, the second bit in the streams is a 1 or 0 which produces a 1, 
the same goes for the third bit but for the fourth and final bit they are both 0 so the 
OR operator produces a 0.

**AND** - Produces a 1 if both input bits are 1 and 0 otherwise. Therefore, if we 
had input streams *1010* and *1100* the AND operator would output *1000*.
:::

## Hashing
A formal definition of hashing is a *hash function* which takes an input of arbitrary
size and produces an output value, a number, of fixed-size, the *hash digest*. Hash functions
are very useful. They appear in data structures, through Hash tables & Merkle
trees, password verification and ofcourse data signatures. 

There are alot of implementations of hash functions but good ones should share the
three following characteristics:

* **Deterministic** - A given input will *always* produce the same output for the same
hash function implementation.
* **Trap-door Function** - You can quickly and easily take an input and calculate the 
output, the *hash digest*, but you cannot take the *hash digest* and quickly find
what the original input was. You could say the hash function is not *invertible*.
* **Collision Resistant** - The number of inputs that share the same output, known 
as a *collision*, should be as small as possible. Ideally one input maps to one output but as 
the output is fixed-size and input can be anything even the best hash functions are
bound to have collisions.

:::{tip}
The three properties of good hash functions are incredibly important. They are the 
properties that make hash functions so useful, so they are definitely something
to remember.
:::

### How it Works
Hash functions typically work with binary but the hash digest is often represented as 
a hexadecimal due to terseness. Implementations always try to provide the three
properties of good hash functions but how they achieve this varies greatly.
We will be looking at SHA-256 because it is the most widely used and 
is also Bitcoin's hash function.

:::{note}
Alot of CS textbooks give examples of very simple hash functions (i.e. converting data
to a number and the output is that number times two). We disagree with this approach.
These kinds of simple hash functions don't have the three good hash function properties and
as such are never used in the real-world making teaching them redundant. We will be showing
you SHA-256 which is incredibly difficult and incredibly useful. We do not expect you to understand it.
We are putting it in this textbook merely, so you are *aware* of its inner-workings, and you can rest 
assured that for 99.5% of blockchain projects you will not have to understand it fully.
:::

From a simplified perspective, SHA-256 works as follows:
1. Convert the input data into binary
2. 'Pad' the data, add 0's to it, and add its original length in binary to it so its
final length is divisible by 512, that is its length is 512k bits where k is some integer.
3. Divide the entire binary sequence into 512 bit long 'chunks'.
4. Taking each chunk one-by-one first divide it further into 16 32-bit 'windows' and then
add another 48 32-bit 'windows' initialized to 0.
5. For the 16 non-empty words run some logic and the XOR bitwise operator on them and 
put the results in the 48 empty words.
6. Starting with 8 pre-defined hash values modify them by compressing the 64 words into them
through XOR and bit-logic. These 8 hash values will only have pre-defined values when 
the words of the first 'chunk' are compressed into them. After that the results of the
compression are changed. If the results are greater than 2^32 the modulo operator is applied
so they are always smaller than 2^32.
7. After we iterated over all the chunks append the 8 resulting hash functions, convert
them to hexadecimal, and you have your hash digest.

We warned you. This is not simple in the slightest and why these steps are the way they
are is far beyond the scope of this textbook. If you want one thing to take away from
all of this then know that hash functions use bitwise operations, logic and math to guarantee
the three good hash function properties.

**SHA-256** is the hash function we described above, and it is part of the SHA-2 family
created by NIST in 2001, and it has a 256-bit hash digest. Its notable use-case in Blockchain
is Bitcoin. 

**Keccak-256** is Ethereum's hash function. It was the *original* version of NIST's 
SHA3 before it was edited by NIST in 2013. These edits caused some concerns that 
NIST was engineering a *backdoor* in SHA3 so the creators of Ethereum decided to use 
the raw unedited SHA3 called Keccak-256. Like SHA-256 it has a 256-bit hash digest.

### Merkle Trees

### Block Validation in PoW

## Public-Private Cryptography

### Digital Signature

:::{admonition} Example
:class: tip
In the first chapter we looked at an example with a blockchain tracking how much
blocks each participant had. There we just assumed that the data about the block 
movements was valid, but now we will analyze how we can guarantee that. 


:::


### PPC Implementations

## Cryptography in Action

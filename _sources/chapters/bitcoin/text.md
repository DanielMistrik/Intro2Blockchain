# Chapter
On Halloween 2008, a month after the more than a hundred years-old investment bank
[Lehman Brothers filed for bankruptcy](https://www.investopedia.com/articles/economics/09/lehman-brothers-collapse.asp)
a user calling themselves *Satoshi Nakamoto* published a whitepaper on a
crypto mailing list. This whitepaper described an electronic peer-to-peer
currency called *Bitcoin*. The genesis block was published in January of 2009
and Bitcoin came to life.

:::{note}
You might be confused why we referred to Bitcoin's creator in plural. This
is because *no one* knows who Satoshi Nakamoto actually is. It could be 
a single person, a group or even a company, we just don't know. Many have tried
to uncover this secret but have come short. For those interested in learning
more about Satoshi Nakamoto we recommend *The Book of Satoshi* by Phil Champagne.
:::

Although Satoshi never mentioned the word in the whitepaper, Bitcoin introduced
us to Blockchain. This was by no means an accident. Bitcoin's goal was to
decentralize finance and in the aftermath of the 2008 financial crisis the 
idea had backing. The whitepaper argued that the financial system being a
*centralized* one was a problem and that a *distributed* system would be 
advantageous. The blockchain solution therefore addresses the drawbacks of a 
distributed system to make it usable.

Bitcoin's blockchain was therefore created to support decentralized token
exchange. Participants could exchange the blockchain's native token, *Bitcoin*,
without the need for any central authority. For much of 2009 the blockchain
was in the realm of enthusiasts as the token didn't even have a trade-able value.
This changed in October, a little less than a year after the whitepaper, when
the first exchange of Bitcoin was launched offering 1,305 BTC for 1$. The
cryptocurrency continued its rise and eventually became the behemoth we know it
as today. 

Bitcoin was the genesis project for Blockchain and as of 2022 it is still the 
most popular project. Understanding Bitcoin will help us get a better 
perspective on blockchain. In this chapter we will go into the details of 
the Bitcoin implementation.

## Bitcoin Summary
Bitcoin is a *PoW* public blockchain that is created to support the exchange
of its token. A run-down of the project is as follows:
<br/>**Token**: The Blockchain's native token is *Bitcoin*. The maximum 
supply of *Bitcoin* is capped at 21 million.
<br/>**Participant Rights**: Bitcoin is completely open and permissionless
so anyone can join as a participant and have full read & write rights.
<br/>**Consensus Algorithm**: Bitcoin uses Proof-of-Work with SHA-256 hash
puzzle. The difficulty is readjusted every 2 weeks to keep *ABCT* constant.
<br/>**Data**: Bitcoin data is token exchanges called transactions.
<br/>**Average Blockchain Creation Time (*ABCT*)**: 10 minutes.
<br/>**Mining Incentive**:The block creator is rewarded with the native token
*Bitcoin* from transaction fees and a general block reward.

While Bitcoin is a network that can be accessed through a publicly-known 
*port* it is typically run on a software implementation called *Bitcoin
Core*. There are multiple implementations out there, you could even create
your own, but 96% of nodes use *Bitcoin Core*. The software manages a bitcoin
full node and typically makes all the choices for the node. Its development
history can be traced all the way back to Satoshi Nakamoto and is now hosted
as an open-source project on GitHub.

## Bitcoin Block & Transaction
Bitcoin's block is around 1MB with the header being 80 bytes. Most of the 
space is taken up by transactions. The block header is composed of:
* **Version** - The version of the software implementation like Bitcoin Core.
* **Previous Block Hash** - The link to the last block in the blockchain linked list.
* **Timestamp** - The unix timestamp for when the block was produced. Can be 2 hours
off and still be valid.
* **Merkle Root** - The root of the merkle tree containing the transactions in the block.
* **Difficulty** - The difficulty of the hash puzzle. Nodes use this value to 
calculate the target.
* **Nonce** - The input field miners can use to change the block header so it
solves the hash puzzle. 

The bitcoin block header is very bare-bones. This is because space is incredibly
limited so any redundancy is removed. When every node, regardless of type, has
to store every block header, with a new one being added every 10 minutes, even 
a single byte adds up. 

:::{tip}
Alot of blockchain projects differ on the block size. This is because it's
a very sensitive issue as **space vs throughput** is a hotly debated topic in
the blockchain world. Some original contributors to Bitcoin got into a bitter
argument about whether Bitcoin's block size should be increased. This feud is
covered in detail in *The Block Size Wars* by Jonathan Bier.
:::

Bitcoin's data is transactions and transactions are concerned with token exchange. Each transaction
is composed of the following:
* **Version** - Like in the block header the version denotes the version of the
software implementation and so the rules the transaction has to follow.
* **Input Counter** - The number of inputs in this transaction.
* **Inputs** - A list of inputs used in this transaction.
* **Output Counter** - The number of outputs in the transaction.
* **Outputs** - A list of the actual outputs used in this transaction.
* **LockTime** - A field that prevents this transaction from being used for 
a given period, largely deprecated except for the Lightning Network. 
* **Delegated Witness** - A Segwit feature that is out of the scope of this textbook.

Transactions are often bigger than the block header itself because there is
a fair level of complexity in them. One of these complexities is deciphering
what inputs and outputs are. For the following paragraph think of them as simply
where the bitcoin is coming from, inputs, and where the money is going, output.

To validate a transaction we need to check the following:
* Are the inputs valid? (We will discuss this shortly)
* Are Inputs > Outputs?

The order in the list is arbitrary we can do either one first. You might be
wondering why the latter point isn't Inputs=Outputs. This is because the difference
between the inputs and outputs forms the transaction fee. The transaction
fee is the incentive for the miner to include your transaction in their 
block as often there are more transactions than space in blocks. Transaction
fees can be 0 and the transaction will eventually be included in a block 
but that could be several hours, if not days, after the transaction was
first broadcast.

Bitcoin transactions can be ordered within the merkle tree in any order
the miner wants except for the first one, the *coinbase transaction*.

:::{tip}
Yes, that is where the popular crypto exchange got its name from. 
:::

If you remember the last chapter, we talked about how one part of the 
mining incentive is a piece of data in every block that is inherently correct.
In Bitcoin this is the coinbase transaction. The coinbase transaction
is how the miner rewards themselves for winning the hash puzzle and publishing
a block. The input of the 
coinbase transaction is irrelevant, it's often the place of the extra nonce. 

:::{Note}
As Bitcoin mining got more competitive miners were discovering that the 2^32 
possible nonce values solved the hash puzzle. They then started to change
the timestamp as time passed by but sometimes not even that was enough. This
is when the data in a coinbase transaction started to be used as an *extra
nonce solution*.
:::

While we don't need to verify coinbase inputs we need to check that the miner has not
rewarded them-self with too much bitcoin. The coinbase transaction output has to
equal the block reward, which is well-known by the network, and the sum of
all the transaction fees of the transactions in the block. If the output is 
larger than that the whole block is rejected. 

## UTxO Model
We will now talk about the inputs and outputs. 

When we think of managing 
money we think of having an account with a balance. This has the benefit 
of being a simple way to think about one's finances, but it has a drawback: space.
Every account, no matter how big or small, needs to have an account and balance
and this leads to redundancy. This is a big problem for the space sensitive
blockchain so Bitcoin decided to only record unspent payments. This way accounts
with no payments or an empty balance won't fill the blockchain.

:::{note}
Bitcoin's model isn't the only one used by blockchains. Projects like 
Ethereum use the standard account & balance model known as the *state model*.
:::

To do this Bitcoin reimagined what a payment is and how we store money. In 
Bitcoin a transaction is just a collection of inputs and outputs. Inputs 
are former outputs that are assigned, via a digital signature, to a 
particular entity. Outputs are a certain amount of Bitcoin being assigned
to a new entity. Your 'balance' is now the sum of all the outputs you 
are yet to put into a transaction. These outputs are known as *unspent
transaction outputs* and give this finance-tracking model its name, the *UTxO
model*.

:::{tip}
By entity we just mean an address that has a public/private key. This 
'entity' could be an individual, group or even company.
:::

<img align="right" width="190" src="images/Output.png">

Bitcoin outputs are relatively simple. They are arranged in a list, where order matters,
and are composed of an amount and *lock script*. This lock script is a 
script verifies the digital signature of the entity it was issued and has to 
be *unlocked*, the digital signature must be verified, for the output to be
used as an input.

<img align="right" width="190" src="images/Input.png">

Bitcoin inputs are a bit trickier. They are composed of the hash of the
transaction they were an output in, their index in the list of outputs of that
transaction and the unlocking script that proves the digital signature. Each
input must reference a single *unspent* output which means this input is
the only input, in the entire blockchain, to reference the output.

Inputs and outputs therefore have a *one-to-one* relationship. This means
that unspent outputs, from now on *UTxOs*, can only be spent in full. But
what if we want to send a payment with an amount that none of our *UTxOs*
can sum to? The answer is in the fact that a single transaction can have 
multiple inputs *and* multiple outputs. To solve our problem we just collect
*UTxOs* that sum to more than we want to pay and anything left over after
the payment and transaction fee is made into a separate output addressed
to ourselves, a refund output.

:::{admonition} Example
:class: tip
Alice wants to send Bob 30 bitcoins with a 1 BTC transaction fee. She
has the following unspent *UTxOs* to choose from:

| *UTxO* | Amount (BTC) |
|--------|--------------|
| A      | 5            |
| B      | 30           |
| C      | 50           |

Alice will be spending a total of 31 BTC and the closest sum **that is greater
than that** is A + B. A&B sum to 35 BTC so Alice will also be creating
a 4 BTC refund output. We can therefore express the inputs & outputs as follows:

<ins>Inputs:</ins>

| Output | Digital Signature |        
|--------|-------------------|
| A      | Alice             |
| B      | Alice             |

<ins>Outputs:</ins>

| To    | Amount (BTC) |        
|-------|--------------|
| Bob   | 30           |
| Alice | 4            |

This transaction would be valid as the sum of inputs is 35 and the sum of 
outputs is 34. The 1 BTC difference is the transaction fee paid to the miner.
Alice has now lost *UTxOs* A&B and her balance has dropped from 85 BTC to 
54 BTC with a new *UTxO* of amount 4 BTC.
Note the table representation is an oversimplification.
:::

Using the UTxO has a number of implications:
* **Flexibility** - Outputs and inputs can be used when the locking/unlocking
script is satisfied. Bitcoin has a number of locking scripts with the 
most popular being [p2pkh](https://learnmeabitcoin.com/technical/p2pkh) which
uses addresses. But different locking scripts work with multi-signature addresses
and even bare public keys.
* **Privacy** - Outputs in a single transaction can have different recipients and
inputs can be unlocked with different digital signatures.
This has the benefit that we can disperse our funds over a number of addresses
increasing one's anonymity as it is more difficult for someone to track
our balance.

:::{note}
A *Dust Attack* is an attack vector that tries to weaken the latter feature.
We will cover this in detail in the following chapters.
:::

**P2PKH** (Pay 2 Public Key Hash) scripts work through Bitcoin addresses. 
Bitcoin addresses are, like classic addresses, hashed public keys but in 
Bitcoin they are encoded in *Base 58* format to make them more readable. 
All Bitcoin addresses begin with a 1 to make them easy to differentiate.

:::{note}
**Base 58** format is an encoding format that is a more-readable version of 
*Base 64*. Base 64 is a number system with 64 numerals. They are represented by all the 
capital letters, un-capitalized letters,numerals 0 to 9 and + and /. Base 58
takes characters that are frequently confused like, 0 and o, and has a built-in
check that detects if you wrote it wrong.
:::

## Bitcoin Script
Reviewing what we now know about transactions and *UTxOs* data(transaction) 
validation in Bitcoin breaks down into 2 steps:
* Is the sum of outputs smaller than that of inputs?
* Are all the inputs unlocked correctly?

If the answer to both is yes then the transaction is valid. But how does
the network actually check this? While it may be tempting to say that the 
software implementation, like Bitcoin Core, solves this that is not the case. 
Because the network has to be unanimous with all of its decisions' data validation
is done through a single programming language, *Script*.

**Script** is a low-level stack-based programming language that does
the processing nodes need to do for Bitcoin. Script, which is reminiscent of
the old programming language Fortran, was originally developed by Satoshi
Nakamoto. Script is used for transaction verification and where locking/unlocking
scripts exist. It is *non-turing complete* which means what it can do is limited.
This might seem like a drawback but is actually a very intentional feature
to prevent a malicious attacker from creating very complicated Script code
that could slow the network down. 

:::{admonition} Stack
Stacks are a very important low-level data structure in CS. They are a 
*FIFO* (First-In First-Out) list where you add and take from,*pop*, from the
top of the stack. You can liken it to a stack of pancakes. You add the pancakes
on top and have to first eat the pancakes on top. 
:::

Script is a programming langauge that isn't like the languages you might know,
such as Python or Java. Script is made up of operation codes, represented through
1 byte of hexadecimal. Script works by adding these *op codes* onto the stack
along with any data, like addresses, the *op codes* need to work with. The *op 
codes* then take the data from the stack and produce a new value. The *op
codes* at the top are executed first.

Below is an example of the code for a p2pkh locking script:
```
OP_DUP OP_HASH160 62e907b15cbf27d5425399ebf6f0fb50ebb88f18 OP_EQUALVERIFY OP_CHECKSIG
```

Script is not intuitive and is very limited for the sake of efficiency. Most
bitcoin developers will rarely, if ever, interact with Bitcoin Script as software
implementations are expected to be the only ones that deal with it. 

## Bitcoin Operation

### SPV Nodes
*Simplified Payment Verification* nodes are Bitcoin's light nodes. They only store
the block headers and are able to prove transactions are recorded in the
blockchain through merkle paths provided by full nodes. They were included
in Bitcoin's whitepaper and are a solution to how space-limited parties
can actively participate in the blockchain.

:::{note}
SPV nodes heavily utilize something called *Bloom Filters*. If you have ever
taken Cornell's [CS2112](https://cornellcswiki.gitlab.io/classes/CS2112.html) this should 
be a refresher. 
:::

## Miscellaneous 
# Chapter
Throughout this textbook we have tried to build our blockchain step-by-step. For Ethereum, 
we can't quite do this. Ethereum is complicated, it achieves functionality through a complex
implementation that we will only scratch the surface of in this chapter.

## Ethereum Summary
Ethereum, unlike Litecoin or ZCash, is a blockchain completely separate from
Bitcoin. It was one of the first unique blockchain implementations after Bitcoin and 
as of 2022 is the second-largest blockchain by market valuation.

Ethereum's blockchain summary goes as follows:
* **Token** - The blockchain's native token is *Ether*.
* **Access Rights** - Ethereum is a public blockchain where anyone and everyone can become
a node.
* **Consensus Algorithm** - Ethereum currently uses a *PoW* algorithm solving a Keccack-256 
hash puzzle but plans to transfer to *PoS* in Ethereum 2.0 .
* **Mining Reward** - Miners who publish valid blocks, even if they are in an unsuccessful 
fork chain, are rewarded.
* **ABCT** - On average 20-30 seconds.
* **Data** - Data records changes to Ethereum's state. This can be essentially anything. 

Now that we have a vague summary of the blockchain, lets look at why it was created at all. 

### Background
In 2013 Bitcoin was one of the few unique Blockchains, there were many duplicate blockchains 
that did nothing new but just had a different genesis block & data. Blockchain, however, was
becoming more popular and people wanted to do more with it than just a currency token. It was
also 2 years ago since Bitcoin's mysterious creator *Satoshi Nakamoto* disappeared leaving 
Bitcoin without a leader to push new changes onto the blockchain. Bitcoin was at this point a 4-year-old
project with increasingly stale technology and limited functionality, something more was wanted.

:::{note}
Colored Coins were the blockchain's first 'nft'. They worked on Bitcoin script being able to 
encode small pieces of data. They were then transferred like normal transactions and could
represent physical assets like movie tickets. The projects were that successful due to 
Bitcoin's limited functionality. One of the contributors to Colored Coins, Vitalik Buterin, went
on to become a key developer for Ethereum. 
:::

That year a Canadian-Russian programmer by the name of Vitalik Buterin published a whitepaper
for a Turing complete blockchain that promised to be the universal blockchain Bitcoin never was.
Called 'Ethereum', the project was further developed by Dr. Gavin Wood in 2014 when he published
its yellow paper. The yellow paper introduced much of Ethereum's unique technical concepts like 
the *EVM* and *gas*. The project was then developed with funding from an *Initial Coin Offering* that 
raised 18 million dollars. In 2015 the project was released under the 'Frontier' version. 

:::{tip}
Confused about what 'Whitepaper', 'Yellow Paper' mean and what's the difference between the two?
Whitepaper is the first publication about a project and goes over the general concept. Yellow paper
on the other hand is a more concrete description of the projects actual implementation, often alot
of math and cs is involved. 
:::

Since its release Ethereum has embodied the Blockchain's universality by being the center-stage
for some of Blockchain's biggest trends such as DeFi, DAOs and NFTs. Ethereum is a different
and arguably newer version of the Blockchain compared to Bitcoin. Because of this reason
many people call Ethereum a *second-generation* blockchain while Bitcoin is a *first-generation*
blockchain. 

### Ethereum's Goals
While the previous section might have presented Ethereum as a single-goal oriented project, this
is not the case. The creators had large plans with Ethereum, these can be summarized by the 
following:
* A platform for decentralized applications that can be created easily and can communicate between each other. 
* A blockchain that is universal in its functionality and not bound to a particular use-case. 
* An open network where anyone can create applications and interact with other participants on the network.
* A place where developers can publish applications without the risk of censorship, blackouts and fraud.

These goals are far-reaching but can generally be defined as creating a *Global Censorship-Resistant
Computer'. This is a powerful idea, and it warrants more investigation. 

**Global Censorship-Resistant Computer** (GCRC). GCRC is a grand concept that requires a 
trusted distributed system to work. This makes a blockchain an ideal technology to use in 
constructing a GCRC. To fully appreciate how the blockchain fits into a GCRC we have to go
over what we need to build a GCRC:
* **Storage** - We need immutable global storage that can record all the activities of 
Ethereum’s GCRC. Everyone needs to agree on what this storage should hold and how to verify it. 
The Blockchain is perfect for this. 
* **Processing** - We need a way to agree on what to process, process it and verify the process. 
We will do this with a *virtual machine* every participant in the network will run. 
* **Programs** - We need something to process, programs. 
These programs need to be adapted to work on our GCRC and are called *smart contracts* and 
can only run for limited amounts of time. 

One key idea to take from this list is that blockchain is *only one part of the solution*. Saying this
we must remember that it is the solution's backbone and without it there would not be a solution.
As we explore different implementations we will see that blockchain is often used with different
technologies to create a solution. Bitcoin is arguably the most 'pure blockchain' project we will
see so this is a new concept for us.

## The Ethereum State Machine
Ethereum fulfills the requirements of a GCRC by being a *distributed state machine*. This is a 
very important definition, so we will define it word-by-word:
* **Distributed** - A machine that is part of a decentralized system.
* **State Machine** - A machine that has a state, it remembers past events. A common example of state machines 
are computers, i.e. it remembers saved files. To better understand ethereum we will have a more
strict definition of a state machine: A machine that has a state and that state changes based on input.

The *state* in question is any and all data stored on the Ethereum blockchain and our input are
the transactions that can alter this data, state. 

:::{note}
This definition is a very theoretical/mathematical one. This shows Ethereum as being defined
in a far more *formal* way than Bitcoin. Bitcoin was introduced in a brief paper that introduced Bitcoin's
goals but largely glossed over technical details, Ethereum did more. In its [*yellow paper*](https://ethereum.github.io/yellowpaper/paper.pdf)
Ethereum was explained in exact detail, and mostly through equations rather than text.
:::

### Transactions
Simply put, transactions are what changes the Ethereum input. More formally transactions are 
one argument in the Ethereum state-change equation:
$$
    Y(S,T) = S' 
$$
In the equation above T is the transaction, S the old state, S' the new state and Y the EVM or 
state transition function, more on this later. This equation is crucial for understanding Ethereum
as it tells us that Ethereum is not just a cryptocurrency or a token exchange but can describe anything
that you can write in an arbitrary state, in other words something truly universal.

Ethereum transactions embody this universality and, unlike their Bitcoin counterparts, can do more 
than just a simple token structure. This can be seen in their structure which is composed as follows:
* **Nonce** - The unique identifier of transactions in-lieu of unique inputs & outputs
* **Gas Price** - Price of Unit Gas. Similar to a transaction fee, will be discussed in detail later
* **Gas Limit** - Amount of Gas available, has to be greater or equal than 21,000
* **Value** - Amount of Ether transferred, can be optional
* **To** - Recipient of the transaction
* **Data** - Messages for smart contracts in bytecode, we will unpack this statement later
* **v,r,s** - The ECDSA signature of the sender, this also identifies the sender.

As you can see there is no input or output. This means transactions have to be made unique by something
else, if they weren't made unique then someone could repeat the same, valid, transaction over and over again.
In Ethereum's case this is done with a *nonce* which is a transaction counter 1-indexed, this means
a transaction with a nonce of 150 is the 150th transaction from the account. The lack of an input
and output also means that Ethereum doesn't follow the UTxO Model but rather the *State Model*. This comes
back to Ethereum's universality through its universal state.


### Ethereum State
#### State Model

### Ethereum Blocks
### EVM 
{Explain what a Virtual Machine means}
#### EVM Bytecode
## Smart Contracts
### How Smart Contracts Work
### Examples
## Ethereum Maintenance
### Mining
### Gas
### Reaching Consensus
## Serenity (Ethereum 2.0)
{Inlcude Pros & Cons of Eth here}

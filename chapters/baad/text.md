# Chapter
When talking about blockchain we are talking about a system.

:::{note}
What is a system? [Merriam-Webster](https://www.merriam-webster.com/dictionary/system)
defines *system* as "interacting or interdependent group of 
items forming a unified group". This isn't the best, or most 
concrete, definition so for this textbook we define system sa *a 
group of computers, nodes, trying to work on a singular application.* 
This singular application can be as big or as small as it wants.
:::

System architecture is a whole course in itself, at
Cornell its [CS4410](https://cornellcswiki.gitlab.io/classes/CS4410.html),
so we will not be going into any sort of depth on the topic.
All we need to know that systems can be structured in three basic
ways:
* **Centralized**: The whole network revolves and depends on
a 'leader' node (i.e. Facebook).
* **Decentralized**: The whole network revolves and depends on
a *group* of 'leader' nodes (i.e. Tor).
* **Distributed**: A network composed of equal *peer* nodes 
that are variously connected between each other. There is no
'leader' node (i.e. I.o.T).

:::{tip}
The definitions of decentralized and distributed are often 
used interchangeably. You might find blockchain being called
distributed on website and decentralized on the next. Formally
the two terms are different but for 95% of sources on the
blockchain, including this textbook, they mean the same thing
which is that the system isn't centralized.
:::

The three types all have their uses and there is no one type 
that is better overall. Saying this we have to concede that 
an overwhelming majority of consumer-facing applications are
built on centralized systems. 

Why? Centralized systems are easy to control and run, so they 
are often the first choice for already centralized companies. 
But centralized systems aren't perfect.
* Centralized systems are fragile in that if the 'leader' node
falls so those the whole system. 
* Censorship is far easier to implement on a centralized system
than on one of the others.
* Centralized systems have limited privacy.

A good example of how centralized systems can go wrong was [Facebook's
2021 outage caused by a maintenance failure on their centralized servers](https://blog.cloudflare.com/october-2021-facebook-outage/).

Distributed and decentralized systems largely address the above, 
so we might think they are the solution. But they, too, have problems.
A *Byzantine* problem.

## The Byzantine General's Problem

<img align="right" width="200" height="200" src="C:\Users\danie\CS1998-Textbook\Github\Intro2Blockchain\Images\Byzantine_Generals.PNG">

Formally, the Byzantine General's Problem (*BGP*) is a *game theory* problem
plaguing distributed systems. The problem goes as follows:

A group of generals besieged the city of Byzantium. To take the 
city they have to attack all at once BUT:
* The Generals have unreliable messengers who might have been
infiltrated by Byzantium.
* Some generals may be traitors.

There are many adaptations of this problem but the issue is the 
same: How do you work with a group of peers you don't trust 
and communication between you is unreliable. 

This problem affects distributed systems because of the equality of nodes. 
There is no gatekeeper, anyone can join the system and communicate
with whom ever they want. This means your peers may be malicious 
actors trying to subvert the system, or they may be honest nodes that
just have very bad internet, so they get only some of the
critical messages needed to support the system. 

Although *BGP* is at heart a theoretical problem it has 
a number of real-world applications. These include:
* Communication between nuclear submarines
* An international team of developers trying to build 
a product (i.e. Whatsapp)
* The Internet

One of the largest distributed/decentralized systems is after all
the internet. Anyone can host a website and anyone can
visit and this made managing payments very hard. Some websites
use credit cards, but these can be misused by malicious websites, 
while others just trusted the customer would pay when the product would
physically be delivered. In short *BGP* meant there was no clean 
solution to online payments.

## Blockchain as a solution
On Halloween in 2008 a solution to the payment problem was announced
under the whitepaper *Bitcoin: A peer-to-peer electronic cash system*.
Although the author never used the term, the whitepaper created *Blockchain*, a 
technology and data-structure that solved *BGP*.

### Storing the Data
The remainder of this chapter will be looking at a 
subset of the solution: storing the information the 
generals send each other. The following two chapters 
will complete the solution.

We need a *data-structure* that can store information 
among *trustless* participants. This data-structure 
needs to address the following:
* How the actual data will be structured.
* How will data be added/edited.
* How will we make sure the data is valid.
* How will we make sure everyone has the same set of data.

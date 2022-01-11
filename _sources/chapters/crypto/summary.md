# Summary
In this chapter we covered what cryptography is and how it acts as blockchain's backbone
guaranteeing its validity when participants are working in a trustless network. 
* Blockchain relies on two main cryptographic methods: Hashing and digital signatures/encryption
* Hashing is the process of taking an input of varying length and assigning it a unique fixed-sized output, a hash digest.
* Hashing allows Merkle Trees to exist and is also the basis for the consensus algorithm called Proof-of-Work
* PoW achieves consensus through an open competition where participants try to solve the hash puzzle, an action
that is difficult to achieve but very easy to verify.
* Digital signatures verify that a piece of data originates, or is attested to, by a given source.
* Digital Signatures depend on Public-Private Cryptography which are algorithms
composed of two keys, public and private, that can encrypt and decrypt each other's messages.
* PPC has two primary uses: one is digital signatures while the other is encrypted p2p communication.
* Large Blockchain projects use a PPC method called ECDSA because of its small size and difficulty to crack
* Most blockchain projects use the concept of an address, the public key after hashing, to represent participants on the network.
* Cryptography largely depends on true randomness which is difficult for computers to achieve on their own.
* Coming back to the Byzantine Generals Problem, cryptography allows 
the generals to verify messages and start achieving consensus.
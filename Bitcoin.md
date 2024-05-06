A peer-to-peer electronic payment system designed in such a way that it can operate decentralized.

## Motive

The interest of its creator was to create a list of protocols that allowed a system of decentralized transactions where a transaction did not have to be validated by a single central authority.

## Proof-Of-Work

The idea of proof of work is to use computational work as the method of determining the legitamacy of a message in the network. 


## BitCoin Protocols

There are a few protocols in place when dealing with Bitcoin:
* Each owner(node in the system) accepts the longest set of blockchain in the system.
* Each transaction or block has to include a number guessed to have a particular number of 0's in its first digits of its SHA256 function's output.
* Each transaction is signed by a digital computer signature.
## Block Chain

Each person has a transaction record which details a record of bitcoin transactions in the system. These transaction records are linked together via the previous block's hex codes. 

Each block contains a few things:

* Block Number
* Nonce
* Transactions
* Hash of previous block
* Hash of current block

The nonce is used as proof of work. For each block to be accepted, it has to find a nonce number that given the block number, transactions and the hash of the previous block, it can generate a hash with a predeterminent amount of zeros at the beginning.

*The probability that a hash code has n amount of zero at the beginning is really low.*

When a transaction occurs in the system, it will be notified to every node in the system and each of them will update their own copy of the blockchain to include that block as well. 

In the case where a block is different than other blocks in the system:

* If one chain is longer, the longer chain is accepted.
* If the chain is of equal length to the rest in the network, if there is a consensus on which hash block that current block is for the rest of the nodes in the network, then the majority hash is the correct and unmodified one.


---
layout: post
title: "Blockchain explained"
date: 2017-10-25
---

Well. Finally got around to putting this old website together. 
Neat thing about it - powered by [Jekyll](http://jekyllrb.com) and I can use Markdown to author my posts. 
It actually is a lot easier than I thought it was going to be.

What is blockchain?

Blockchain is a new technology invented by satoshi nakamoto when he introduced his white paper on the cryptocurrency bitcoin.
https://bitcoin.org/bitcoin.pdf

Blockchain is the fundamental technology that underpins bitcoin and when explaining blockchain it is hard to not talk about bitcoin at the same time.

The blockchain itself is a public distributed database, that runs across an infinite number of nodes (computers).  It is an add only database, you cannot edit or delete any blocks (records) from the chain (database).
What makes it unique is that each node on the system holds the entire copy of the blockchain, everyone can see all transactions that take place.

In bitcoin, the blockchain is constantly being synchronised, and this takes time, currently 10 minutes in bitcoin to add a block, so when reading the blockchain you have to understand that it is not in real time, there is about 1 hour lag time before your snapshot of the blockchain is the current valid one after adding a transaction.

Bitcoins blockchain is a shared ledger, that records transactions of money between two parties.  I like to think of a bitcoin transaction the same way you complete a cash transaction, it is between two parties, that agree a price and trust each other.
This is only my opinion but i can see bitcoin being used in transactions where you get the goods first, or you exchange the goods/service at the same time as paying for the transaction.  I don’t see it being used in an online transaction where you pay for a good/service and then trust the other party to send you the goods at a later date.  This is why ebay/paypal is so popular when buying online, if the seller does not send a good, then paypal will give you your money back, and infact paying with a credit card also has some form of recall when buying goods online, bitcoin does not.

What it does do in the circumstances when you would like to use bitcoin is cut out the middleman in transaction, i.e.. the credit card company/paypal, which may charge high fees for their services, usually on the seller of a good.  Bitcoin also charges fees for transactions but they are much less that business currently pay today.
In fact with bitcoin, they have completely taken out the need for a central bank and a regulatory system the like of which we see in every country today.  Bitcoin essentially prints its own money by allowing people to become part of the system using computers to host the blockchain, manage transactions and get a fee for doing this.  The fee is in new bitcoins, and is known as mining bitcoins (as it is akin to mining for gold, only it uses cpu and electricity), this will continue to happen until there is a certain number of total bitcoins in circulation, currently projected to be in 2140.  This is how new money enters the system, so what happens to the computing infrastructure after all bitcoins have been mined?  well, it is said that when the amount of bitcoins get that big the number of transactions will generate fees which the nodes will benefit from also, and this is what will keep the distributed network up  and running.

I have some questions about scenarios that might possibly happen that could affect the bitcoin network, such as power outages on a global scale, or apparently nearly 50% of bitcoin miners are in china, what happens if the government bans them?  turns them off?  or even, what if there were a credit crunch type scenario where people where hoarding their bitcoins and not spending them, if the nodes can not generate any fees they may shut down, loosing computers from the network, will make it smaller to a point of failure?  will it slow to a halt!
Anyway there are just hypothetical scenarios, at the minute, or maybe they are prophecies!  we’ll see….

Bitcoin’s Block contains the following data:

Block Number: Most recent Block
Message: Data
Hash: Hash of the current block and the block chain (merkle tree)
Nonce:  A number used in the calculation of the hash
Timestamp: Of when the block was created
Previous: hash of previous block

Merle trees are hashes of hashes, which is the concept  behind blockchain.  It makes comparison easy, as two blockchains that are the same size will have the same hash, and if anything has changed within the blockchain (maliciously, because you cannot edit the chain), you can easily detect as the hash will be different.

Read more on merle trees here; https://www.codeproject.com/Articles/1176140/Understanding-Merkle-Trees-Why-use-them-who-uses-t

The nonce is a number used in the calculation of the hash.  Im not sure how it works but I think it works like this……You create a transaction, it goes to a node to add a block, the block can only be added when the hash created is made up of four leading zeros.  So, the first time a hash is created it may not have four leading zeros, this is where the nonce comes in, the nonce is incremented by 1 and the hash is executed again, the nonce will be continually incremented by one until a hash is created that starts with four leading zeros.  Once complete the block is added and sent around the network for validation and this I think is usually what take around 10 minutes to complete depending upon difficulty (which I think is the number of leading zeros your hash mush have).   Once your block is added to the chain the chain is sent around the system, and once up to 6 other chains have accepted your chain then that transaction has been added.  This usually take 1 hour.  

There is risk of reversal if you send out a double transaction, e.g. you only have 5 bitcoins and you make two transactions with same money.  which one will be saved to the block?  the one that goes to a node with the most computing power and sends the block around the network quickest.  I read something about bitcoin transactions getting stuck, and this is because nodes can choose which blocks to mine based on their fees, so if you have a low fee nodes will just ignore the transaction and they are said to be stuck.  I think to unstick them, you need to edit the transaction? and increase the fee!

Thats enough of bitcoin for now, any questions can mostly be answered here:

There is a bitcoin wiki that will answer all your questions: https://en.bitcoin.it/wiki/Help:FAQ

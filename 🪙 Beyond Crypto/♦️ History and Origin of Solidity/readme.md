<img src="./What%20is%20Solidity.png" alt="Cover">

<br>

Roughly a decade ago, blockchain technology was largely just synonymous with the Bitcoin cryptocurrency. It wasn’t until Ethereum’s inception, when developers started using [Solidity](https://soliditylang.org/) to write smart contracts, that blockchain technology would truly come into its own. As programmers began using Solidity smart contracts, numerous blockchain use cases emerged, unleashing the true power of Web3.

---

> ### Solidity is a statically-typed curly-braces programming language designed for developing smart contracts that run on Ethereum.

---

<br>

## 📍 History of Solidity

<br>

-  Vitalik Buterin, an early advocate of Bitcoin, first proposed the creation of another protocol to run more elaborate smart contracts than that of the Bitcoin network. Bitcoin’s structure is simple, and for good reason – it was created primarily to be sound money. 
-  However, to do more complex transactions that do not require the presence of an intermediary to validate them, Buterin suggested the idea of Ethereum, a complex system of smart contracts interacting with each other. 
-  With Ethereum, the idea of a transaction was no longer limited to money per se, but a host of other types of arrangements that could be executed using a highly malleable and programmable protocol. 
-  In other words, Ethereum expanded the possibilities and brought creativity and a certain level of human-type problem-solving into the mix. As a result, this new blockchain protocol needed to be Turing complete (or as close to Turing completeness as possible) to achieve its new objectives.

<br>

<img src="./solidity-eth.png" alt="Ethereum and Solidity">

<br>

---

## 📍 What are Turing Machines and Turing Completeness?

<br>

To understand Soldity, we first have to take a look at Turing completeness and Turing machines. The term “Turing complete” itself hints at the origins of the concept of a Turing machine. As you may have guessed, the legendary Alan Turing had something to do with it. In his 1936 paper, “[On Computable Numbers, with an application to the Entscheidungsproblem](https://www.cs.virginia.edu/~robins/Turing_Paper_1936.pdf)”, Turing imagined a machine that could solve any type of problem that humans are capable of solving. In short, a system of data-manipulation rules is seen as being Turing complete if it can simulate a Turing machine.

Well, a Turing machine was originally imagined as being an infinite tape divided into squares, on which you can write and read data. The data takes the form of 1s and 0s, or binary code, which the machine then interprets into a set of symbols. This finite set of symbols is known as the machine’s alphabet. 

The machine has a computational – or read-write – head that can be moved along the tape and that can write or erase a symbol on a square. The computational head directs the machine’s attention to one square at a time, meaning the machine is “conscious” and aware of only a single square at any given time, depending on where the read-write head directs it.

<br>

<img src="https://miro.medium.com/max/560/1*l9EcdsiimFIKlM6ARGc6DQ.jpeg" alt="Turing Machine">

<br>

From this basic architecture, the machine can “compute” or “decide” which set of behaviors or operations to run based on what is called a “finite state machine”. Different types of Turing machines can be created whose operations are defined by their unique or respective state machines. In Turing’s paper, he mentions m-configurations, which refer to the finite number of states within a finite state machine. The machine switches between this fixed set of possible configurations.

---

## 📍 Ethereum and Solidity Smart Contracts

<br>

Ethereum is free and open-source software that runs on a decentralized network of computers. Being part of this network is voluntary. This network supports the Ethereum blockchain and ensures that programs called smart contracts are run using the voluntary participant computers or nodes.

Bitcoin started the crypto revolution, but Ethereum took Bitcoin’s initial blockchain thesis and ran with it. Ethereum goes beyond data validation and storage in distributed ledgers. Using Ethereum, many different transactions and high-level functions can be performed, using multiple currencies or tokens (including Bitcoin) across an entire network. This network ensures that the code is run equivalently and that the resulting states are recorded and validated via consensus.

---

## 📍 The Ethereum Virtual Machine or EVM

<br>

The Ethereum Virtual Machine (or) EVM is a virtual stack that is embedded within every fully participating node in the network, or Ethereum node, that executes contract bytecode. The EVM is a Turing complete system, which means it can perform any type of logical step associated with computational functions.

While Bitcoin provides rewards for running a transaction, Ethereum charges fees for executing software instructions. The gas mechanism in Ethereum lets users pre-pay for the instructions they want to execute on the EVM using Ether, its native currency. 

EVMs are pretty versatile in that they can be implemented using JavaScript, C++, Ruby, Python, and a variety of other languages.

> The contracts that are run using the EVM are written using Solidity. So, once and for all, what is Solidity? It is a high-level programming language that is compatible with how humans express instructions – using numbers and letters instead of binary code.

> Solidity smart contracts are instructions that are then compiled to the EVM’s bytecode. The nodes in the Ethereum network, as mentioned, run EVM instances that permit them to agree on the execution of the same set of instructions. 

---

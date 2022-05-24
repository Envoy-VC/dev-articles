<img src="./cover.png" alt="Cover" >

<br>

You’re here because, like me, you’re psyched about the rise of Cryptocurrencies, NFTs and decentralized Web/Apps. And you want to know how Blockchains work—the fundamental technology behind them.

---

### Before you get started…

Remember that a blockchain is an _immutable, sequential_ chain of records called Blocks. They can contain transactions, files or any data you like, really. But the important thing is that they’re _chained_ together using _hashes_.

**_Hash Function_** -


`A hash function is simply a function that takes in input value, and from that input creates an output value deterministic of the input value. For any x input value, you will always receive the same y output value whenever the hash function is run. In this way, every input has a determined output. `



**_What do I need?_** Make sure that [Python 3.6](https://www.python.org/downloads)+ (along with pip) is installed. You’ll also need to install Flask and the wonderful Requests library:
```bash
pip install Flask requests
```

you’ll also need an HTTP Client, like [Postman](https://www.getpostman.com/?ref=hackernoon.com) or cURL. But anything will do.

---

## 📍 Step 1: Building a Blockchain

Open up your favourite text editor or IDE, personally I use  [Visual Studio Code](https://code.visualstudio.com/). Create a new file, called `main.py`

##### 🪙 **Representing a Blockchain**

We’ll create a `Blockchain` class whose constructor creates an initial empty list (to store our blockchain), and another to store transactions.

Here’s the blueprint for our class:

```python
class Blockchain(object):
    def __init__(self):
        self.chain = []
        self.current_transactions = []
        
    def new_block(self):
        # Creates a new Block and adds it to the chain
        pass
    
    def new_transaction(self):
        # Adds a new transaction to the list of transactions
        pass
    
    @staticmethod
    def hash(block):
        # Hashes a Block
        pass

    @property
    def last_block(self):
        # Returns the last Block in the chain
        pass
```

Our `Blockchain` class is responsible for managing the chain. It will store transactions and have some helper methods for adding new blocks to the chain. Let’s start fleshing out some methods.

##### 🪙 **What does a Block look like?**

Each Block has an _index_, a _timestamp_ (in Unix time), a _list of transactions_, a _proof_, and the _hash of the previous Block_.

Here’s an example of what a single Block looks like:

```python
block = {
    'index': 1,
    'timestamp': 1506057125.900785,
    'transactions': [
        {
            'sender': "8527147fe1f5426f9dd545de4b27ee00",
            'recipient': "a77f5cdfa2934df3954a5c7c7da5df1f",
            'amount': 5,
        }
    ],
    'proof': 324984774000,
    'previous_hash': "2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824"
}
```

At this point, the idea of a _chain_ should be apparent—each new block contains within itself, the hash of the previous Block. **This is crucial because it’s what gives blockchains immutability:** If an attacker corrupted an earlier Block in the chain then **_all_** subsequent blocks will contain incorrect hashes.

---

##### 🪙 **Adding Transactions to a Block**

We’ll need a way of adding transactions to a Block. Our `new_transaction()` method is responsible for this, and it’s pretty straight-forward:

```python
class Blockchain(object):
    ...
    
    def new_transaction(self, sender, recipient, amount):
        """
        Creates a new transaction to go into the next mined Block
        :param sender: <str> Address of the Sender
        :param recipient: <str> Address of the Recipient
        :param amount: <int> Amount
        :return: <int> The index of the Block that will hold this transaction
        """

        self.current_transactions.append({
            'sender': sender,
            'recipient': recipient,
            'amount': amount,
        })

        return self.last_block['index'] + 1
```

After  `new_transaction()` adds a transaction to the list, it returns the _index_ of the block which the transaction will be added to—_the next one to be mined._ This will be useful later on, to the user submitting the transaction.

---

##### 🪙 **Creating new Blocks**

When our `Blockchain` is instantiated we’ll need to seed it with a _genesis_ block—a block with no predecessors. We’ll also need to add a _“proof”_ to our genesis block which is the result of mining (or proof of work).

In addition to creating the _genesis_ block in our constructor, we’ll also flesh out the methods for `new_block()`,`new_transaction()` and `hash()`.


```python
import hashlib
import json
from time import time


class Blockchain(object):
    def __init__(self):
        self.current_transactions = []
        self.chain = []

        # Create the genesis block
        self.new_block(previous_hash=1, proof=100)

    def new_block(self, proof, previous_hash=None):
        """
        Create a new Block in the Blockchain
        :param proof: <int> The proof given by the Proof of Work algorithm
        :param previous_hash: (Optional) <str> Hash of previous Block
        :return: <dict> New Block
        """

        block = {
            'index': len(self.chain) + 1,
            'timestamp': time(),
            'transactions': self.current_transactions,
            'proof': proof,
            'previous_hash': previous_hash or self.hash(self.chain[-1]),
        }

        # Reset the current list of transactions
        self.current_transactions = []

        self.chain.append(block)
        return block

    def new_transaction(self, sender, recipient, amount):
        """
        Creates a new transaction to go into the next mined Block
        :param sender: <str> Address of the Sender
        :param recipient: <str> Address of the Recipient
        :param amount: <int> Amount
        :return: <int> The index of the Block that will hold this transaction
        """
        self.current_transactions.append({
            'sender': sender,
            'recipient': recipient,
            'amount': amount,
        })

        return self.last_block['index'] + 1

    @property
    def last_block(self):
        return self.chain[-1]

    @staticmethod
    def hash(block):
        """
        Creates a SHA-256 hash of a Block
        :param block: <dict> Block
        :return: <str>
        """

        # We must make sure that the Dictionary is Ordered, or we'll have inconsistent hashes
        block_string = json.dumps(block, sort_keys=True).encode()
        return hashlib.sha256(block_string).hexdigest()
```

The above should be straight-forward—I’ve added some comments and _docstrings_ to help keep it clear. We’re almost done with representing our blockchain. But at this point, you must be wondering how new blocks are created, forged or mined.

---

##### 🪙 **Understanding Proof of Work**

A Proof of Work algorithm (PoW) is how new Blocks are created or _mined_ on the _blockchain_. The goal of PoW is to discover a number which solves a problem. The number must be **difficult to find** **but easy to verify**—computationally speaking—by anyone on the network. This is the core idea behind Proof of Work.

We’ll look at a very simple example to help this sink in.

Let’s decide that the _hash_ of some integer x multiplied by another y must end in 0. So, `hash(x * y) = ac23dc...0`. And for this simplified example, let’s fix `x = 5` . Implementing this in Python:

```python
from hashlib import sha256
x = 5
y = 0  # We don't know what y should be yet...
while sha256(f'{x*y}'.encode()).hexdigest()[-1] != "0":
    y += 1
print(f'The solution is y = {y}')
```

The solution here is `y = 21`. Since, the produced hash ends in 0:

```python
hash(5 * 21) = 1253e9373e...5e3600155e860
```

In Bitcoin, the Proof of Work algorithm is called [_Hashcash_](https://en.wikipedia.org/wiki/Hashcash). And it’s not too different from our basic example above. It’s the algorithm that miners race to solve in order to create a new block. In general, the difficulty is determined by the number of characters searched for in a string. The miners are then rewarded for their solution by receiving a coin—in a transaction.

The network is able to _easily_ verify their solution.

---

##### 🪙 **Implementing basic Proof of Work**

Let’s implement a similar algorithm for our blockchain. Our rule will be similar to the example above:

> _Find a number_ p _that when hashed with the previous block’s solution a hash with 4 leading_ 0s _is produced._


```python
import hashlib
import json

from time import time
from uuid import uuid4


class Blockchain(object):
    ...
        
    def proof_of_work(self, last_proof):
        """
        Simple Proof of Work Algorithm:
         - Find a number p' such that hash(pp') contains leading 4 zeroes, where p is the previous p'
         - p is the previous proof, and p' is the new proof
        :param last_proof: <int>
        :return: <int>
        """

        proof = 0
        while self.valid_proof(last_proof, proof) is False:
            proof += 1

        return proof

    @staticmethod
    def valid_proof(last_proof, proof):
        """
        Validates the Proof: Does hash(last_proof, proof) contain 4 leading zeroes?
        :param last_proof: <int> Previous Proof
        :param proof: <int> Current Proof
        :return: <bool> True if correct, False if not.
        """

        guess = f'{last_proof}{proof}'.encode()
        guess_hash = hashlib.sha256(guess).hexdigest()
        return guess_hash[:4] == "0000"
```

To adjust the difficulty of the algorithm, we could modify the number of leading zeroes. But 4 is sufficient. You’ll find out that the addition of a single leading zero makes a mammoth difference to the time required to find a solution.

Our class is almost complete and we’re ready to begin interacting with it using HTTP requests.

---
---

## 📍 Step 2: Our Blockchain as an API

We’re going to use the Python Flask Framework. It’s a micro-framework and it makes it easy to map endpoints to Python functions. This allows us talk to our blockchain over the web using HTTP requests.

We’ll create three methods:

-   `/transactions/new` to create a new transaction to a block
-   `/mine` to tell our server to mine a new block.
-   `/chain` to return the full Blockchain

##### 🪙 **Setting up Flask**

Our “server” will form a single node in our blockchain network. Let’s create some boilerplate code:

```python
import hashlib
import json
from textwrap import dedent
from time import time
from uuid import uuid4

from flask import Flask


class Blockchain(object):
    ...


# Instantiate our Node
app = Flask(__name__)

# Generate a globally unique address for this node
node_identifier = str(uuid4()).replace('-', '')

# Instantiate the Blockchain
blockchain = Blockchain()


@app.route('/mine', methods=['GET'])
def mine():
    return "We'll mine a new Block"
  
@app.route('/transactions/new', methods=['POST'])
def new_transaction():
    return "We'll add a new transaction"

@app.route('/chain', methods=['GET'])
def full_chain():
    response = {
        'chain': blockchain.chain,
        'length': len(blockchain.chain),
    }
    return jsonify(response), 200

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

---

##### 🪙 **The Transactions Endpoint**

This is what the request for a transaction will look like. It’s what the user sends to the server:

```python
{
 "sender": "my address",
 "recipient": "someone else's address",
 "amount": 5
}
```

Since we already have our class method for adding transactions to a block, the rest is easy. Let’s write the function for adding transactions:

```python
import hashlib
import json
from textwrap import dedent
from time import time
from uuid import uuid4

from flask import Flask, jsonify, request

...

@app.route('/transactions/new', methods=['POST'])
def new_transaction():
    values = request.get_json()

    # Check that the required fields are in the POST'ed data
    required = ['sender', 'recipient', 'amount']
    if not all(k in values for k in required):
        return 'Missing values', 400

    # Create a new Transaction
    index = blockchain.new_transaction(values['sender'], values['recipient'], values['amount'])

    response = {'message': f'Transaction will be added to Block {index}'}
    return jsonify(response), 201
```

---

##### 🪙 **The Mining Endpoint**

Our mining endpoint is where the magic happens, and it’s easy. It has to do three things:

-   Calculate the Proof of Work
-   Reward the miner (us) by adding a transaction granting us 1 coin
-   Forge the new Block by adding it to the chain

```python
import hashlib
import json

from time import time
from uuid import uuid4

from flask import Flask, jsonify, request

...

@app.route('/mine', methods=['GET'])
def mine():
    # We run the proof of work algorithm to get the next proof...
    last_block = blockchain.last_block
    last_proof = last_block['proof']
    proof = blockchain.proof_of_work(last_proof)

    # We must receive a reward for finding the proof.
    # The sender is "0" to signify that this node has mined a new coin.
    blockchain.new_transaction(
        sender="0",
        recipient=node_identifier,
        amount=1,
    )

    # Forge the new Block by adding it to the chain
    previous_hash = blockchain.hash(last_block)
    block = blockchain.new_block(proof, previous_hash)

    response = {
        'message': "New Block Forged",
        'index': block['index'],
        'transactions': block['transactions'],
        'proof': block['proof'],
        'previous_hash': block['previous_hash'],
    }
    return jsonify(response), 200
```

Note that the recipient of the mined block is the address of our node. And most of what we’ve done here is just interact with the methods on our `Blockchain` class. At this point, we’re done, and can start interacting with our blockchain.

---
---

## 📍 Step 3: Interacting with our Blockchain

You can use plain old cURL or Postman to interact with our API over a network.

Fire up the server:

```bash
$ python main.py
* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Let’s try mining a block by making a `GET` request to `http://127.0.0.1:5000/mine` :

![mine](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lwqc067dd8bwnrhgmrom.png)

---
Let’s inspect the full chain by requesting `http://127.0.0.1:5000/chain`


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/835e6ef5ctpfdilwbtui.png)

---

## 📍 Step 4: Consensus

This is very cool. We’ve got a basic Blockchain that accepts transactions and allows us to mine new Blocks. But the whole point of Blockchains is that they should be _decentralized_. And if they’re decentralized, how on earth do we ensure that they all reflect the same chain? This is called the problem of _Consensus_, and we’ll have to implement a Consensus Algorithm if we want more than one node in our network.

---
 
##### 🪙 **Registering new Nodes**

Before we can implement a Consensus Algorithm, we need a way to let a node know about neighboring nodes on the network. Each node on our network should keep a registry of other nodes on the network. Thus, we’ll need some more endpoints:

-   `/nodes/register` to accept a list of new nodes in the form of URLs.
-   `/nodes/resolve`  to implement our Consensus Algorithm, which resolves any conflicts—to ensure a node has the correct chain.

We’ll need to modify our Blockchain’s constructor and provide a method for registering nodes:

```python
...
from urllib.parse import urlparse
...


class Blockchain(object):
    def __init__(self):
        ...
        self.nodes = set()
        ...

    def register_node(self, address):
        """
        Add a new node to the list of nodes
        :param address: <str> Address of node. Eg. 'http://192.168.0.5:5000'
        :return: None
        """

        parsed_url = urlparse(address)
        self.nodes.add(parsed_url.netloc)
```


Note that we’ve used a `set()` to hold the list of nodes. This is a cheap way of ensuring that the addition of new nodes is idempotent—meaning that no matter how many times we add a specific node, it appears exactly once.

---

##### 🪙 **Implementing the Consensus Algorithm**

As mentioned, a conflict is when one node has a different chain to another node. To resolve this, we’ll make the rule that _the longest valid chain is authoritative._ In other words, the longest chain on the network is the _de-facto_ one. Using this algorithm, we reach _Consensus_ amongst the nodes in our network.

```python
...
import requests


class Blockchain(object)
    ...
    
    def valid_chain(self, chain):
        """
        Determine if a given blockchain is valid
        :param chain: <list> A blockchain
        :return: <bool> True if valid, False if not
        """

        last_block = chain[0]
        current_index = 1

        while current_index < len(chain):
            block = chain[current_index]
            print(f'{last_block}')
            print(f'{block}')
            print("\n-----------\n")
            # Check that the hash of the block is correct
            if block['previous_hash'] != self.hash(last_block):
                return False

            # Check that the Proof of Work is correct
            if not self.valid_proof(last_block['proof'], block['proof']):
                return False

            last_block = block
            current_index += 1

        return True

    def resolve_conflicts(self):
        """
        This is our Consensus Algorithm, it resolves conflicts
        by replacing our chain with the longest one in the network.
        :return: <bool> True if our chain was replaced, False if not
        """

        neighbours = self.nodes
        new_chain = None

        # We're only looking for chains longer than ours
        max_length = len(self.chain)

        # Grab and verify the chains from all the nodes in our network
        for node in neighbours:
            response = requests.get(f'http://{node}/chain')

            if response.status_code == 200:
                length = response.json()['length']
                chain = response.json()['chain']

                # Check if the length is longer and the chain is valid
                if length > max_length and self.valid_chain(chain):
                    max_length = length
                    new_chain = chain

        # Replace our chain if we discovered a new, valid chain longer than ours
        if new_chain:
            self.chain = new_chain
            return True

        return False
```

The first method `valid_chain()`is responsible for checking if a chain is valid by looping through each block and verifying both the hash and the proof.

`resolve_conflicts()` is a method which loops through all our neighbouring nodes, _downloads_ their chains and verifies them using the above method. **If a valid chain is found, whose length is greater than ours, we replace ours.**

Let’s register the two endpoints to our API, one for adding neighbouring nodes and the another for resolving conflicts:

```python
@app.route('/nodes/register', methods=['POST'])
def register_nodes():
    values = request.get_json()

    nodes = values.get('nodes')
    if nodes is None:
        return "Error: Please supply a valid list of nodes", 400

    for node in nodes:
        blockchain.register_node(node)

    response = {
        'message': 'New nodes have been added',
        'total_nodes': list(blockchain.nodes),
    }
    return jsonify(response), 201


@app.route('/nodes/resolve', methods=['GET'])
def consensus():
    replaced = blockchain.resolve_conflicts()

    if replaced:
        response = {
            'message': 'Our chain was replaced',
            'new_chain': blockchain.chain
        }
    else:
        response = {
            'message': 'Our chain is authoritative',
            'chain': blockchain.chain
        }

    return jsonify(response), 200
```

At this point you can grab a different machine if you like, and spin up different nodes on your network. Or spin up processes using different ports on the same machine. I spun up another node on my machine, on a different port, and registered it with my current node. Thus, I have two nodes: `http://127.0.0.1:5000` and `http://127.0.0.1:5001`.


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/e0niz6ibru9zgx6vut3s.png)


I then mined some new Blocks on node 2, to ensure the chain was longer. Afterward, I called `GET /nodes/resolve` on node 1, where the chain was replaced by the Consensus Algorithm:


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/rfj2etyluxm49k6rswsh.png)

And that’s a wrap...

---

I hope that this has inspired you to create something new.
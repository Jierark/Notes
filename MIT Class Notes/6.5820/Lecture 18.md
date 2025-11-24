P2P technology was very popular in the 2000s, but the hype died off after some time. Bitcoin managed to start another wave which has persisted even today. Interestingly, the author of the bitcoin paper is unknown, but the concepts of blockchains and proof-of-work were novel during those times. 

# The Coin
A coin represents a series of transactions, with each one passing ownership to another user.
#drawing transaction

Each user has a public and private key. A transaction between two users A and B consist of the following steps:
- A wants to transfer a coin they possess to B.
- The transaction is handled by a hash of B and the previous transaction (which part bruh)
- This result is then signed by A using its private key. This signature can be verified using A's public key, completing the transfer of the coin.

Forging a transaction is difficult - it requires breaking a public key cryptography scheme. However, another more obvious problem is known as double spending - A could possible use the same coin and sign two different transactions at the same time. In a typical financial setting, this is managed by a central financial institute, such as a bank, which verifies all transactions.

Here, these transactions are verified using a public ledger (the blockchain). As the name suggests, the blockchain is a series of blocks, each one representing a batch of transactions that are verified. 

# Mining a New Block
A new block has the following inputs:
- The hash of the previous block $Prev$
- A list of transactions to verify $Tx$
Mining a block is done by computationally looking for a nonce such that the following is satisfied for some hash function H:
$$H(Prev,Tx,nonce) < threshold$$
This is the core of the computation power required for Bitcoin mining. Finding these nonces are difficult. Each new mined block has an incentive to be mined: ~3 coins are granted to the user. The Genesis block represents the first in the chain, and there is only one chain representing the truth. On average, the threshold is set such that it takes 10 minutes to mine a new block.

There is still a possible vulnerability known as the Private Double Spend Attack.
# Private Double Spend Attack
#todo get details

There are a few methods to prevent this attack. Firstly, this is dependent on the computation power of an adversary. If more than 50% of the network is honest, then on average, the honest chain will outpace an adversarial chain with high probability.

Another method is k-deep confirmation. To prevent double spending, a transaction waits for k blocks to be published before accepting some transaction. A higher value of k provides a stronger guarantee that an adversarial chain won't be larger than the honest chain.

# Performance
- Security of Bitcoin depends on computation power of the adversary. So long as adversaries don't control more than 50% of computation, Bitcoin will be secure.
- The throughput of bitcoin can only handle 3-7 transactions per second, and 1 block is mined per 10 minutes. This rate is mainly due to the computation difficulty of mining a block.
- The confirmation latency depends on the value of k, but it typically takes hours to complete.
- A lower threshold does speed up the rate of mining, but it will also weaken the security guarantee.
- Forking


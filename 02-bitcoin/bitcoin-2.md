
## Bitcoin

The merkle root is stored in the block header. Each block also stores the hash of the previous block’s header, chaining the blocks together. This ensures a transaction cannot be modified without modifying the block that records it and all following blocks [1].

![](https://bitcoin.org/img/dev/en-blockchain-overview.svg)

The block header provides several easy-to-modify fields, such as a dedicated nonce field, so obtaining new hashes doesn’t require waiting for new transactions. Also, only the 80-byte block header is hashed for proof-of-work, so including a large volume of transaction data in a block does not slow down hashing with extra I/O, and adding additional transaction data only requires the recalculation of the ancestor hashes in the merkle tree.

---

Outputs are tied to transaction identifiers (TXIDs), which are the hashes of signed transactions.

For a payment to be valid, it must only use Unspent Transaction Outputs (UTXOs) as inputs.

Ignoring coinbase transactions (described later), if the value of a transaction’s outputs exceed its inputs, the transaction will be rejected—but if the inputs exceed the value of the outputs, any difference in value may be claimed as a transaction fee by the Bitcoin miner who creates the block containing that transaction.

---

To prove you did some extra work to create a block, you must create a hash of the block header which does not exceed a certain value.

Every 2,016 blocks, the network uses timestamps stored in each block header to calculate the number of seconds elapsed between generation of the first and last of those last 2,016 blocks. The ideal value is 1,209,600 seconds (two weeks). The ideal value corresponds to a time lapse of 10 min every block on an average.

* If it took fewer than two weeks to generate the 2,016 blocks, the expected difficulty value is increased proportionally (by as much as 300%) so that the next 2,016 blocks should take exactly two weeks to generate if hashes are checked at the same rate.

* If it took more than two weeks to generate the blocks, the expected difficulty value is decreased proportionally (by as much as 75%) for the same reason.

---
When miners produce simultaneous blocks at the end of the block chain, each node individually chooses which block to accept. In the absence of other considerations, discussed below, nodes usually use the first block they see.

Eventually a miner produces another block which attaches to only one of the competing simultaneously-mined blocks. This makes that side of the fork stronger than the other side. Assuming a fork only contains valid blocks, normal peers always follow the the most difficult chain to recreate and throw away stale blocks belonging to shorter forks.

Long-term forks are possible if different miners work at cross-purposes, such as some miners diligently working to extend the block chain at the same time other miners are attempting a 51 percent attack to revise transaction history.

![](https://bitcoin.org/img/dev/en-blockchain-fork.svg)

Since multiple blocks can have the same height during a block chain fork, block height should not be used as a globally unique identifier. Instead, blocks are usually referenced by the hash of their header.

---

Every block must include one or more transactions. The first one of these transactions must be a coinbase transaction, also called a generation transaction, which should collect and spend the block reward (comprised of a block subsidy and any transaction fees paid by transactions included in this block).

The UTXO of a coinbase transaction has the special condition that it cannot be spent (used as an input) for at least 100 blocks. This temporarily prevents a miner from spending the transaction fees and block reward from a block that may later be determined to be stale (and therefore the coinbase transaction destroyed) after a block chain fork.

---

Blocks are not required to include any non-coinbase transactions, but miners almost always do include additional transactions in order to collect their transaction fees.

All transactions, including the coinbase transaction, are encoded into blocks in binary rawtransaction format.

The rawtransaction format is hashed to create the transaction identifier (txid). From these txids, the merkle tree is constructed by pairing each txid with one other txid and then hashing them together. If there are an odd number of txids, the txid without a partner is hashed with a copy of itself.

The resulting hashes themselves are each paired with one other hash and hashed together. Any hash without a partner is hashed with itself. The process repeats until only one hash remains, the merkle root.

---

Sometimes the consensus rules are changed to introduce new features or prevent network abuse. When the new rules are implemented, there will likely be a period of time when non-upgraded nodes follow the old rules and upgraded nodes follow the new rules, creating two possible ways consensus can break.

* The upgraded nodes follow new rules: The blocks generated by the upgraded nodes are accepted by upgraded nodes but rejected by non-upgraded nodes. This creates permanently divergent chains—one for non-upgraded nodes and one for upgraded nodes—called a hard fork.

![](https://bitcoin.org/img/dev/en-hard-fork.svg)

* The upgraded nodes follow new rules as well as old rules: The blocks generated by the upgraded nodes are accepted by upgraded nodes as well as non-upgraded nodes. In this case, it’s possible to keep the block chain from permanently diverging if upgraded nodes control a majority of the hash rate. That’s because, in this case, non-upgraded nodes will accept as valid all the same blocks as upgraded nodes, so the upgraded nodes can build a stronger chain that the non-upgraded nodes will accept as the best valid block chain. This is called a soft fork.

![](https://bitcoin.org/img/dev/en-soft-fork.svg)

---

During Bitcoin’s first two years, Satoshi Nakamoto performed several soft forks by just releasing the backwards-compatible change in a client that began immediately enforcing the new rule. Multiple soft forks such as BIP30 have been activated via a flag day where the new rule began to be enforced at a preset time or block height. Such forks activated via a flag day are known as User Activated Soft Forks (UASF) as they are dependent on having sufficient users (nodes) to enforce the new rules after the flag day.

Later soft forks waited for a majority of hash rate (typically 75% or 95%) to signal their readiness for enforcing the new consensus rules. Once the signalling threshold has been passed, all nodes will begin enforcing the new rules. Such forks are known as Miner Activated Soft Forks (MASF) as they are dependent on miners for activation.

---

![](https://bitcoin.org/img/dev/en-tx-overview.svg)

Each transaction has at least one input and one output. Each input spends the satoshis paid to a previous output. Each output then waits as an Unspent Transaction Output (UTXO) until a later input spends it.

When your Bitcoin wallet tells you that you have a 10,000 satoshi balance, it really means that you have 10,000 satoshis waiting in one or more UTXOs.

---

Apart from financial privacy, avoiding key reuse can also provide security against attacks which might allow reconstruction of private keys from public keys (hypothesized) or from signature comparisons (possible today under certain circumstances described below, with more general attacks hypothesized).

---

A Bitcoin wallet can refer to either a wallet program or a wallet file.

#### Wallet Programs

Wallet programs create public keys to receive satoshis and use the corresponding private keys to spend those satoshis. Wallet files store private keys and (optionally) other information related to transactions for the wallet program. Wallet programs also need to interact with the peer-to-peer network to get information from the block chain and to broadcast new transactions.

This leaves us with three necessary, but separable, parts of a wallet system:
* a public key distribution program,
* a signing program, and
* a networked program.

![](https://bitcoin.org/img/dev/en-wallets-full-service.svg)

Almost all popular wallets can be used as full-service wallets.

The main advantage of full-service wallets is that they are easy to use. A single program does everything the user needs to receive and spend satoshis.

The main disadvantage of full-service wallets is that they store the private keys on a device connected to the Internet.

To increase security, private keys can be generated and stored by a separate wallet program operating in a more secure environment. These signing-only wallets work in conjunction with a networked wallet which interacts with the peer-to-peer network.

![](https://bitcoin.org/img/dev/en-wallets-signing-only.svg)

Several full-service wallets programs will also operate as two separate wallets: one program instance acting as a signing-only wallet (often called an “offline wallet”) and the other program instance acting as the networked wallet (often called an “online wallet” or “watching-only wallet”).

Hardware wallets are devices dedicated to running a signing-only wallet. Their dedication lets them eliminate many of the vulnerabilities present in operating systems designed for general use, allowing them to safely communicate directly with other devices so users don’t need to transfer data manually.

Distributing-Only Wallets which run in difficult-to-secure environments, such as webservers, can be designed to distribute public keys (including P2PKH or P2SH addresses) and nothing more.

![](https://bitcoin.org/img/dev/en-wallets-distributing-only.svg)

#### Wallet Files

Bitcoin wallets at their core are a collection of private keys. These collections are stored digitally in a file, or can even be physically stored on pieces of paper.

---

[1] https://bitcoin.org/en/developer-guide

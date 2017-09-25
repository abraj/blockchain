
# Bitcoin

### Table of Contents

- [Basic concepts](#basic-concepts)
	- [Downsides of decentralization](#downsides-of-decentralization)
	- [Deflationary nature of bitcoin currency](#deflationary-nature-of-bitcoin-currency)
	- [Bitcoin software universe](#bitcoin-software-universe)
	- [Public-key cryptography](#public-key-cryptography)
	- [Digital signature](#digital-signature)
	- [Bitcoin addresses](#bitcoin-addresses)
- [Bitcoin consensus](#bitcoin-consensus)
	- [Bitcoin consensus: Voting mechanism](#bitcoin-consensus-voting-mechanism)
	- [Solving a block](#solving-a-block)
- [Transactions](#transactions)
	- [How wallet balances are maintained in bitcoin blockchain?](#how-wallet-balances-are-maintained-in-bitcoin-blockchain)
	- [UTXOs - Unspent Transaction Outputs](#utxos-unspent-transaction-outputs)
	- [What is transaction fee for a transaction?](#what-is-transaction-fee-for-a-transaction)
	- [What does n confirmations to a transaction mean?](#what-does-n-confirmations-to-a-transaction-mean)
	- [Coinbase transaction](#coinbase-transaction)
	- [Double spending fraud - Security hole due to order of transactions](#double-spending-fraud-security-hole-due-to-order-of-transactions)
	- [How to reverse a transactions?](#how-to-reverse-a-transactions)
	- [Order of transactions](#order-of-transactions)
	- [Unconfirmed transactions](#unconfirmed-transactions)
	- [Complex transactions (Multi-signature transactions)](#complex-transactions-multi-signature-transactions)
	- [Escrow using Multisignature address](#escrow-using-multisignature-address)
	- [Payment channels](#payment-channels)
	- [Lightning Network](#lightning-network)
	- [Payment Protocol](#payment-protocol)
	- [CoinJoin](#coinjoin)
- [Wallets](#wallets)
	- [SPV (Simplified Payment Verification) Wallets & Merkle Tree](#spv-simplified-payment-verification-wallets-merkle-tree)
	- [JBOK (Just a Bunch Of Keys) Wallets](#jbok-just-a-bunch-of-keys-wallets)
	- [HD (Hierarchical Deterministic) Wallets](#hd-hierarchical-deterministic-wallets)
	- [Generating public keys without private keys](#generating-public-keys-without-private-keys)
	- [Soft fork vs Hard fork](#soft-fork-vs-hard-fork)
	- [Pay to Script Hash feature](#pay-to-script-hash-feature)
	- [Segregated Witness (SegWit)](#segregated-witness-segwit)
	- [Transaction Malleability](#transaction-malleability)
- [Terminology](#terminology)
	- [What is BIP?](#what-is-bip)
	- [Script signature:](#script-signature)
- [Software for developers](#software-for-developers)
	- [Bitcore](#bitcore)
- [Links and online tools](#links-and-online-tools)
	- [Open-source Bitcoin wallet](#open-source-bitcoin-wallet)
	- [Popular web-based wallets](#popular-web-based-wallets)
	- [Popular exchanges](#popular-exchanges)
	- [Desktop Bitcoin wallet](#desktop-bitcoin-wallet)
	- [Bitcoin Paper wallet](#bitcoin-paper-wallet)
	- [Check Bitcoin Address Balance](#check-bitcoin-address-balance)
	- [Accepting bitcoin on a website](#accepting-bitcoin-on-a-website)
	- [Bitcoin QR Code Generator](#bitcoin-qr-code-generator)
- [References and Learning Resources](#references-and-learning-resources)
	- [Videos](#videos)
	- [Reference docs](#reference-docs)
	- [Blog articles](#blog-articles)

---

### Basic concepts

##### Downsides of decentralization
* Large time to wait for high value transactions
* Proof-of-work burns a lot of energy by design
* Protocol change difficult - Widespread agreement needed

##### Deflationary nature of bitcoin currency
People will likely lose private keys due to hard drive crashes and insufficient backups.

#####  Bitcoin software universe
* Wallets / Thin Clients
  - Create private keys & addresses
  - Create and sign transactions
* Full Node / Bitcoin Core
  - Wallet functionality
  - Check and relay transactions
  - Maintain full ledger
* Voters / Miners
  - Solve Proof-of-work puzzle in voting process

##### Public-key cryptography
Message    ==(Public key)==>  CipherText ==(Private key)==> Message

##### Digital signature
TxnMessage ==(Private key)==> Signature  ==(Public key)==>  TxnMessage

##### Bitcoin addresses
Private key ==> Public key ==> Bitcoin address

Public key =/=> Private key

* Public key can be created from private key, but not vice-versa.
* Bitcoin addresses started out just as public keys, but now there are a few more operations on public key to help prevent errors and enhance security and privacy.
* Total number of Bitcoin addresses: 2^160 (~ 10^48)

---

### Bitcoin consensus

##### Bitcoin consensus: Voting mechanism
* *Proof-of-Work* Voting system
* *Enables consensus*: Enables continual consensus about *Transaction order* and *System rules*
* *Proof of majority will*: Proves to the world which ledger the majority of them support  
* Voting happens ~ every 10 min
* Voters / Miners solve Proof-of-work puzzle in voting process
* SHA256 (Secure Hash Algorithm): cryptographic hash used in Bitcoin

##### Solving a block
SHA256(prev-block-id, transactions, _NONCE_) = hash result < target

* _NONCE_ is chosen randomly, and attempted again and again till the above equation is solved.
* Smaller the *target*, harder to solve the equation
* *transactions* in the above equation are chosen from *Unconfirmed Transaction Pool*

---

### Transactions

##### How wallet balances are maintained in bitcoin blockchain?
* Ledger balances are not maintained directly, rather bitcoin blockchain keeps all the transactions in its ledger.
* Ownership of funds is verified through links to previous transactions (*Transaction chain of Bitcoin ownership*).
* These reference transactions should be unspent.
* This process is made fast by using an index of unspent transactions (UTXOs).
* So, account balance of an address = sum of all unspent transactions for the address from the list of all transactions.

##### UTXOs - Unspent Transaction Outputs
* New transactions must source unspent outputs
* Checked frequently
* Kept in a separate database
* As of now, about 15% of actual blockchain size

##### What is transaction fee for a transaction?
(Total input value - Total output value) for a transaction

##### What does n confirmations to a transaction mean?
It means that the transaction has been buried n blocks deep in the live chain.

##### Coinbase transaction
* The first transaction in a block.
* Always created by a miner.
* The coinbase transaction is not relayed by the network.
* Coinbase payment = Block reward + Txn fees of all the transactions in the block

##### Double spending fraud - Security hole due to order of transactions
If the seller sends the product and then the buyer reverses the transaction by double spending.
This can happen frequently if the buyer accepts transactions with no confirmations.

##### How to reverse a transactions?
After you perform a transactions to someone else, you can do another transaction (double spending) sending the amount to yourself.
However, for this to happen, you should do the second transaction before any block confirmation of the first transaction.
For the reversal to happen, you can prioritize the second transaction by giving it a large transaction fee. Due to this reason, many merchants don't accept 0 confirmation transactions to avoid double spending. So, it becomes difficult to buy a cup of coffee with bitcoin since 0 confirmation transactions are riskier.

##### Order of transactions
Transactions in the same block are considered to happen in the same time.

##### Unconfirmed transactions
Transactions not yet in a block. Also known as unordered transactions.

##### Complex transactions (Multi-signature transactions)
2/3 signatures required for an escrow-based transaction.

##### Escrow using Multisignature address
* Module 4 | Lecture 4

##### Payment channels
* Sending small payments *without high fees* and *without trusting a third party*
* Module 4 | Lecture 8 & 9

##### Lightning Network
* Module 4 | Lecture 9

##### Payment Protocol
* a protocol for communication between a merchant and their customer, enabling both a better customer experience and better security against man-in-the-middle attacks on the payment process.
* BIP 70 (https://github.com/bitcoin/bips/blob/master/bip-0070.mediawiki)
* Module 4 | Lecture 10

##### CoinJoin
* *Mixing transactions*: In this protocol, several people submit similar amounts as inputs to a transaction and then get the money returned in outputs. In this way, it's not clear which input went to which output.
* Module 4 | Lecture 10

---

### Wallets

##### SPV (Simplified Payment Verification) Wallets & Merkle Tree
* Generally used as mobile phone wallets.
* For verifying a Txn, SPV wallets are provided the block (with merkle tree nodes only) containing the transaction and all the subsequent block headers.
* SPV wallets and Merkle tree: Module 3 | Lecture 10

##### JBOK (Just a Bunch Of Keys) Wallets
* For each bitcoin address in the wallet, a corresponding private key was stored
* If a new bitcoin address is used for each incoming transaction, a new private key  would also be created

##### HD (Hierarchical Deterministic) Wallets
* Deterministic
* priv_key_n = Hash(SEED | n)
  - pub_key_n = F (priv_key_n)
  - wallet address = G (priv_key_n)
* Mycelium wallet allows users to genearate the SEED from 12 words (Mnemonic codes)
* For different departments, you can generate separate deterministic set of private keys by giving every department a separate CHAIN_CODE
  - priv_key_n = Hash(SEED | n | CHAIN_CODE)

##### Generating public keys without private keys
* Good for exposed web servers, Can derive new public keys without having access to the corresponding private keys
* Without this ability, we would need to pre-generate a list of public keys and keep it on web servers.

Instead of ```Hash(SEED | n) --> priv_key_n --> pub_key_n```, we use
the following way:
```
pub_key = priv_key * G
```
The above operation ```*``` is not normal multiplication, rather it is multiplication over an elliptic curve. ```G``` is actually a point on an elliptic curve.

```
If,   priv_key_3 = priv_key_1 + priv_key_2
then, pub_key_3  = pub_key_1  + pub_key_2
```
*Proof*:
```
      pub_key_3 = priv_key_3 * G
                = (priv_key_1 + priv_key_2) * G
                = priv_key_1 * G + priv_key_2 * G
                = pub_key_1 + pub_key_2
```

*Formula for generating public keys*:
```
pub_key_n = pub_key_master + Hash(SEED | n) * G
```
Later, you can generate private keys as:
```
priv_key_n = priv_key_master + Hash(SEED | n)
```
*Proof*:
```
 priv_key_n * G = pub_key_n
                = pub_key_master + Hash(SEED | n) * G
                = priv_key_master * G + Hash(SEED | n) * G
                = (priv_key_master + Hash(SEED | n)) * G

Therefore,
     priv_key_n = priv_key_master + Hash(SEED | n)
```

##### Soft fork vs Hard fork
* ```Hard fork``` : Software / rule change that old software sees as invalid.
* ```Soft fork``` : Software / rule change that produces blocks and transactions old software still accepts.
* Example of Hard fork:
  - Increasing block size to 2 MB
* Example of Soft fork:
  - Pay to Script Hash
  - Segregated Witness (SegWit)

*Voting mechanism for creating a fork*:
* Developers build a voting mechanism that enables miners to make sure a majority are upgraded before activating a change. For example, 75% of last 1000 blocks (about 1 week) have upgraded.
* Usually done with a version setting in each block.


##### Pay to Script Hash feature
* Old rule: Any X that hashed currectly
* New rule: X must hash correctly AND be a valid script

##### Segregated Witness (SegWit)
Increased block size as a side-effect of a soft fork.

In Bitcoin transactions, the signature in most cases in more than a simple signature from public key cryptography, but rather a script that fulfills a spend condition specified in a transaction output.

Segregated Witness (SegWit) feature separates the signatures (or witnesses) from the transaction.

*Advantages of Segregated Witness*:
* Avoid Transaction Malleability since signatures are stored separately.
* After a transaction is verified, you can forget about the signatures since you don't need them to calculate who owns the coins.

##### Transaction Malleability
If ```h``` is a valid signature, then ```h``` and ```-h``` both are valid since ```abs(h) = abs(-h) = h```.

```
     Txn                   Txn'            
|---------------|     |---------------|   
| ...           |     | ...           |   
| ScriptSig: h  |     | ScriptSig:-h  |   
| ...           |     | ...           |   
|_______________|     |_______________|   
 hash(txn) = Id1       hash(txn') = Id2   
```
As a result of change in hash, the transaction id also gets changed.

So, if you made a transaction that someone malleates (changes), you won't be able to find the transaction based on its original hash id.
However, you can still look it up based on your address.

---

### Terminology

##### What is BIP?
- 'Bitcoin Improvement proposal'
- The main method using which new features are proposed and evaluated
- https://github.com/bitcoin/bips
- https://bitcoincore.org (Info for core developers)

##### Script signature:
Signature used to unlock funds

---

### Software for developers

##### Bitcore

* *Installing Bitcore and Creating an address:*
 Module 3 | Lecture 2
* *Creating and sending a transaction:*
 Module 3 | Lecture 5
* *HD Wallet setup using Bitcore:*
Module 4 | Lecture 2

---

### Links and online tools

##### Open-source Bitcoin wallet
* http://arcbit.io (https://github.com/arcbit)
* https://blockchain.info (https://github.com/blockchain)

##### Popular web-based wallets
* https://blockchain.info
* https://www.coinbase.com

##### Popular exchanges
* https://www.gdax.com
* https://www.kraken.com
* http://poloniex.com

##### Desktop Bitcoin wallet
* https://electrum.org/

##### Bitcoin Paper wallet
* https://www.bitaddress.org/

##### Check Bitcoin Address Balance
* https://bitref.com

##### Accepting bitcoin on a website
* https://bitpay.com

##### Bitcoin QR Code Generator
* http://www.bitcoinqrcode.org

---

### References and Learning Resources

##### Videos
* https://www.youtube.com/watch?v=Lx9zgZCMqXE
* https://app.pluralsight.com/library/courses/bitcoin-decentralized-technology/

##### Reference docs
* https://bitcoin.org/bitcoin.pdf
* https://bitcoin.org/en/developer-guide
* https://bitcoin.org/en/developer-glossary

##### Blog articles
* https://medium.com/@micheledaliessi/how-does-the-blockchain-work-98c8cd01d2ae
* https://blog.elidourado.com/stop-saying-bitcoin-transactions-arent-reversible-51a74003e226

---

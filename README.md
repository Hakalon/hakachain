# hakachain

In this project, I'm going to build a private chain of ethereum on Windows.
There will be 4 nodes as normal node and miner on this private chain that is using PoW for consensus.

## Enviroment

First, we should install [Geth(Go Ethereum)](https://geth.ethereum.org/downloads/) for running a full ethereum node.

Be notice that there is a *checkbox named Development tool* you should checked during the installation, it brings some tools for you to build blockchain.

Now we are able to type **geth --help** on CMD or Bash to get help information for Geth.

## Generate account for 4 nodes

After setting up enviroment, we need to create accounts for nodes.

Be careful that each node should have **its own folder** to save data.

Use commands below to generate accounts for each node.
```=bash
cd hakachain
geth --datadir ./node1/data account new
geth --datadir ./node2/data account new
geth --datadir ./miner1/data account new
geth --datadir ./miner2/data account new

// After that, Geth will ask you enter password, and you should remember it for each node.
```

In sample, I use "node1@hakachain", "node2@hakachain", "miner1@hakachain" and "miner2@hakachain" as password.

Now there are a folder named "keystore" inside each nodes' "data" folder.

## Generate genesis block

Next job is to create the first block(genesis block) for our private chain, and we are going to use tool built in Geth for this job.

Use commands below to generate genesis block.
```=bash
puppeth
```

This command will show you some information about it like:
```=bash
+-----------------------------------------------------------+
| Welcome to puppeth, your Ethereum private network manager |
|                                                           |
| This tool lets you create a new Ethereum network down to  |
| the genesis block, bootnodes, miners and ethstats servers |
| without the hassle that it would normally entail.         |
|                                                           |
| Puppeth uses SSH to dial in to remote servers, and builds |
| its network components out of Docker containers using the |
| docker-compose toolset.                                   |
+-----------------------------------------------------------+

Please specify a network name to administer (no spaces or hyphens, please)
> hakachain
```

And the first question is **the name of your private chain**, I use hakachain in sample.

```=bash
What would you like to do? (default = stats)
 1. Show network stats
 2. Configure new genesis
 3. Track new remote server
 4. Deploy network components
```

Enter 2 for configure new genesis.

```=bash
Which consensus engine to use? (default = clique)
 1. Ethash - proof-of-work
 2. Clique - proof-of-authority
```

There are two different algorithm of consensus, I use **proof-of-work** in sample.
(You can try proof-of-authority yourself, and there are some different setting you need to set.)

```=bash
Which accounts should be pre-funded? (advisable at least one)
> 0x
```

Here you can pre-fund some currency to whatever accounts you want.

I choose **node1 and miner1** to be pre-funded.

And the address of each node can be find inside a file located at 

**hakachain/node1/data/keystore/UTC--....**  
**hakachain/node2/data/keystore/UTC--....**  
**hakachain/miner1/data/keystore/UTC--....**  
**hakachain/miner2/data/keystore/UTC--....**

```=bash
// There is address:
{"address":"1d8745ae5d1c759e5da9655f18143c0c87d9005a",
//
 "crypto":{"cipher":"aes-128-ctr","ciphertext":"7eb1375710b15cde1401cfa86b12375aaef06e0553540ba6add2751f801ef7ed","cipherparams":{"iv":"b96c9f8c73626232c07b660d84e9d96f"},"kdf":"scrypt","kdfparams":{"dklen":32,"n":262144,"p":1,"r":8,"salt":"3550797d9ca06b3460b6d9d3d1469fb2e34915ffdda31e0ce3dd06ed6b609b3f"},"mac":"61883465ed66f2b350fd478f490faca8303371f9984dbd4538cef426886d2994"},"id":"ace2a200-97aa-4be2-b9a4-c050583fc309","version":3}
```

By the way, just **leave blank and press enter** to finish pre-fund.

```=bash
Specify your chain/network ID if you want an explicit one (default = random)
> 111
```

Pick a number you like except 1, 2, 3(already used) to specify your private chain, I use 111 in sample.

Now you finish the setting of genesis block, and type 2 againg to export it.

```=bash
What would you like to do? (default = stats)
 1. Show network stats
 2. Manage existing genesis
 3. Track new remote server
 4. Deploy network components
> 2

 1. Modify existing fork rules
 2. Export genesis configuration
 3. Remove genesis configuration
> 2
```

Then you can decide the name of the genesis block file, I keep it default(hakachain.json).

```=bash
Which file to save the genesis into? (default = hakachain.json)
>
```

Ok, now you are done, press Ctrl + C to leave puppeth.

## 

[link](https://google.com)  

[Anchor](#description)

*Italic*  

**Strong**

# Chapter

```=bash
Code or Command ...
```

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

The genesis block file(hakachain.json) will be created at your current working directory.

Now you are done, press Ctrl + C to leave puppeth.

## Initialize nodes

use command below to initialize each node.

```=bash
// Be notice your current working directory
geth --datadir node1/data init hakachain.json
geth --datadir node2/data init hakachain.json
geth --datadir miner1/data init hakachain.json
geth --datadir miner2/data init hakachain.json
```

## Open node console by Geth

use command below to start the console of each node.

```=bash
geth --datadir ./node1/data --networkid 111 --port 2000 console  
geth --datadir ./node2/data --networkid 111 --port 2001 console  
geth --datadir ./miner1/data --networkid 111 --port 2002 console  
geth --datadir ./miner2/data --networkid 111 --port 2003 console

//if you are using windows like me, you sholud put --ipcdisable in command when you open more than 2 console on the same time.

// geth --ipcdisable --datadir ./node2/data --networkid 111 --port 2001 console

```

More detail about IPC problem, please check [here](https://github.com/ethereum/go-ethereum/issues/1714)

## Adding peers to node

Now we can start 4 node in the same private chain, but we need them to be connected with each other.

Use below command in node2, miner1 and miner2 console to add node1 as peer.

```=bash
// The formation is " enode:// [ID] @ [IP] : [port] "
// Use "admin.nodeInfo" to find node's ID
admin.addPeer("enode://4ddc9ef3a5265a05672f6266a7bedb252c554ef1e7c83976defe52f4e75f5ed36e094568b505482a488ad53d63dc29230071c614131f88150802b3533584d507@127.0.0.1:2000")
```

Now we can use **admin.peers** in node1 console to check peers list.

```=bash
// You should see something like this:
> admin.peers
[{
    caps: ["eth/63"],
    id: "175ff7dc4f253c2bafb807b09d00adf2ee9daf5b078b492e8e0f4dc712418a52cdea449d361cd326b54cc94f1e723c1a80a340dccb4760741c9fe90786de7637",
    name: "Geth/v1.8.10-stable-eae63c51/windows-amd64/go1.10.2",    network: {
      inbound: true,
      localAddress: "127.0.0.1:2000",
      remoteAddress: "127.0.0.1:60680",
      static: false,
      trusted: false
    },    protocols: {      eth: {
        difficulty: 17179869184,
        head: "0xd4e56740f876aef8c010b86a40d5f56745a118d0906a34e69aec8c0db1cb8fa3",
        version: 63
      }
    }
}, {
    ...
}]
```

## Sending Transaction between nodes

Use below command to check whether or not node1 and node2 has currency.
(node 1 should have, but node2 shouldn't)

```=bash
// node1 console
eth.getBalance(eth.accounts[0]).toString(10)
// node2 console
eth.getBalance(eth.accounts[0]).toString(10)
```

Before node1 sends Tx to node2, you should **unlock node1**, or you will get an error.

```=bash
personal.unlockAccount(eth.accounts[0])
// Now it will ask you to enter the password of node1, I use node1@hakachain in sample for node1.
...

// Now you can use command below to transfer some currency from node1 to node2.
eth.sendTransaction({from:"1d8745ae5d1c759e5da9655f18143c0c87d9005a", to:"c202a7b62222aa27bd4bbf4d5062cc68d5578ca3", value: web3.toWei(0.05, "ether")})
```

If there is nothing wrong, you can see such information on your CLI:
```=bash
INFO [06-10|15:14:52] Submitted transaction                    fullhash=0x79b6291e2a783466014ffe7d61a4138a70522adb19704649a2d41c955435dd75 recipient=0xC202a7B62222AA27bD4BBf4d5062Cc68D5578cA3
"0x79b6291e2a783466014ffe7d61a4138a70522adb19704649a2d41c955435dd75"
```

However, This transaction is still pending, because there is no miner to mine.

Use command below to check the transaction pool.

```=bash
txpool

// then you may see something like this:
> txpool
{
  content: {
    pending: {      0x1D8745aE5d1C759E5Da9655f18143c0c87d9005a: {        0: {...}
      }
    },
    queued: {}
  },
  inspect: {
    pending: {
      0x1D8745aE5d1C759E5Da9655f18143c0c87d9005a: {
        0: "0xC202a7B62222AA27bD4BBf4d5062Cc68D5578cA3: 50000000000000000 wei + 90000 gas × 18000000000 wei"
      }
    },
    queued: {}  },
  status: {
    pending: 1,
    queued: 0
  },
  getContent: function(callback),
  getInspect: function(callback),
  getStatus: function(callback)
}
```

## Start Mining



[link](https://google.com)  

[Anchor](#description)

*Italic*  

**Strong**

# Chapter

```=bash
Code or Command ...
```

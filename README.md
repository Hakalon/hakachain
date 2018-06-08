# hakachain

In this project, I'm going to build a private chain of ethereum on Windows.
There will be 4 nodes as normal node and miner on this private chain that is using PoW for consensus.

## Enviroment

First, you should install [Geth(Go Ethereum)](https://geth.ethereum.org/downloads/) for running a full ethereum node.

Be notice that there is a *checkbox named Development tool* you should checked during the installation.

Now you are able to type **geth --help** on CMD or Bash to get help information for Geth.

## Generate account for 4 nodes

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

Now there is a folder name "keystore" inside node1/data and others.

## Generate genesis block

Use commands below to generate genesis block.
```=bash
puppeth
```

[link](https://google.com)  

[Anchor](#description)

*Italic*  

**Strong**

# Chapter

```=bash
Code or Command ...
```

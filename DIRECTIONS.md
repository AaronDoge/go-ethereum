# 说明

以太坊的官方Golang实现版本，直接从[ethereum/go-ethereum](https://github.com/ethereum/go-ethereum) fork来的，目的是用于对以太坊源码的研究。

主要研究在以下几个方面的代码：
- 以太坊core
- P2P网络
- RPC
- 智能合约contract
等。

目前，对区块链各技术在应用层面是如何实现的，还不是很清晰。所以借对以太坊源码的研究来加深对区块链的理解。本来想用比特币源码，但一是考虑到实现语言的区别，bitcoin的官方实现语言是CPP，我对CPP并不是很熟悉，而且Golang很可能是以后的工作语言(担忧。。)；再就是相比bitcoin，ethereum在技术层面有更好的前景。

研究以上几个方面的实现算法，以及如何与论文中的内容相结合。
- 基于新的数据结构的完整性检验技术研究
- 基于区块链的可扩展的安全多方计算协议研究


## core

1. 区块Block
先看区块链中的基本单位，区块Block
位于 core/types/block.go

```go
// Block represents an entire block in the Ethereum blockchain.
type Block struct {
	header       *Header
	uncles       []*Header
	transactions Transactions

	// caches
	hash atomic.Value
	size atomic.Value

	// Td is used by package core to store the total difficulty
	// of the chain up to and including the block.
	td *big.Int

	// These fields are used by package eth to track
	// inter-peer block relay.
	ReceivedAt   time.Time
	ReceivedFrom interface{}
}
```

由以上源码可以看出，Block是一个结构体。
包含header、uncles、交易transactions
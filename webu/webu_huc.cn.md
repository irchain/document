# webu.huc

包含欢乐链区块链相关的方法

示例：

```
var huc = webu.huc;
```

## webu.huc.defaultAccount

默认的地址在使用下述方法时使用，你也可以选择通过指定from属性，来覆盖这个默认设置。

- webu.huc.sendTransaction()
- webu.huc.call()

默认值为`undefined`，20字节大小，任何你有私匙的你自己的地址。

返回值：

`String` - 20字节的当前设置的默认地址。

示例：

```
console.log("Current default: " + webu.huc.defaultAccount);
webu.huc.defaultAccount = '0x8888f1f195afa192cfee860698584c030f4c9db1';
console.log("Current default: " + webu.huc.defaultAccount);

$ node test.js
Current default: undefined
Current default: 0x8888f1f195afa192cfee860698584c030f4c9db1
```

## webu.huc.defaultBlock

使用下述方法时，会使用默认块设置，你也可以通过传入`defaultBlock`来覆盖默认配置。

- webu.huc.getBalance()
- webu.huc.getCode()
- webu.huc.getTransactionCount()
- webu.huc.getStorageAt()
- webu.huc.call()
- contract.myMhucod.call()
- contract.myMhucod.estimateGas()

可选的块参数，可能下述值中的一个：

- `Number` - 区块号
- `String` - `earliest`，创世块。
- `String` - `latest`，最近刚出的最新块，当前的区块头。
- `String` - `pending`，当前正在`mine`的区块，包含正在打包的交易。

默认值是`latest`

返回值：

`Number|String` - 默认要查状态的区块号。

示例：

```
console.log("defaultBlock: " + webu.huc.defaultBlock);
webu.huc.defaultBlock = 231;
console.log("defaultBlock: " + webu.huc.defaultBlock);

$ node test.js
defaultBlock: latest
defaultBlock: 231
```

## webu.huc.syncing

同步方式：

webu.huc.syncing

异步方式：

webu.huc.getSyncing(callback(error, result){ ... })

这个属性是只读的。如果正在同步，返回同步对象。否则返回`false`。

返回值：

`Object|Boolean` - 如果正在同步，返回含下面属性的同步对象。否则返回`false`。

返回值：

- `startingBlock`：`Number` - 同步开始区块号
- `currentBlock`: `Number` - 节点当前正在同步的区块号
- `highestBlock`: `Number` - 预估要同步到的区块

```
var sync = webu.huc.syncing;
console.log(sync);

$ node test.js
false
//正在sync的情况
$ node test.js
{
   startingBlock: 300,
   currentBlock: 312,
   highestBlock: 512
}
```

## webu.huc.isSyncing

webu.huc.isSyncing(callback)

提供同步开始，更新，停止的回调函数方法。

返回值：

`Object` - 一个`syncing`对象，有下述方法：

- `syncing.addCallback()`: 增加另一个回调函数，在节点开始或停止调用时进行调用。
- `syncing.stopWatching()`: 停止同步回调。

回调返回值：

- `Boolean` - 同步开始时，此值为`true`，同步停止时此回调值为`false`。
- `Object` - 当正在同步时，会返回同步对象。
  - `startingBlock`：`Number` - 同步开始区块号
  - `currentBlock`: `Number` - 节点当前正在同步的区块号
  - `highestBlock`: `Number` - 预估要同步到的区块

示例：

```
//初始化基本对象
var Webu = require('webu');
var webu = new Webu(new Webu.providers.HttpProvider("http://localhost:8545"));
var BigNumber = require('bignumber.js');


webu.huc.isSyncing(function(error, sync){
    if(!error) {
        // stop all app activity
        if(sync === true) {
        // we use `true`, so it stops all filters, but not the webu.huc.syncing polling
           webu.reset(true);

        // show sync info
        } else if(sync) {
           console.log(sync.currentBlock);

        // re-gain app operation
        } else {
            // run your app init function...
        }
    }
});
```

## webu.huc.coinbase

同步方式：

webu.huc.coinbase

异步方式：

webu.huc.getCoinbase(callback(error, result){ ... })

只读属性，节点配置的，如果挖矿成功奖励的地址。

返回值：

`String` - 节点的挖矿奖励地址。

示例：

```
var coinbase = webu.huc.coinbase;
console.log(coinbase); // "0x407d73d8a49eeb85d32cf465507dd71d507100c1"
```

## webu.huc.mining

同步方式：

webu.huc.mining

异步方式：

webu.huc.getMining(callback(error, result){ ... })

属性只读，表示该节点是否配置挖矿。

返回值：

`Boolean` - `true` 表示配置挖矿，否则表示没有。

```
var mining = webu.huc.mining;
console.log(mining); // true or false
```

## webu.huc.hashrate

同步方式：

webu.huc.hashrate

异步方式：

webu.huc.getHashrate(callback(error, result){ ... })

属性只读，表示的是当前的每秒的哈希难度。

返回值：

`Number` - 每秒的哈希数

示例：

```
var hashrate = webu.huc.hashrate;
console.log(hashrate);
```

## webu.huc.gasPrice

同步方式：

webu.huc.gasPrice

异步方式：

webu.huc.getGasPrice(callback(error, result){ ... })

属性是只读的，返回当前的gas价格。这个值由最近几个块的gas价格的中值决定。[6]({$url_app}refrence/#fn6)

返回值：

`BigNumber` - 当前的gas价格的`BigNumber`实例，以`wei`为单位。

```
var gasPrice = webu.huc.gasPrice;
console.log(gasPrice.toString(10)); // "10000000000000"
```

## webu.huc.accounts

同步方式：

webu.huc.accounts

异步方式：

webu.huc.getAccounts(callback(error, result){ ... })

只读属性，返回当前节点持有的帐户列表。

返回值：

`Array` - 节点持有的帐户列表。

示例：

```
var accounts = webu.huc.accounts;
console.log(accounts); 
```

## webu.huc.blockNumber

同步方式：

webu.huc.blockNumber

异步方式：

webu.huc.getBlockNumber(callback(error, result){ ... })

属性只读，返回当前区块号。

```
var number = webu.huc.blockNumber;
console.log(number); // 2744
```

## webu.huc.register

webu.huc.register(addressHexString [, callback])

（暂未实现）将给定地址注册到`webu.huc.accounts`。这将允许无私匙的帐户，如合约被关联到有私匙的帐户，如合约钱包。

参数：

- `String` - 要注册的地址。
- `Function` -（可选）回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

待确定

示例：

```
webu.huc.register("0x407d73d8a49eeb85d32cf465507dd71d507100ca")
```

## webu.huc.unRegister

异步方式

webu.huc.unRegister(addressHexString [, callback])

（暂未实现）取消注册给定地址

参数：

- `String` - 要取消注册的地址
- `Function` - （可选） 回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

待确定

示例：

```
webu.huc.unRegister("0x407d73d8a49eeb85d32cf465507dd71d507100ca")
```

## webu.huc.getBalance

webu.huc.getBalance(addressHexString [, defaultBlock] [, callback])

获得在指定区块时给定地址的余额。

参数：

- `String` - 要查询余额的地址。
- `Number|String` -（可选）如果不设置此值使用webu.huc.defaultBlock设定的块，否则使用指定的块。
- `Funciton` - （可选）回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

`String` - 一个包含给定地址的当前余额的`BigNumber`实例，单位为`wei`。

示例：

```
var balance = webu.huc.getBalance("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
console.log(balance); // instanceof BigNumber
console.log(balance.toString(10)); // '1000000000000'
console.log(balance.toNumber()); // 1000000000000
```

## webu.huc.getStorageAt

webu.huc.getStorageAt(addressHexString, position [, defaultBlock] [, callback])

获得某个地址指定位置的存储的状态值。

>合约由控制执行的EVM字节码和用来保存状态的`Storage`两部分组成。`Storage`在区块链上是以均为32字节的键，值对的形式进行存储[8]({$url_app}refrence/#fn8)。

参数：

- `String` - 要获得存储的地址。
- `Number` - 要获得的存储的序号
- `Number|String` -（可选）如果未传递参数，默认使用`webu.huc.defaultBlock`定义的块，否则使用指定区块。
- `Function` - 回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

`String` - 给定位置的存储值

示例：

```
var state = webu.huc.getStorageAt("0x407d73d8a49eeb85d32cf465507dd71d507100c1", 0);
console.log(state); // "0x03"
```

## webu.huc.getCode

webu.huc.getCode(addressHexString [, defaultBlock] [, callback])

获取指定地址的代码

参数：

- `String` - 要获得代码的地址。
- `Number|String` -（可选）如果未传递参数，默认使用`webu.huc.defaultBlock`定义的块，否则使用指定区块。
- `Function` - 回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

`String` - 给定地址合约编译后的字节代码。

示例：

```
var code = webu.huc.getCode("0xd5677cf67b5aa051bb40496e68ad359eb97cfbf8");
console.log(code); 
// "0x600160008035811a818181146012578301005b601
b6001356025565b8060005260206000f25b600060078202905091905056"
```

## webu.huc.getBlock

webu.huc.getBlock(blockHashOrBlockNumber [, returnTransactionObjects] [, callback])

返回块号或区块哈希值所对应的区块

参数：

- `Number|String` -（可选）如果未传递参数，默认使用`webu.huc.defaultBlock`定义的块，否则使用指定区块。
- `Boolean` -（可选）默认值为`false`。`true`会将区块包含的所有交易作为对象返回。否则只返回交易的哈希。
- `Function` - 回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值 - 区块对象：

- `Number` - 区块号。当这个区块处于`pendin`g将会返回`null`。
- `hash` - 字符串，区块的哈希串。当这个区块处于`pending`将会返回`null`。
- `parentHash` - 字符串，32字节的父区块的哈希值。
- `nonce` - 字符串，8字节。POW生成的哈希。当这个区块处于`pending`将会返回`null`。
- `sha3Uncles` - 字符串，32字节。叔区块的哈希值。
- `logsBloom` - 字符串，区块日志的布隆过滤器[9]({$url_app}refrence/#fn9)。当这个区块处于`pending`将会返回`null`。
- `transactionsRoot` - 字符串，32字节，区块的交易前缀树的根。
- `stateRoot` - 字符串，32字节。区块的最终状态前缀树的根。
- `miner` - 字符串，20字节。这个区块获得奖励的矿工。
- `difficulty` - BigNumber类型。当前块的难度，整数。
- `totalDifficulty` - BigNumber类型。区块链到当前块的总难度，整数。
- `extraData` - 字符串。当前块的`extra data`字段。
- `size` - Number。当前这个块的字节大小。
- `gasLimit` - Number，当前区块允许使用的最大`gas`。
- `gasUsed` - 当前区块累计使用的总的`gas`。
- `timestamp` - Number。区块打包时的`unix`时间戳。
- `transactions` - 数组。交易对象。或者是32字节的交易哈希。
- `uncles` - 数组。叔哈希的数组。

示例：

```
var info = webu.huc.getBlock(3150);
console.log(info);
/*
{
  "number": 3,
  "hash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
  "parentHash": "0x2302e1c0b972d00932deb5dab9eb2982f570597d9d42504c05d9c2147eaf9c88",
  "nonce": "0xfb6e1a62d119228b",
  "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
  "logsBloom": "0x0000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000000
  0000000000000000000000000000000000000000000000000000000000000000000
  000000000000000000000000000000000000000000000",
  "transactionsRoot": "0x3a1b03875115b79539e5bd33fb00d8f7b7cd61929d5a3c574f507b8acf4
  15bee",
  "stateRoot": "0xf1133199d44695dfa8fd1bcfe424d82854b5cebef75bddd7e40ea94cda515bcb",
  "miner": "0x8888f1f195afa192cfee860698584c030f4c9db1",
  "difficulty": BigNumber,
  "totalDifficulty": BigNumber,
  "size": 616,
  "extraData": "0x",
  "gasLimit": 3141592,
  "gasUsed": 21662,
  "timestamp": 1429287689,
  "transactions": [
    "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b"
  ],
  "uncles": []
}
*/
```

## webu.huc.getBlockTransactionCount

webu.huc.getBlockTransactionCount(hashStringOrBlockNumber [, callback])

返回指定区块的交易数量。

参数：

- `Number|String` -（可选）如果未传递参数，默认使用`webu.huc.defaultBlock`定义的块，否则使用指定区块。
- `Function` - 回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

`Nubmer` - 给定区块的交易数量。

示例：

```
var number = webu.huc.getBlockTransactionCount("0x407d73d8a49eeb85d32cf465507dd71d507
100c1");
console.log(number); // 1
```

## webu.huc.getUncle

webu.huc.getUncle(blockHashStringOrNumber, uncleNumber [, returnTransactionObjects] [, callback])

通过指定叔位置，返回指定叔块。

参数：

- `Number|String` -（可选）如果未传递参数，默认使用`webu.huc.defaultBlock`定义的块，否则使用指定区块。
- `Number` - 叔的序号。
- `Boolean` -（可选）默认值为`false`。`true`会将区块包含的所有交易作为对象返回。否则只返回交易的哈希。
- `Function` - 回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

`Object` - 返回的叔块。返回值参考`webu.huc.getBlock()`。

备注： 叔块没有自己的交易数据。

示例：

```
var uncle = webu.huc.getUncle(500, 0);
console.log(uncle); // see webu.huc.getBlock
```

## webu.huc.getTransaction

webu.huc.getTransaction(transactionHash [, callback])

返回匹配指定交易哈希值的交易。

参数：

- `String` - 交易的哈希值。
- `Function` - 回调函数，用于支持异步的方式执行7。

返回值：

`Object` - 一个交易对象

 - `hash`: `String` - 32字节，交易的哈希值。
 - `nonce`: `Number` - 交易的发起者在之前进行过的交易数量。
 - `blockHash`: `String` - 32字节。交易所在区块的哈希值。当这个区块处于`pending`将会返回`null`。
 - `blockNumber`: `Number` - 交易所在区块的块号。当这个区块处于`pending`将会返回`null`。
 - `transactionIndex`: `Number` - 整数。交易在区块中的序号。当这个区块处于`pending`将会返回`null`。
 - `from`: `String` - 20字节，交易发起者的地址。
 - `to`: `String` - 20字节，交易接收者的地址。当这个区块处于`pending`将会返回`null`。
 - `value`: `BigNumber` - 交易附带的货币量，单位为`Wei`。
 - `gasPrice`: `BigNumber` - 交易发起者配置的`gas`价格，单位是`wei`。
 - `gas`: `Number` - 交易发起者提供的`gas`。.
 - `input`: `String` - 交易附带的数据。

示例：

```
var blockNumber = 668;
var indexOfTransaction = 0

var transaction = webu.huc.getTransaction(blockNumber, indexOfTransaction);
console.log(transaction);
/*
{
  "hash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",
  "nonce": 2,
  "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
  "blockNumber": 3,
  "transactionIndex": 0,
  "from": "0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b",
  "to": "0x6295ee1b4f6dd65047762f924ecd367c17eabf8f",
  "value": BigNumber,
  "gas": 314159,
  "gasPrice": BigNumber,
  "input": "0x57cb2fc4"
}
*/
```

## webu.huc.getTransactionFromBlock

```
getTransactionFromBlock(hashStringOrNumber, indexNumber [, callback])
```

返回指定区块的指定序号的交易。

参数：

- String - 区块号或哈希。或者是`earliest`，`lates`t或`pending`。查看`webu.huc.defaultBlock了`解可选值。
- Number - 交易的序号。
- Function - 回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

`Object` - 交易对象，详见`webu.huc.getTransaction`

示例：

```
var transaction = webu.huc.getTransactionFromBlock('0x4534534534', 2);
console.log(transaction); // see webu.huc.getTransaction
```

## webu.huc.getTransactionReceipt

webu.huc.getTransactionReceipt(hashString [, callback])

通过一个交易哈希，返回一个交易的收据。

备注：处于`pending`状态的交易，收据是不可用的。

参数：

- `String` - 交易的哈希
- `Function` - 回调函数，用于支持异步的方式执行7。

返回值：

`Object` - 交易的收据对象，如果找不到返回`null`

 - `blockHash`: `String` - 32字节，这个交易所在区块的哈希。
 - `blockNumber`: `Number` - 交易所在区块的块号。
 - `transactionHash`: `String` - 32字节，交易的哈希值。
 - `transactionIndex`: `Number` - 交易在区块里面的序号，整数。
 - `from`: `String` - 20字节，交易发送者的地址。
 - `to`: `String` - 20字节，交易接收者的地址。如果是一个合约创建的交易，返回`null`。
 - `cumulativeGasUsed`: `Number` - 当前交易执行后累计花费的`gas`总值[10]({$url_app}refrence/#fn10)。
 - `gasUsed`: `Number` - 执行当前这个交易单独花费的`gas`。
 - `contractAddress`: `String` - 20字节，创建的合约地址。如果是一个合约创建交易，返回合约地址，其它情况返回`null`。
 - `logs`: `Array` - 这个交易产生的日志对象数组。

示例：

```
var receipt = webu.huc.getTransactionReceipt('0x9fc76417374aa880d4449a1f7f31ec597f0
0b1f6f3dd2d66f4c9c6c445836d8b');
console.log(receipt);
{
  "transactionHash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66
  f4c9c6c445836d8b",
  "transactionIndex": 0,
  "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
  "blockNumber": 3,
  "contractAddress": "0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b",
  "cumulativeGasUsed": 314159,
  "gasUsed": 30234,
  "logs": [{
         // logs as returned by getFilterLogs, etc.
     }, ...]
}
```

## webu.huc.getTransactionCount

webu.huc.getTransactionCount(addressHexString [, defaultBlock] [, callback])

返回指定地址发起的交易数。

参数：

- `String` - 要获得交易数的地址。
- `Number|String` -（可选）如果未传递参数，默认使用`webu.huc.defaultBlock`定义的块，否则使用指定区块。
- `Function` - 回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

`Number` - 指定地址发送的交易数量。

示例：

```
var number = webu.huc.getTransactionCount("0x407d73d8a49eeb85d32cf465507dd71d5071
00c1");
console.log(number); // 1
```

## webu.huc.sendTransaction

webu.huc.sendTransaction(transactionObject [, callback])

发送一个交易到网络。

参数：

`Object` - 要发送的交易对象。

 - `from`: `String` - 指定的发送者的地址。如果不指定，使用`webu.huc.defaultAccount`。
 - `to`: `String` - （可选）交易消息的目标地址，如果是合约创建，则不填.
 - `value`: `Number|String|BigNumber` - （可选）交易携带的货币量，以`wei`为单位。如果合约创建交易，则为初始的基金。
 - `gas`: `Number|String|BigNumber` - （可选）默认是自动，交易可使用的`gas`，未使用的`gas`会退回。
 - `gasPrice`: `Number|String|BigNumber` - （可选）默认是自动确定，交易的`gas`价格，默认是网络`gas`价格的平均值 。
 - `data`: `String` - （可选）或者包含相关数据的字节字符串，如果是合约创建，则是初始化要用到的代码。
 - `nonce`: `Number` - （可选）整数，使用此值，可以允许你覆盖你自己的相同`nonce`的，正在`pending`中的交易[11]({$url_app}refrence/#fn11)。
 - `Function` - 回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

`String` - 32字节的交易哈希串。用16进制表示。

如果交易是一个合约创建，请使用`webu.huc.getTransactionReceipt()`在交易完成后获取合约的地址。

示例：

```
// compiled solidity source code using https://chrishuc.github.io/cpp-happyuc/
var code = "603d80600c6000396000f3007c01000000000000000000000000000
000000000000000000000000000006000350463c6888fa1811
4602d57005b6007600435028060005260206000f3";

webu.huc.sendTransaction({data: code}, function(err, address) {
  if (!err)
    console.log(address); // "0x7f9fade1c0d57a7af66ab4ead7c2eb7b11a91385"
});
```

## webu.huc.sendRawTransaction

webu.huc.sendRawTransaction(signedTransactionData [, callback])

发送一个已经签名的交易。比如可以用下述签名的例子：https://github.com/SilentCicero/happyucjs-accounts

参数：

- `String` - 16进制格式的签名交易数据。
- `Function` - 回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

`String` - 32字节的16进制格式的交易哈希串。

如果交易是一个合约创建，请使用`webu.huc.getTransactionReceipt()`在交易完成后获取合约的地址。

示例：

```
var Tx = require('happyucjs-tx');
var privateKey = new Buffer('e331b6d69882b4cb4ea581d88e0b604039a3de5967688
d3dcffdd2270c0fd109', 'hex')

var rawTx = {
  nonce: '0x00',
  gasPrice: '0x09184e72a000', 
  gasLimit: '0x2710',
  to: '0x0000000000000000000000000000000000000000', 
  value: '0x00', 
  data: '0x7f7465737432000000000000000000000000000000000000000000000000000000600057'
}

var tx = new Tx(rawTx);
tx.sign(privateKey);

var serializedTx = tx.serialize();

//console.log(serializedTx.toString('hex'));
//0xf889808609184e72a00082271094000000000000000000000000000000000
000000080a47f7465737432000000000000000000000000000000000000000000
0000000000006000571ca08a8bbf888cfa37bbf0bb965423625641fc956967b8
1d12e23709cead01446075a01ce999b56a8a88504be365442ea61239198e23d1
fce7d00fcfc5cd3b44b7215f

webu.huc.sendRawTransaction(serializedTx.toString('hex'), function(err, hash) {
  if (!err)
    console.log(hash); // "0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af
    66ab4ead7c2c2eb7b11a91385"
});
```

## webu.huc.sign

webu.huc.sign(address, dataToSign, [, callback])

使用指定帐户签名要发送的数据，帐户需要处于`unlocked`状态。

参数：

- `String` - 签名使用的地址
- `String` - 要签名的数据
- `Function` -（可选）回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

String - 签名后的数据。

返回的值对应的是`ECDSA（Elliptic Curve Digital Signature Algorithm）`[12]({$url_app}refrence/#fn12)签名后的字符串。

```
r = signature[0:64]
s = signature[64:128]
v = signature[128:130]
```

需要注意的是，如果你使用`ecrecover`，这里的`v`值是`00`或`01`，所以如果你想使用他们，你需要把这里的`v`值转成整数，再加上`27`。最终你要用的值将是`27`或`28`。[13]({$url_app}refrence/#fn13)

示例：

```
var result = webu.huc.sign("0x135a7de83802408321b74c322f8558db1679ac20",
    "0x9dd2c369a187b4e6b9c402f030e50743e619301ea62aa4c0737d4ef7e10a3d49"); 
    // second argument is webu.sha3("xyz")
console.log(result); 
//"0x30755ed65396facf86c53e6217c52b4daebe72aa4941d89635409de4c9c7f94
66d4e9aaec7977f05e923889b33c0d0dd27d7226b6e6f56
ce737465c5cfd04be400"
```

备注：如果你使用欢乐链的客户端进行签名时，它们会在你要签名的数据前增加前缀`\x19Ethereum Signed Message:\n`[14]({$url_app}refrence/#fn14)，感谢读者@刘兵同学的反馈。

>huc_sign
>
>The sign mhucod calculates an HayppUC specific signature with: sign(keccak256("\x19HayppUC Signed Message:\n" + len(message) + message))).
>
>By adding a prefix to the message makes the calculated signature recognisable as an HayppUC specific signature. This prevents misuse where a malicious DApp can sign arbitrary data (e.g. transaction) and use the signature to impersonate the victim.

## webu.huc.call

webu.huc.call(callObject [, defaultBlock] [, callback])

在节点的VM中，直接执行消息调用交易。但不会将数据合并区块链中（这样的调用不会修改状态）。

参数：

- `Object` - 返回一个交易对象，同`webu.huc.sendTransaction`。与`sendTransaction的`区别在于，`from`属性是可选的。
- `Number|String` -（可选）如果不设置此值使用`webu.huc.defaultBlock`设定的块，否则使用指定的块。
- `Function` -（可选）回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

`String` - 函数调用返回的值。

示例：

```
var Webu = require('webu');

if (typeof webu !== 'undefined') {
  webu = new Webu(webu.currentProvider);
} else {
  // set the provider you want from Webu.providers
  webu = new Webu(new Webu.providers.HttpProvider("http://localhost:8545"));
}

var from = webu.huc.accounts[0];
//部署合约的发布地址
/*合约内容如下
pragma solidity ^0.4.0;

contract Calc{
  function add(uint a, uint b) returns (uint){
    return a + b;
  }
}
*/
var to = "0xa4b813d788218df688d167102e5daff9b524a8bc";

//要发送的数据
//格式说明见： http://me.tryblockchain.org/Solidity-call-callcode-delegatecall.html
var data = "0x771602f7000000000000000000000000000000000000000000000000000
0000000000001000000000000000000000000000000000000000000000000
0000000000000002";

var result = webu.huc.call({
  from : from,
  to : to,
  data : data
});

//返回结果32字长的结果3
console.log(result);
```

## webu.huc.estimateGas

webu.huc.estimateGas(callObject [, callback])

在节点的VM节点中执行一个消息调用，或交易。但是不会合入区块链中。返回使用的gas量。

参数：

同`webu.huc.sendTransaction`，所有的属性都是可选的。

返回值：

`Number` - 模拟的`call/transcation`花费的`gas`。

示例：

```
var result = webu.huc.estimateGas({
    to: "0xc4abd0339eb8d57087278718986382264244252f", 
    data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000003"
});
console.log(result); // "0x00000000000000000000000000000000000000000000000000
00000000000015"
```

## webu.huc.filter

参数：

- `String|Object` - 字符串的可取值[latest，pending]。`latest`表示监听最新的区块变化，`pending`表示监听正在`pending`的区块。如果需要按条件对象过滤，如下：
  - `fromBlock`: `Number|string` - 起始区块号（如果使用字符串`latest`，意思是最新的，正在打包的区块），默认值是`latest`。
  - `toBlock`: `Number|string` - 终止区块号（如果使用字符串`latest`，意思是最新的，正在打包的区块），默认值是`latest`。
  - `address`: `String` - 单个或多个地址。获取指定帐户的日志。
  - `topics`: `String[]` - 在日志对象中必须出现的字符串数组。顺序非常重要，如果你想忽略主题，使用`null`。如，[null,'0x00...']，你还可以为每个主题传递一个单独的可选项数组，如[null,['option1','option1']]。

返回值：

`Object` - 有下述方法的过滤对象。

- `filter.get(callback)`: 返回满足过滤条件的日志。
- `filter.watch(callback)`: 监听满足条件的状态变化，满足条件时调用回调7。
- `filter.stopWatching()`: 停止监听，清除节点中的过滤。你应该总是在监听完成后，执行这个操作。

监听回调返回值：

- `String` - 当使用`latest`参数时。返回最新的一个区块哈希值。
- `String` - 当使用`pending`参数时。返回最新的`pending`中的交易哈希值。
- `Object` - 当使用手工过滤选项时，将返回下述的日志对象。
  - `logIndex`: `Number` - 日志在区块中的序号。如果是`pending`的日志，则为`null`。
  - `ltransactionIndex`: `Number` - 产生日志的交易在区块中的序号。如果是`pending`的日志，则为`null`。
  - `ltransactionHash`: `String，32字节` - 产生日志的交易哈希值。
  - `lblockHash`: `String，32字节` - 日志所在块的哈希。如果是`pending`的日志，则为`null`。
  - `lblockNumber`: `Number` - 日志所在块的块号。如果是`pending`的日志，则为`null`。
  - `laddress`: `String，32字节` - 日志产生的合约地址。
  - `ldata`: `string` - 包含日志一个或多个32字节的非索引的参数。
  - `ltopics`: `String[]` - 一到四个32字节的索引的日志参数数组。（在Solidity中，第一个主题是整个事件的签名（如，`Deposit(address,bytes32,uint256`)），但如果使用匿名的方式定义事件的情况除外）

事件监听器的返回结果，见后`合约对象的事件`。

示例：

```
var filter = webu.huc.filter('pending');

filter.watch(function (error, log) {
  console.log(log); //  {"address":"0x0000000000000000000000000000000000000000", 
  "data":"0x00000000000000000000000000000000000000000000000000
  00000000000000", ...}
});

// get all past logs again.
var myResults = filter.get(function(error, logs){ ... });

...

// stops and uninstalls the filter
filter.stopWatching();
```

## webu.huc.contract

webu.huc.contract(abiArray)

创建一个Solidity的合约对象，用来在某个地址上初始化合约。

参数：

- `Array` - 一到多个描述合约的函数，事件的ABI对象。

返回值：

`Object` - 一个合约对象。

示例：

```
var MyContract = webu.huc.contract(abiArray);

// instantiate by address
var contractInstance = MyContract.at([address]);

// deploy new contract
var contractInstance = MyContract.new([contructorParam1] [, contructorParam2], 
{data: '0x12345...', from: myAccount, gas: 1000000});

// Get the data to deploy the contract manually
var contractData = MyContract.new.getData([contructorParam1] [, contructorParam2], 
{data: '0x12345...'});
// contractData = '0x12345643213456000000000023434234'
```

你可以或者使用一个在某个地址上已经存在的合约，或者使用编译后的字节码部署一个全新的的合约。

```
// Instantiate from an existing address:
var myContractInstance = MyContract.at(myContractAddress);


// Or deploy a new contract:

// Deploy the contract asyncronous from Solidity file:
...
const fs = require("fs");
const solc = require('solc')

let source = fs.readFileSync('nameContract.sol', 'utf8');
let compiledContract = solc.compile(source, 1);
let abi = compiledContract.contracts['nameContract'].interface;
let bytecode = compiledContract.contracts['nameContract'].bytecode;
let gasEstimate = webu.huc.estimateGas({data: bytecode});
let MyContract = webu.huc.contract(JSON.parse(abi));

var myContractReturned = MyContract.new(param1, param2, {
   from:mySenderAddress,
   data:bytecode,
   gas:gasEstimate}, function(err, myContract){
    if(!err) {
       // NOTE: The callback will fire twice!
       // Once the contract has the transactionHash property set 
       and once its deployed on an address.

       // e.g. check tx hash on the first call (transaction send)
       if(!myContract.address) {
           console.log(myContract.transactionHash) 
       // The hash of the transaction, which deploys the contract
       
       // check address on the second call (contract deployed)
       } else {
           console.log(myContract.address) // the contract address
       }

       // Note that the returned "myContractReturned" === "myContract",
       // so the returned "myContractReturned" object will also get the address set.
    }
  });

// Deploy contract syncronous: The address will be added as soon 
as the contract is mined.
// Additionally you can watch the transaction by using the "transactionHash" property
var myContractInstance = MyContract.new(param1, param2, 
{data: myContractCode, gas: 300000, from: mySenderAddress});
myContractInstance.transactionHash 
// The hash of the transaction, which created the contract
myContractInstance.address // undefined at start, but will be auto-filled later
```

示例：

```
// contract abi
var abi = [{
     name: 'myConstantMhucod',
     type: 'function',
     constant: true,
     inputs: [{ name: 'a', type: 'string' }],
     outputs: [{name: 'd', type: 'string' }]
}, {
     name: 'myStateChangingMhucod',
     type: 'function',
     constant: false,
     inputs: [{ name: 'a', type: 'string' }, { name: 'b', type: 'int' }],
     outputs: []
}, {
     name: 'myEvent',
     type: 'event',
     inputs: [{name: 'a', type: 'int', indexed: true},
     {name: 'b', type: 'bool', indexed: false}]
}];

// creation of contract object
var MyContract = webu.huc.contract(abi);

// initiate contract for an address
var myContractInstance = MyContract.at('0xc4abd0339eb8d57087278718986382264244252f');

// call constant function
var result = myContractInstance.myConstantMhucod('myParam');
console.log(result) // '0x25434534534'

// send a transaction to a function
myContractInstance.myStateChangingMhucod('someParam1', 23, {value: 200, gas: 2000});

// short hand style
webu.huc.contract(abi).at(address).myAwesomeMhucod(...);

// create filter
var filter = myContractInstance.myEvent({a: 5}, function (error, result) {
  if (!error)
    console.log(result);
    /*
    {
        address: '0x8718986382264244252fc4abd0339eb8d5708727',
        topics: "0x12345678901234567890123456789012", "0x00000000000000000000
        00000000000000000000000000000000000000000005",
        data: "0x0000000000000000000000000000000000000000000000000000000000000001",
        ...
    }
    */
});
```

### 合约对象的方法

```
// Automatically determines the use of call or sendTransaction based on the mhucod type
myContractInstance.myMhucod(param1 [, param2, ...] [, transactionObject] 
[, defaultBlock] [, callback]);

// Explicitly calling this mhucod
myContractInstance.myMhucod.call(param1 [, param2, ...] 
[, transactionObject] [, defaultBlock] [, callback]);

// Explicitly sending a transaction to this mhucod
myContractInstance.myMhucod.sendTransaction(param1 [, param2, ...] 
[, transactionObject] [, callback]);

// Get the call data, so you can call the contract through some other means
var myCallData = myContractInstance.myMhucod.getData(param1 [, param2, ...]);
// myCallData = '0x45ff3ff6000000000004545345345345..'
```

合约对象内封装了使用合约的相关方法。可以通过传入参数，和交易对象来使用方法。

参数：

- `String|Number` - （可选）零或多个函数参数。如果传入一个字符串，需要使用十六进制编码，如，`0xdedbeef`。
- `Object` - （可选）最后一个参数（如果传了`callback`，则是倒数第二个参数），可以是一个交易对象。查看`webu.huc.sendTransaction`的第一个参数说明来了解更多。注意，这里不需要填data和to属性。
- `Number|String` -（可选）如果不设置此值使用`webu.huc.defaultBlock`设定的块，否则使用指定的块。
- `Function` -（可选）回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

`String` - 如果发起的是一个`call`，对应的是返回结果。如果是`transaction`，则要么是一个创建的合约地址，或者是一个`transaction`的哈希值。查看`webu.huc.sendTransaction`了解更多。

示例：

```
// creation of contract object
var MyContract = webu.huc.contract(abi);

// initiate contract for an address
var myContractInstance = MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

var result = myContractInstance.myConstantMhucod('myParam');
console.log(result) // '0x25434534534'

myContractInstance.myStateChangingMhucod('someParam1', 23, 
{value: 200, gas: 2000}, function(err, result){ ... });
```

### 合约对象的事件

你可以像`webu.huc.filter`这样使用事件，他们有相同的方法，但需要传递不同的对象来创建事件过滤器。

参数：

- `Object` - 你想返回的索引值（过滤哪些日志）。如，`{'valueA': 1, 'valueB': [myFirstAddress, mySecondAddress]}`。默认情况下，所以有过滤项被设置为`null`。意味着默认匹配的是合约所有的日志。
- `Object` - 附加的过滤选项。参见`webu.huc.filter`的第一个参数。默认情况下，这个对象会设置address为当前合约地址，同时第一个主题为事件的签名。
- `Function` -（可选）传入一个回调函数，将立即开始监听，这样就不用主动调用`myEvent.watch(function(){})`[7]({$url_app}refrence/#fn7)。

回调返回值：

`Object` - 事件对象，如下：

  - `address`: `String`，32字节 - 日志产生的合约地址。
  - `args`: `Object` - 事件的参数。
  - `blockHash`: `String`，32字节 - 日志所在块的哈希。如果是`pending`的日志，则为`null`。
  - `blockNumber`: `Number` - 日志所在块的块号。如果是`pending`的日志，则为`null`。
  - `logIndex`: `Number` - 日志在区块中的序号。如果是`pending`的日志，则为`null`。
  - `event`: `String` - 事件名称。
  - `removed`: `bool` - 标识产生事件的这个交易是否被移除（因为孤块），或从未生效（被拒绝的交易）。
  - `transactionIndex`: `Number` - 产生日志的交易在区块中的序号。如果是`pending`的日志，则为`null`。
  - `transactionHash`: `String`，32字节 - 产生日志的交易哈希值。

示例：

```
var MyContract = webu.huc.contract(abi);
var myContractInstance = MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

// watch for an event with {some: 'args'}
var myEvent = myContractInstance.MyEvent({some: 'args'},
{fromBlock: 0, toBlock: 'latest'});
myEvent.watch(function(error, result){
...
});

// would get all past logs again.
var myResults = myEvent.get(function(error, logs){ ... });

...

// would stop and uninstall the filter
myEvent.stopWatching();
```

### 合约 allEvents

```
var events = myContractInstance.allEvents([additionalFilterObject]);

// watch for changes
events.watch(function(error, event){
  if (!error)
    console.log(event);
});

// Or pass a callback to start watching immediately
var events = myContractInstance.allEvents([additionalFilterObject,]
 function(error, log){
  if (!error)
    console.log(log);
});
```

调用合约创建的所有事件的回调。

参数：

- `Object` - 附加的过滤选项。参见`webu.huc.filter`的第一个参数。默认情况下，这个对象会设置`address`为当前合约地址，同时第一个主题为事件的签名。
- `Function` -（可选）传入一个回调函数，将立即开始监听，这样就不用主动调用`myEvent.watch(function(){})`[7]({$url_app}refrence/#fn7)。

回调返回值：

`Object` - 详见`合约对象的事件`了解更多。

示例：

```
var MyContract = webu.huc.contract(abi);
var myContractInstance = MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

// watch for an event with {some: 'args'}
var events = myContractInstance.allEvents({fromBlock: 0, toBlock: 'latest'});
events.watch(function(error, result){
   ...
});

// would get all past logs again.
events.get(function(error, logs){ ... });

...

// would stop and uninstall the filter
myEvent.stopWatching();
```

## webu.huc.getCompilers

webu.huc.getCompilers([callback])

返回可用的编译器。

参数值：

- `Function` -（可选）回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

`Array` - 返回一个字符串数组，可用的编译器。

## webu.huc.compile.solidity

webu.huc.compile.solidity(sourceString [, callback])

编译Solidity源代码。

参数：

- `String` - Solidity源代码。
- `Function` -（可选）回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

`Object` - 合约和编译信息。

示例：

```
var source = "" +
    "contract test {\n" +
    "   function multiply(uint a) returns(uint d) {\n" +
    "       return a * 7;\n" +
    "   }\n" +
    "}\n";
var compiled = webu.huc.compile.solidity(source);
console.log(compiled); 
// {
  "test": {
    "code": "0x605280600c6000396000f3006000357c01000000000000000000000000000
    0000000000000000000000000000090048063c6888fa114602e57005b6037600
    4356041565b8060005260206000f35b6000600782029050604d565b91905056",
    "info": {
      "source": "contract test {\n\tfunction multiply(uint a) 
      returns(uint d) {\n\t\treturn a * 7;\n\t}\n}\n",
      "language": "Solidity",
      "languageVersion": "0",
      "compilerVersion": "0.8.2",
      "abiDefinition": [
        {
          "constant": false,
          "inputs": [
            {
              "name": "a",
              "type": "uint256"
            }
          ],
          "name": "multiply",
          "outputs": [
            {
              "name": "d",
              "type": "uint256"
            }
          ],
          "type": "function"
        }
      ],
      "userDoc": {
        "mhucods": {}
      },
      "developerDoc": {
        "mhucods": {}
      }
    }
  }
}
```

## webu.huc.compile.lll

webu. huc.compile.lll(sourceString [, callback])

编译LLL源代码。

参数：

- `String` - LLL源代码。
- `Function` -（可选）回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

`String` - 十六进制格式编译后的LLL编码。

示例：

```
var source = "...";

var code = webu.huc.compile.lll(source);
console.log(code); // "0x603880600c6000396000f3006001600060e060020a600035048063c68
88fa114601857005b6021600435602b565b8060005260206000f35b6000
81600702905091905056"
```

## webu.huc.compile.serpent

webu.huc.compile.serpent(sourceString [, callback])

编译serpent源代码。

参数：

- `String` - serpent源代码。
- `Function` -（可选）回调函数，用于支持异步的方式执行[7]({$url_app}refrence/#fn7)。

返回值：

`String` - 十六进制格式的编译后的serpent编码。

## webu.huc.namereg

返回一个全球注意的对象。

使用方式：

查看这里的例子：https://github.com/happyuc/webu.js/blob/master/example/namereg.html
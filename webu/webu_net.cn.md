# webu.net

## webu.net.listening

同步方式：

webu.net.listening

异步方式：

webu.net.getListener(callback(error, result){ ... })

此属性是只读的，表示当前连接的节点，是否正在`listen`网络连接与否。`listen`可以理解为接收。

返回值：

`Boolean` - `true`表示连接上的节点正在listen网络请求，否则返回`false`。

示例：

```
var listening = webu.net.listening;
console.log("client listening: " + listening);

$ node test.js
client listening: true
```

备注： 如果关闭我们要连接的测试节点，会报错`Error: Invalid JSON RPC response: undefined`。所以这个方法返回的是我们连上节点的`listen`状态。

## webu.net.peerCount

同步方式：

webu.net.peerCount

异步方式：

webu.net.getPeerCount(callback(error, result){ ... })

属性是只读的，返回连接节点已连上的其它欢乐链节点的数量。

返回值：

`Number` - 连接节点连上的其它欢乐链节点的数量

示例：

```
var peerCount = webu.net.peerCount;
console.log("Peer count: " + peerCount); 

$ node test.js
Peer count: 0
```
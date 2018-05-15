Webu.js API 基本

为了让你的Ðapp运行上欢乐链，一种选择是使用[webu.js library](https://github.com/happyuc-project/webu.js)提供的`webu。`对象。底层实现上，它通过[RPC 调用](https://github.com/happyuc-project/wiki/wiki/JSON-RPC)与本地节点通信。webu.js可以与任何暴露了RPC接口的欢乐链节点连接。

`webu`中有`huc`对象 - `webu.huc` 具体来表示与欢乐链区块链之间的交互。`shh`对象 - `webu.shh`表示`Whisper`协议的相关交互。后续我们会继续介绍其它一些webu协议中的对象。可用的[example can be found here](https://github.com/happyuc-project/webu.js/tree/master/example)

# 入门

## 添加webu

首先你需要将webu引入到你的工程中，通过如下步骤：

- npm: `npm install webu`
- bower: `bower install webu`
- metor: `meteor add happyuc:webu`
- vanilla: `dist./webu.min.js`

然后你需要创建一个webu的实例，设置一个`provider`。为了保证你不会覆盖一个已有的`provider`，比如使用Mist时有内置，需要先检查是否`webu`实例已存在。

```
if (typeof webu !== 'undefined') {
webu = new Webu(webu.currentProvider);
} else {
// set the provider you want from Webu.providers
webu = new Webu(new Webu.providers.HttpProvider("http://localhost:8545"));
}
```

成功引入后，你现在可以使用`webu`的相关[API](https://github.com/happyuc-project/wiki/wiki/webujs-api-reference)了。

## 使用callback

由于这套API被设计来与本地的RPC结点交互，所有函数默认使用同步的HTTP的请求。

如果你想发起一个异步的请求。大多数函数允许传一个跟在参数列表后的可选的回调函数来支持异步。回调函数支持[error first callback](http://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js/)的风格。

```
webu.huc.getBlock(48, function(error, result){
    if(!error)
        console.log(result)
    else
        console.error(error);
})
```

## 批量请求

可以允许将多个请求放入队列，并一次执行。

注意：批量请求并不会更快，在某些情况下，同时发起多个请求，由于是异步的，会变得更快。但这里的批量请求主要目的是用来保证请求的串行执行。

## 关于webu.js中的Big Number的说明

数据类型的返回结果，你将始终会得到一个`BigNumber`对象，因为Javascript不能正确的处理`BigNumber`，如下面的例子：

```
"101010100324325345346456456456456456456"
// "101010100324325345346456456456456456456"
101010100324325345346456456456456456456
// 1.0101010032432535e+38
```

所以webu.js依赖[BigNumber Library](https://github.com/MikeMcl/bignumber.js/)，且会自动进行引入。

```
var balance = new BigNumber('131242344353464564564574574567456');
// or var balance = webu.huc.getBalance(someAddress);

balance.plus(21).toString(10); 
// toString(10) converts it to a number string
// "131242344353464564564574574567477"
```

下一个例子中，我们会看到，如果有20位以上的浮点值，仍会导致出错。所以，我们推荐尽量让帐户余额以wei为单位，仅仅在需要向用户展示时，才转换为其它单位。

```
var balance = new BigNumber('13124.234435346456466666457455567456');

balance.plus(21).toString(10); 
// toString(10) converts it to a number string, but can only show max 20 
floating points
// "13145.23443534645646666646" // you number would be cut after the 20 floating point
```

- - -

1.[Big Number 文档链接](https://github.com/MikeMcl/bignumber.js)
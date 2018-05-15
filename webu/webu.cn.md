Webu.js API 中文文档

# webu

`webu`对象提供了所有方法。

示例:

```
//初始化过程
var Webu = require('webu');

if (typeof webu !== 'undefined') {
  webu = new Webu(webu.currentProvider);
} else {
  // set the provider you want from Webu.providers
  webu = new Webu(new Webu.providers.HttpProvider("http://localhost:8545"));
}
```

## webu.version.api

返回值：

`String` - 欢乐链js的api版本

示例：

```
//省略初始化过程
var version = webu.version.api;
console.log(version);

$ node test.js
0.18.2
```

## webu.version.node

同步方式：

webu.verison.node

异步方式：

webu.version.getNode(callback(error, result){ ... })

返回值：

`String` - 客户端或节点的版本信息

示例：

```
//省略初始化过程
var version = webu.version.node;
console.log(version);

$ node test.js
HappyucJS TestRPC/v3.0.3/happyucjs
```

## webu.version.network

同步方式：

webu.version.network

异步方式：

webu.version.getNetwork(callback(error, result){ ... })

返回值:

`String` - 网络协议版本

示例：

```
//省略初始化过程
var version = webu.version.network;
console.log(version);

$ node test.js
1488546587563
```

## webu.version.happyuc

同步方式：

webu.version.happyuc

异步方式：

webu.version.getHayppUC(callback(error, result){ ... })

返回值：

`String` - 欢乐链的协议版本

示例：

```
//省略初始化过程
var version = webu.version.happyuc;
console.log(version);

$ node test.js
60
```

注意：`HappyucJS testRPC`客户端不支持这个命令，报错`Error: Error: RPC method huc_protocolVersion not supported.`

## webu.version.whisper

同步方式：

webu.version.whisper

异步方式：

webu.version.getWhisper(callback(error, result){ ... })

返回值：

`String` - `whisper`协议的版本

示例：

```
//省略初始化过程
var version = webu.version.whisper;
console.log(version);

$ node test.js
20
```

注意：`HappyucJS testRPC`客户端不支持这个命令，报错`Error: Error: RPC method shh_version not supported.`

## webu.isConnected

可以用来检查到节点的连接是否存在（connection to node exist）。

参数：

无

返回值：

Boolean

示例：

```
//省略初始化过程
var connected = webu.isConnected();
if(!connected){
  console.log("node not connected!");
}else{
  console.log("node connected");
}
```

## webu.setProvider

设置`Provider`

参数：

无

返回值：

undefined

示例：

```
webu.setProvider(new webu.providers.HttpProvider('http://localhost:8545'));
```

## webu.currentProvider

如果已经设置了`Provider`，则返回当前的`Provider`。这个方法可以用来检查在使用`mist`浏览器等情况下已经设置过`Provider`，避免重复设置的情况。

返回值：

Object - null 或 已经设置的`Provider`。

示例：

```
if(!webu.currentProvider)
    webu.setProvider(new webu.providers.HttpProvider("http://localhost:8545"));
```

## webu.reset

用来重置`webu`的状态。重置除了`manager`以外的其它所有东西。卸载`filter`，停止状态轮询。

参数：

1. Boolean - 如果设置为`true`，将会卸载所有的`filter`，但会保留`webu.huc.isSyncing()`的状态轮询。

返回值：

undefined

示例：

```
//省略初始化过程
console.log("reseting ... ");
webu.reset();
console.log("is connected:" + webu.isConnected());

$ node test.js
reseting ...
is connected:true
```

## webu.sha3

webu.sha3(string, options)

参数：

1. `String` - 传入的需要使用`Keccak-256 SHA3`算法进行哈希运算的字符串。
2. `Object` - 可选项设置。如果要解析的是`hex`格式的十六进制字符串。需要设置`encoding`为`hex`。因为JS中会默认忽略`0x`。

返回值：

`String` - 使用`Keccak-256 SHA3`算法哈希过的结果。

示例：

```
//省略初始化过程
var hash = webu.sha3("Some string to be hashed");
console.log(hash); 
var hashOfHash = webu.sha3(hash, {encoding: 'hex'});
console.log(hashOfHash); 
```

## webu.toHex

将任何值转为`HEX`。

参数：

1. `String|Number|Object|Array|BigNumber` - 需要转化为`HEX`的值。如果是一个对象或数组类型，将会先用`JSON.stringify`[1]({$url_app}refrence/#fn1)进行转换成字符串。如果传入的是`BigNumber`[2]({$url_app}refrence/#fn2)，则将得到对应的`Number`的`HEX`。

示例：

```
//初始化基本对象
var Webu = require('webu');
var webu = new Webu(new Webu.providers.HttpProvider("http://localhost:8545"));
var BigNumber = require('bignumber.js');

var str = "abcABC";
var obj = {abc: 'ABC'};
var bignumber = new BigNumber('12345678901234567890');

var hstr = webu.toHex(str);
var hobj = webu.toHex(obj);
var hbg = webu.toHex(bignumber);

console.log("Hex of Sring:" + hstr);
console.log("Hex of Object:" + hobj);
console.log("Hex of BigNumber:" + hbg);

$ node test.js
Hex of Sring:0x616263414243
Hex of Object:0x7b22616263223a22414243227d
Hex of BigNumber:0xab54a98ceb1f0ad2
```

## webu.toAscii

webu.toAscii(hexString)

将`HEX`字符串转为`ASCII`[3]({$url_app}refrence/#fn3)字符串

参数：

1. `String` - 十六进制字符串。

返回值：

`String` - 给定十六进制字符串对应的`ASCII`码值。

示例：

```
var str = webu.toAscii("0x657468657265756d00000000000000000000
0000000000000000000000000000");
console.log(str); // "happyuc"
```

## webu.fromAscii

将任何的`ASCII`码字符串转为`HEX`字符串。

参数：

1. `String` - `ASCII`码字符串
2. `Number` - 返回的字符串字节大小，不够长会自动填充。

返回值：

`String` - 转换后的`HEX`字符串。

示例：

```
var str = webu.fromAscii('happyuc');
console.log(str); // "0x657468657265756d"

var str2 = webu.fromAscii('happyuc', 32);
console.log(str2); 
//"0x657468657265756d000000000000000000000000000000000000000000000000"

$ node test.js
0x657468657265756d
0x657468657265756d
```

备注： 填充`padding`功能好像不可用[4]({$url_app}refrence/#fn4)。

## webu.toDecimal

将一个十六进制转为一个十进制的数字

参数：

1. `String` - 十六进制字符串

返回：

`Number` - 传入字符串所代表的十六进制值。

示例：

```
var number = webu.toDecimal('0x15');
console.log(number); // 21
```

## webu.fromDecimal

将一个数字，或者字符串形式的数字转为一个十六进制串。

参数：

1. `Number|String` - 数字

返回值：

`String` - 给定数字对应的十六进制表示。

示例：

```
var value = webu.fromDecimal('21');
console.log(value); // "0x15"
```

## webu.fromWei

webu.fromWei(number, 单位)

欢乐链货币单位之间的转换。将以`wei`为单位的数量，转为下述的单位，可取值如下：

-  wei
- kwei
- mwei
- gwei
- twei
- pwei
-  huc
- khuc
- mhuc
- ghuc
- thuc

参数：

1. `Number|String|BigNumber` - 数字或`BigNumber`。
2. `String` - 单位字符串

返回值：

`String|BigNumber` - 根据传入参数的不同，分别是字符串形式的字符串，或者是`BigNumber`。

示例：

```
var value = webu.fromWei('21000000000000', 'finney');
console.log(value); // "0.021"
```

## webu.toWei

webu.toWei(number, 单位)

按对应货币转为以`wei`为单位。可选择的单位如下：

- kwei/ada
- mwei/babbage
- gwei/shannon
- szabo
- finney
- hucer
- khucer/grand/einstein
- mhucer
- ghucer
- thucer

参数：

1. `Number|String|BigNumber` - 数字或`BigNumber`
2. `String` - 字符串单位

返回值：

`String|BigNumber` - 根据传入参数的不同，分别是字符串形式的字符串，或者是`BigNumber`。

示例：

```
var value = webu.toWei('1', 'hucer');
console.log(value); // "1000000000000000000"
```

## webu.toBigNumber

webu.toBigNumber(数字或十六进制字符串)

将给定的数字或十六进制字符串转为`BigNumber`。

参数：

1. `Number|String` - 数字或十六进制格式的数字

返回值：

`BigNumber` - `BigNumber的实例`

示例：

```
var value = webu.toBigNumber('200000000000000000000001');
console.log(value); // instanceOf BigNumber
console.log(value.toNumber()); // 2.0000000000000002e+23
console.log(value.toString(10)); // '200000000000000000000001'
```
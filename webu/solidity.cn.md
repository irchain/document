webu.js编译Solidity，发布，调用全部流程（手把手教程）

下面教程是打算在尽量牵涉可能少的欢乐链的相关工具，主要使用webu.js这个欢乐链提供的工具包，来完成合约的编译，发布，合约方法调用的一整个流程。一方面来了解欢乐链开发到底需要什么，另一方面来对webu.js的API有个基本的了解。由于所有其它工具都或多或少的是对webu.js的底层函数的包装，所以对webu.js使用流程有个认识之后，也能更好的入门，使用相关的工具。

# 准备工作

## 安装Node.js

由于我们要使用webu.js[1]({$url_app}#fn1)。这里使用Node来集成webu.js模块（当然，你还可以使用其它的方式）。你可以通过参考官网文档安装[2]({$url_app}#fn2)。

### Ubuntu

如果你使用ubuntu，可以使用下述命令：

```
//安装Node
$ sudo apt-get install nodejs
//安装Node的包管理器
$ sudo apt-get install npm
```

### MAC

如果你使用`Homebrew`，可以使用下述命令：

```
//安装Node
$ brew install node
//安装Node的包管理器
$ brew install npm
```

### 安装检查

安装成功后，可以查看下当前的版本，确认正常安装：

``` bash
$ node -v
v7.2.0
```

## 欢乐链的节点

由于整个合约代码的执行需要一个虚拟机环境，所以在开始之前，我们不得不安装一个实现了欢乐链虚拟机的节点。

可以选择一个轻量级的节点，比如`HappyucJS TestRPC`，它是一个完整的在内存中的区块链仅仅存在于你开发的设备上。它在执行交易时是实时返回，而不等待默认的出块时间，这样你可以快速验证你新写的代码，当出现错误时，也能即时反馈给你。

``` bash
$ npm install -g happyucjs-testrpc
```

安装好后，你就可以通过`testrpc`命令来启动了，启动与大多数欢乐链节点一样，运行在`localhost:8545`。

如果你安装`ghuc`这样的客户端也是可以的。

## Webu的支持

安装`webu`的模块[1]({$url_app}#fn1)：

``` bash
$ npm install webu
```

# 合约编译

## 一个简单的合约

我们打算用来测试的合约如下：

```
pragma solidity ^0.4.0;

contract Calc{
  /*区块链存储*/
  uint count;

  /*执行会写入数据，所以需要`transaction`的方式执行。*/
  function add(uint a, uint b) returns(uint){
    count++;
    return a + b;
  }

  /*执行不会写入数据，所以允许`call`的方式执行。*/
  function getCount() constant returns (uint){
    return count;
  }
}
```

`add()`方法用来返回输入两个数据的和，并会对`add()`方法的调用次数进行计数。需要注意的是这个计数是存在区块链上的，对它的调用需要使用`transaction`。

`getCount()`返回`add()`函数的调用次数。由于这个函数不会修改区块链的任何状态，对它的调用使用`call`就可以了。

## 编译合约

由于合约是使用`Solidity`编写，所以我们可以使用`webu.huc.compile.solidity`来编译合约[3]({$url_app}#fn3)：

```
//编译合约
let source = "pragma solidity ^0.4.0;contract Calc{  
    /*区块链存储*/  
    uint count;  
    /*执行会写入数据，所以需要`transaction`的方式执行。*/  
    function add(uint a, uint b) returns(uint){    count++;    return a + b;  }  
    /*执行不会写入数据，所以允许`call`的方式执行。*/  
    function getCount() returns (uint){    return count;  }
    }";

let calc = webu.huc.compile.solidity(source);
```

如果编译成功，结果如下：

```
{
    code: '0x606060405234610000575b607e806100176000396000f3606060405260e0600
    20a6000350463771602f781146026578063a87d942c146048575b6000565b34600
    05760366004356024356064565b60408051918252519081900360200190f35b346
    0005760366077565b60408051918252519081900360200190f35b6000805460010
    190558181015b92915050565b6000545b9056',
    info: {
        source: 'pragma solidity ^0.4.0;contract Calc{  
            /*区块链存储*/  
            uint count;  
            /*执行会写入数据，所以需要`transaction`的方式执行。*/  
            function add(uint a, uint b) returns(uint){    count++;    return a + b;  }  
            /*执行不会写入数据，所以允许`call`的方式执行。*/  
            function getCount() returns (uint){    return count;  }
            }',
        language: 'Solidity',
        languageVersion: '0.4.6+commit.2dabbdf0.Emscripten.clang',
        compilerVersion: '0.4.6+commit.2dabbdf0.Emscripten.clang',
        abiDefinition: [
            [
                Object
            ],
            [
                Object
            ]
        ],
        userDoc: {
            methods: {
                
            }
        },
        developerDoc: {
            methods: {
                
            }
        }
    }
}
```

# 发布合约

`webu.js`其实也像框架一样对合约的操作进行了封装。发布合约时，可以使用`webu.huc.contract`的`new`方法[4]({$url_app}#fn4)。

```
let myContractReturned = calcContract.new({
    data: deployCode,
    from: deployeAddr
}, function(err, myContract) {
    if (!err) {
        // 注意：这个回调会触发两次
        //一次是合约的交易哈希属性完成
        //另一次是在某个地址上完成部署

        // 通过判断是否有地址，来确认是第一次调用，还是第二次调用。
        if (!myContract.address) {
            console.log("contract deploy transaction hash: " + 
            myContract.transactionHash) //部署合约的交易哈希值

            // 合约发布成功
        } else {
        }
});
```

部署过程中需要主要的是，`new`方法的回调会执行两次，第一次是合约的交易创建完成，第二次是在某个地址上完成部署。需要注意的是只有在部署完成后，才能进行方法该用，否则会报错`TypeError: myContractReturned.add is not a function`。

# 调用合约

由于`webu.js`封装了合约调用的方法。我们可以使用可以使用`webu.huc.contract`的里的`sendTransaction`来修改区块链数据。在这里有个坑，有可能会出现`Error: invalid address`，原因是没有传`from`，交易发起者的地址。在使用`webu.js`的API都需留意，出现这种找不到地址的，都看看`from字段吧。`

```
//使用transaction方式调用，写入到区块链上
myContract.add.sendTransaction(1, 2,{
    from: deployeAddr
});

console.log("after contract deploy, call:" + myContract.getCount.call());
```

需要注意的是，如果要修改区块链上的数据，一定要使用`sendTransaction`，这会消耗`gas`。如果不修改区块链上的数据，使用`call`，这样不会消耗`gas`。

# 使用webu.js编译，发布，调用的完整源码

```
let Webu = require('webu');
let webu;

if (typeof webu !== 'undefined') {
    webu = new Webu(webu.currentProvider);
} else {
    // set the provider you want from Webu.providers
    webu = new Webu(new Webu.providers.HttpProvider("http://localhost:8545"));
}

let from = webu.huc.accounts[0];

//编译合约

let source = "pragma solidity ^0.4.0;contract Calc{  
    /*区块链存储*/  
    uint count;  
    /*执行会写入数据，所以需要`transaction`的方式执行。*/  
    function add(uint a, uint b) returns(uint){    count++;    return a + b;  }  
    /*执行不会写入数据，所以允许`call`的方式执行。*/  
    function getCount() constant returns (uint){    return count;  }
    }";
let calcCompiled = webu.huc.compile.solidity(source);

console.log(calcCompiled);
console.log("ABI definition:");
console.log(calcCompiled["info"]["abiDefinition"]);

//得到合约对象
let abiDefinition = calcCompiled["info"]["abiDefinition"];
let calcContract = webu.huc.contract(abiDefinition);

//2. 部署合约

//2.1 获取合约的代码，部署时传递的就是合约编译后的二进制码
let deployCode = calcCompiled["code"];

//2.2 部署者的地址，当前取默认账户的第一个地址。
let deployeAddr = webu.huc.accounts[0];

//2.3 异步方式，部署合约
let myContractReturned = calcContract.new({
    data: deployCode,
    from: deployeAddr
}, function(err, myContract) {
    if (!err) {
        // 注意：这个回调会触发两次
        //一次是合约的交易哈希属性完成
        //另一次是在某个地址上完成部署

        // 通过判断是否有地址，来确认是第一次调用，还是第二次调用。
        if (!myContract.address) {
            console.log("contract deploy transaction hash: " +
             myContract.transactionHash) //部署合约的交易哈希值

            // 合约发布成功后，才能调用后续的方法
        } else {
            console.log("contract deploy address: " + 
            myContract.address) // 合约的部署地址

            //使用transaction方式调用，写入到区块链上
            myContract.add.sendTransaction(1, 2,{
                from: deployeAddr
            });

            console.log("after contract deploy, call:" + myContract.getCount.call());
        }

        // 函数返回对象`myContractReturned`和回调函数对象`myContract`是 
        "myContractReturned" === "myContract",
        // 所以最终`myContractReturned`这个对象里面的合约地址属性也会被设置。
        // `myContractReturned`一开始返回的结果是没有设置的。
    }
});

//注意，异步执行，此时还是没有地址的。
console.log("returned deployed didn't have address now: " + 
myContractReturned.address);

//使用非回调的方式来拿到返回的地址，但你需要等待一段时间，直到有地址，建议使用上面的回调方式。
/*
setTimeout(function(){
  console.log("returned deployed wait to have address: " + myContractReturned.address);
  console.log(myContractReturned.getCount.call());
}, 20000);
*/

//如果你在其它地方已经部署了合约，你可以使用at来拿到合约对象
//calcContract.at(["0x50023f33f3a58adc2469fc46e67966b01d9105c4"]);
```
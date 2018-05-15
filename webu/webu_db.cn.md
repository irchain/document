# webu.db

## webu.db.putString

webu.db.putString(db, key, value)

这个方法应当在我们打算以一个本地数据库的级别存储一个字符串时使用。

参数：

- `String` - 存储使用的数据库。
- `String` - 存储的键。
- `String` - 存储的值。

返回值：

`Boolean` - `true`表示成功，否则返回`false`。

示例：

```
webu.db.putString('testDB', 'key', 'myString') // true
```

## webu.db.getString

webu.db.getString(db, key)

从本地的leveldb数据库中返回一个字符串。

参数：

- `String` - 存储使用的数据库。
- `String` - 存储的键。

返回值：

String - 存储的值。

示例：

```
var value = webu.db.getString('testDB', 'key');
console.log(value); // "myString"
```

## webu.db.putHex

webu.db.putHex(db, key, value)

在本地的leveldb中存储二进制数据。

参数：

- `String` - 存储使用的数据库。
- `String` - 存储的键。
- `String` - 十六进制格式的二进制。

返回值：

`Boolean` - 成功返回`true`，失败返回`false`。

示例：

```
webu.db.putHex('testDB', 'key', '0x4f554b443'); // true
```

## webu.db.getHex

webu.db.getHex(db, key)

返回本地的leveldb中的二进制数据。

参数：

- `String` - 存储使用的数据库。
- `String` - 存储的键。

返回值：

`String` - 存储的十六进制值。

示例：

```
var value = webu.db.getHex('testDB', 'key');
console.log(value); // "0x4f554b443"
```
- - -

1. https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify

2. Big Number 文档链接 https://github.com/MikeMcl/bignumber.js

3. ASCII码表 http://baike.baidu.com/link?url=fz4Ytl_tjpUhsPbUb4fa3hyXHKJDMcRB5M3K1p0VStnminbvLX4-UmwPCovk1pZUOIemosGv2hRT-r0flGMtEGk3ON8sQctG4-KU67G3fBiOJX6r1CHoKTi-K6BlEEa6egHulQju1p1n1ce1axyeBK

4. 参见[Webu.js API基本](https://www2.happyuc.org:1443/docs/webu/base/)中的`使用callback`的章节。

5. [https://zh.wikipedia.org/wiki/椭圆曲线密码学](https://zh.wikipedia.org/wiki/%E6%A4%AD%E5%9C%86%E6%9B%B2%E7%BA%BF%E5%AF%86%E7%A0%81%E5%AD%A6)
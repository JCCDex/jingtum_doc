# 井畅RPC服务

通过井畅http服务调用井通节点服务接口.

## 为什么使用JCC-RPC

井畅rpc服务对外暴露众多http服务节点, 这些http服务节点在后端对应众多socket服务, 比直接连接井通websocket节点能获取更稳定的服务能力和连接速度.

## 保证更高的吞吐量和稳定性

交易服务器和信息服务器会不定时更新, 所以请定时[获取配置文件](https://github.com/JCCDex/jcc_rpc#usage-example-of-config)更新交易服务器和信息服务器.

## 主要接口分类

[接口文档](https://github.com/JCCDex/jcc_server_doc)

1. 交易服务
   1. [获取历史交易数据](https://github.com/JCCDex/jcc_rpc#gethistorictransactions)
   2. [获取历史转账数据](https://github.com/JCCDex/jcc_rpc#gethistoricpayments)
   3. [获取余额](https://github.com/JCCDex/jcc_rpc#getbalances)
   4. [获取当前委托](https://github.com/JCCDex/jcc_rpc#getorders)
   5. [创建挂单](https://github.com/JCCDex/jcc_rpc#createorder)
   6. [取消挂单](https://github.com/JCCDex/jcc_rpc#deleteorder)
   7. [获取交易序列号](https://github.com/JCCDex/jcc_rpc#getsequence)
   8. [转账](https://github.com/JCCDex/jcc_rpc#transferaccount)

2. 信息服务
   1. [获取某交易对24小时行情](https://github.com/JCCDex/jcc_rpc#getticker)
   2. [获取所有交易对24小时行情](https://github.com/JCCDex/jcc_rpc#getalltickers)
   3. [获取市场挂单深度](https://github.com/JCCDex/jcc_rpc#getdepth)
   4. [获取K线数据](https://github.com/JCCDex/jcc_rpc#getkline)
   5. [获取最新成交](https://github.com/JCCDex/jcc_rpc#gethistory)

## 语言版本

目前这些版本广泛应用在诸如[威链](https://weidex.vip)、埃及汇兑、书签购物、诸多量化交易等应用中.

1. [java](https://github.com/JCCDex/jcc_rpc_java)

2. [javascript](https://github.com/JCCDex/jcc_rpc)

3. [php](https://github.com/JCCDex/jcc_rpc_php)

4. [object-c](https://github.com/JCCDex/jcc_rpc_oc)

5. [易语言](https://github.com/JCCDex/jcc_rpc_yi)

## 使用示例

```javascript
// 以javascript版本获取钱包余额举例
// npm i jcc_rpc

// 首先通过https://github.com/JCCDex/jcc_rpc#usage-example-of-config获取井畅交易服务器或信息服务器, 交易服务器为响应数据中exHosts字段，信息服务器为infoHosts字段
// 创建JcExchange实例, 实例对象上hosts包含可用的井畅交易节点
// 创建挂单会随机选取任一交易服务器, 达到前端分流的目的

import { JcConfig, JcExchange } from 'jcc_rpc';

let instance = new JcConfig(["jccdex.cn", "weidex.vip"], 443, true);
let res = await instance.getConfig();
const hosts = res.exHosts;
const port = 80;
const https = false;
instance = new JcExchange(hosts, port, https)

const address = "";

instance.getBalances(address).then(balance => {
    console.log(balance);
}).catch(error => {
    console.log(error);
});

```

## 交易

jcc_exchange在jcc_rpc基础上封装了转账、挂单和取消挂单交易.

```javascript

// npm i jcc_exchange
import JCCExchange from "jcc_exchange";

// example
const hosts = ["localhost"];
const port = 80;
const https = false;

// 初始化hosts、port和https
JCCExchange.init(hosts, port, https);

// 创建挂单
// 以一个swt买一个jjcc
const address = "";
const secret = "";
const amount = "1";
const base = "jjcc";
const counter = "swt";
const sum = "1";
const type = "buy"; // 如果卖, type值为"sell"
const issuer; // 发行地址默认值 "jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or"
try {
    const hash = await JCCExchange.createOrder(address, secret, amount, base, counter, sum, type, issuer);
    console.log(hash);
} catch (error) {
    console.log(error);
}

// 取消挂单
const address = "";
const secret = "";
const orderSequence = 0;
try {
    const hash = await JCCExchange.cancelOrder(address, secret, orderSequence);
    console.log(hash);
} catch (error) {
    console.log(error);
}

// 转账
// 从"jKTtq57iqHoHg3cP7Rryzug9Q2puLX1kHh"到 "jpgWGpfHz8GxqUjz5nb6ej8eZJQtiF6KhH"转1jjcc
const address = "jpgWGpfHz8GxqUjz5nb6ej8eZJQtiF6KhH";
const secret = "";
const amount = "1";
const memo = "test";
const to = "jKTtq57iqHoHg3cP7Rryzug9Q2puLX1kHh";
const token = "jjcc";
const issuer; // 发行地址默认值"jGa9J9TkqtBcUoHe2zqhVFFbgUVED6o9or"
try {
    const hash = await JCCExchange.transfer(address, secret, amount, memo, to, token, issuer);
    console.log(hash);
} catch (error) {
    console.log(error);
}
```

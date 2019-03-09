---
title: websocket使用案例
date: 2018-12-31 23:29:10
categories:
- http
tags:
- http
- 全双工
- websocket
---

# websocket

* WebSocket_API 规范定义了一个 API 用以在网页浏览器和服务器建立一个 socket 连接。通俗地讲：在客户端和服务器之间有一个持久的连接，两边可以在任意时间开始发送数据。
* html5 开始提的一种浏览器与服务器进行全双工的网络技术
* 属于应用层协议，它基于 tcp 传输协议，并复用 http 的握手通道。

## 1.websocket 优势

* 支持双向通信，实时性更强
* 更好的二进制支持
* 较少的控制开销，连接创建后，ws 客户端、服务端进行数据交换时，协议控制数据包头部较小。

## 2.websocket 实战

### 客户端代码

```
let ws = new WebSocket('ws://localhost:8888')
ws.onopen = function () {
    console.log('客户端连接成功')
    ws.send({ type: 'login', payload: { username: 'zbc', password: '123' } })
}
ws.onmessage = function (event) {
    console.log('收到服务器端的响应', event.data)
}
```

### 服务端代码

```
let express = require("express");
let app = express();
app.use(express.static(__dirname));
//http服务器
app.listen(3000, () => {
  console.log(3000);
});
let WebSocketServer = require("ws").Server;
let wsServer = new WebSocketServer({ port: 8888 });
wsServer.on("connection", socket => {
  console.log("客户端连接成功");
  socket.on("message", message => {
    console.log("接受客户端的消息", message);
    socket.send("服务器回应" + message);
  });
});
```

### 开启服务端，浏览器打开 http://localhost:3000/index.html

* 浏览器控制台

![](./client.png)

* 服务器日志

![](./server.png)

## 3.如何建立连接

* websocket 复用了 http 的握手通道。具体指的是，客户端通过 http 请求与 websocket 服务端协商升级协议。协议升级完成后，后续的数据交换则遵照 websocket 的协议。

### 1.客户端：申请协议升级

* 首先，客户端发起协议升级请求。可以看到，采用的是标准的 http 格式，并且只支持 get 方法。

```
GET ws://localhost:8888/HTTP/1.1
Host:localhost:监听8888端口
Connection:Upgrade
Sec-WebSocket-Version:13
Sec-WebSocket-Key: xl0sD4LsfBfipcm8jULjYA==
```

* Connection:Upgrade:表示要升级协议
* Upgrade：websocket：表示要升级到 websocket 协议
* Sec-WebSocket-Version： 13 :表示 websocket 的版本
* Sec-WebSocket-Key：与后面服务端响应首部的 Sec-WebSocket-Accept 是配套的，提供基本的防护，比如恶意的连接，或者无意的连接

### 2 服务端：响应协议升级

* 服务端返回内容如下，状态代码 101 表示协议切换。到此完成协议升级，后续的交互按照新的协议来。

```
HTTP/1.1 101 Switching Protocols
Upgrade:websocket
Connection:Upgrade
Sec-WebSocket-Accept:QVi4FvBm2f5rkYzKe4E4ObfpDOE=
```

### 3.Sec-WebSocket-Accept 的计算

#### Sec-WebSocket-Accept 根据客户端请求首部的 Sec-WebSocket-Key 计算出来。计算公式为：

* 将 Sec-WebSocket-Key 和 258EAFA5-E914-47DA-95CA-C5AB0DC85B11 拼接。
* 通过 SHA1 计算出摘要，并转成 base64 字符串。

```
const crypto=require('crypto')
const number='258EAFA5-E914-47DA-95CA-C5AB0DC85B11'
cosnt webSocket='QVi4FvBm2f5rkYzKe4E4ObfpDOE='
let websocketAccept= require('crypto').createHash('sha1').update(webSocketKey+number).digest('base64');
console.log(websocketAccept)
```

### 4.Sec-WebSocker-Key/Accept 的作用

* 避免服务端收到非法的 websocket 连接
* 确保服务端理解 websocket 连接
* 用浏览器里发起 ajax 请求，设置 header 时，Sec-WebSocket-Key 以及其他相关的 header 是被禁止的
* Sec-WebSocket-Key 主要目的并不是确保数据的安全性，因为 Sec-WebScoket-Key、Sec-WebScoket-Accept 的转换计算公式是公开的，而且非常简单，最主要的作用是预防一些常见的意外情况（非故意的）

## 数据帧格式

### 1.WebSocket 客户端、服务端通信的最小单位是帧，由 1 个或多个帧组成一条完整的消息（message）

* 发送端：将消息切割成多个帧，并发送给服务端
* 接收端：接收消息帧，并将关联的帧重新组装成完整的消息

### 2.数据帧格式

* 单位是比特。比如 FIN、RSV1 各占据 1 比特，opcode 占据 4 比特。
  ![](./dataform.png)

* FIN:1 比特如果是 1，表示这是消息（message）的最后一个分片（fragment）,如果是 0，表示不是消息（message）的最后一个分片（fragment）
* RSV1,RSV2,RSV3:各占 1 个比特。一般情况下全为 0。当客户端、服务端协商采用 websocket 扩展时，者三个标志可以非 0，且值的含义由扩展进行定义。如果出现非零的值，并且没有采用 webSocket 扩展，连接出错。
* Opcode:4 个比特。操作代码，Opcode 的值决定了应该如何解析后续的数据载荷（data payload）。如果操作代码是不认识的，那么接收端该断开连接（fail the connection）
  * %x0:表示一个延续帧。当 Opcode 为 0 时，表示本次数据传输采用了数据分片，当前收到的数据帧为其中一个数据分片。
  * %x1:表示这是一个文本帧（frame）
  * %x2：表示这是一个二进制帧（frame）
  * %x3-7：保留的操作代码，用于后续定义的非控制帧。
  * %x8:表示连接断开。
  * %x9:表示这是一个 ping 操作。
  * %xA:表示这是一个 pong 操作。
  * %xB-F：保留的操作代码，用于后续定义的控制帧。
* Mask：1 个比特。表示是否要对数据载荷进行掩码操作。
  * 从客户端向服务端发送数据时，需要对数据进行掩码操作；从服务端向客户端发送数据时，不需要对数据进行掩码操作，如果服务端接受到的数据没有进行过掩码操作，服务端需要端口连接。
  * 如果 Mask 是 1，那么在 Masking-key 中会定义一个掩码键（masking key），并用这个掩码键来对数据载荷进行反掩码。所有客户端发送到服务端的数据帧，Mask 都是 1。
* Payload length：数据载荷的长短，单位是字节。为 7 位，或 7+16 位，或 7+64 位。
  * Payload length =x 为 0~125：数据的长度为 x 字节。
  * Payload length =x 为 126:后续 2 个字节代表一个 16 位的无符号整数，该无符号整数的值为数据的长度。
  * Payload length =x 为 127：后续 8 个字节代表一个 64 位的无符号整数（最高为为 0），该无符号整数的值为数据的长度。
  * 如果 payload length 占用了多个字节的话，payload length 的二进制表达采用网络序（big endian ，重要的位在前）
* Masking-key：0 或 4 字节（32 位）所有从客户端传送到服务端的数据帧，数据载荷都进行了掩码操作，Mask 为 1，且携带了 4 个字节的 Masking-key。如果 Mask 为 0，则没有 Masking-key。载荷数据的长度，不包括 mask key 的长度。
* Payload data:(x+y)字节
  * 载荷数据：包括了扩展数据、应用数据。其中，扩展数据 x 字节，应用数据 y 字节。
  * 扩展数据：如果没有协商使用扩展的话，扩展数据为 0 字节。所有的扩展必须声明扩展数据的长度，或者可以如何扩展数据的长度。此外，扩展如何使用必须在握手阶段就协商好。如果扩展数据存在，那么载荷数据长度必须将扩展数据的长度包含在内。
  * 应用数据：任意的应用数据，在扩展数据之后（如果存在扩展数据），占据了数据帧剩余的位置。载荷数据长度 扩展数据长度，就得到应用数据的长度。

### 3 掩码算法

掩码键（Masking-Key）是由客户端挑选出来的 32 位的随机数。掩码操作不会影响数据载荷的长度。掩码、反掩码操作都采用如下算法：

* 对索引 i 取余 4 得到 j,因为掩码一共就是 4 个字节。
* 对原来的索引进行异或对应的掩码字节。
* 异或就是两个数的二进制形式，按位对比，相同取 0，不同取 1

```
function unmask(buffer,mask){
  const length=buffer.length;
  for(let i=0;i<length;i++){
    buffer[i]^=mask[!&3]
  }
}
```

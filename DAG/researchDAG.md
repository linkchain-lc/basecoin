## 目的

以DAG为技术基础，构造区块链3.0系统。

## 名词
区块等同于交易


地址等同于账户，DAG用户可以拥有多个账户地址，类似于比特币

## 账户与交易

	一个地址就是一个账户，每个账户都有自己的交易链

### 1. 账户组织
账户组织成类以太坊世界树形式，可以根据账户地址快速定位到账户信息。
账户主要信息包括用户地址，用户Token余额
![](https://github.com/linkchain-lc/basecoin/blob/master/DAG/source/researchDAG/pic1.png?raw=true)

### 2. 创世区块
创世区块预分配交易到指定账户
![](https://github.com/linkchain-lc/basecoin/blob/master/DAG/source/researchDAG/pic2.png?raw=true)

### 3. 用户交易链
一笔从a发送到b的交易划分为发送部分和接收部分，分别由a, b签名，发送到a, b各自对应的交易链上
![](https://github.com/linkchain-lc/basecoin/blob/master/DAG/source/researchDAG/pic3.png?raw=true)

### 4. 新增账户
当一笔交易发送到一个新地址的时候，就会新增一个对应账户的交易链
![](https://github.com/linkchain-lc/basecoin/blob/master/DAG/source/researchDAG/pic4.png?raw=true)

### 5. 签名验证
- 单一签名：

 + a发送一笔token到b, 则a对发送交易签名， b对接收交易签名，各自附加到对应交易链

- 多重签名：
 + <font color=#ff0000 size=3>待讨论</font>
		
## 网络组织
### 1. 节点类型
* 代表节点: 记录和观察整个网络交易，裁定双花交易。

* 普通节点: 记录和观察整个网络交易。

* 轻节点:  观察它兴趣的账户

* 引导节点:提供部分或整体账本。用于向新加入网络节点提供账本数据。

### 2. 传输协议
* 交易的广播通过UDP协议

* 投票的广播通过UDP协议

* 引导节点的数据传输通过TCP协议

## 分叉与双花

### 1. 分叉/双花场景

描述：用户的交易链上产生了两个交易指向相同前继的场景，导致交易链出现分叉

原因：在于用户自身构造了双花交易
![](https://github.com/linkchain-lc/basecoin/blob/master/DAG/source/researchDAG/pic5.png?raw=true)

### 2. 双花处理
核心思路是通过代表投票。<font color=#ff0000>细节待讨论</font>

## 激励机制
<font color=#ff0000>待讨论</font>
## 分层设计
<font color=#ff0000>待讨论</font>
## 攻击防御
<font color=#ff0000>待讨论</font>

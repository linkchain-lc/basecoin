## 一个跨区块链的DAPP开发平台(uniplatfrom)


### 功能
---
> #### 1. 通用开发平台
>  使用平台提供的API和开发语言，开发通用DAPP, 一键发布到指定外部链
> #### 2. 跨链通讯
>  通过平台通讯组件，DAPP之间具备相互传递消息能力
> #### 3. 快速链搭建
>  利用平台本身提供基于区块链3.0的专有链，快速搭建一条新的区块链
>

### 总体架构
---
> 整个系统呈现三层结构，dapp, uniplatform系统和外部链
> uniplatform与外部链通过各自的协议通讯
> uniplatform提供接口和服务给dapp使用
> ![](https://github.com/linkchain-lc/basecoin/blob/master/platform/source/pic1.png?raw=true)

### 通用开发平台描述
---
> - 提供一种通用开发语言（language）
> - 提供支持开发语言运行的中间字节码（bytecode）和虚拟机（vm)
> - 提供用户开发库，组件支持（component)
> - 提供用户数据存储支持(storage)
> - 提供安全机制，保障用户交易(security)



### 跨链通讯描述
---
> - 支持跨链消息路由
> - 支持消息的缓存和离线存储
> - 支持跨链通信的原子性和交易的一致性
> - 数据的容错机制
> - 数据传输的安全保障


### 快速链搭建描述
---
> - 系统自带专有链，通过简单的配置可以快速搭建用户链
> - 链采用区块链3.0DAG技术
> - 链设计充分考虑扩展性和可插拔特性，分层设计合理
> ![](https://github.com/linkchain-lc/basecoin/blob/master/platform/source/pic2.png?raw=true)

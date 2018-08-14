# ByteBall

## 名词
区块=交易

## 网络
### 节点:
* 中继节点（Relay）：负责向与其连接的节点转发单元，存储整个 Byteball 区块链数据库，但它本身不保存任何私钥，也不发送任何单元；
* 中枢节点（Hub）：负责为连接到它的设备提供端到端的加密消息传输通道，用于比如收发私密资产、多签名交易、聊天信息等，其它功能与中继节点相同，默认的 Hub 地址为wss://byteball.org/bb；
* 播报节点（Oracle）：负责不间断地向 Byteball 网络播报数据，数据可以是时间、价格、甚至是 Bitcoin 交易；
* 见证人节点（Witness）：负责不间断地以固定地址发送单元，任何满足该条件的节点都有可能成为见证人；
* 钱包节点（Wallet）：负责与用户交互，收发交易、消息等。

![](http://www.byteball.cn/wp-content/uploads/2018/06/2018-01-25-byteball_network-1.jpg)
## 账户与交易
byteball账户模型使用的是UTXO模型。

图中每个节点代表一笔交易。每笔交易使用父交易作为输入。一笔新交易的生成代表该交易的父交易（祖先交易）被直接或间接的验证。

交易=单元

单元的结构如下所示，其主要由三部分组成：

* 单元数据：数据以message的形式构成；
* 地址签名：输入所需的相应地址签名；
* 父单元：当前单元的父单元列表。
![](http://www.byteball.cn/wp-content/uploads/2017/02/DAG.png)


当某个单元达到稳定之后，就可以生成球(ball)，此时它的状态（是否有效）将确定性的固定下来，球的结构如下所示：

```
{
  unit: "hash of unit",
  parent_balls: [array of hashes of balls based on parent units], 
  is_nonserial: true, // this field included only if the unit is nonserial
  skiplist_balls: [array of earlier balls used to build skiplist]
}
```

## 主链
在 Byteball 中，从任何一个顶端单元出发到达创世单元的最优路径称为候选主链（Candidate Mainchain）。最优路径通过选择最优父单元产生，选择策略用于保证整个网络的安全性。不同的候选主链会在某个单元位置交叉（最差的情况是在创世单元交叉），该交叉点称为稳定点（Stable Point）。对于所有候选主链，从稳定点到创世单元的路径完全相同，该路径称为稳定主链（Stable Mainchain）。稳定主链是一条确定的路径，从候选路径变为稳定主链是一个从不确定逐渐变成确定的过程。

![](http://www.byteball.cn/wp-content/uploads/2017/12/2017122506133241.jpg)

给定一条主链，与之相关的所有单元均可以在此基础上进行排序，其序号称为主链序号（MCI, Main Chain Index）。创世单元的 MCI 为 0，依次加 1 直到链尾。对于不在主链上的单元，其 MCI 等于主链上最先包含（直接或者间接）该单元的那个单元的 MCI。MCI 代表了从主链视角来看单元在 DAG 中的总序，对于发生冲突的双花交易，MCI 较小的单元为有效单元。

## 双花
在相同地址的所有单元都连通的情况下，在路径上出现较早的交易为有效交易。如果有攻击者特意制造出双花交易，那么可以通过主链序号来解决，主链序号较小的交易为有效交易。

![](http://www.byteball.cn/wp-content/uploads/2017/12/2017122506132777.jpg)
上图给出了一种攻击场景，攻击者制造出一条影子链，并在上面发布双花交易。当影子链接入到真实的 DAG 中时，根据最优父单元选择策略，影子链上的见证人个数少，因此它不会成为主链的一部分，从而解决了这种场景下的双花问题。值得注意的是，如果大多数见证人与攻击者合谋，并在其影子链上发布单元，则攻击者有可能攻击成功。
#### Tx由Inputs和Outputs构成。Inputs是上一个Tx的Outputs
<img src="https://bitcoin.org/img/dev/en-tx-overview.svg" />


#### 花费的详细过程，从CTxOut到COutPoint,  构造一个CTxIn:
<img src="https://bitcoin.org/img/dev/en-tx-overview-spending.svg" />


#### UTXO:
没有被花费的Outputs构成UTXO集合。

#### Balance:
与本地钱包相关的UTXO集合就是balance

#### Tx生成步骤：
1。生成rawtransaction。包括输入输出。输出的scriptPubKey写入（scriptPubKey知道输出地址就可以得到）。scriptSig留空
2。signrawtransaction。进行签名，把签名填入TxIn的对应字段。签名就是生成输入scriptSig（因此一个输入会签一次名）。

#### 签名的作用有两个：
- 证明输入所有权。即计算公钥私钥的匹配
- 保证交易没有被更改。（通过摘要实现）



### query.go
响应 `peer chaincode query` 命令，查询某个 chaincode 中变量。

例如

```bash
peer chaincode query -n test_cc -c '{"Args":["query","a"]}'
```

命令会调用 chaincodeQuery。

首先会调用 InitCmdFactory，初始化 Endosermentclient、Signer 等结构。这一步对于所有 chaincode 子命令来说都是类似的，个别会初始化不同的结构。

之后调用 chaincodeInvokeOrQuery 方法。

chaincodeInvokeOrQuery 方法主要过程如下：

* 生成 ChaincodeSpec。
* 根据 CS、chainID、签名实体等，生成 ChaincodeInvocationSpec。
* 根据 CIS，生成 Proposal，并进行签名。
* 签名后，通过 endorserClient 发送给指定的 peer。
* 成功的话，获取到 ProposalResponse，打印出 proposalResp.Response.Payload。


注意 invoke 和 query 的区别，query 不需要创建 SignedTx 发送到 orderer，而且会返回结果。

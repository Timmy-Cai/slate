# 使用说明

## 服务器API地址
前缀：
```http://xxx```


完整的API地址为：```前缀```+```具体接口路径```

比如，获取账户信息接口为：
```http://xxx/``` + ```/api/v1/getAccount?name=prefsabi@homes```

->

```http://xxx/api/v1/getAccount?name=prefsabi@homes```

## 调用接口说明
- 如果参数格式是JSON的话：提交request请求时必须添加header头：Content-Type:application/json


+ 所有的接口的返回形式都是统一为：
 
 正常返回: 
    
```
{
  "errorCode": 0,
  "errorMsg": "",
  "data": 某种类型的数据，比如字符串，数字，字典等等
}
```
 异常返回: 
  
```
{
  "errorCode": 具体的错误码,
  "errorMsg": "具体的错误信息字符串"
  "data": null
}
```

账户相关
-------------

### 获取账户信息
#### Request
- Method: **GET**
- URL:  ```/api/v1/getAccount?name={name}```
- Example:  ```/api/v1/getAccount?name=prefsabi@homes```

#### Parameter
名称 |备注 | 是否必填
---|---|---
name | 账户名 | 是

#### Response
- Body
```
{
    "data": {
        "accountName": "prefsabi@homes", //账户名
        "founder": "@homes",  //账户founder
        "accountId": 4341,   //账户ID
        "accountType": 4,   //账户类型 1-系统根账户 2-系统子账户 3-社区账户 4-社区子账户 5-个人账户
        "number": 153574, //创建该账户时的高度
        "code": "xxx",  //合约code
        "codeHash": "xxx", //合约的code hash
        "threshold": 1,  //active 权重
        "updateAuthorThreshold": 1, //owner权重
        "balances": [
            {
                "assetId": 0, //资产ID
                "balance": 9461000000  //资产余额
            }
        ],
        "authors": [
            {
                "authorType": "pubKey", //author类型 pubKey-公钥 account-账户 address-地址
                "author": "0x04474da0e3024d2089888dd60b7cdd79784725c4074f207b09dd7caa63a5b6ceb226b7141f4720c921003b54be581c84a05072c2d4752a1f5fae698684753424c5", //author值
                "weight": 1, //权重
                "index": 0 //索引
            }
        ],
        "description": "" //账户描述
    },
    "errorCode": 0,
    "errorMsg": ""
}
```


### 获取账户的Nonce
#### Request
- Method: **GET**
- URL:  ```/api/v1/getNonce?name={name}```
- Example:  ```/api/v1/getNonce?name=prefsabi@homes```

#### Parameter
名称 |备注 | 是否必填
---|---|---
name | 账户名 | 是

#### Response
- Body
```
{
    "data": 4, //账户的nonce
    "errorCode": 0,
    "errorMsg": ""
}
```


### 获取账户余额
#### Request
- Method: **GET**
- URL:  ```/api/v1/getAccountBalance?name={name}&assetId={assetId}```
- Example:  ```/api/v1/getAccountBalance?name=prefsabi@homes&assetId=0```

#### Parameter
名称 |备注 | 是否必填
---|---|---
name | 账户名 | 是
assetId | 资产ID | 是

#### Response
- Body
```
{
    "data": "9461000000", //账户的资产余额
    "errorCode": 0,
    "errorMsg": ""
}
```


### 构建创建账户结构体
#### Reuqest

- Method: **POST**
- URL: ```/api/v1/triggerCreatAccount```
- Headers： Content-Type:application/json
- Body:
```
{
    "accountName":"@homes", //账户名
    "assetId":0, //资产ID
    "amount":1, //创建账户时转账数量
    "newAccountName":"ta4@homes", //新账户名称
    "publicKey":"xxx",//公钥地址
    "description":"", //账户描述
    "remark":"创建账户" //账户备注
}
```

#### Response
- Body
```
{
    "data": {
        "parameter": { //同请求参数
            "accountName": "@homes",
            "assetId": 0,
            "remark": "创建账户", 
            "amount": 1,
            "newAccountName": "ta4@homes",
            "publicKey": "xxx",
            "description": ""
        },
        "result": { //参考 构造交易体返回属性列表
            "chainId": 1, 
            "gasAssetId": 0, 
            "gasPrice": 1000, 
            "action": {
                "accountName": "@homes",
                "actionType": 256, 
                "assetId": 0, 
                "toAccountName": "account@gon",
                "gasLimit": 500000, 
                "amount": 1,
                "payload": "xxx",
                "remark": "创建账户",
                "nonce": 67
            }
        }
    },
    "errorCode": 0,
    "errorMsg": ""
}
```


### 账户是否存在
#### Request
- Method: **GET**
- URL:  ```/api/v1/accountExist?name={name}```
- Example:  ```/api/v1/accountExist?name=prefsabi@homes```

#### Parameter
名称 |备注 | 是否必填
---|---|---
name | 账户名 | 是

#### Response
- Body
```
{
    "data": true, //true-存在 false-不存在
    "errorCode": 0,
    "errorMsg": ""
}
```

合约相关
-------------
### 获取合约信息
#### Request
- Method: **GET**
- URL:  ```/api/v1/getContract?name={name}```
- Example:  ```/api/v1/getContract?name=prefsabi@homes```

#### Parameter
名称 |备注 | 是否必填
---|---|---
name | 合约名称 | 是


#### Response
- Body
```
{
    "data": {
        "abi": "xxx", //abi文件内容
        "accountName": "prefsabi@homes", //合约所属账户
        "founder": "@homes", //账户founder
        "accountId": 4341, //账户ID
        "accountType": 4, //账户类型 1-系统根账户 2-系统子账户 3-社区账户 4-社区子账户 5-个人账户
        "number": 153574, //创建账户高度
        "code": "xxx", //合约code
        "codeHash": "xxx", //合约的hash
        "threshold": 1,   //active 权重
        "updateAuthorThreshold": 1,  //owner权重
        "balances": [
            {
                "assetId": 0, //资产ID
                "balance": 9461000000  //资产余额
            }
        ],
        "authors": [
            {
                "authorType": "pubKey",  //author类型 pubKey-公钥 account-账户 address-地址
                "author": "0x04474da0e3024d2089888dd60b7cdd79784725c4074f207b09dd7caa63a5b6ceb226b7141f4720c921003b54be581c84a05072c2d4752a1f5fae698684753424c5",//author值
                "weight": 1,  //权重
                "index": 0 //索引
            }
        ],
        "description": "" //合约账户描述
    },
    "errorCode": 0,
    "errorMsg": ""
}
```


### 调用合约查看结果
#### Reuqest

- Method: **POST**
- URL: ```/api/v1/getContractResult```
- Headers： Content-Type:application/json
- Body:
```
{
    "abi":"xxx", //abi文件内容
    "accountName":"@homes",//账户名
    "toAccountName":"prefsabi@homes",//合约账户
    "function":"xx",//合约的方法
    "parameters":["xx"] //方法参数(参考abi input参数)
}
```

#### Response
- Body
```
{
    "data": {
        "result": "xxx", //合约结果
        "parameter": { //同请求参数
            "abi": "",
            "accountName": "@homes",
            "toAccountName": "prefsabi@homes",
            "function": "xx",
            "parameters": ["xx"]
        }
    },
    "errorCode": 0,
    "errorMsg": ""
}
```

### 构建调用智能合约结构体
#### Reuqest

- Method: **POST**
- URL: ``` /api/v1/triggerSmartContract ```
- Headers： Content-Type:application/json
- Body:
```
{
    "accountName":"3@homes", //账户名
    "toAccountName":"3@homes",//合约账户
    "assetId":0, //资产ID
    "amount": 1, //资产数量
    "abi":"xxx", //abi文件内容
    "function":"", //合约的方法
    "parameters":[], //方法参数(参考abi input参数)
    "remark":"" //备注
}

```

#### Response
- Body
```
{
    "data": {
        "parameter": { //同请求参数
            "accountName": "3@homes",
            "assetId": 0,
            "remark": "",
            "toAccountName": "4@homes",
            "amount": 0,
            "abi": "",
            "function": "",
            "parameters": []
        },
        "result": { //参考构造交易体返回属性列表
            "chainId": 1,
            "gasAssetId": 0,
            "gasPrice": 1000,
            "action": {
                "accountName": "4@homes",
                "actionType": 0,
                "assetId": 0,
                "toAccountName": "4@homes", 
                "gasLimit": 1000000, 
                "amount": 0, 
                "payload": "xxx", 
                "remark": "你好",
                "nonce": 4
            }
        }
    },
    "errorCode": 0,
    "errorMsg": ""
}

```


### 构建创建合约结构体
#### Reuqest

- Method: **POST**
- URL: ``` /api/v1/triggerCreateContract ```
- Headers： Content-Type:application/json
- Body:
```
{
    "accountName":"10@homes", //账户名
    "remark":"xxx", //备注
    "abi":"xxx", //ABI文件内容
    "bin":"xxx", //BIN文件内容
    "parameters":[] //参数(合约构造器参数，参考abi 合约构造器input)
}

```

#### Response
- Body
```
{
    "data": {
        "parameter": { //请求参数
            "accountName": "10@homes",
            "remark": "xxx",
            "abi": "",
            "bin": "",
            "parameters": []
        },
        "result": {//参考构造交易体返回属性列表
            "chainId": 1,
            "gasAssetId": 0,
            "gasPrice": 1000, 
            "action": {
                "accountName": "10@homes",
                "actionType": 1,
                "assetId": 0,
                "toAccountName": "10@homes",
                "gasLimit": 10000000,
                "amount": 0,
                "payload": "",
                "remark": "xxx",
                "nonce": 2
            }
        }
    },
    "errorCode": 0,
    "errorMsg": ""
}

```



资产相关
-------------

### 构建增发资产结构体
#### Reuqest

- Method: **POST**
- URL: ``` /api/v1/triggerIncrementAsset ```
- Headers： Content-Type:application/json
- Body:
```
{
    "accountName":"prefsabi@homes", //账户名称
    "assetId":0, //账户ID
    "remark":"xxx", //备注
    "incrAssetId":1, //增发资产ID
    "assetAmount":1000, //增发数量
    "acceptor":"xxx" //接受者
}
```

#### Response
- Body
```
{
    "data": {
        "parameter": { //同请求参数
            "accountName": "prefsabi@homes",
            "assetId": 0,
            "remark": "xxx",
            "incrAssetId": 1,
            "assetAmount": 1000,
            "acceptor": "xxx"
        },
        "result": { //参考构造交易体返回属性列表
            "chainId": 1,
            "gasAssetId": 0,
            "gasPrice": 1000,
            "action": {
                "accountName": "prefsabi@homes",
                "actionType": 512,
                "assetId": 0,
                "toAccountName": "asset@gon",
                "gasLimit": 100000,
                "amount": 0,
                "payload": "c8018203e883616263",
                "remark": "hello",
                "nonce": 4
            }
        }
    },
    "errorCode": 0,
    "errorMsg": ""
}
```

### 构建发行资产结构体
#### Reuqest

- Method: **POST**
- URL: ``` /api/v1/triggerIssueAsset ```
- Headers： Content-Type:application/json
- Body:
```
{
    "accountName": "prefsabi@homes",//账户名
    "assetId": 0, //资产ID
    "remark": "xxx", //备注
    "assetName": "xxx", //发行资产全称
    "symbol": "xx", //发行资产简称
    "assetAmount": 1000, //发行量
    "decimals": 1, //精度
    "upperLimit": 2000, //发行上限 0表示无上限
    "contract": "xxx", //资产协议
    "description": "xxx" //描述
}
```

#### Response
- Body
```
{
    "data": {
        "parameter": {//同请求参数
            "accountName": "prefsabi@homes",
            "assetId": 0,
            "remark": "hello",
            "assetName": "timmyhahahaha",
            "symbol": "th",
            "assetAmount": 1000,
            "decimals": 1,
            "upperLimit": 2000,
            "contract": "hello world",
            "description": "hello"
        },
        "result": {//参考构造交易体返回属性列表
            "chainId": 1,
            "gasAssetId": 0,
            "gasPrice": 1000,
            "action": {
                "accountName": "prefsabi@homes",
                "actionType": 513,
                "assetId": 0,
                "toAccountName": "asset@gon",
                "gasLimit": 10000000,
                "amount": 0,
                "payload": "f8488d74696d6d7968616861686168618274688203e8018e707265667361626940686f6d65738e707265667361626940686f6d65738207d08b68656c6c6f20776f726c648568656c6c6f",
                "remark": "hello",
                "nonce": 4
            }
        }
    },
    "errorCode": 0,
    "errorMsg": ""
}
```


### 根据资产ID查询币种的信息
#### Request
- Method: **GET**
- URL:  ``` /api/v1/getAssetInfoById?assetId={assetId} ```
- Example:  ``` /api/v1/getAssetInfoById?assetId=0 ```

#### Parameter
名称 |备注 | 是否必填
---|---|---
assetId | 资产ID | 是

#### Response
- Body
```
{
    "data": {
        "stats": 241, //持有人数
        "assetName": "gcoin", //资产全称
        "symbol": "gcoin", //资产简称
        "amount": 1000000000000000000, //资产数量
        "decimals": 9, //精度
        "founder": "owner@gon", //资产founder
        "owner": "owner@gon", //资产所属人
        "addIssue": 1000000000000000000, //累计发行
        "upperLimit": 0, //发行上限 0表示无上限
        "contract": "", //资产协议
        "description": "GCoin" //描述
    },
    "errorCode": 0,
    "errorMsg": ""
}
```


### 根据币种全称获取币种信息
#### Request
- Method: **GET**
- URL:  ``` /api/v1/getAssetInfoByName?assetName={assetName} ```
- Example:  ``` /api/v1/getAssetInfoByName?assetName=gcoin ```

#### Parameter
名称 |备注 | 是否必填
---|---|---
assetName | 资产名称 | 是

#### Response
- Body
```
{
    "data": { //同根据资产ID查询币种的信息接口
        "stats": 241,
        "assetName": "gcoin",
        "symbol": "gcoin",
        "amount": 1000000000000000000,
        "decimals": 9,
        "founder": "owner@gon",
        "owner": "owner@gon",
        "addIssue": 1000000000000000000,
        "upperLimit": 0,
        "contract": "",
        "description": "GCoin"
    },
    "errorCode": 0,
    "errorMsg": ""
}
```

交易相关
-------------


### 查询交易信息
#### Request
- Method: **GET**
- URL:  ``` /api/v1/getTransactionInfoByHash?hash={hash} ```
- Example:  ``` /api/v1/getTransactionInfoByHash?hash=0x3823383e6232d77016f1451a5b6955a977a1f254f73f9e5bdd4ba8c408a96a58 ```

#### Parameter
名称 |备注 | 是否必填
---|---|---
hash | 交易hash | 是

#### Response
- Body
```
{
    "data": {
        "blockHash": "0x55a58ff4ba4b649479fdb26c5cc34109d7c65f0465e8394bb44b137dcfcbcfab", //区块hash
        "blockNumber": 268305, //区块高度
        "txHash": "0x3823383e6232d77016f1451a5b6955a977a1f254f73f9e5bdd4ba8c408a96a58", //交易hash
        "transactionIndex": 0, //交易索引
        "actions": [
            {
                "actionType": 256, //action类型
                "nonce": 2, //nonce
                "from": "prefsabi@homes", //from账户
                "to": "account@gon", //to账户
                "assetId": 0, //资产ID
                "gasLimit": 500000, //gasLimit
                "value": 49, 
                "remark": "0x68656c6c6f", //备注
                "payload": "0xf85188743140686f6d657380b841047db227d7094ce215c3a0f57e1bcc732551fe351f94249471934567e0f5dc1bf795962b8cccb87a2eb56b29fbe37d614e2f4c3c45b789ae4f1f51f4cb21972ffd83313131",//payload
                "actionHash": "0x7593b62625852c7109633c269abb3bef92f72ab3032ea716f09ff09c9e1063bd",//action hash
                "actionIndex": 0,
                "signIndex": [
                    0
                ]
            }
        ],
        "gasAssetID": 0,
        "gasPrice": 1000,
        "gasCost": 500000000
    },
    "errorCode": 0,
    "errorMsg": ""
}
```


### 查询交易状态
#### Request
- Method: **GET**
- URL:  ```/api/v1/getTransactionReceipt?hash={hash} ```
- Example:  ``` /api/v1/getTransactionReceipt?hash=0x3823383e6232d77016f1451a5b6955a977a1f254f73f9e5bdd4ba8c408a96a58 ```

#### Parameter
名称 |备注 | 是否必填
---|---|---
hash | 交易hash | 是

#### Response
- Body
```
{
    "data": {
        "blockHash": "0x55a58ff4ba4b649479fdb26c5cc34109d7c65f0465e8394bb44b137dcfcbcfab",//区块hash
        "blockNumber": 268305,//区块高度
        "txHash": "0x3823383e6232d77016f1451a5b6955a977a1f254f73f9e5bdd4ba8c408a96a58",//交易hash
        "transactionIndex": 0,//交易索引
        "postState": "0x36b1879eba50bba87bafb2efeaee84c152643ccc7606b86530baff943dcd81af",//发布交易状态根
        "actionResults": [
            {
                "gasAllot": [
                    {
                        "name": "spongebob117",//帐户名称
                        "gasLimit": 100000,//gasLimit
                        "typeId": 0// 0-交易手续费，1-合约，2-区块奖励
                    },
                    {
                        "name": "founder@gon",
                        "gasLimit": 400000,
                        "typeId": 1
                    }
                ],
                "authors": [
                    {
                        "index": 0, //authors 索引
                        "owner": "0x04474da0e3024d2089888dd60b7cdd79784725c4074f207b09dd7caa63a5b6ceb226b7141f4720c921003b54be581c84a05072c2d4752a1f5fae698684753424c5",//authors owner
                        "weight": 1 //authors权重
                    }
                ],
                "actionType": 256,//action类型
                "status": 1, //状态 1-成功 0-失败
                "index": 0, //action 交易中的索引位置
                "gasUsed": 500000,//实际消耗的gas
                "parentIndex": 0, //父级账号索引
                "permitType": 0, //签名类型 0-active权限交易 1-owner权限交易
                "permitThreshold": 1 //签名所需权重
            }
        ],
        "cumulativeGasUsed": 500000,//在区块中执行此交易时使用gas总量
        "totalGasUsed": 500000, //仅此特定交易使用的天然气量
        "logsBloom": "xxx",//隆过滤器，以快速检索相关的日志
        "logs": null //日志
    },
    "errorCode": 0,
    "errorMsg": ""
}
```

### 构建转账结构体
#### Reuqest

- Method: **POST**
- URL: ``` /api/v1/triggerTransfer ```
- Headers： Content-Type:application/json
- Body:
```
{
    "accountName":"prefsabi@homes",//账户名
    "assetId":0, //转账资产
    "amount":1, //转账数量
    "remark":"转账", //备注
    "toAccountName":"abc@@homes" //转账目标账户
}
```

#### Response
- Body
```
{
    "data": {//参考 构造交易体返回属性列表
        "chainId": 1, 
        "gasAssetId": 0,
        "gasPrice": 1000,
        "action": {
            "accountName": "prefsabi@homes",
            "actionType": 515,
            "assetId": 0,
            "toAccountName": "abc@@homes",
            "gasLimit": 100000,
            "amount": 1,
            "payload": "",
            "remark": "转账",
            "nonce": 4
        }
    },
    "errorCode": 0,
    "errorMsg": ""
}
```

其他
-------------

### 获取GAS价格
#### Request
- Method: **GET**
- URL:  ``` /api/v1/getGasPrice ```
- Example:  ``` /api/v1/getGasPrice ```


#### Response
- Body
```
{
    "data": 1000,
    "errorCode": 0,
    "errorMsg": ""
}
```

### 签名(强烈不建议开发人员在开发环境中使用该接口)
#### Reuqest

- Method: **POST**
- URL: ``` /api/v1/getTransactionSign ```
- Headers： Content-Type:application/json
- Body:
```
{ 
    "chainId": 1, //参考构造交易体返回属性列表
    "gasAssetId": 0,
    "gasPrice": 1000,
    "action": {
        "accountName": "@homes",
        "actionType": 256,
        "assetId": 0,
        "toAccountName": "account@gon",
        "gasLimit": 500000,
        "amount": 1,
        "payload": "f8528974613440686f6d657380b841047db227d7094ce215c3a0f57e1bcc732551fe351f94249471934567e0f5dc1bf795962b8cccb87a2eb56b29fbe37d614e2f4c3c45b789ae4f1f51f4cb21972ffd83313131",
        "remark": "签名",
        "nonce": 66
    },
    "privateKey": "2f5949fb7ffd80cf1be53fe7d840bec0dc0042f1d0e1fa6ee0f85b8f010ac17e"//私钥
}
```

#### Response
- Body
```
{
    "data": "0xf8ce808203e8f8c8f8c682010042808640686f6d65738b6163636f756e7440676f6e8307a12001b854f8528974613440686f6d657380b841047db227d7094ce215c3a0f57e1bcc732551fe351f94249471934567e0f5dc1bf795962b8cccb87a2eb56b29fbe37d614e2f4c3c45b789ae4f1f51f4cb21972ffd8331313186e7adbee5908df84a80f847f84526a0af5c6f67c342380f2873d5ca694e8cbc904d6bf9b91e3e1358ee54c5953c7a0aa05fab18143fac5bbb1e8e03ade6aa76c542ace6503925d5e897e591f798ce105cc180",//签名结果
    "errorCode": 0,
    "errorMsg": ""
}
```


### 广播交易
#### Reuqest

- Method: **POST**
- URL: ``` /api/v1/broadcast ```
- Headers： Content-Type:application/json
- Body:
```
{
   "rawData":"0xf8cd808203e8f8c7f8c582010042808640686f6d65738b6163636f756e7440676f6e8307a12001b854f8528974613440686f6d657380b841047db227d7094ce215c3a0f57e1bcc732551fe351f94249471934567e0f5dc1bf795962b8cccb87a2eb56b29fbe37d614e2f4c3c45b789ae4f1f51f4cb21972ffd833131318568656c6c6ff84a80f847f84525a01848d1854c25ee5bdfe418001a5d7a3f3215935ed0cbbd75ecb504122fdf52e1a0250b0c2ec2823f69b99b1c212ef52ac151af6debf3c17b49831bbf7d971173e6c180" //签名数据
}
```

#### Response
- Body
```
{
    "data": "0xf8ce808203e8f8c8f8c682010042808640686f6d9b91e3e105cc180",//交易hash
    "errorCode": 0,
    "errorMsg": ""
}
```


### 获取链配置
#### Request
- Method: **GET**
- URL:  ``` /api/v1/getChainConfig ```
- Example:  ``` /api/v1/getChainConfig ```

#### Response
- Body
```
{
    "data": {
        "bootnodes": [ //节点
            "gnode://0469430d2d237a97f932e3aa62eca2d396458f8c41814f860c1afd5b4bed8b7c1e2fd4bba8277d016eb410994a2fe44812fc3edbe61dfb5ec275c7c375d10c2339@boot1.testnet.gaiaopen.network:30000",
            "gnode://04e24a53e2885fa60a9d6ecfc9e582bf11db0bea0881949268edafa5c4d35f9cbcfbaf7513a445f1dfb26ba465d752944078bfd21e22f263bb8bdc53d28248a727@boot2.testnet.gaiaopen.network:30000",
            "gnode://046f82c6136f72dbc56f3ff78048d9becc03e65d086c94ca426ceffee9bea637654924669d8c969c95f52aa2ffeb85a75337f9d6c37f4eb321b1064d6abe0d242d@boot3.testnet.gaiaopen.network:30000"
        ],
        "chainId": 1, //链ID
        "chainName": "gon", //链名称
        "chainUrl": "https://www.gaiaopen.network", //链URL
        "accountParams": {//账户参数
            "level": 2//二级账户
        },
        "assetParams": {//资产参数
            "level": 2 //二级资产
        },
        "chargeParams": { 
            "assetRatio": 80,
            "contractRatio": 80
        },
        "upgradeParams": {//分叉参数
            "blockCnt": 10000,
            "upgradeRatio": 80
        },
        "dposParams": {//dpos 参数
            "maxURLLen": 512,
            "unitStake": 1,
            "candidateMinQuantity": 100000,
            "voterMinQuantity": 1,
            "activatedMinQuantity": 600000,
            "blockInterval": 3000,
            "blockFrequency": 6,
            "candidateScheduleSize": 6,
            "backupScheduleSize": 4,
            "extraBlockReward": 0,
            "blockReward": 0
        },
        "systemName": "founder@gon",//系统founder名称
        "accountName": "account@gon",//系统账户名称
        "assetName": "asset@gon",//系统资产名称
        "dposName": "dpos@gon",//dPos名称
        "snapshotInterval": 3600000, //快照时间间隔
        "feeName": "fee@gon",//手续费账户名称
        "systemToken": "gcoin",//系统token名称
        "sysTokenDecimal": 9,//token精度
        "referenceTime": 1598284800000000000 //链启动时间戳
    },
    "errorCode": 0,
    "errorMsg": ""
}
```


### 返回参数列表
#### 构造交易体返回属性列表

名称 | 备注
---|---
gasAssetId | gas资产ID
gasPrice | gasPrice
accountName | 账户名
actionType | action类型值
assetId | 资产ID
toAccountName | to账户
gasLimit | 账户名
accountName | gasLimit
amount | 资金数量
payload | 账户名
accountName | payload
remark | 备注
nonce | nonce


actionType 参数
名称 | 备注
---|---
0 | 调用合约
1 | 创建合约
256 | 创建账户
257 | 更新账户
259 | 更新账户权重
512 | 增发资产
513 | 发行资产 
514 | 销毁资产
515 | 转账
516 | 更新资产协议

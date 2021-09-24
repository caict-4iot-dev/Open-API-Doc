## 1 BID标识应用

### 1.1 BID注册

结合子链AC号，按照星火·链网开放的BID-SDK开发BID标识注册功能，并按照《BID文档规范》管理BID标识数据信息

### 1.2 BID标识模板同步

将BID标识模版同步到主链，便于标识数据属性信息标准化管理

请求参数说明：

| **字段名**    | **类型** | **是否必填** | **描述**     |
| ------------- | -------: | ------------ | ------------ |
| chainCode     |   String | 是           | AC号         |
| name          |   String | 是           | 模板名称     |
| desc          |   String | 否           | 模板描述     |
| data          |     List | 是           | 模板属性数据 |
| data.nameCn   |   String | 是           | 中文名称     |
| data.nameEn   |   String | 是           | 英文名称     |
| data.dataType |   String | 是           | 数据类型     |
| data.minLen   |  Integer | 是           | 最小长度     |
| data.maxLen   |  Integer | 是           | 最大长度     |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| templateNo | String   | 模版编号 |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/bid/template/syn

{
"accessToken":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhcHBJZCI6IkZDMjFnN2JzMVI2TjJGWjMiLCJpc3MiOiJidW1vIiwiZXhwIjoxNTYxMDAyODIyfQ.hgVH0T6fLxk973U2fIj_ejDx5aJuzRFlg1VAUA2RgzM",
  "params":{
  	"chainCode":"",
    "name": "模板名称",
    "desc": "模板描述",
		"data":[
    	{
         "nameCn":"中文名称",
         "nameEn":"英文名称",
         "dataType":"1",
         "minLen":1,
         "maxLen":100
        },
        {
         "nameCn":"中文名称",
         "nameEn":"英文名称",
         "dataType":"1",
         "minLen":1,
         "maxLen":100
        }
    ]
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
    "data":{
      "templateNo":""
    },
    "errorCode": 0,
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部异常"
}
```

### 1.3 BID解析

按照BID解析协议规范，为骨干节点(或骨干链)设计BID解析功能，骨干节点的标识数据存放在其所在基础子链，通过基础子链实现BID标识全网解析能力

接口说明：

通过该接口可以直接解析出BID DDO文档信息。

请求参数：

| **字段名** | **类型** | **是否必填** | **描述** |
| :--------- | -------: | :----------- | :------- |
| bid        |   String | 是           | bid标识  |


返回数据：

| **字段名**                                        | **类型** | **描述**                                            |
| :------------------------------------------------ | :------- | :-------------------------------------------------- |
| didDocument                                       | Object   | 文档内容                                            |
| didDocument.@context                              | List     | 一组url数组                                         |
| didDocument.version                               | String   | BID文档的版本                                       |
| didDocument.id                                    | String   | 解析的BID                                           |
| didDocument.publicKey                             | List     | 一组公钥                                            |
| didDocument.publicKey.id                          | String   | 公钥id                                              |
| didDocument.publicKey.type                        | String   | 公钥算法类型                                        |
| didDocument.publicKey.controller                  | String   | 一个BID,表明此公钥的归属                            |
| didDocument.publicKey.publicKeyHex                | String   | 十六进制公钥                                        |
| didDocument.authentication                        | List     | 一组公钥id                                          |
| didDocument.alsoKnownAs                           | String   | 关联id                                              |
| didDocument.alsoKnownAs.type                      | int      | 关联id的类型 **见 11BID对象类型表**                 |
| didDocument.extension                             | Object   | 扩展字段                                            |
| didDocument.extension.recovery                    | List     | 一组公钥id                                          |
| didDocument.extension.ttl                         | Long     | 缓存时间，单位秒                                    |
| didDocument.extension.delegateSign                | Object   | 第三方对publicKey的签名，可信解析使用               |
| didDocument.extension.delegateSign.signer         | String   | 签名公钥id                                          |
| didDocument.extension.delegateSign.signatureValue | String   | 签名的base64编码                                    |
| didDocument.extension.type                        | int      | **见 12BID对象类型表**                              |
| didDocument.extension.attributes                  | List     | 一组属性,属性结构见属性结构章节                     |
| didDocument.extension.acsns                       | List     | AC号列表                                            |
| didDocument.extension.acsns.acsn                  | String   | AC号                                                |
| didDocument.extension.acsns.acsnAddress           | String   | AC号对应的子链的解析地址BID                         |
| didDocument.extension.verifiableCredentials       | List     | 凭证列表，只有主链是凭证类型的BID文档才可能有本字段 |
| didDocument.extension.verifiableCredentials.id    | String   | 凭证ID                                              |
| didDocument.extension.verifiableCredentials.type  | Int      | 凭证类型 14凭证类型表                               |
| didDocument.service                               | List     | 一组服务地址                                        |
| didDocument.service.id                            | String   | 服务地址的ID                                        |
| didDocument.service.type                          | String   | 服务的类型 **见15服务类型表**                       |
| didDocument.service.serviceEndpoint               | String   | 服务的URL地址                                       |
| didDocument.created                               | String   | 创建时间                                            |
| didDocument.updated                               | String   | 上次的更新时间                                      |
| didDocument.proof                                 | Object   | 签名信息                                            |
| didDocument.proof.creator                         | String   | 签名公钥id                                          |
| didDocument.proof.signatureValue                  | String   | 签名的base64编码                                    |

**当文档属性为凭证类型时，attributes结构如下**

| **字段名**                                                   | **类型** | **描述**                          |
| :----------------------------------------------------------- | :------- | :-------------------------------- |
| didDocument.extension.attributes.issuer                      | String   | 发证者BID                         |
| didDocument.extension.attributes.issuanceDate                | String   | 发证日期                          |
| didDocument.extension.attributes.effectiveDate               | String   | 生效日期                          |
| didDocument.extension.attributes.expirationDate              | String   | 失效日期                          |
| didDocument.extension.attributes.revocationId                | String   | 凭证吊销服务地址ID                |
| didDocument.extension.attributes.credentialSubject           | Object   | 凭证主体                          |
| didDocument.extension.attributes.credentialSubject.id        | String   | 凭证拥有者的BID                   |
| didDocument.extension.attributes.credentialSubject.type      | Int      | 凭证类型                          |
| didDocument.extension.attributes.credentialSubject.name      | String   | 被颁发者机构名称                  |
| didDocument.extension.attributes.credentialSubject.description | String   | 描述                              |
| didDocument.extension.attributes.content                     | Object   | 凭证的具体内容                    |
| didDocument.extension.attributes.proof                       | List     | 签名证明                          |
| didDocument.extension.attributes.proof.creator               | String   | proof的创建者，这里是一个公钥的id |
| didDocument.extension.attributes.proof.signatureValue        | String   | 使用相应私钥对凭证内容的签名      |

**当文档属性为其他类型时，attributes结构如下**

| **字段名**                               | **类型** | **描述**                              |
| :--------------------------------------- | :------- | :------------------------------------ |
| didDocument.extension.attributes.key     | String   | 属性的key                             |
| didDocument.extension.attributes.desc    | String   | 属性的描述                            |
| didDocument.extension.attributes.encrypt | int      | 是否加密,0非加密，1加密               |
| didDocument.extension.attributes.format  | String   | image、text、video、mixture等数据类型 |
| didDocument.extension.attributes.value   | String   | 属性自定义value                       |

**当service.type为子链解析服务时,service结构如下：**

| **字段名**                          | **类型** | **描述**                      |
| :---------------------------------- | :------- | :---------------------------- |
| didDocument.service.id              | String   | 服务地址的ID                  |
| didDocument.service.type            | String   | 服务的类型 **见16服务类型表** |
| didDocument.service.version         | int      | 解析服务支持的BID协议版本     |
| didDocument.service.protocol        | String   | 解析服务支持的传输协议        |
| didDocument.service.serverType      | String   | 解析地址类型                  |
| didDocument.service.serviceEndpoint | String   | 解析地址                      |
| didDocument.service.port            | int      | 解析端口                      |

示例：

（1）请求示例：

```plain
http请求方式：GET
https://{url}/public/resolve/did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2
```

（2）返回结果示例：

a. 普通BID文档 数据示例为：

```plain
{
    "errorCode": 0,
    "data": {
        "didDocument": {
            "@context": ["https://www.w3.org/ns/did/v1"],
            "version": "1.0.0",
            "id": "did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2",
            "publicKey": [{
                "id": "did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2#key-1",
                "type": "Ed25519",
                "controller": "did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2",
                "publicKeyHex": "b9906e1b50e81501369cc777979f8bcf27bd1917d794fa6d5e320b1ccc4f48bb"
            }],
            "authentication": ["did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2#key-1"],
            "extension": {
                "recovery": ["did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2#key-2"],
                "ttl": 86400,
                "delegateSign ": {
                    "signer": "did:bid:efJgt44mNDewKK1VEN454R17cjso3mSG#key-1",
                    "signatureValue": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19"
                },
                "type": 206
            },
            "service": [{
                "id": "did:bid:ef24NBA7au48UTZrUNRHj2p3bnRzF3YCH#subResolve",
                "type": "DIDSubResolve",
                "version": "1.0.0",
                "serverType": 1,
                "protocol": 3,
                "serviceEndpoint": "192.168.1.23",
                "port": 8080
            }],
           "created": "2021-05-10T06:23:38Z",
           "updated": "2021-05-10T06:23:38Z",
            "proof": {
                "creator": "did:bid:efJgt44mNDewKK1VEN454R17cjso3mSG#key-1",
                "signatureValue": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19"
            }
        }
    },
    "message": "success"
}
```

b. 凭证BID文档 数据示例为：

```plain
{
    "errorCode": 0,
    "message": "success",
    "data": {
        "didDocument": {
            "@context": [
                "https://www.w3.org/ns/did/v1"
            ],
            "version": "1.0.0",
            "id": "did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2",
            "publicKey": [
                {
                    "id": "did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2#key-1",
                    "type": "Ed25519",
                    "controller": "did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2",
                    "publicKeyHex": "b9906e1b50e81501369cc777979f8bcf27bd1917d794fa6d5e320b1ccc4f48bb"
                }
            ],
            "authentication": [
                "did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2#key-1"
            ],
            "extension": {
                "recovery": [
                    "did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2#key-2"
                ],
                "ttl": 86400,
                "delegateSign ": {
                    "signer": "did:bid:efJgt44mNDewKK1VEN454R17cjso3mSG#key-1",
                    "signatureValue": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19"
                },
                "type": 205,
                "attributes": [
                    {
                        "issuer": "did:bid:efJgt44mNDewKK1VEN454R17cjso3mSG",
                        "issuanceDate": "2021-01-20T12:01:20Z",
                        "effectiveDate": "2021-01-20T12:01:20Z",
                        "expirationDate": "2021-04-02T12:01:20Z",
                        "revocationId": "did:bid:efJgt44mNDewKK1VEN454R17cjso3mSG#revocation",
                        "credentialSubject": {
                            "id": "did:bid:efJgt44mNDewKK1VEN454R17cjso3mSG",
                            "type": 202,
                            "name": "北京大学",
                            "content": ""
                        },
                        "proof": [
                            {
                                "creator": "did:bid:efJgt44mNDewKK1VEN454R17cjso3mSG#key-1",

                                "signatureValue": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19"
                            }
                        ]
                    }
                ]
            },
            "service": [
                {
                    "id": "did:bid:ef24NBA7au48UTZrUNRHj2p3bnRzF3YCH#revocation",
                    "type": " DIDRevocation",
                    "serviceEndpoint": "https://did.bif.com"
                }
            ],
            "proof": {
                "creator": "did:bid:efJgt44mNDewKK1VEN454R17cjso3mSG#key-1",

                "signatureValue": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19"
            }
        }
    }
}
```

C. 包含子链解析服务地址BID文档 示例为：

```plain
{
    "errorCode": 0,
    "message": "success",
    "data": {
        "didDocument": {
            "@context": [
                "https://www.w3.org/ns/did/v1"
            ],
            "version": "1.0.0",
            "id": "did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2",
            "publicKey": [
                {
                    "id": "did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2#key-1",
                    "type": "Ed25519",
                    "controller": "did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2",
                    "publicKeyHex": "b9906e1b50e81501369cc777979f8bcf27bd1917d794fa6d5e320b1ccc4f48bb"
                }
            ],
            "authentication": [
                "did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2#key-1"
            ],
            "extension": {
                "recovery": [
                    "did:bid:efnVUgqQFfYeu97ABf6sGm3WFtVXHZB2#key-2"
                ],
                "ttl": 86400,
                "delegateSign ": {
                    "signer": "did:bid:efJgt44mNDewKK1VEN454R17cjso3mSG#key-1",
                    "signatureValue": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19"
                },
                "type": 206
            },
            "service": [
                {
                    "id": "did:bid:ef24NBA7au48UTZrUNRHj2p3bnRzF3YCH#subResolve",
                    "type": "DIDSubResolve",
                    "version": "1.0.0",
                    "serverType": 1,
                    "protocol": 3,
                    "serviceEndpoint": "192.168.1.23",
                    "port": 8080
                }
            ],
            "proof": {
                "creator": "did:bid:efJgt44mNDewKK1VEN454R17cjso3mSG#key-1",

                "signatureValue": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19"
            }
        }
    }
}
```

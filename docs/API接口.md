# 四.API接口

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

## 2 星火·链网数字身份

星火·链网数字身份内容包括BID标识及标识对象属性信息（如企业信息、个人信息、智能设备信息等）、公私钥管理、凭证信息等，适用于标识对象需要自主控制私钥的应用场景。

### 2.1 生成主链数字身份

接口说明：

子链用户在子链自生成星火·链网子链数字身份后，需要获取主链数字身份才能使用主链或其他子链的服务。因此，骨干节点需具备调用数字身份接口的功能，子链可根据具体业务，有选择地为用户获取主链数字身份。bid的生成规则请见13节bid数字身份生成算法。

请求参数：

|**字段名**|**类型**|**是否必填**|**描述**|
|:----|----:|:----|:----|
|bid|String(64)|是|账户子链数字身份BID|
|type|String(4)|是|数字身份类型： 101人、102企业、103节点 、104智能设备、105智能合约|
|publicKey|String(128)|是|数字身份公钥|


返回数据：

|**字段名**|**类型**|**描述**|
|:----|:----|:----|
|bid|String|主链数字身份BID|
|txHash|String|链上交易hash|
|errorCode|int|0|
|message|String|成功|


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/bid/identity/generate
{
 "accessToken":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhcHBJZCI6IkZDMjFnN2JzMVI2TjJGWjMiLCJpc3MiOiJidW1vIiwiZXhwIjoxNTYxMDAyODIyfQ.hgVH0T6fLxk973U2fIj_ejDx5aJuzRFlg1VAUA2RgzM",
  "params":{
    "bid":"did:bid:by01:efhbk6Y1kF7SuuvT2389JExuQtUVEX4n",
    "type":"101",
    "publicKey":""
  }
}
```
（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
    "data":{
      "bid":"did:bid:efhbk6Y1kF7SuuvT2389JExuQtUVEX4n",
      "txHash":"aa87a25080fa03cdd5ea51fb06440646b9dde93319ff403f2bf095c8b5edff47"
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
### 2.2 可信认证申请

#### 2.2.1上传图片

接口说明：

可信认证需要上传企业营业执照、法人身份证等图片。只用于数字身份上传使用，图片格式要求为PNG、BMP、JPG或GIF，单张照片大小不超过2M。

请求参数：

form表单格式

| **字段名**  | **类型** | **是否必填** | **描述** |
| :---------- | -------: | :----------- | :------- |
| accessToken |   String | 是           | API令牌  |
| file        |     file | 是           | 图片     |


返回数据：

| **字段名** | **类型** | **描述**             |
| :--------- | :------- | :------------------- |
| filePath   | String   | 文件路径             |
| errorCode  | int      | 错误码 0成功 非0失败 |
| message    | String   | 错误描述             |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/bid/id-info-bin/upload
{
  "accessToken":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhcHBJZCI6IkZDMjFnN2JzMVI2TjJGWjMiLCJpc3MiOiJidW1vIiwiZXhwIjoxNTYxMDAyODIyfQ.hgVH0T6fLxk973U2fIj_ejDx5aJuzRFlg1VAUA2RgzM",
  "file":a.img
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
    "data":{
     "filePath":"/oss/file/get/QmV1Z7syFkw8Tg41RuWChn7Ad3KjwgTexHeJ2LiYEEK2rd"
    },
    "errorCode": 0,
    "message": "成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "成功"
}
```

#### 2.2.2 企业可信认证申请

接口说明：

企业获取主链数字身份后，可通过接口进行可信认证

请求参数：

| **字段名**              | **类型** | **是否必填** | **描述**                                             |
| :---------------------- | -------: | :----------- | :--------------------------------------------------- |
| userBid                 |   String | 是           | 用户bid                                              |
| companyName             |   String | 是           | 企业全称                                             |
| companyShortName        |   String | 是           | 企业简称                                             |
| companyIntroduce        |   String | 是           | 企业简介                                             |
| companyIndustry         |   String | 是           | 所属行业                                             |
| companyProperty         |   String | 是           | 企业性质(0国有企业、1私有企业、2事业单位 3国家行政)  |
| registeredAddressDetail |   String | 是           | 企业注册地址                                         |
| qualifieProve           |   String | 是           | 资质证明类型（默认 见10证件类型表 ）                 |
| qualifieNumber          |   String | 是           | 资质号码                                             |
| qualifieValidateStart   |   String | 是           | 资质有效期（开始）                                   |
| qualifieValidateEnd     |   String | 是           | 资质有效期（结束）                                   |
| qualifiePic             |   String | 是           | 资质照片（需要填写1.2.1接口返回的文件路径 filePath） |
| legalPersonName         |   String | 是           | 法人名称                                             |
| legalIdType             |   String | 是           | 法人证件类类型（见9证件类型表）                      |
| legalIdNumber           |   Object | 是           | 证件号码                                             |
| legalIdValidateStart    |   String | 是           | 证件有效期(开始)                                     |
| legalIdValidateEnd      |   String | 是           | 证件有效期(结束)                                     |
| legalIdPic              |   String | 是           | 证件正面（需要填写2.2.1接口返回的文件路径 filePath)  |
| legalIdBackPic          |   String | 是           | 证件背面（需要填写2.2.1接口返回的文件路径 filePath)  |
| linkmanName             |   String | 是           | 联系人名称                                           |
| linkmanPhone            |   String | 是           | 手机号                                               |
| linkmanEmail            |   String | 是           | 邮箱                                                 |
| linkmanAuthCert         |   Object | 是           | 授权书（需要填写1.2接口返回的文件路径 filePath)      |


返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| applyNo    | String   | 申请编号 |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/bid/company/credential/apply
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJqQ3h0eVJBayIsImlzcyI6IkJJRi1DSEFJTiIsImV4cCI6MTYwOTM4NzkxMiwiYmlkIjoiZGlkOmJpZDplZnozb1RRRzdMWWZLSkVpQ3lNZzU4TjlURGY5dnBXeSJ9.YHAsE6fiuFBISWeS06HpJr-GBfMTDCuekbZ-WTK1BjE",
  "params": {
	"companyName": "美团",
	"companyShortName": "美团",
	"companyIntroduce": "1",
	"companyIndustry": "A",
	"companyProperty": "0",
	"registeredAddressDetail": "河南省洛阳市新安县",
	"qualifieProve": "8",
	"qualifieNumber": "111111111111111",
	"qualifieValidateStart": "2021-07-26",
	"qualifieValidateEnd": "2021-08-12",
	"qualifiePic": "/oss/file/get/QmV1Z7syFkw8Tg41RuWChn7Ad3KjwgTexHeJ2LiYEEK2rd",
	"legalPersonName": "王兴",
	"legalIdType": "1",
	"legalIdNumber": "142766199701031224",
	"legalIdValidateStart": "2021-06-28",
	"legalIdValidateEnd": "2021-07-08",
	"legalIdPic": "/oss/file/get/QmV1Z7syFkw8Tg41RuWChn7Ad3KjwgTexHeJ2LiYEEK2rd",
	"legalIdBackPic": "/oss/file/get/QmV1Z7syFkw8Tg41RuWChn7Ad3KjwgTexHeJ2LiYEEK2rd",
	"linkmanName": "zzz",
	"linkmanPhone": "15833333333",
	"linkmanEmail": "9@qq.com",
	"linkmanAuthCert": "/oss/file/get/QmV1Z7syFkw8Tg41RuWChn7Ad3KjwgTexHeJ2LiYEEK2rd"
	}
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
     applyNo:""
  }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940026,
    "message": "认证已申请完成或申请中"
}
```

#### 2.2.3 个人可信认证申请

接口说明：

个人获取主链数字身份后，可通过接口进行可信认证，

请求参数：

| **字段名**    | **类型** | **是否必填** | **描述**                                               |
| :------------ | -------: | :----------- | :----------------------------------------------------- |
| userBid       |   String | 是           | 用户bid                                                |
| realName      |   String | 是           | 真实名称                                               |
| idType        |   String | 是           | 证件类型                                               |
| idNumber      |   String | 是           | 证件号                                                 |
| validateStart |   String | 是           | 证件有效期（开始）                                     |
| validateEnd   |   String | 是           | 证件有效期（结束）                                     |
| idPic         |   String | 是           | 证件照片（需要填写2.2.1接口返回的文件路径 filePath）   |
| idBackPic     |   String | 是           | 证件背面（需要填写2.2.1接口返回的文件路径 filePath）   |
| idHandPic     |   String | 是           | 手持证件照（需要填写2.2.1接口返回的文件路径 filePath） |


返回数据：

| 字段名  | String | 交易hash |
| :------ | :----- | :------- |
| applyNo | String | 申请编号 |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/bid/person/credential/apply
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJqQ3h0eVJBayIsImlzcyI6IkJJRi1DSEFJTiIsImV4cCI6MTYwOTM4NzkxMiwiYmlkIjoiZGlkOmJpZDplZnozb1RRRzdMWWZLSkVpQ3lNZzU4TjlURGY5dnBXeSJ9.YHAsE6fiuFBISWeS06HpJr-GBfMTDCuekbZ-WTK1BjE",
  "params": {
	"idType": "1",
	"idBackPic": "/oss/file/get/QmeCBcxCrTZDKK1m2BbqMxUWeSovJ7XwhY8MHWHdBdFBN6",
	"idHandPic": "/oss/file/get/Qmd1d4CNbrhXbRuDcJdHodYBZtNHmVDYH6Mfxo7aD1pwTA",
	"idNumber": "520181199405272642",
	"realName": "晓鹏",
	"idPic": "/oss/file/get/QmRkNfCvJEsQyjTLw7v9Nmgx1sY7xePhHpDJJTKoLdokME",
	"validateEnd": "2040-07-03",
	"validateStart": "2020-11-03"
}
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
     applyNo:""
  }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940026,
    "message": "认证已申请完成或申请中"
}
```

### 2.3 查询可信认证申请结果

接口说明：

查询可信认证结果，用于可信认证校验。

请求参数：

| **字段名** | **类型** | **是否必填** | **描述**            |
| :--------- | -------: | :----------- | :------------------ |
| bid        |   String | 是           | 用户主链数字身份BID |


返回数据：

| **字段名**  | **类型** | **描述**                              |
| :---------- | :------- | :------------------------------------ |
| applyStatus | String   | 0未申请 1待审核 2审核通过 3审核不通过 |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/bid/auth/trusted
{
  "accessToken":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
  "params":{
    "bid":"did:bid:efNY98KmDLXQFvJuJEtMUvFX3fTEvxVS"
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
    "errorCode": 0,
    "message": "操作成功",
    "data": {
        "applyStatus":"1"
    }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 2.4 凭证创建（暂缓开通）

接口说明：

用户通过该接口可以创建凭证，凭证创建完成后，在星火·链网管理平台审核通过后，其他用户可以申请该凭证

请求参数：

| **字段名** | **类型** | **是否必填** | **描述**                    |
| ---------- | -------: | ------------ | --------------------------- |
| userBid    |   String | 是           | 用户BID(凭证创建人)         |
| name       |   string | 是           | 凭证名字                    |
| industryId |  Integer | 是           | 所属行业Id                  |
| categoryId |  Integer | 是           | 所属分类Id                  |
| version    |   string | 是           | 凭证版本                    |
| remark     |   string | 否           | 凭证备注                    |
| data       |   string | 是           | 元数据和审核数据            |
| userType   |   string | 是           | 面向用户类型 1-个人，2-企业 |


返回数据：

| **字段名**         | **类型** | **描述**         |
| :----------------- | :------- | :--------------- |
| applyNo            | String   | 凭证创建申请编号 |
| voucherTemplateBid | String   | 凭证模版BID      |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/bid/voucher/create
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJqQ3h0eVJBayIsImlzcyI6IkJJRi1DSEFJTiIsImV4cCI6MTYwOTM4NzkxMiwiYmlkIjoiZGlkOmJpZDplZnozb1RRRzdMWWZLSkVpQ3lNZzU4TjlURGY5dnBXeSJ9.YHAsE6fiuFBISWeS06HpJr-GBfMTDCuekbZ-WTK1BjE",
  "params":{
      "userBid":"",
      "name":"",
      "industryId":"",
      "categoryId":"",
      "version":"",
      "remark":"",
      "data":"",
      "userType":""
	}
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
  	"voucherBid":"",
     "applyNo":""
  }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 2.5 凭证申请（暂缓开通）

接口说明：

用户通过该接口可以申请凭证

请求参数说明：

| **字段名**         | **类型** | **是否必填** | **描述**                       |
| ------------------ | -------: | ------------ | ------------------------------ |
| userBid            |   String | 是           | 用户BID（凭证申请人）          |
| content            |   String | 是           | 申请内容(凭证模版生成json数据) |
| voucherTemplateBid |   String | 是           | 凭证模版bid                    |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| applyNo    | String   | 申请编号 |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/bid/voucher/apply
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJqQ3h0eVJBayIsImlzcyI6IkJJRi1DSEFJTiIsImV4cCI6MTYwOTM4NzkxMiwiYmlkIjoiZGlkOmJpZDplZnozb1RRRzdMWWZLSkVpQ3lNZzU4TjlURGY5dnBXeSJ9.YHAsE6fiuFBISWeS06HpJr-GBfMTDCuekbZ-WTK1BjE",
  "params":{
  		"userBid":"",
      "content":"",
      "voucherTemplateBid":""
	}
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
     applyNo:""
  }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

## 3 底层链业务接入

本接口为强制必选项，需要骨干节点将子链申请信息同步至主链，由超级节点对其子链签发AC号，以获取子链接入星火·链网的许可权限。提报的子链、节点、用户信息需要变更时需要重新提报申请，由主链审核或备案。

### 3.1 申请子链

接口说明：

请确认在调用该接口前，您已在星火·链网主链业务管理平台申请成为骨干节点，获得调用该接口权限。

接口功能：

通过该接口，骨干节点可以为新加入的子链申请AC号，并由超级节点审核并为该子链签发。AC号是由四位小写字母和数字组成，代表子链在星火·链网中代表其合法身份的唯一代码，用于子链身份识别及全网寻址功能。主链无AC号。

调用流程：

申请子链需要两步操作，（1）通过主链获取申请子链blob，用于将审批过程上链；（2）使用骨干节点私钥blob进行签名，并提交上链。

#### 3.1.1 获取申请子链blob

接口说明：

提交申请子链的材料，获取申请子链的blob数据（blob是星火·链网上交易结构序列化后的十六进制格式），用于执行申请信息上链。

请求参数：

| **字段名**                        |     **类型** | **是否必填** | **描述**                                                     |
| :-------------------------------- | -----------: | :----------- | :----------------------------------------------------------- |
| backboneNodeBid                   |   String(64) | 是           | 骨干节点主链bid                                              |
| chainInfo                         |       Object | 是           | 链基本信息                                                   |
| chainInfo.industry                |    String(4) | 是           | 链的行业（见第9节行业类型）                                  |
| chainInfo.name                    |   String(30) | 是           | 链名称                                                       |
| chainInfo.subChainType            |       String | 是           | 链类型。0是子链，1是骨干链                                   |
| chainInfo.chainArch               |   String(10) | 是           | 链架构 10000:星火·链网、10001:Fabric、10002:Ethereum 10003:BubiChain、10004:Quorum、10005：Xuperchain,10006:cita , 10007：FISCO |
| chainInfo.genesisAccount          |   String(64) | 否           | 创世账户                                                     |
| chainInfo.genesisAmount           |   String(30) | 否           | 创世账户Token总数                                            |
| chainInfo.unit                    |  String (32) | 否           | Token单位                                                    |
| chainInfo.precision               |   String(30) | 否           | Token精度                                                    |
| chainInfo.baseReserve             |   String(30) | 否           | 账户Token保留数                                              |
| chainInfo.feeGasPrice             |   String(30) | 否           | 最低Gas单价                                                  |
| chainInfo.rewardInitValue         |   String(30) | 否           | 出块奖励                                                     |
| chainInfo.genesisSlogan           | String(1024) | 否           | 区块链slogan                                                 |
| chainInfo.description             | String(1024) | 是           | 区块链介绍                                                   |
| chainInfo.gatewayNodeList         |        Array | 是           | 子链网关列表（需要填写主链的BID）                            |
| chainInfo.remark                  | String(1024) | 否           | 申请备注                                                     |
| system                            |       Object | 是           | 链系统信息                                                   |
| system.visitUrl                   |  String(255) | 是           | 子链访问地址                                                 |
| system.publicStatus               |    String(2) | 是           | 开放许可，0 许可, 1开放                                      |
| system.chainCode                  |    String(4) | 是           | 链AC号（不能超过4位 格式小写字母或数字组合）。此处填写的是意向AC号，最终由超级节点签发的为准。 |
| resolveServiceList                |       Object | 否           | 解析服务对象列表                                             |
| resolveServiceList.ipv4[i].ip     |   String(64) | 否           | IPv4地址                                                     |
| resolveServiceList.ipv4[i].port   |    String(5) | 否           | 服务端口                                                     |
| resolveServiceList.ipv4[i].remark |  String(128) | 否           | 备注信息                                                     |
| resolveServiceList.ipv6[i].ip     |   String(64) | 否           | IPv6地址                                                     |
| resolveServiceList.ipv6[i].port   |    String(5) | 否           | 服务端口                                                     |
| resolveServiceList.ipv6[i].remark |  String(128) | 否           | 备注信息                                                     |
| resolveServiceList.url[i].address |  String(255) | 否           | URL地址，HTTP,HTTPS开始，不支持IP，以"/"为结尾               |
| resolveServiceList.url[i].remark  |  String(128) | 否           | 备注信息                                                     |


返回数据：

| **字段名** | **类型** | **描述**     |
| :--------- | :------- | :----------- |
| blobId     | String   | blobId       |
| txHash     | String   | 链上交易hash |
| blob       | String   | blob串       |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/chain/apply/blob
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjE5MjU2OTEwLCJiaWQiOiJkaWQ6YmlkOmVmaXV5cXVHV1R2UkVzWTltclZvS1ZoRmU1Wkp4WmRSIn0.0nUb20tqYIhcD-cS_a6YCGFM9AwZDyB7x_MFeIQv_Pc",
  "params": {
    "backboneNodeBid":"did:bid:ef28pM9MG3TGXGyWAW4JpWCFsJDd5MBnc",
    "chainInfo":{
      "subChainType": "0",
      "chainArch": "10000",
      "industry": "A",
      "name": "openapi测试链",
      "genesisAmount": "123",
      "genesisAccount": "did:bid:2hTooMvcA4Xr7aAnFH7gT2Z5o67XER7",
      "unit": "unit",
      "precision": "1",
      "baseReserve": "100",
      "feeGasPrice": "1000",
      "rewardInitValue": "10000",
      "genesisSlogan": "genesisSlogan",
      "description": "description",
      "gatewayNodeList":["gatewayNodeList1", "gatewayNodeList2"],
      "remark": "remark"
    },
    "system":{
      "visitUrl": "http://www.baidu.com",
      "chainCode": "t700",
      "publicStatus": "0"
    },
    "resolveServiceList":{
      "ipv4": [{
        "ip": "192.168.1.1",
        "port": "18122",
        "remark": "4444"
      },
        {
          "ip": "192.168.2.2",
          "port": "18122",
          "remark": "55555"
        }
      ],
      "ipv6": [{
        "ip": "1111:0410:0000:1234:FB00:1400:5000:45FF",
        "port": "18122",
        "remark": "aaa"
      },
        {
          "ip": "2222:0410:0000:1234:FB00:1400:5000:45FF",
          "port": "18122",
          "remark": "bbb"
        }
      ],
      "url": [{
        "address": "",
        "remark": ""
      }]
    }
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
 "data":{
  "blobId": "2",
  "txHash": "985ac10a6cda2f4b6c910f11bffed35fa504e53125c045d1fbbade6b335da50",
 "blob":"A296469643A6269643A6652665770673644675A6B3232346F7A5570644343654C5A6231394635705238511001225A080712296469643A6269643A666D6E7A42315A724156375966727933665759795071646B6A57776F7458635258622B0A296469643A6269643A6841706E5959474C4550614678467A35543946383374485A6242474338483776513080DAC40938E807"
 }, 
 "errorCode": 0,
 "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

#### 3.1.2 申请子链提交交易（上链）

接口说明：

对blob数据进行签名（签名详见13签名算法），并提交至主链执行上链操作；上链完成后由超级节点对申请信息进行审核，并为子链签发AC号。

请求参数：

用于将申请子链流程提交至主链并上链存储，请示参数由4.1.1接口中获取，参数如下：

| **字段名**           |    **类型** | **是否必填** | **描述**                         |
| :------------------- | ----------: | :----------- | :------------------------------- |
| blobId               |  String(19) | 是           | blobID                           |
| signerList           |        List | 是           | 签名者列表（骨干节点签名）       |
| signerList.signBlob  | String(128) | 是           | 签名串（骨干节点对blob进行签名） |
| signerList.publicKey | String(128) | 是           | 签名者公钥（骨干节点公钥）       |


返回数据：

（1）成功：

返回的交易哈希，可通过4.3接口查询子链申请详情。

| **字段名** | **类型** | **描述**     |
| :--------- | :------- | :----------- |
| txHash     | String   | 链上交易hash |
| errorCode  | int      | 0            |
| message    | String   | 成功         |


（2）失败：

说明调用接口失败，请参考错误码执行下一步操作。

| **字段名** | **类型** | **描述**                 |
| :--------- | :------- | :----------------------- |
| errorcode  | int      | 错误码，如940000         |
| message    | String   | 错误信息，如系统内部错误 |


示例：

（1）提交获取到的申请子链blob数据，示例如下：

```plain
http请求方式：POST
https://{url}/v1/chain/apply/submit
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjE5MjU2OTEwLCJiaWQiOiJkaWQ6YmlkOmVmaXV5cXVHV1R2UkVzWTltclZvS1ZoRmU1Wkp4WmRSIn0.0nUb20tqYIhcD-cS_a6YCGFM9AwZDyB7x_MFeIQv_Pc",
  "params": {
    "blobId": "144",
    "blob":"",
    "signerList": [{
      "signBlob": "5DFE2A2136B49321DDB7F5E83B23B27BE3E05FBCE8240782EE75BF592D53C7EEFEDF1D7CCA33FC8FE972E94FA67E3D083E58DFCC81E4DF78DC3B910341413701",
      "publicKey": "b065660ae694d678de4d648e04f4b86247fae59cef3590da8370664a520d9aaaaf1de3"
    }]
  }
}
```

（2）返回结果示例：

a. 成功：

接口调用成功，则返回JSON数据示例如下：

```plain
{
  "data":{
        "txHash": "985ac10a6cda2f4b6c910f11bffed35fa504e53125c045d1fbbade6b335da50"
  }, 
  "errorCode": "0",
  "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 3.2 查询子链申请状态

接口说明：

骨干节点可通过该接口查询子链申请状态，用于子链申请进度跟踪。

请求参数：

将在3.1.2中申请子链时返回的交易哈希通过接口进行查询，参数信息如下：

| **字段名** |   **类型** | **是否必填** | **描述**           |
| :--------- | ---------: | :----------- | :----------------- |
| txHash     | String(64) | 是           | 申请子链的交易哈希 |


返回数据：

（1）成功：

接口将结果返回给骨干节点，返回数据见下表：

| **字段名** | **类型** | **描述**                                |
| :--------- | :------- | :-------------------------------------- |
| chainCode  | String   | AC号                                    |
| status     | String   | 状态（0待审核，1审核通过，2审核不通过） |
| errorCode  | int      | 0                                       |
| message    | String   | 成功                                    |


（2）失败：

说明调用接口失败，请参考错误码执行下一步操作。

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | int      | 940000   |
| message    | String   | 失败     |


示例：

（1）骨干节点可通过该接口查询子链申请状态，示例如下：

```plain
http请求方式：POST
https://{url}/v1/chain/apply/status
{
"accessToken":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
  "params":{    
    "txHash": "985ac10a6cda2f4b6c910f11bffed35fa504e53125c045d1fbbade6b335da50"
  }
}
```

（2）返回结果示例：

a. 成功：

接口调用成功，则返回JSON数据示例如下：

```plain
{
  "data":{
    "chainCode":"0",
    "status":"1"
  }, 
        "errorCode": 0,
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 3.3查询链信息

接口说明：

骨干节点通过该接口可查询其申请子链的信息，便于骨干节点对其子链信息查询和校验。

请求参数：

通过向接口请求子链AC号，获取该子链信息；若不填写具体AC号，则获取该骨干节点下属所有子链信息。

| **字段名** |  **类型** | **是否必填** | **描述** |
| :--------- | --------: | :----------- | :------- |
| chainCode  | String(4) | 否           | AC号     |


返回数据：

（1）成功：

接口调用成功，将结果返回给骨干节点，返回数据见下表：

| **字段名**       | **类型** | **描述**                                                     |
| :--------------- | -------: | :----------------------------------------------------------- |
| chainList        |     List | 链列表                                                       |
| chainCode        |   String | 链AC号                                                       |
| name             |   String | 链名称                                                       |
| chainType        |   String | 链类型 0是子链，1是骨干链                                    |
| chainArch        |   String | 链架构 10000:星火·链网、10001:Fabric、10002:Ethereum 10003:BubiChain、10004:Quorum |
| createTime       |     Long | 链创建时间（时间戳 毫秒）                                    |
| industryType     |   String | 行业类型                                                     |
| genesisBid       |   String | 创世账户BID                                                  |
| genesisInitValue |     Long | 创世账户初始化积分数                                         |
| baseReserve      |     Long | 账户最小积分保留数                                           |
| gasPrice         |     Long | 最小积分单价                                                 |
| firstReward      |     Long | 首次出块奖励数                                               |
| introduce        |   String | 区块链介绍                                                   |
| version          |   String | 区块链版本号                                                 |
| backboneNodeBid  |   String | 骨干节点地址                                                 |


（2）失败：

调用接口失败，返回失败信息。请参考错误码执行下一步操作。

| **字段名** | **类型** | **描述**         |
| :--------- | :------- | :--------------- |
| errorCode  | int      | 错误码           |
| message    | String   | 接口调用失败信息 |


示例：

（1）数据骨干节点通过该接口可查询其所申请子链的信息，接口示例如下：

```plain
http请求方式：POST
https://{url}/v1/chain/list
{
  "accessToken":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
  "params":{
 		"chainCode":"by01"
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例如下：

```plain
{
  "data": {
    "chainList": [
      {
        "chainCode": "by01",  //主链为"0"， 子链为code
        "name": "营口行业链", //链
        "chainType" : "bif", //bif:星火·链网， eth:以太坊链, bif类型的为同构，其他的为异构
        "createTime" : 1593338217000, //"2020-06-28 17:56:57",链申请时间/建设时间
        "backBoneNodeBid" : "did:bid:hHTsFXTTqQfVH1oThGEKf6iwCK2c31Bes", //申请子链的骨干节点BID，bid不带chainCode
        "industryType" : "A", //行业类型，参照国标
        "genesisBid": "did:bid:hHTsFXTTqQfVH1oThGEKf6iwCK2c31Bes", //创世账户BID
        "genesisInitValue": 100000000,//创世账户初始化积分数
        "baseReserve": 10000000,//账户最小积分保留数
        "gasPrice": 1000,//最小积分单价
        "firstReward": 100000000,//首次出块奖励数
        "introduce": "",//区块链介绍
        "version": "1" // 区块链版本号
      }
    ]
  },
  "errorCode": 0,
  "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940033,
    "message": "链码不存在"
}
```

### 3.4用户备案

接口说明：

通过该接口，将骨干节点用户及其下属子链用户信息同步到主链，用于超级节点备案管理。

请求参数：

| **字段名**    |    **类型** | **是否必填** | **描述**                    |
| :------------ | ----------: | :----------- | :-------------------------- |
| bid           |  String(64) | 是           | 子链数字身份bid             |
| chainCode     |   String(4) | 是           | 所属AC号                    |
| userType      |   String(4) | 是           | 101人 102企业               |
| userName      | String(100) | 否           | 用户名                      |
| userPublicKey | String(100) | 是           | 用户公钥                    |
| registerTime  |        Long | 是           | 注册时间（时间戳 单位毫秒） |
| linkPhone     |      String | 否           | 联系电话                    |


返回数据：

（1）成功：

用户信息备案成功，返回成功信息：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | int      | 0        |
| message    | String   | 成功     |


（2）失败：

用户信息备案失败，返回失败信息。请参考错误码执行下一步操作。

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | int      | 940000   |
| message    | String   | 失败     |


示例：

（1）用户信息同步至主链，示例如下：

```plain
http请求方式：POST
https://{url}/v1/chain/user/record
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjE5MjY0OTI4LCJiaWQiOiJkaWQ6YmlkOmVmaXV5cXVHV1R2UkVzWTltclZvS1ZoRmU1Wkp4WmRSIn0.hTw23Ihi49nuX94Am_f5nKdIrrW6X09QAXIJk96LZBw",
  "params": {
    "bid": "did:bid:by01:efHAvQztBKA7hT2BgHLeqtez5QQ964w8",
    "chainCode":"by01",
    "userType": "101",
    "userName": "张三",
    "userPublicKey": "b065660ad4b4d603c6794115a6749a4b7ea9987fb61f457680b8751c5c12dab9c1a19e",
    "registerTime":1624521999286
  }
}
```

（2）返回结果示例：

a. 用户信息备案成功，则返回JSON数据示例如下：

```plain
{  
  "errorCode": 0,
  "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 3.5活跃用户同步记录

接口说明：

通过该接口将骨干节点下属子链用户（该用户需在星火·链网主链备案）的登录记录同步至主链

请求参数：

| **字段名**                |   **类型** | **是否必填** | **描述**        |
| :------------------------ | ---------: | :----------- | :-------------- |
| loginRecordList           |  list(100) | 是           | 用户登录列表    |
| loginRecordList.bid       | String(64) | 是           | 子链数字身份bid |
| loginRecordList.loginTime |       Long | 是           | 登录时间        |


返回数据：

（1）成功：

用户登录记录同步成功，返回成功信息：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | int      | 0        |
| message    | String   | 成功     |


（2）失败：

用户登录记录同步失败，返回失败信息。请参考错误码执行下一步操作。

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | int      | 940000   |
| message    | String   | 失败     |


示例：

（1）用户登录记录同步至主链，示例如下：

```plain
http请求方式：POST
https://{url}/v1/chain/user/login/record
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjE5MjY0OTI4LCJiaWQiOiJkaWQ6YmlkOmVmaXV5cXVHV1R2UkVzWTltclZvS1ZoRmU1Wkp4WmRSIn0.hTw23Ihi49nuX94Am_f5nKdIrrW6X09QAXIJk96LZBw",
  "params": {
  	"loginRecordList":[
  		{
  			"bid": "did:bid:by01:efHAvQztBKA7hT2BgHLeqtez5QQ964w8",
  			"loginTime":1624521999286
  		}
  	]
  }
}
```

（2）返回结果示例：

a. 用户信息备案成功，则返回JSON数据示例如下：

```plain
{  
  "errorCode": 0,
  "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 3.6 查询用户入链申请列表

接口说明：

骨干节点可向主链查询骨干节点子链的入链申请用户列表

请求参数：

| **字段名**  |  **类型** | **是否必填** | **描述**                            |
| :---------- | --------: | :----------- | :---------------------------------- |
| chainCode   | String(4) | 是           | AC号                                |
| auditStatus |   Integer | 是           | 审核状态（0待审核、1通过、2不通过） |
| pageStart   |   Integer | 否           | 开始页 默认1                        |
| pageSize    |   Integer | 否           | 每页条数 默认100条                  |


返回数据：

| **字段名**                   | **类型** | **描述**                            |
| :--------------------------- | :------- | :---------------------------------- |
| inchainApplyList             | List     | 入链列表                            |
| inchainApplyList.applyNo     | String   | 申请编号                            |
| inchainApplyList.chainCode   | String   | 申请子链的AC号                      |
| inchainApplyList.userBid     | String   | 用户主链Bid                         |
| inchainApplyList.applyTime   | String   | 申请时间                            |
| inchainApplyList.applyRemark | String   | 申请备注                            |
| inchainApplyList.auditStatus | String   | 审核状态（0待审核、1通过、2不通过） |
| page                         | Object   | 分页对象                            |
| page.pageSize                | Integer  | 每页条数                            |
| page.pageStart               | Integer  | 开始页                              |
| page.pageTotal               | Integer  | 总条数                              |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/chain/inchain/apply/list
{
"accessToken":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
  "params":{
   	"chainCode":"by01",
    "auditStatus":1,
    "pageStart":1,
    "pageSize":20
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
    "errorCode": 0,
    "message": "操作成功",
    "data": {
        "inchainApplyList": [{
            "applyNo": "20210407001",
            "chainCode": "by01",
            "userBid": "did:bid:efhbk6Y1kF7SuuvT2389JExuQtUVEX4n",
            "applyTime": 1618195086782,
            "applyRemark": ""
        }],
        "page": {
            "pageSize": 20,
            "pageStart": 1,
            "pageTotal": 4
        }
    }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 3.7用户入链申请状态同步

接口说明：

骨干节点可向主链查询骨干节点子链的入链申请用户列表，骨干节点可以审核这些入链申请的用户，并将审核的结果同步到主链

请求参数：

| **字段名**  |   **类型** | **是否必填** | **描述**                           |
| :---------- | ---------: | :----------- | :--------------------------------- |
| applyNo     | String(32) | 是           | 申请编号                           |
| chainCode   |  String(4) | 是           | AC号                               |
| auditStatus |  String(2) | 是           | 审核状态(0待审核、1通过、2不通过） |

返回数据：

| **类型** | **字段名** | **描述** |
| :------- | :--------- | :------- |
| Integer  | errorCode  | 错误码   |
| String   | message    | 错误描述 |

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/chain/inchain/status/syn
{
"accessToken":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
  "params":{
    "chainCode":"by01",
    "applyNo":"20210407001",
    "auditStatus":"1"
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
    "errorCode": 0,
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 3.8 链标识数据同步

接口说明：

用于将子链的标识数据上报到主链（要求每5分钟同步一次。将5分钟内的标识数据同步上来）

请求参数：

| **字段名**                  |   **类型** | **是否必填** | **描述**                                                     |
| :-------------------------- | ---------: | :----------- | :----------------------------------------------------------- |
| identList                   |       List | 是           | 标识列表                                                     |
| identList.chainCode         |  String(4) | 是           | AC号                                                         |
| identList.name              | String(64) | 是           | 标识名称                                                     |
| identList.type              |  String(2) | 是           | 标识体系类型 0:bid 1:vaa 2:handle 3:dns 4其它                |
| identList.identBid          | String(64) | 是           | 标识bid                                                      |
| identList.registerTime      |       Long | 是           | 标识注册时间(时间戳 毫秒)                                    |
| identList.userBid           | String(64) | 是           | 标识所属企业或者用户bid                                      |
| identList.origin            | String(64) | 是           | 原体系标识（考虑到兼容DNS/HANDLE等标识体系，会为原来的标识生成BID标识，原标识体系相对BID体系而言，就是DNS/HANDLE等体系） |
| identList.userName          | String(64) | 否           | 标识所属企业或者用户的名称                                   |
| identList.location          |     Object | 否           | 解析时地里位置                                               |
| identList.location.district | String(64) | 否           | 区域编码，填写示例：810008                                   |



返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | Integer  | 错误码   |
| message    | String   | 错误描述 |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/identdata/syn
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJkaWQ6YmlkOmVmb1R5cHVUeFloR25vNkM2V0JYeG9TZVo4SjJCaFRHIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjI5ODkwNTgzLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.N9Bh-m965GLDtV0lrscUSiXY-cLXyepqEfvFmj91gDY",
  "params": {
    "identList": [{
      "chainCode": "by01",
      "name": "VAA标识222",
      "type": "1",
      "identBid": "did:bid:by01:efphXYwrDzCgE5RTCC1HesQKJvyDrQDm",
      "registerTime": 1593338217001,
      "userBid": "did:bid:ef76xXoCF9xXqkhgSadKK3vy5khZR8sn",
      "userName": "",
      "origin":"vaa111",
      "location": {
        "district": "810008"
      }
    }]
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
    "errorCode": "0",
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 3.9 链标识解析数据同步

接口说明：

用于子链解析标识数据记录上报到主链（要求每5分钟同步一次。将5分钟内的解析标识数据同步上来）

请求参数：

| **字段名**                          |   **类型** | **是否必填** | **描述**                                     |
| :---------------------------------- | ---------: | :----------- | :------------------------------------------- |
| identAnalysisList                   |       List | 是           | 标识解析列表                                 |
| identAnalysisList.chainCode         |  String(4) | 是           | AC号                                         |
| identAnalysisList.recordId          | String(32) | 是           | 解析记录ID唯一(最长32位，所属子链下唯一即可) |
| identAnalysisList.identBid          | String(64) | 是           | 标识bid                                      |
| identAnalysisList.analysisTime      |       Long | 是           | 标识解析时间（Unix时间戳 毫秒）              |
| identAnalysisList.userBid           | String(64) | 是           | 解析用户bid                                  |
| identAnalysisList.userName          | String(64) | 否           | 解析用户的名称                               |
| identAnalysisList.location          |     Object | 否           | 解析时地理位置                               |
| identAnalysisList.location.district | String(64) | 否           | 区域编码，填写示例：810008                   |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | Integer  | 错误码   |
| message    | String   | 错误描述 |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/identAnalysis/syn

{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJkaWQ6YmlkOmVmb1R5cHVUeFloR25vNkM2V0JYeG9TZVo4SjJCaFRHIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjI5ODkwNTgzLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.N9Bh-m965GLDtV0lrscUSiXY-cLXyepqEfvFmj91gDY",
  "params": {
    "identAnalysisList": [{
      "chainCode": "by01",
      "recordId": "21af7f1398edbcc9a71eff24311",
      "identBid": "did:bid:by01:efphXYwrDzCgE5RTCC1HesQKJvyDrQDm",
      "analysisTime": 1593338217000,
      "userBid": "did:bid:ef76xXoCF9xXqkhgSadKK3vy5khZR8sn",
      "userName": "",
      "location": {
        "district": "810008"
      }
    }]
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
    "errorCode": "0",
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 3.10 链应用服务信息同步

接口说明：

子链应用服务信息同步到主链（要求每5分钟同步一次。将5分钟内的子链应用服务信息同步上来）

请求参数：

| **字段名**     |   **类型** | **是否必填** | **描述**                                                     |
| :------------- | ---------: | :----------- | :----------------------------------------------------------- |
| chainCode      |  String(4) | 是           | 子链AC号                                                     |
| businessType   |    Integer | 是           | 业务类型：1溯源，2存证，3供应链金融 20 其他                  |
| dataType       |    Integer | 是           | 数据类型： 当业务类型为溯源时，1-溯源产品记录、2-生产环节记录； 当业务类型为存证时，1-存证作品记录、2-作品权利列表、3-作品侵权列表； 当业务类型为供应链金融时，1-参与主体记录、2-供应链金融流程记录 |
| dataList       |      Array | 是           | 数据数组                                                     |
| dataList.key   | String(64) | 是           | 记录ID，用于标记同步的某条数据                               |
| dataList.time  |       Long | 是           | 毫秒级时间戳，用于数据查询时排序                             |
| dataList.value |     Object | 是           | 业务数据对象，具体结构详见后表                               |

##### 1） 溯源类应用服务同步数据

###### 1.1）**同步溯源产品记录**：

```
http请求方式：POST
https://{url}/v1/chain/appService/info/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "businessType": 1,
        "dataType": 1,
        "dataList": [{
            "key": "{key}",
            "time": 1593338217000,
            "value": {
                "name": "名称",
                "type": "名称",
                "num": "123",
                "creator": "123",
                "time": 1593338217000,
                "picture": ""
            }
        }]
    }
}
```

业务数据对象：溯源类产品记录核心字段说明

| **字段名** | **类型** | **是否必填** | **描述**                 |
| :--------- | -------: | :----------- | :----------------------- |
| name       |   String | 是           | 产品名称                 |
| type       |   String | 是           | 业务类型                 |
| time       |  Integer | 是           | 毫秒级时间戳，生成时间   |
| num        |   String | 是           | 产品信息编号，子链上唯一 |
| creator    |   String | 是           | 创建用户                 |
| picture    |   String | 是           | 产品图片                 |

###### 1.2）**同步生产环节记录**：

```
http请求方式：POST
https://{url}/v1/chain/appService/info/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "businessType": 1,
        "dataType": 1,
        "dataList": [{
            "key": "{key}",
            "time": 1593338217000,
            "value": {
                "productName": "名称",
                "stepName": "名称",
                "stepType": 0,
                "infoType": 0,
                "operator": "123",
                "lastOps": "123",
                "totalOps": 89,
                "blockHeight": 78668,
                "txHash": "{txHash}",
                "lastStatus": 0,
                "time": 1593338217000,
                "num": ""
            }
        }]
    }
}
```



业务数据对象：溯源类生产环节记录核心字段说明

| **字段名**  | **类型** | **是否必填** | **描述**                                                    |
| :---------- | -------: | :----------- | :---------------------------------------------------------- |
| productName |   String | 是           | 产品名称                                                    |
| stepName    |   String | 是           | 环节名称                                                    |
| stepType    |  Integer | 是           | 环节类型，0：生产 1：加工 2：运输 3：销售                   |
| infoType    |  Integer | 是           | 录入信息，0：产品信息 1：生产信息 2：溯源信息 3：溯源码导出 |
| operator    |   String | 是           | 操作人员名称                                                |
| lastOps     |   String | 是           | 最近操作                                                    |
| totalOps    |  Integer | 是           | 操作总数                                                    |
| blockHeight |  Integer | 是           | 区块高度                                                    |
| txHash      |   String | 是           | 交易哈希                                                    |
| time        |  Integer | 是           | 毫秒级时间戳，记录时间                                      |
| lastStatus  |  Integer | 是           | 最近状态， 0：操作中 1：已完成                              |
| num         |   String | 是           | 产品信息编号，子链上唯一                                    |

##### 2） 存证类应用服务同步数据

###### 2.1）**同步存证作品记录**：

```
http请求方式：POST
https://{url}/v1/chain/appService/info/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "businessType": 2,
        "dataType": 1,
        "dataList": [{
            "key": "{key}",
            "time": 1593338217000,
            "value": {
                "name": "名称",
                "businessType": "",
                "type": 0,
                "nature": 0,
                "creator": "贾平凹",
                "notaryOffice": "陕西省文学认证机构",
                "issueStatus": 1,
                "blockHeight": 148838,
                "txHash": "590f278351091ed924e70f7611558bae0b7d6faaade3f3cc8ad16c1242573fa3",
                "time": 1623032433,
                "num": "ffd6baaed492b3195764b4b2febfde73"
            }
        }]
    }
}
```

业务数据对象：（存证业务）存证作品列表核心字段说明

| **字段名**   | **类型** | **是否必填** | **描述**                                                     |
| :----------- | -------: | :----------- | :----------------------------------------------------------- |
| name         |   String | 是           | 作品名称                                                     |
| businessType |   String | 是           | 业务类型                                                     |
| type         |  Integer | 是           | 作品类别，0：文学 1：歌曲 2：视频 3：电影 4：其他            |
| nature       |  Integer | 是           | 作品性质， 0：原创 1：改编 2：翻译 3：汇编 4：注释 5：整理 6：其他 |
| creator      |   String | 是           | 作者名称                                                     |
| notaryOffice |   String | 是           | 公证机构                                                     |
| issueStatus  |  Integer | 是           | 发表状态，0：未发表，1：已发表                               |
| blockHeight  |  Integer | 是           | 区块高度                                                     |
| txHash       |   String | 是           | 交易哈希                                                     |
| time         |  Integer | 是           | 存证时间（Unix时间戳 毫秒）                                  |
| num          |   String | 是           | 作品编号，子链上唯一                                         |

###### 2.2）**同步作品权利列表**：

```
http请求方式：POST
https://{url}/v1/chain/appService/info/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "businessType": 2,
        "dataType": 1,
        "dataList": [{
            "key": "{key}",
            "time": 1593338217000,
            "value": {
                "name": "名称",
                "rightName": [{
                  "type": 0,
                  "trans": "张三",
                  "proxies":[{
                    "key":"{key}",
                    "value":"{value}"
                  }]
                }],
                "time": 1593338217000,
                "num": "11e9c0a13f2cef98d366929de72be286"
            }
        }]
    }
}
```



业务数据对象：（存证业务）作品权利列表核心字段说明

| **字段名**        |           **类型** | **是否必填** | **描述**                          |
| :---------------- | -----------------: | :----------- | :-------------------------------- |
| name              |             String | 是           | 作品名称                          |
| rightName         |       List<Object> | 是           | 权益名称                          |
| rightName.type    |            Integer | 是           | 权利类型，0：复制权、1：发行权、2：出租权、3：展览权、4：表演权、5：放映权、6：广播权、7：信息网络传播权、8：摄影权、9：改编权、10：翻译权、11：汇编权、12：其他权利 |
| rightName.trans   |             String | 是           | 转让人                            |
| rightName.proxies | Map<String,String> | 是           | 代理人列表，Map结构的key用于排序  |
| time              |            Integer | 是           | 权益创建时间 （Unix 时间戳 毫秒） |
| num               |             String | 是           | 作品编号，子链上唯一              |

###### 2.3）**同步作品侵权列表**：

```
http请求方式：POST
https://{url}/v1/chain/appService/info/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "businessType": 2,
        "dataType": 1,
        "dataList": [{
            "key": "{key}",
            "time": 1593338217000,
            "value": {
                "name": "名称",
                "website": "",
                "blockHeight": 869855,
                "txHash": "5016e1e7cca7b83c3d0a89d90e01e50ed587d2a6b6fbabae9105584015c9ab87",
                "certHash": "15970183d8aa7538bf06030f7784f91e220a6d05b80c6aedeb89e030756e26b3",
                "time": 1593338217000,
                "num": "11e9c0a13f2cef98d366929de72be286"
            }
        }]
    }
}
```

业务数据对象：（存证业务）作品侵权列表核心字段说明

| **字段名**  | **类型** | **是否必填** | **描述**                     |
| :---------- | -------: | :----------- | :--------------------------- |
| name        |   String | 是           | 作品名称                     |
| website     |   String | 是           | 侵权网站                     |
| blockHeight |  Integer | 是           | 区块高度                     |
| txHash      |   String | 是           | 交易哈希                     |
| time        |  Integer | 是           | 取证时间（Unix 时间戳 毫秒） |
| num         |   String | 是           | 作品编号，子链上唯一         |
| certHash    |   String | 是           | 证书哈希                     |

##### 3） 供应链金融类应用服务同步数据

###### 3.1）**同步参与主体记录**：

```
http请求方式：POST
https://{url}/v1/chain/appService/info/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "businessType": 2,
        "dataType": 1,
        "dataList": [{
            "key": "{key}",
            "time": 1593338217000,
            "value": {
                "name": "名称",
                "type": 0,
                "quota": 869855,
                "invoiceAmount": "5487",
                "financingAmount": "3453",
                "time": 1593338217000,
                "repaymentAmount": "3456",
                "overdueAmount": "4234",
                "blockHeight": 499884
            }
        }]
    }
}
```

业务数据对象：（供金业务）参与主体列表核心字段说明

| **字段名**      | **类型** | **是否必填** | **描述**                                               |
| :-------------- | -------: | :----------- | :----------------------------------------------------- |
| name            |   String | 是           | 主体名称，子链上唯一                                   |
| type            |  Integer | 是           | 主体类型， 0：国有企业 1：私有企业 2：国外企业 3：其他 |
| quota           |   String | 是           | 授信额度                                               |
| invoiceAmount   |   String | 是           | 开具金额                                               |
| financingAmount |   String | 是           | 融资金额                                               |
| repaymentAmount |   String | 是           | 还款金额                                               |
| overdueAmount   |   String | 是           | 逾期金额                                               |
| time            |  Integer | 是           | 毫秒级时间戳，上链时间                                 |
| blockHeight     |  Integer | 是           | 区块高度                                               |

###### 3.2）**同步供应链金融流程记录**：

```
http请求方式：POST
https://{url}/v1/chain/appService/info/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "businessType": 2,
        "dataType": 1,
        "dataList": [{
            "key": "{key}",
            "time": 1593338217000,
            "value": {
                "amount": "名称",
                "num": "0d77d483634cd4e8e4ddd712e0a15910",
                "type": 0,
                "from": "did:bid:eftnSnkPZQhoUnaySFhKgGAuK4gxUeFc",
                "to": "did:bid:efhX1bSmnC8pwd7KW8SHxeFLQYB6UwTg",
                "preNum": "07d38c31850033fff867094cf05b8308",
                "status": 0,
                "txHash": "9b687618d61a61e3c9caaf8039f486a3b6be8f87447935200f9bc810930298e5",
                "blockHeight": 499884,
                "time": 1593338217000
            }
        }]
    }
}
```

业务数据对象：（供金业务）供金流程列表

| **字段名**  | **类型** | **是否必填** | **描述**                                          |
| :---------- | -------: | :----------- | :------------------------------------------------ |
| amount      |   String | 是           | 资金额                                            |
| num         |   String | 是           | 单据编号                                          |
| type        |  Integer | 是           | 流程类型，0：授信 1：开具 2：融资 3：转让 4：还款 |
| from        |   String | 是           | 转出主体名称                                      |
| to          |   String | 是           | 转入主体名称                                      |
| preNum      |   String | 是           | 上一环节编号                                      |
| status      |  Integer | 是           | 状态，0：未完成 1：已完成                         |
| txHash      |   String | 是           | 交易哈希                                          |
| blockHeight |  Integer | 是           | 区块高度                                          |
| time        |  Integer | 是           | 上链时间（Unix时间戳 毫秒）                       |

## 4 底层链锚定

通过该接口由骨干链或子链底层数据锚定至主链，确保数据不可篡改。

### 4.1 链基础信息同步

接口说明：骨干节点将链基础信息同步至主链 

请求参数：

| **字段名**         |    **类型** | **是否必填** | **描述**                                                     |
| :----------------- | ----------: | :----------- | :----------------------------------------------------------- |
| chainCode          |   String(4) | 是           | 子链AC号                                                     |
| name               | String(128) | 是           | 链名称                                                       |
| chainType          |      String | 是           | 链架构类型，10000:星火·链网、10001:Fabric、10002:Ethereum 10003:BubiChain、10004:Quorum，10005：Xuperchain,10006:cita , 10007：FISCO |
| time               |        Long | 是           | 链申请时间/建设时间  （Unix时间戳 毫秒）                     |
| backBoneNodeBid    |  String(64) | 是           | 申请子链的骨干节点BID，bid不带chainCode                      |
| industryType       |   String(1) | 是           | 所属行业类型值，**见行业类型9**                              |
| trafficWarning     |        Long | 是           | 节点网络流量告警值，单位字节                                 |
| trafficOverload    |        Long | 是           | 节点网络流量过载值，单位字节                                 |
| consensus          | String(128) | 是           | 区块链共识机制, 如 POW 或 DPOS 或 DPOS + PBFT （自定义填写） |
| genesisBid         | String(128) | 否           | 创世账户地址                                                 |
| genesisInitValue   |  String(30) | 否           | 创世账户初始化TOKEN数                                        |
| baseReserve        |  String(30) | 否           | 账户最小TOKEN保留数                                          |
| gasPrice           |  String(30) | 否           | 最小TOKEN单价                                                |
| firstReward        |  String(30) | 否           | 首次出块奖励数                                               |
| introduce          |      String | 否           | 区块链介绍                                                   |
| version            |  String(16) | 是           | 区块链版本号                                                 |
| ledgerDB           | String(128) | 是           | 支持的账本数据库类型名称（全小写，英文逗号间隔）             |
| topTps             |     Integer | 是           | 理论tps峰值                                                  |
| organization       | String(256) | 是           | 链的运营机构名称                                             |
| explorerUrl        | String(256) | 否           | 区块链浏览器地址，如：https://explorer.bubi.cn               |
| defaultDbType      |  String(20) | 是           | 链上默认账本数据库类型，（规则：全小写，英文逗号间隔，比如leveldb） |
| location           |      Object | 是           | 链的运营机构地理位置                                         |
| location.latitude  |  String(16) | 是           | 地理位置纬度                                                 |
| location.longitude |  String(16) | 是           | 地理位置经度                                                 |
| location.province  |  String(32) | 是           | 省、直辖市、自治区，比如：陕西省                             |
| location.city      |  String(32) | 是           | 城市，比如：西安市                                           |
| location.district  |  String(32) | 是           | 区、市、县，比如：雁塔区                                     |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | Integer  | 错误码   |
| message    | String   | 错误描述 |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/info/syn
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjIxNTk0NDQ4LCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.MxWdV-mwCScUq8QwmTXTaxKli8jvXq1sNVvAYQWWsSo",
   "params": {
    "chainCode": "cc1",
    "name": "营口行业链",
    "chainType" : "10000",
    "time" : 1593338217000,
    "backBoneNodeBid" : "did:bid:hHTsFXTTqQfVH1oThGEKf6iwCK2c31Bes",
    "industryType" : "A",
    "trafficWarning" : 4096,
    "trafficOverload" : 8192,
    "consensus" : "DPOS+PBFT",
    "genesisBid": "did:bid:hHTsFXTTqQfVH1oThGEKf6iwCK2c31Bes",
    "genesisInitValue": 100000000,
    "baseReserve": 10000000,
    "gasPrice": 1000,
    "firstReward": 100000000,
    "introduce": "",
    "version": "1",
    "ledgerDB": "rocksdb,tidb,mysql",
    "topTps": 1000,
    "organization": "信通院",
    "explorerUrl": "https://explore.bif.com",
    "defaultDbType": "",
    "location": {
        "latitude": "29.5",
        "longitude": "113.4",
        "province": "辽宁省",
        "city": "营口市",
        "district": "渝中区"
    }
  }
}

```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
    "errorCode": 0,
    "message": "成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 4.2 链个性化信息同步

接口说明：

用于骨干链、异构链数据上报到主链（要求有变动时将数据同步上来）

请求参数：

| **字段名**     |   **类型** | **是否必填** | **描述**                                                     |
| :------------- | ---------: | :----------- | :----------------------------------------------------------- |
| chainCode      |  String(4) | 是           | 子链AC号                                                     |
| chainType      | String(10) | 是           | 链架构类型，10000:星火·链网、10001:Fabric、10002:Ethereum、 10003:BubiChain、10004:Quorum，10005：Xuperchain,10006:cita , 10007：FISCO |
| dataType       |    Integer | 是           | 提交的数据类型，用于区分异构链同步数据，具体使用请按照示例进行 |
| dataList       |       List | 是           | 数据数组 (最大100)                                           |
| dataList.key   | String(64) | 是           | 数据唯一值，用于更新value                                    |
| dataList.value |     Object | 是           | 数据对象，具体使用请按照示例进行                             |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | Integer  | 错误码   |
| message    | String   | 错误描述 |

示例：

##### 1） Xuperchain链同步数据

###### 1.1）**同步总览数据**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc5",
        "chainType": "10005", // Xuperchain
        "dataType": 1, // 同步总览数据
        "dataList": [{
            "key": "aaa1",
            "value": {
                "chainType": "xuperchain",
                "chainCnt": 1,
                "nodeCnt": 1,
                "contractCnt": 10,
                "blockHeight": 1,
                "createdTime": 1593338217000,
                "consensus": ""
            }
        }]
    }
}
```

###### 1.2）**同步权限管理数据**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10005",
        "dataType": 2,
        "dataList": [{
            "key": "",
            "value": {
                "maxBlockSize": "", // 块大小最大值
                "reward": "", // 出块奖励
                "declineRate": "", // 奖励衰退率
                "blockGap": "", // 出块时间间隔
                "turnGap": "", // 提案节点轮换间隔
                "voteCost": "", // 投票单元成本
                "gas": "", // GAS成本
                "initNodeAddr": "", // 初始提案节点地址
            }
        }]
    }
}
```

###### 1.3）**同步平行链列表**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10005",
        "dataType": 3,
        "dataList": [{
            "key": "",
            "value": {
                "chainID": 123, // chainID
                "chainName": "", // 平行链名称
                "owner": 1, // 创建者
                "fee": 2, // 创建花费utxo
                "hasGrp": 2, // 是否有群组
                "statue": 0, // 平行链状态
                "blockHeight": 1, // 区块高度
                "lastModifyTime": date, // 最新修改时间
                "createdTime": data, // 创建时间
            }
        }]
    }
}
```

###### 1.4）**同步平行链群组节点白名单列表**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10005",
        "dataType": 4,
        "dataList": [{
            "key": "",
            "value": {
                "nodeId": "", // node id
                "nodeName": "", // 节点名
                "host": "", // 域名
                "port": 4004, // 端口
            }
        }]
    }
}
```

###### 1.5）**同步合约账号列表**

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10005",
        "dataType": 5,
        "dataList": [{
            "key": "",
            "value": {
                "id": "", // account id
                "accountName": "", // 账号名
                "privType": "", // 合约权限类型 签名阈值、AKSet签名
                "akCnt": "", // AK数量
                "deployContractCnt": "", // 部署合约量
                "InvokeContractCnt": "", // 调用合约量
                "createdTime": date, // 创建时间
                "akSet": "", // 基础AK集
            }
        }]
    }
}
```

###### 1.6）**同步背书权重列表**

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10005",
        "dataType": 6,
        "dataList": [{
            "key": "",
            "value": {
                "address": "", // 地址
                "weight": 0.1, // 权重
            }
        }]
    }
}
```

###### 1.7）**同步集合列表**

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10005",
        "dataType": 6,
        "dataList": [{
            "key": "",
            "value": {
                "address": "", // 地址
                "weight": 0.1, // 权重
            }
        }]
    }
}
```

##### 2） Cita链同步数据

###### 2.1）**同步总览数据**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10006",
        "dataType": 1,
        "dataList": [{
          "key":"",
          "value":{
            "chainType": "CITA", // 区块链类型
                "accountCnt": 1, //账号数量
                "nodeCnt": 1, //节点数量
                "contractCnt": 1, //合约数量
                "blockHeight": 1, // 区块高度
                "createdTime": 1593338217000, //"2020-06-28 17:56:57",子链建设时间
                "consensus": "" // 共识机制
            }
        }]
    }
}
```

###### 2.2）**同步详细配置**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10006",
        "dataType": 2,
        "dataList": [{
            "key": "",
            "value": {
                "host": "www.cita.chain", //域名
                "isAward": false, // 出块激励选择开关（关）
                "isInvokeContractCheck": false, // 合约调用权限检查开关（关）
                "isTxCheck": false, // 发送交易权限检查开关（关）
                "isCreateContactCheck": false, // 创建合约权限检查开关（关）
                "token": "", // 通证
                "consensusList": [{
                    "consensus": "ip:port"
                }]
            }
        }]
    }
}
```

###### 2.3）**同步高阶服务**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10006",
        "dataType": 3,
        "dataList": [{
            "key": "",
            "value": {
                "supportSM": 0, // 国密支持   返回0即不支持该服务，返回1即支持该服务
                "supportMigrate": 0, //数据迁移   节点容量超过X%时启动，返回0即不支持该服务，返回1-100即支持该服务，且节点容量超过该百分比时启动数据迁移
                "supportSGX": 0, //SGX安全防护   返回0即不支持该服务，返回1即支持该服务
                "supportOracle": 0 // 预言机    返回0即不支持该服务，返回1即支持该服务
            }
        }]
    }
}
```

###### 2.4）同步账号统计：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10006",
        "dataType": 4,
        "dataList": [{
            "key": "",
            "value": {
                "accountCnt": 10, // 账号总数
                "externalAccountCnt": 10, // 外部账号数
                "contractAccountCnt": 10, // 合约账号数
                "grpCnt": 2 // 账号组数
            }
        }]
    }
}
```

##### 3） Fisco链同步数据

###### 3.1）**同步总览数据**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10007",
        "dataType": 1,
        "dataList": [{
            "key": "",
            "value": {
                "chainType": "FISCO BCOS", // 区块链类型
                "grpCnt": 1, //群组数量
                "nodeCnt": 1, //节点数量
                "contractCnt": 1, //合约数量
                "blockHeight": 1, // 区块高度
                "createdTime": 1593338217000, //"2020-06-28 17:56:57",子链建设时间
                "consensus": "" // 共识机制
            }
        }]
    }
}
```

###### 2.2）**同步详细配置**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10007",
        "dataType": 2,
        "dataList": [{
            "key": "",
            "value": {
                "threshold": 100, //委员投票阈值
                "account": "委员账户", //委员账户
                "ticketCnt": 1 // 委员票数
            }
        }]
    }
}
```

###### 2.3）**同步高阶服务**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10007",
        "dataType": 3,
        "dataList": [{
            "key": "",
            "value": {
                "supportSM": 0, // 国密支持   返回0即不支持该服务，返回1即支持该服务
                "supportMigrate": 0, //数据迁移   节点容量超过X%时启动，返回0即不支持该服务，返回1-100即支持该服务，且节点容量超过该百分比时启动数据迁移
                "supportSGX": 0, //SGX安全防护   返回0即不支持该服务，返回1即支持该服务
                "supportOracle": 0 // 预言机    返回0即不支持该服务，返回1即支持该服务
            }
        }]
    }
}
```

###### 2.4）同步群组列表：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10007",
        "dataType": 4,
        "dataList": [{
            "key": "",
            "value": {
                "grpName": "群组1", //群组名称
                "crtCnt": 1, //证书数
                "conNodeCnt": 1, //共识节点数
                "absNodeCnt": 1, // 观察者节点数
                "contractCnt": 1, //合约部署数
                "createTime": 1593338217000, //创建时间 
                "lastModifyTime": 1593338217000, //最近修改时间 
                "grpId": 1, //群组ID
                "consensus": "", //共识机制
                "maxTxCnt": 1, //最大交易量
                "delay": 100, //打包延时，单位是毫秒
                "periodBlockCnt": 1, //共识周期出块数
                "periodConNodeCnt": 1, //每周共识节点数
                "syncPolicy": "", //区块同步策略
                "consensusNode": "", // 共识节点
                "abserveNode": "", //观察节点
                "blockHeight": 1 //区块高度
            }
        }]
    }
}
```

###### 2.5）同步群组列表：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10007",
        "dataType": 5,
        "dataList": [{
            "key": "",
            "value": {
                "certName": "证书1", // 证书名称
                "certType": 1, //证书类型  0:委员会证书、1:SDK证书、2:机构证书、3:节点证书、4:其他证书
                "certIssuer": "", // 证书签发者
                "status": 1, //  证书状态 
                "time": "YYYY.MM.DD-YYYY.MM.DD" //  生效时间  返回示例：YYYY.MM.DD-YYYY.MM.DD
            }
        }]
    }
}
```

##### 4） Fabric链同步数据

###### 4.1）**同步总览数据**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10001",
        "dataType": 1,
        "dataList": [{
          "key":"",
          "value":{
            "chainType": "Fabric", // 区块链类型
                "channelCnt": 1, // 通道数量
                "orgCnt": 1, // 组织数量
                "nodeCnt": 1, //节点数量
                "chainCodeCnt": 1, // 链码数量
                "blockHeight": 1, // 区块高度
                "createdTime": 1593338217000, //"2020-06-28 17:56:57",子链建设时间
                "consensus": "" // 共识机制
          }
        }]
    }
}
```

###### 4.2）**同步共识节点列表**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10001",
        "dataType": 2,
        "dataList": [{
          "key":"",
          "value":{
            "host": "", // 域名
                "address": "", //接口
                "consensus" : "", // 所属共识 例"Raft"、"PBFT"
                "certStatus" : "",  //证书状态 返回0即无效，返回1即有效
          }
        }]
    }
}

```

###### 4.3）**同步策略列表**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10001",
        "dataType": 3,
        "dataList": [{
          "key":"",
          "value":{
            "policyName": "", // 策略名
                "policyType": "", //策略类别 返回1即背书策略，返回2即投票策略，返回3即其他策略
                "policyDetail" : "", // 具体策略集
                "purpose" : ""  // 策略应用对象 返回1即成员，返回2即链码，返回3即其他
          }
        }]
    }
}
```

###### 4.4）**同步通道列表**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10001",
        "dataType": 4,
        "dataList": [{
          "key":"",
          "value":{
                "channelID": 123, // channelID
                "name": "", // 通道名称
                "orderOrgCnt": 1, // 排序组织数
                "orderNodeCnt" : 2, // 排序节点数 例"Raft"、"PBFT"
                "peerOrgCnt" : 2,  // peer组织数
                "peerNodeCnt" : 2,  // peer节点数
                "chainCodeDeployCnt" : 100,  // 链码部署数
                "blockHeight": 1111, // 区块高度
                "statue": 0, // 通道状态
                "hashWidth": 1, // 区块数据哈希结构宽度
                "hashAlgorithm": "", // 哈希算法
                "delay": 0.1, // 打包延时 ms 
                "maxSize": 22, // 打包最大字节数
                "maxTxCnt": 12, // 打包最大交易数
                "defaultMaxSize": 1234 // 推荐打包最大字节数
            }
        }]
    }
}
```

###### 4.5）**同步**排序组织列表：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10001",
        "dataType": 5,
        "dataList": [{
          "key":"",
          "value":{
                "channel": "", // channel id
                "channelName": "", // 所属通道名称
                "orgName" : "", // 组织名称
                "orgType" : "",  // 组织类型
                "nodeType" : "", // 节点类型
                "nodeName" : "", // 节点名
                "host": "", // 域名
                "port": 4004 // 端口
            }
        }]
    }
}
```

###### 4.6）**同步**Peer组织列表：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10001",
        "dataType": 6,
        "dataList": [{
          "key":"",
          "value":{
                "channel": "", // channel id
                "channelName": "", // 所属通道名称
                "orgName" : "", // 组织名称
                "nodeType" : "", // 节点类型
                "nodeName" : "", // 节点名
                "host": "", // 域名
                "port": 4004 // 端口
            }
        }]
    }
}
```

###### 4.7）**同步组织列表**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10001",
        "dataType": 7,
        "dataList": [{
          "key":"",
          "value":{
                "orgId": "", // org id
                "orgName": "", // 组织名称
                "host": "", // 域名
                "orgType" : "", // 组织类型
                "nodeCnt" : 10, // 节点数量
                "channel" : "", // 加入通道
                "consortuim" : "", // 加入联盟
                "anchor" : "" // 锚节点
            }
        }]
    }
}
```

###### 4.8）**同步组织节点列表**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10001",
        "dataType": 8,
        "dataList": [{
          "key":"",
          "value":{
                "orgId": "", // org id
                "orgName": "", // 组织名称
                "nodeName" : "", // 节点名称
                "host": "", // 域名，包含端口
                "nodeType" : "", // 节点类型
                "channel" : "" // 加入通道
            }
        }]
    }
}
```

###### 4.9）**同步组织证书列表**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10001",
        "dataType": 9,
        "dataList": [{
          "key":"",
          "value":{
                "orgId": "", // org id
                "orgName": "", // 组织名称
                "certId" : "", // 证书ID
                "certIdentification": "", // 证书标识
                "certType" : 1, // 证书类型，1管理员证书，2组织证书，3用户证书，4节点证书
                "validTime" : "", // 有效时间
                "statue" : "", // 状态
            }
        }]
    }
}
```

###### 4.10）**同步高阶服务**：

```
http请求方式：POST
https://{url}/v1/chain/isomerism/data/syn
{
    "accessToken": "",
    "params": {
        "chainCode": "cc1",
        "chainType": "10001",
        "dataType": 10,
        "dataList": [{
          "key":"",
          "value":{
                "supportSM": 0, // 国密支持   返回0即不支持该服务，返回1即支持该服务
                "supportMigrate": 0, //数据迁移   节点容量超过X%时启动，返回0即不支持该服务，返回1-100即支持该服务，且节点容量超过该百分比时启动数据迁移
                "supportSGX": 0, //SGX安全防护   返回0即不支持该服务，返回1即支持该服务
                "supportOracle": 0 // 预言机    返回0即不支持该服务，返回1即支持该服务
            }
        }]
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
    "errorCode": "0",
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 4.3 链状态信息同步

骨干节点将子链的动态信息同步到主链，需要两步操作：3.1.1获取链动态信息同步交易blob；3.1.2链动态信息同步提交。要求每隔5分钟同步一次数据。

#### 4.3.1  获取子链状态信息同步blob

接口说明：

子链状态信息同步时需要将信息同步到链上

请求参数：

| **字段名**          |   **类型** | **是否必填** | **描述**                                                     |
| :------------------ | ---------: | :----------- | :----------------------------------------------------------- |
| chainCode           |  String(4) | 是           | 链AC号                                                       |
| gatewayAddress      | String(64) | 是           | 链网关地址（在平台链详情中查看该地址），如：**did:bid:efb8Ehu9SGVCTm2BbvpPiQjzNWo4LQAA** |
| blockHeight         |       Long | 是           | 区块高度                                                     |
| validationNodeCount |       Long | 是           | 参与共识的节点数量                                           |
| totalNodeCount      |       Long | 是           | 节点总数量                                                   |
| systemContractCount |       Long | 是           | 系统合约数量                                                 |
| normalContractCount |       Long | 是           | 普通合约数量                                                 |
| serviceNodeCount    |       Long | 是           | 业务节点个数                                                 |
| txNumber            |       Long | 是           | 总交易数                                                     |
| accountCount        |       Long | 是           | 总账户数                                                     |
| tps                 |       Long | 是           | 平均tps                                                      |
| extra               |     String | 否           | 额外字段                                                     |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| blobId     | String   | blodId   |
| txHash     | String   | 交易hash |
| blob       | String   | blod串   |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/status/syn/blob
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjIxNTcxMjg0LCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.9tTPWasYgCs1yC3qYJYSjCGvd9Syl1mr6G2EVrKeq2E",
  "params": {
    "chainCode": "by01",
    "gatewayAddress": "did:bid:efb8Ehu9SGVCTm2BbvpPiQjzNWo4LQAA",
    "blockHeight": 50003,
    "txNumber": 200000,
    "tps": 101,
    "validationNodeCount":0,
    "totalNodeCount": 0,
    "systemContractCount": 0,
    "normalContractCount": 0,
    "serviceNodeCount": 0,
    "accountCount": 0,
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "blobId": "15",
    "txHash": "d60c15bacbb7f98f00921d5ad17d49fafe0c65ea706d5d4ced25e490ac35e9e0",
    "blob": "0A286469643A6269643A65663477776B727A53564650545A4A627257433439446B416554535169377241101E228304080712286469643A6269643A656662384568753953475643546D32426276705069516A7A4E576F344C51414152D4030A286469643A6269643A6566427070397753676564315A5A72437566426D36374A4C55635255396375321AA7037B226D6574686F64223A227375626D6974426C6F636B486561646572222C22706172616D73223A7B22626C6F636B486561646572223A7B226163636F756E745F747265655F68617368223A22222C22636C6F73655F74696D65223A2231353933333338343030303032303030222C22636F6E73656E7375735F76616C75655F68617368223A22222C22666565735F68617368223A22222C2268617368223A2235323134303538363966626533633132396237383434383232333566616365633435656438653432313666633834653962623665336136386661653430636232222C2270726576696F75735F68617368223A2231323334363966626533633132396237383434383232333566616365633435656438653233313435363537646161313233343131326136386661653430636232222C22736571223A223938222C2274785F636F756E74223A223132222C2276616C696461746F72735F68617368223A22696E7465726E616C436861696E496432222C2276657273696F6E223A22227D2C22636861696E436F6465223A2262793031222C2264796E616D6963496E666F223A7B7D7D7D3080EAADE907"
  }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

#### 4.3.2 子链的状态信息提交（上链）

接口说明：

用于链的状态信息同步提交至主链并上链备案。

请求参数：

| **字段名**           |    **类型** | **是否必填** | **描述**                           |
| :------------------- | ----------: | :----------- | :--------------------------------- |
| blobId               |  String(20) | 是           | Blob序号，通过获取交易Blob接口获取 |
| signerList           |        List | 是           | 签名者列表（注意：需要链网关签名） |
| signerList.signBlob  | String(128) | 是           | 签名串（链网关签名）               |
| signerList.publicKey | String(128) | 是           | 签名者公钥（链网关公钥）           |

返回数据：

| **字段名** | **类型** | **描述**                                    |
| :--------- | :------- | :------------------------------------------ |
| txHash     | String   | 链上交易hash,（可用于查询上链交易最终状态） |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/status/syn/submit
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjE5MzQyNDIyLCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.3gcQQBqvqtZH1q-TfUiGne68R1TnBcvLAA6nIJ8qDGU",
  "params": {
    "blobId": "1",
    "signerList": [{
      "signBlob": "BDE955FF0A0F52A39E95815962836D13172D6EF92C55A72A97ECCF2CE882E2E2EAC73FA1A624D73C959E18126B4A35947375621D819F539DDC0692EEF0F4D702",
      "publicKey": "b06566086febd16adf6553cc9c000dba5f9f410d34f8541392303561dddc045b15ef4c"
    }]
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
    "errorCode": "0",
    "message": "操作成功",
      "data": {
      "txHash": "083b797ea00204d2c17a9e1bd77cab11385f475da756f76a96b922e4c7b8f7c7"
    }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 4.4 链节点信息同步

骨干节点将链的信息同步到主链，需要两步操作：3.2.1获取链节点信息同步交易blob；3.2.2链节点信息同步提交。节点有新增和修改时需要同步至主链。

#### 4.4.1  获取链节点信息同步blob

接口说明：

节点信息同步时需要将信息同步到链上

请求参数：

| **字段名**                 | **类型**    | 是否必填 | **描述**                                                     |
| :------------------------- | :---------- | :------- | ------------------------------------------------------------ |
| chainCode                  | String(4)   | 是       | AC号                                                         |
| gatewayAddress             | String(64)  | 是       | 链网关地址（在平台链详情中查看该地址），如：**did:bid:efb8Ehu9SGVCTm2BbvpPiQjzNWo4LQAA** |
| nodeList                   | List        | 是       | 节点对象集合                                                 |
| nodeList.nodeAddress       | String(128) | 是       | 节点地址（BID格式，即对子链原节点地址映射为BID格式），例如壹诺区块链华东节点地址：adxSdh5znTeNgjLNoX1e4CuHJoC9hDUbgMrtf，需要映射一个新的BID格式: did:bid:ynjr:ef6bRF4FDPzjvatNmFu6YAvegBtcv86r |
| nodeList.name              | String(128) | 是       | 节点名称                                                     |
| nodeList.createTime        | Long        | 是       | 节点申请时间 （Unix时间戳 毫秒）                             |
| nodeList.bindNodeBid       | String(64)  | 是       | 所属骨干节点bid                                              |
| nodeList.owner             | String(64)  | 是       | 该节点归属的企业bid                                          |
| nodeList.location          | Object      | 否       | 节点地理位置                                                 |
| nodeList.location.district | String(32)  | 否       | 区域编码(比如：810008)                                       |
| nodeList.type              | Integer     | 是       | 节点类型：0 业务节点（目前仅支持 业务节点,后续扩展其他类型） |
| nodeList.p2pAddress        | String(64)  | 是       | 节点间p2p通信地址，如：172.10.3.1:49900                      |
| nodeList.serverType        | Integer     | 是       | 服务器类型: 0自建服务器，1云服务                             |
| nodeList.hostIp            | String      | 是       | 节点公网IP，没有公网IP填内网IP                               |
| nodeList.cloudName         | Integer     | 是       | 云类型，0非公有云，1阿里云，2百度云、3金山云、4华为云、5亚马逊云、6微软云、7腾讯云、8京东云、9物理机、100其他云 |
| nodeList.extra             | String      | 否       | 扩展字段                                                     |
| nodeList.extra.nodeType    | Integer     | 否       | 节点类型：0 共识节点  1 观察者节点，当子链架构为cita、fiscobcos时必填 |
| nodeList.extra.group       | String      | 否       | 群组id，当子链架构为fiscobcos、fabric时必填                  |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| blobId     | String   | blodId   |
| txHash     | String   | 交易hash |
| blob       | String   | blod串   |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/nodeInfo/syn/blob
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjIxNTcxMjg0LCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.9tTPWasYgCs1yC3qYJYSjCGvd9Syl1mr6G2EVrKeq2E",
  "params": {
    "chainCode": "ynjr",
    "gatewayAddress": "did:bid:efb8Ehu9SGVCTm2BbvpPiQjzNWo4LQAA",
    "nodeList": [{
      "nodeAddress": "did:bid:ynjr:ef6bRF4FDPzjvatNmFu6YAvegBtcv86r",
      "name": "业务节点1",
      "createTime": 1593338217000,
      "bindNodeBid": "did:bid:by01:WWBVgCnREaLjegUvkpeiY8BaiAojvcv1b",
      "owner": "did:bid:zg01:WWBVgCnREaLjegUvkpeiY8BaiAojvcv11",
      "location": {
        "district": "810008"
      },
      "type": 1,
      "p2pAddress": "127.0.0.1:18001",
      "serverType": 0,
      "hostIp": "192.168.1.12",
      "cloudName": 1
    }]
  }
}

```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "blobId": "15",
    "txHash": "d60c15bacbb7f98f00921d5ad17d49fafe0c65ea706d5d4ced25e490ac35e9e0",
    "blob": "0A286469643A6269643A65663477776B727A53564650545A4A627257433439446B416554535169377241101E228304080712286469643A6269643A656662384568753953475643546D32426276705069516A7A4E576F344C51414152D4030A286469643A6269643A6566427070397753676564315A5A72437566426D36374A4C55635255396375321AA7037B226D6574686F64223A227375626D6974426C6F636B486561646572222C22706172616D73223A7B22626C6F636B486561646572223A7B226163636F756E745F747265655F68617368223A22222C22636C6F73655F74696D65223A2231353933333338343030303032303030222C22636F6E73656E7375735F76616C75655F68617368223A22222C22666565735F68617368223A22222C2268617368223A2235323134303538363966626533633132396237383434383232333566616365633435656438653432313666633834653962623665336136386661653430636232222C2270726576696F75735F68617368223A2231323334363966626533633132396237383434383232333566616365633435656438653233313435363537646161313233343131326136386661653430636232222C22736571223A223938222C2274785F636F756E74223A223132222C2276616C696461746F72735F68617368223A22696E7465726E616C436861696E496432222C2276657273696F6E223A22227D2C22636861696E436F6465223A2262793031222C2264796E616D6963496E666F223A7B7D7D7D3080EAADE907"
  }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

#### 4.4.2 子链节点信息提交（上链）

接口说明：

用于节点信息同步提交至主链并上链备案。

请求参数：

| **字段名**           |    **类型** | **是否必填** | **描述**                           |
| :------------------- | ----------: | :----------- | :--------------------------------- |
| blobId               |  String(20) | 是           | Blob序号，通过获取交易Blob接口获取 |
| signerList           |        List | 是           | 签名者列表（注意：需要链网关签名） |
| signerList.signBlob  | String(128) | 是           | 签名串（链网关签名）               |
| signerList.publicKey | String(128) | 是           | 签名者公钥（链网关公钥）           |

返回数据：

| **字段名** | **类型** | **描述**                                    |
| :--------- | :------- | :------------------------------------------ |
| txHash     | String   | 链上交易hash,（可用于查询上链交易最终状态） |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/nodeInfo/syn/submit
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjE5MzQyNDIyLCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.3gcQQBqvqtZH1q-TfUiGne68R1TnBcvLAA6nIJ8qDGU",
  "params": {
    "blobId": "1",
    "signerList": [{
      "signBlob": "BDE955FF0A0F52A39E95815962836D13172D6EF92C55A72A97ECCF2CE882E2E2EAC73FA1A624D73C959E18126B4A35947375621D819F539DDC0692EEF0F4D702",
      "publicKey": "b06566086febd16adf6553cc9c000dba5f9f410d34f8541392303561dddc045b15ef4c"
    }]
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
    "errorCode": "0",
    "message": "操作成功",
      "data": {
      "txHash": "083b797ea00204d2c17a9e1bd77cab11385f475da756f76a96b922e4c7b8f7c7"
    }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 4.5 链节点运行状态同步

接口说明：

骨干节点将链下节点运行状态到主链。（每隔5分钟同步一次）

请求参数：

| 字段名                            | 类型       | 是否必填 | 描述                                                         |
| :-------------------------------- | :--------- | :------- | ------------------------------------------------------------ |
| chainCode                         | String(4)  | 是       | AC号                                                         |
| nodeRunningList                   | List       | 是       | 运行状态信息对象                                             |
| nodeRunningList.nodeAddress       | String(64) | 是       | 节点地址（BID格式，即对子链原节点地址映射为BID格式），例如壹诺区块链华东节点地址：adxSdh5znTeNgjLNoX1e4CuHJoC9hDUbgMrtf，需要映射一个新的BID格式: did:bid:ynjr:ef6bRF4FDPzjvatNmFu6YAvegBtcv86r |
| nodeRunningList.status            | Integer    | 是       | 节点运行状况，1：正常， 2：故障                              |
| nodeRunningList.delay             | Long       | 是       | 节点响应时延, 单位毫秒                                       |
| nodeRunningList.upTraffic         | Long       | 是       | 上行流量, 单位字节                                           |
| nodeRunningList.downTraffic       | Long       | 是       | 下行流量, 单位字节                                           |
| nodeRunningList.latestBlockHeight | Long       | 是       | 最新区块高度                                                 |
| nodeRunningList.latestBlockHash   | String(64) | 是       | 最新区块哈希                                                 |
| nodeRunningList.latestTxNum       | Long       | 是       | 最新交易总量                                                 |
| nodeRunningList.latestAccountNum  | Long       | 是       | 最新账户总数                                                 |
| nodeRunningList.unblockedTxNum    | Long       | 是       | 实时未打包交易数                                             |
| nodeRunningList.cpuUsage          | Integer    | 是       | 节点处理器使用率，为百分数整数，如78%请填写78                |
| nodeRunningList.memoryUsage       | Integer    | 是       | 节点内存使用率，为百分数整数，如78%请填写78                  |
| nodeRunningList.diskUsage         | Integer    | 是       | 节点磁盘使用率，为百分数整数，如78%请填写78                  |
| nodeRunningList.memoryUsed        | Long       | 是       | 节点内存占用，单位字节，比如：占用3G，请填写 3221225472      |
| nodeRunningList.diskUsed          | Long       | 是       | 节点磁盘占用总大小，单位字节，比如：占用3G，请填写3221225472 |
| nodeRunningList.ledgerDBUsed      | Long       | 是       | 节点的账本数据库磁盘占用大小，单位字节，比如：占用3G，请填写 3221225472 |
| nodeRunningList.accountDBUsed     | Long       | 是       | 节点的账户数据库磁盘占用大小，单位字节，比如：占用3G，请填写 3221225472 |
| nodeRunningList.kvDBUsed          | Long       | 是       | 节点的kv数据库磁盘占用大小，单位字节，比如：占用3G，请填写 3221225472 |
| nodeRunningList.loadavg           | Long       | 是       | 一分钟内的系统平均负载值，由于原始值为保留2位小数的浮点值，这里扩大100提供整数,如，原始值是20.88 请填写 2088 |
| nodeRunningList.diskRead          | Long       | 是       | 磁盘实时读取速度，单位字节每秒，如：3M/s 请填写 3145728      |
| nodeRunningList.diskWrite         | Long       | 是       | 磁盘实时写入速度，单位字节每秒，如：3M/s 请填写 3145728      |
| nodeRunningList.tcpCount          | Integer    | 是       | tcp链接总数                                                  |
| nodeRunningList.time              | Long       | 是       | 节点状态上报的时间戳，目前节点每分钟上报一次信息             |
| nodeRunningList.extra             | String     | 否       | 额外数据，（数据格式JSON字符串）                             |
| nodeRunningList.extra.group       | String     | 否       | 群组，当子链架构为xuperchain时必填                           |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | Integer  | 错误码   |
| message    | String   | 错误描述 |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/nodeRunningStatus/syn
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjIxNTEzODE4LCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.kqeuSumhvdHLnrvw0WkWxrY3Cy6bThVKBmv7h2-a1po",
  "params": {
    "chainCode": "ynjr",
    "nodeRunningList": [{
      "nodeAddress": "did:bid:ynjr:ef6bRF4FDPzjvatNmFu6YAvegBtcv86r",
      "status": 1,
      "delay": 300,
      "upTraffic": 10000,
      "downTraffic": 1000,
      "latestBlockHeight": 1000,
      "latestBlockHash": "71a4abdc812bb6d94c56e3a2a849250747a76dbab5559f6fc89338876d4b879f",
      "latestTxNum": 1000,
      "latestAccountNum": 1000,
      "unblockedTxNum": 1000,
      "cpuUsage": 10,
      "memoryUsage": 30,
      "diskUsage": 5,
      "memoryUsed": 1048576,
      "diskUsed": 104857600,
      "ledgerDBUsed": 1024,
      "accountDBUsed": 100,
      "kvDBUsed": 1024,
      "loadavg": 70,
      "diskRead": 1024,
      "diskWrite": 1024,
      "tcpCount": 10,
      "time": 1593338217000,
      "extra": "{}"
    }]
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
    "errorCode": "0",
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 4.6 链的区块信息同步

骨干节点将链的区块信息同步到主链锚定，需要两步操作：3.4.1获取链的区块信息同步blob ；3.4.2链的区块信息提交（上链）。（要求每隔5分钟同步一次数据。）

#### 4.6.1 获取链的区块信息同步blob

接口说明：

获取链的区块信息同步blob

请求参数：

| **字段名**                  |    **类型** | 是否必填 | **描述**                                                     |
| :-------------------------- | ----------: | :------- | ------------------------------------------------------------ |
| chainCode                   |   String(4) | 是       | AC号                                                         |
| gatewayAddress              |  String(64) | 是       | 链网关地址（在平台链详情中查看该地址），如：**did:bid:efb8Ehu9SGVCTm2BbvpPiQjzNWo4LQAA** |
| blockList                   |        List | 是       | 区块头信息列表                                               |
| blockList.height            |        Long | 是       | 区块高度                                                     |
| blockList.nodeAddress       |  String(64) | 是       | 出块节点地址（BID格式，即对子链原节点地址映射为BID格式），例如壹诺区块链华东节点地址：adxSdh5znTeNgjLNoX1e4CuHJoC9hDUbgMrtf，需要映射一个新的BID格式: did:bid:ynjr:ef6bRF4FDPzjvatNmFu6YAvegBtcv86r |
| blockList.nodeName          | String(128) | 是       | 出块节点名称                                                 |
| blockList.txNum             |        Long | 是       | 区块交易数                                                   |
| blockList.preBlockHash      |  String(64) | 否       | 上一个区块hash                                               |
| blockList.blockHash         |  String(64) | 是       | 区块hash                                                     |
| blockList.time              |        Long | 是       | 区块生成时间(Unix时间戳 毫秒)                                |
| blockList.size              |        Long | 是       | 区块大小，单位字节                                           |
| blockList.internalChainId   | String(128) | 否       | 标记链内子链id，用于兼容channel字段(Fabric链)、chainID字段(XuperChain链)、grpId字段(FISCO BCOS链)等，需要全局唯一且不可修改，默认为空字符串 |
| blockList.extra             |      Object | 否       | 扩展字段                                                     |
| blockList.extra.channelName |      String | 否       | 通道名称，当子链架构为fabric时必填                           |
| blockList.extra.chainName   |      String | 否       | 平行链名称，当子链架构为xuperchain时必填                     |

返回数据：

| **字段名** | **类型** | **描述**     |
| :--------- | :------- | :----------- |
| blobId     | String   | blodId       |
| txHash     | String   | 链上交易hash |
| blod       | String   | blod串       |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/block/syn/blob

{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjIxNTcxMjg0LCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.9tTPWasYgCs1yC3qYJYSjCGvd9Syl1mr6G2EVrKeq2E",
  "params": {
    "chainCode": "ynjr",
    "gatewayAddress": "did:bid:efb8Ehu9SGVCTm2BbvpPiQjzNWo4LQAA",
    "blockList": [{
      "height": 98,
      "nodeAddress": "did:bid:ynjr:ef6bRF4FDPzjvatNmFu6YAvegBtcv86r",
      "nodeName": "节点2",
      "txNum": 12,
      "preBlockHash": "123469fbe3c129b784482235facec45ed8e23145657daa1234112a68fae40cb2",
      "blockHash": "521405869fbe3c129b784482235facec45ed8e4216fc84e9bb6e3a68fae40cb2",
      "time": 1593338400002,
      "size": 1282,
      "internalChainId":"internalChainId2",
      "extra":"extra2"
    }]
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "blobId": "15",
    "txHash": "d60c15bacbb7f98f00921d5ad17d49fafe0c65ea706d5d4ced25e490ac35e9e0",
    "blob": "0A286469643A6269643A65663477776B727A53564650545A4A627257433439446B416554535169377241101E228304080712286469643A6269643A656662384568753953475643546D32426276705069516A7A4E576F344C51414152D4030A286469643A6269643A6566427070397753676564315A5A72437566426D36374A4C55635255396375321AA7037B226D6574686F64223A227375626D6974426C6F636B486561646572222C22706172616D73223A7B22626C6F636B486561646572223A7B226163636F756E745F747265655F68617368223A22222C22636C6F73655F74696D65223A2231353933333338343030303032303030222C22636F6E73656E7375735F76616C75655F68617368223A22222C22666565735F68617368223A22222C2268617368223A2235323134303538363966626533633132396237383434383232333566616365633435656438653432313666633834653962623665336136386661653430636232222C2270726576696F75735F68617368223A2231323334363966626533633132396237383434383232333566616365633435656438653233313435363537646161313233343131326136386661653430636232222C22736571223A223938222C2274785F636F756E74223A223132222C2276616C696461746F72735F68617368223A22696E7465726E616C436861696E496432222C2276657273696F6E223A22227D2C22636861696E436F6465223A2262793031222C2264796E616D6963496E666F223A7B7D7D7D3080EAADE907"
  }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

#### 4.6.2 链的区块信息提交（上链）

接口说明：

用于子链的区块信息提交至主链并上链存储

请求参数：

| **字段名**           |    **类型** | **是否必填** | **描述**                           |
| :------------------- | ----------: | :----------- | :--------------------------------- |
| blobId               |  String(20) | 是           | blobId(4.6.1返回blobId)            |
| signerList           |        List | 是           | 签名者列表（注意：需要链网关签名） |
| signerList.signBlob  | String(128) | 是           | 签名串                             |
| signerList.publicKey | String(128) | 是           | 签名者公钥                         |

返回数据：

| **字段名** | **类型** | **描述**                                    |
| :--------- | :------- | :------------------------------------------ |
| txHash     | String   | 链上交易hash,（可用于查询上链交易最终状态） |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/block/syn/submit
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjE5MzQyNDIyLCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.3gcQQBqvqtZH1q-TfUiGne68R1TnBcvLAA6nIJ8qDGU",
  "params": {
    "blobId": "3",
    "signerList": [{
      "signBlob": "4D47D46AA467C8DF8FD1D60374D9D2378AC536612C3B84E1AC9BA4EA55E10810482EC634EADED63A389F82573AB1E055698189F7497DF23C5E32CF26A7D8270F",
      "publicKey": "b06566086febd16adf6553cc9c000dba5f9f410d34f8541392303561dddc045b15ef4c"
    }]
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
    "errorCode": "0",
    "message": "操作成功",
      "data": {
      "txHash": "083b797ea00204d2c17a9e1bd77cab11385f475da756f76a96b922e4c7b8f7c7"
    }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 4.7 交易信息同步

接口说明：

用于将子链的交易上报到主链（要求每隔5分钟同步一次，将5分钟内的交易都同步上来）

请求参数：

| **字段名**             |      **类型** | **是否必填** | **描述**                                                     |
| :--------------------- | ------------: | :----------- | :----------------------------------------------------------- |
| chainCode              |     String(4) | 是           | AC号                                                         |
| txList                 |          List | 是           | 交易列表                                                     |
| txList.hash            |    String(64) | 是           | 交易hash                                                     |
| txList.content         |        String | 是           | 交易内容 JSON字符串 或者交易内容hash                         |
| txList.time            |          Long | 是           | 交易时间（Unix时间戳 毫秒）                                  |
| txList.height          |          Long | 是           | 交易高度                                                     |
| txList.status          |     String(2) | 是           | 交易状态，0成功，1失败                                       |
| txList.txSign          | String(65535) | 是           | 交易签名                                                     |
| txList.contractBid     |   String(128) | 否           | 合约地址，合约调用交易为调用的合约地址，非合约调用交易留空   |
| txList.size            |          Long | 是           | 交易大小，单位字节                                           |
| txList.internalChainId |   String(128) | 否           | 链内子链id，默认留空。如Fabric链填写channel，XuperChain链填写chainID，FISCO链填写grpId |
| txList.extra           |        String | 否           | 额外数据                                                     |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | Integer  | 错误码   |
| message    | String   | 错误描述 |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/tx/syn

{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjIxNTEzODE4LCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.kqeuSumhvdHLnrvw0WkWxrY3Cy6bThVKBmv7h2-a1po",
  "params": {
    "chainCode":"by01",
    "txList": [{
      "hash": "71a4abdc812bb6d94c56e3a2a849250747a76dbab5559f6fc89338876d4b879f",
      "content": "a6f9f1e8a18674ac915932e1d700b12b676d22b542f952d0d0f96517f9f5f806",
      "time": 1593338217000,
      "height": 10,
      "status":"0",
      "txSign":"{sign:1234}",
      "contractBid":"0",
      "size":100,
      "internalChainId": "",
      "extra": "123"
    }]
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
    "errorCode": "0",
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 4.8 合约信息同步

骨干节点将合约信息同步到主链，便于备案审查。需要两步操作：3.6.1获取合约信息同步blob ；3.6.2合约信息同步提交（上链）。要求每隔5分钟同步一次数据。

#### 4.8.1 获取合约信息同步blob

接口说明：

获取合约信息同步blob。

请求参数：

| **字段名**                             |    **类型** | 是否必填 | **描述**                                                     |
| :------------------------------------- | ----------: | :------- | ------------------------------------------------------------ |
| chainCode                              |   String(4) | 是       | 链AC号                                                       |
| gatewayAddress                         |  String(64) | 是       | 链网关地址（在平台链详情中查看该地址），如：**did:bid:efb8Ehu9SGVCTm2BbvpPiQjzNWo4LQAA** |
| smartContractList                      |        List | 是       | 合约列表                                                     |
| smartContractList.ownerAddress         |  String(64) | 是       | 合约拥有人BID（BID格式）                                     |
| smartContractLis.contractAddress       |  String(64) | 是       | 合约地址（BID格式，即对子链原合约地址映射为BID格式），例如原合约地址：adxSBrVVtaWZVuGxGGPZCmyna7C6dKo3WVXd4，需要映射一个新的BID格式: did:bid:ynjr:did:bid:efv7DynieC5sF1UoKAthKTfUCkuzaBfR |
| smartContractList.time                 |        Long | 是       | 合约创建时间（Unix时间戳 毫秒）                              |
| smartContractList.name                 | String(128) | 否       | 合约名称                                                     |
| smartContractList.contractType         |     Integer | 是       | 合约类型，0 :普通合约， 1：主子链管理系统合约， 2：DPOS系统合约 3：部署系统合约 4：认证系统合约 5：隐私保护系统合约 6：跨链系统合约 7：BID注册合约 |
| smartContractList.industryType         |   String(2) | 是       | 行业类型，参考 证件及行业类型                                |
| smartContractList.payload              |    String() | 是       | 合约代码                                                     |
| smartContractList.vm                   |  String(16) | 是       | 合约引擎                                                     |
| smartContractList.internalChainId      | String(128) | 否       | 标记链内子链id，如Fabric链填写channel，XuperChain链填写chainID，FISCO链填写grpId等，需要全局唯一且不可修改，默认为空字符串 |
| smartContractList.version              |  String(16) | 是       | 合约版本                                                     |
| smartContractList.extra                |      String | 否       | 扩展字段,json字符串                                          |
| smartContractList.extra.identification |      String | 否       | 链码标识，当子链架构为fabric时必填                           |
| smartContractList.extra.nodeCnt        |        Long | 否       | 安装节点数，当子链架构为fabric时必填                         |
| smartContractList.extra.channelCnt     |        Long | 否       | 部署通道数，当子链架构为fabric时必填                         |
| smartContractList.extra.address        |      String | 否       | 合约地址，当子链架构为xuperchain时必填                       |
| smartContractList.extra.creator        |      String | 否       | 合约创建账号，当子链架构为xuperchain时必填                   |
| smartContractList.extra.type           |      String | 否       | 合约类型， wasm合约、native合约，当子链架构为xuperchain时必填 |
| smartContractList.extra.chainName      |      String | 否       | 平行链名称，当子链架构为xuperchain时必填                     |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| blobId     | String   | blodId   |
| txHash     | String   | 交易hash |
| blob       | String   | blob串   |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/contract/syn/blob
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjIxNTcxMjg0LCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.9tTPWasYgCs1yC3qYJYSjCGvd9Syl1mr6G2EVrKeq2E",
  "params": {
    "chainCode": "ynjr",
    "gatewayAddress": "did:bid:efb8Ehu9SGVCTm2BbvpPiQjzNWo4LQAA",
    "smartContractList": [{
      "ownerAddress":"did:bid:efPGyex2nVvTJZPg8RzdtzEqF5UWCzdL22",
      "contractAddress": "did:bid:ynjr:did:bid:efv7DynieC5sF1UoKAthKTfUCkuzaBfR",
      "time": 1593338217002,
      "name": "部署系统合约2",
      "contractType": 2,
      "industryType": "A",
      "payload": "'use strict'; function init(){return;}  function main(){return;}",
      "vm": "v2",
      "version":"1.0.2",
      "extra":"extra2",
      "internalChainId":"internalChainId2"
    }]
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "blobId": "15",
    "txHash": "d60c15bacbb7f98f00921d5ad17d49fafe0c65ea706d5d4ced25e490ac35e9e0",
    "blob": "0A286469643A6269643A65663477776B727A53564650545A4A627257433439446B416554535169377241101E228304080712286469643A6269643A656662384568753953475643546D32426276705069516A7A4E576F344C51414152D4030A286469643A6269643A6566427070397753676564315A5A72437566426D36374A4C55635255396375321AA7037B226D6574686F64223A227375626D6974426C6F636B486561646572222C22706172616D73223A7B22626C6F636B486561646572223A7B226163636F756E745F747265655F68617368223A22222C22636C6F73655F74696D65223A2231353933333338343030303032303030222C22636F6E73656E7375735F76616C75655F68617368223A22222C22666565735F68617368223A22222C2268617368223A2235323134303538363966626533633132396237383434383232333566616365633435656438653432313666633834653962623665336136386661653430636232222C2270726576696F75735F68617368223A2231323334363966626533633132396237383434383232333566616365633435656438653233313435363537646161313233343131326136386661653430636232222C22736571223A223938222C2274785F636F756E74223A223132222C2276616C696461746F72735F68617368223A22696E7465726E616C436861696E496432222C2276657273696F6E223A22227D2C22636861696E436F6465223A2262793031222C2264796E616D6963496E666F223A7B7D7D7D3080EAADE907"
  }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

#### 4.8.2 合约信息同步提交（上链）

接口说明：

用于子链合约信息同步提交至主链并上链存储

请求参数：

| **字段名**           |    **类型** | **是否必填** | **描述**       |
| :------------------- | ----------: | :----------- | :------------- |
| blobId               |  String(20) | 是           | blobId         |
| signerList           |        List | 是           | 签名者列表     |
| signerList.signBlob  | String(128) | 是           | 签名串         |
| signerList.publicKey | String(128) | 是           | 合约发布者公钥 |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| txHash     | String   | 交易hash |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/contract/syn/submit
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjE5MzQyNDIyLCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.3gcQQBqvqtZH1q-TfUiGne68R1TnBcvLAA6nIJ8qDGU",
  "params": {
    "blobId": "4",
    "signerList": [{
      "signBlob": "5EC044231995FD4AF61CCA0AA7BA2479021509E48F3E3C061ED3CDB1CFECC79FCFE1BD28571236E55F72BA1260A8707CAF2D1D104D79A9218EBCCA9225066B06",
      "publicKey": "b06566086febd16adf6553cc9c000dba5f9f410d34f8541392303561dddc045b15ef4c"
    }]
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
    "errorCode": "0",
    "message": "操作成功",
      "data": {
      "txHash": "083b797ea00204d2c17a9e1bd77cab11385f475da756f76a96b922e4c7b8f7c7"
    }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 4.9 链智能合约调用信息同步

接口说明：

用于子链合约调用记录上报到主链（要求每5分钟同步一次。将5分钟内的合约调用记录数据同步上来）

请求参数：

| **字段名**                               |    **类型** | **是否必填** | **描述**                                                     |
| :--------------------------------------- | ----------: | :----------- | :----------------------------------------------------------- |
| chainCode                                |   String(4) | 是           | 子链AC号                                                     |
| contractCallList                         |        List | 是           | 数据数组                                                     |
| contractCallList.bid                     |  String(64) | 是           | 智能合约调用者账户地址                                       |
| contractCallList.time                    |        Long | 是           | 调用时间（Unix 时间戳 毫秒）                                 |
| contractCallList.contractBid             |  String(64) | 是           | 智能合约地址（BID格式，即对子链原合约地址映射为BID格式），例如原合约地址：adxSBrVVtaWZVuGxGGPZCmyna7C6dKo3WVXd4，需要映射一个新的BID格式: did:bid:ynjr:did:bid:efv7DynieC5sF1UoKAthKTfUCkuzaBfR |
| contractCallList.name                    |  String(64) | 否           | 合约调用者名称                                               |
| contractCallList.internalChainId         | String(128) | 否           | 链内子链id，如Fabric链填写channel，XuperChain链填写chainID，FISCO链填写grpId等，需要全局唯一且不可修改，默认为空字符串 |
| contractCallList.extra                   |      Object | 否           | 额外数据                                                     |
| contractCallList.extra.org               |      String | 否           | 调用组织，当子链架构为fabric时必填                           |
| contractCallList.extra.grp               |      String | 否           | 调用群组，当子链架构为xuperchain时必填                       |
| contractCallList.extra.invokeGroup       |      String | 否           | 调用群组，当子链架构为fiscobcos时必填                        |
| contractCallList.extra.invokeCertificate |      String | 否           | 调用证书，当子链架构为fiscobcos和cita时必填                  |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | Integer  | 错误码   |
| message    | String   | 错误描述 |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/contractCall/syn
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjIxNTk0NDQ4LCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.MxWdV-mwCScUq8QwmTXTaxKli8jvXq1sNVvAYQWWsSo",
    "params": {
        "chainCode": "ynjr",
        "contractCallList": [{
            "bid": "bid:bid:ynjr:hHTsFXTTqQfVH1oThGEKf6iwCK2c31Bes",
            "name": "aaa",
            "contractBid": "did:bid:ynjr:did:bid:efv7DynieC5sF1UoKAthKTfUCkuzaBfR",
            "time": 1593338217000,
            "internalChainId": "",
            "extra": "{}"
        }]
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
    "errorCode": "0",
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 4.10 链智能合约部署信息同步

接口说明：

用于子链合约部署记录上报到主链（要求每5分钟同步一次。将5分钟内的合约部署记录数据同步上来）

请求参数：

| **字段名**                           |    **类型** | **是否必填** | **描述**                                                     |
| :----------------------------------- | ----------: | :----------- | :----------------------------------------------------------- |
| chainCode                            |   String(4) | 是           | 子链AC号                                                     |
| contractDeployList                   |        List | 是           | 合约部署记录                                                 |
| contractDeployList.bid               |  String(64) | 是           | 智能合约地址（BID格式，即对子链原合约地址映射为BID格式），例如原合约地址：adxSBrVVtaWZVuGxGGPZCmyna7C6dKo3WVXd4，需要映射一个新的BID格式: did:bid:ynjr:did:bid:efv7DynieC5sF1UoKAthKTfUCkuzaBfR |
| contractDeployList.time              |        Long | 是           | 部署时间 （Unix时间戳 毫秒）                                 |
| contractDeployList.contractName      |  String(64) | 否           | 智能合约名称                                                 |
| contractDeployList.version           |  String(16) | 是           | 智能合约版本                                                 |
| contractDeployList.operateType       |     Integer | 是           | 合约操作类型，0：第一次部署  1：合约升级                     |
| contractDeployList.internalChainId   | String(128) | 否           | 链内子链id，如Fabric链填写channel，XuperChain链填写chainID，FISCO链填写grpId等，需要全局唯一且不可修改，默认为空字符串 |
| contractDeployList.extra             |      Object | 否           | 额外数据                                                     |
| contractDeployList.extra.channelName |      String | 否           | 通道名称，当子链架构为fabric时必填                           |
| contractDeployList.extra.nodeName    |      String | 否           | 节点名称，当子链架构为fabric时必填                           |
| contractDeployList.extra.status      |      String | 否           | 状态，当子链架构为fabric时必填                               |
| contractDeployList.extra.chainName   |      String | 否           | 平行链名称，当子链架构为xuperchain时必填                     |
| contractDeployList.extra.status      |      String | 否           | 状态，当子链架构为xuperchain时必填                           |
| contractDeployList.extra.grpName     |      String | 否           | 群组名称，当子链架构为fiscobcos时必填                        |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | Integer  | 错误码   |
| message    | String   | 错误描述 |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/contractDeploy/syn
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjIxNTk0NDQ4LCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.MxWdV-mwCScUq8QwmTXTaxKli8jvXq1sNVvAYQWWsSo",
    "params": {
        "chainCode": "ynjr",
        "contractDeployList": [{
            "bid": "did:bid:ynjr:did:bid:efv7DynieC5sF1UoKAthKTfUCkuzaBfR",
            "contractName": "跨链合约",
            "time": 1593338217000,
            "version": "1.0",
            "operateType": 0,
            "internalChainId": "",
            "extra": "{}"
        }]
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
    "errorCode": "0",
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 4.11 未打包的交易数同步

接口说明：

每隔一小时同步一次，同步最近24小时每小时未打包交易数，按整点取值。

请求参数：

| **字段名**               |  **类型** | **是否必填** | **描述**                   |
| :----------------------- | --------: | :----------- | :------------------------- |
| chainCode                | String(4) | 是           | AC号                       |
| txNumList                |      List | 是           | 每个小时未打包交易总数列表 |
| txNumList.time           |      Long | 是           | 整点时间戳 （单位 毫秒）   |
| txNumList.unblockedTxNum |      Long | 是           | 未打包的交易数             |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | Integer  | 错误码   |
| message    | String   | 错误描述 |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/unblockedTxNum/syn
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjIxNTEzODE4LCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.kqeuSumhvdHLnrvw0WkWxrY3Cy6bThVKBmv7h2-a1po",
  "params": {
    "chainCode": "by01",
    "txNumList": [
      {"time":1600128000000,"unblockedTxNum":10},
      {"time":1600124400000,"unblockedTxNum":10},
      {"time":1600131400000,"unblockedTxNum":14},
      {"time":1600041600000,"unblockedTxNum":10}
    ]
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
    "errorCode": "0",
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 4.12 底层链节点故障日志同步

接口说明：

用于底层链节点故障信息上报到主链（要求每5分钟同步一次。将5分钟内的底层链节点故障日志数据同步上来）

请求参数：

| **字段名**               |   **类型** | **是否必填** | **描述**                                                     |
| :----------------------- | ---------: | :----------- | :----------------------------------------------------------- |
| chainCode                |  String(4) | 是           | 子链AC号                                                     |
| nodeFaultList            |       List | 是           | 节点故障数据                                                 |
| nodeFaultList.id         | String(32) | 是           | 上报数据索引号（子链上唯一，建议用UUID再进行MD5运算生成的字符串） |
| nodeFaultList.time       |       Long | 是           | 故障发生时间(Unix时间戳 毫秒）                               |
| nodeFaultList.bid        | String(64) | 是           | 节点地址（bid格式）                                          |
| nodeFaultList.type       |    Integer | 是           | 标示产生或者恢复日志，1：产生， 2：恢复                      |
| nodeFaultList.faultType  |    Integer | 是           | 故障类型，1：区块停止产生， 2：节点已宕机                    |
| nodeFaultList.faultLevel |    Integer | 是           | 故障等级，1：严重 2：一般 3：轻微，                          |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | Integer  | 错误码   |
| message    | String   | 错误描述 |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/nodeFault/syn
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjIxNTk0NDQ4LCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.MxWdV-mwCScUq8QwmTXTaxKli8jvXq1sNVvAYQWWsSo",
    "params": {
        "chainCode": "by02",
        "nodeFaultList": [{
            "id": 1,
            "time": 1593338217000,
            "bid": "did:bid:WWBVgCnREaLjegUvkpeiY8BaiAojvcv1b",
            "type": 2,
            "faultType": 1,
            "faultLevel": 2
        }]
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
    "errorCode": "0",
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 4.13 获取区域经纬度

接口说明：

用于获取区域的经纬度信息

请求参数：

| **字段名** |  **类型** | **是否必填** | **描述**                 |
| :--------- | --------: | :----------- | :----------------------- |
| pcode      | String(8) | 否           | 父级编码(为空获取所有省) |

返回数据：

| **字段名**         | **类型** | **描述** |
| :----------------- | :------- | :------- |
| errorCode          | Integer  | 错误码   |
| message            | String   | 错误描述 |
| areaList.name      | String   | 区域名称 |
| areaList.adcode    | String   | 区域编码 |
| areaList.longitude | String   | 经度     |
| areaList.latitude  | String   | 纬度     |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/chain/nodeFault/syn
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjIxNTk0NDQ4LCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.MxWdV-mwCScUq8QwmTXTaxKli8jvXq1sNVvAYQWWsSo",
    "params": {
        "pcode":"510000"
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "areaList": [
      {
        "name": "巴中市",
        "adcode": "511900",
        "longitude": "106.753669",
        "latitude": "31.858809"
      },
      {
        "name": "成都市",
        "adcode": "510100",
        "longitude": "104.065735",
        "latitude": "30.659462"
      },
      {
        "name": "广元市",
        "adcode": "510800",
        "longitude": "105.829757",
        "latitude": "32.433668"
      },
      {
        "name": "南充市",
        "adcode": "511300",
        "longitude": "106.082974",
        "latitude": "30.795281"
      },
      {
        "name": "德阳市",
        "adcode": "510600",
        "longitude": "104.398651",
        "latitude": "31.127991"
      },
      {
        "name": "绵阳市",
        "adcode": "510700",
        "longitude": "104.741722",
        "latitude": "31.46402"
      },
      {
        "name": "遂宁市",
        "adcode": "510900",
        "longitude": "105.571331",
        "latitude": "30.513311"
      },
      {
        "name": "资阳市",
        "adcode": "512000",
        "longitude": "104.641917",
        "latitude": "30.122211"
      },
      {
        "name": "广安市",
        "adcode": "511600",
        "longitude": "106.633369",
        "latitude": "30.456398"
      },
      {
        "name": "内江市",
        "adcode": "511000",
        "longitude": "105.066138",
        "latitude": "29.58708"
      },
      {
        "name": "达州市",
        "adcode": "511700",
        "longitude": "107.502262",
        "latitude": "31.209484"
      },
      {
        "name": "眉山市",
        "adcode": "511400",
        "longitude": "103.831788",
        "latitude": "30.048318"
      },
      {
        "name": "自贡市",
        "adcode": "510300",
        "longitude": "104.773447",
        "latitude": "29.352765"
      },
      {
        "name": "泸州市",
        "adcode": "510500",
        "longitude": "105.443348",
        "latitude": "28.889138"
      },
      {
        "name": "宜宾市",
        "adcode": "511500",
        "longitude": "104.630825",
        "latitude": "28.760189"
      },
      {
        "name": "乐山市",
        "adcode": "511100",
        "longitude": "103.761263",
        "latitude": "29.582024"
      },
      {
        "name": "攀枝花市",
        "adcode": "510400",
        "longitude": "101.716007",
        "latitude": "26.580446"
      },
      {
        "name": "凉山彝族自治州",
        "adcode": "513400",
        "longitude": "102.258746",
        "latitude": "27.886762"
      },
      {
        "name": "雅安市",
        "adcode": "511800",
        "longitude": "103.001033",
        "latitude": "29.987722"
      },
      {
        "name": "阿坝藏族羌族自治州",
        "adcode": "513200",
        "longitude": "102.221374",
        "latitude": "31.899792"
      },
      {
        "name": "甘孜藏族自治州",
        "adcode": "513300",
        "longitude": "101.963815",
        "latitude": "30.050663"
      }
    ]
  }
}

```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

## 5 服务市场

服务市场是为星火·链网网生态内企业提供的区块链应用的市场平台。服务市场联合众多合作伙伴，共同打造区块链应用服务生态，为用户推出一系列有价值的区块链商品服务。企业可开发基于区块链的服务或应用，上架到服务市场进行售卖。服务市场由星火·链网网超级节点共同运营，平等共治。

### 5.1服务商认证申请（暂缓开通）

服务商认证需要用户支付1星火链令，

服务商认证申请需要两步操作，（1）获取服务商认证blob，用于将服务商认证费用上链；（2）用户私钥对blob进行签名，并提交上链。

#### 5.1.1 获取服务商认证blob

接口说明：

获取服务商认证blob。

请求参数：

| **字段名**     |     **类型** | **是否必填** | **描述**                  |
| -------------- | -----------: | ------------ | ------------------------- |
| userBid        |       String | 是           | 用户bid（需要有星火链令） |
| companyName    |  String(128) | 是           | 公司名称                  |
| companyPicUrl  | String(1024) | 是           | 公司海报                  |
| companyWebsite | String(1024) | 是           | 企业网站                  |
| companyAddress | String(1024) | 是           | 公司地址                  |
| linkmanName    |  String(128) | 是           | 商务联系人                |
| linkmanPhone   |   String(20) | 是           | 商务联系人电话            |
| linkmanMail    |  String(128) | 是           | 商务联系人邮箱            |
| applyReason    | String(1024) | 是           | 认证理由                  |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| blobId     | String   | blodId   |
| txHash     | String   | 交易hash |
| blob       | String   | blob串   |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/gpds/service/provider/blob
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjIxNTcxMjg0LCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.9tTPWasYgCs1yC3qYJYSjCGvd9Syl1mr6G2EVrKeq2E",
    "params": {
        "userBid": "did:bid:efUVp9QZuJRQRRWsRU4ZanZukpNcLksL",
        "companyName": "链通科技",
        "companyPicUrl": "/oss/file/get/QmV1Z7syFkw8Tg41RuWChn7Ad3KjwgTexHeJ2LiYEEK2rd",
          "companyWebsite": "http://lt369.com",
        "companyAddress": "北京市朝阳区",
        "linkmanName": "张三",
        "linkmanPhone": "13812348989",
        "linkmanMail": "zhangsan@126.com",
        "applyReason": "申请服务商"
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "blobId": "15",
    "txHash": "d60c15bacbb7f98f00921d5ad17d49fafe0c65ea706d5d4ced25e490ac35e9e0",
    "blob": "0A286469643A6269643A65663477776B727A53564650545A4A627257433439446B416554535169377241101E228304080712286469643A6269643A656662384568753953475643546D32426276705069516A7A4E576F344C51414152D4030A286469643A6269643A6566427070397753676564315A5A72437566426D36374A4C55635255396375321AA7037B226D6574686F64223A227375626D6974426C6F636B486561646572222C22706172616D73223A7B22626C6F636B486561646572223A7B226163636F756E745F747265655F68617368223A22222C22636C6F73655F74696D65223A2231353933333338343030303032303030222C22636F6E73656E7375735F76616C75655F68617368223A22222C22666565735F68617368223A22222C2268617368223A2235323134303538363966626533633132396237383434383232333566616365633435656438653432313666633834653962623665336136386661653430636232222C2270726576696F75735F68617368223A2231323334363966626533633132396237383434383232333566616365633435656438653233313435363537646161313233343131326136386661653430636232222C22736571223A223938222C2274785F636F756E74223A223132222C2276616C696461746F72735F68617368223A22696E7465726E616C436861696E496432222C2276657273696F6E223A22227D2C22636861696E436F6465223A2262793031222C2264796E616D6963496E666F223A7B7D7D7D3080EAADE907"
  }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

#### 5.1.2 服务商认证提交（上链）

接口说明：

用于服务商认证提交

请求参数：

| **字段名**           |    **类型** | **是否必填** | **描述**                         |
| :------------------- | ----------: | :----------- | :------------------------------- |
| blobId               |  String(20) | 是           | blobId(5.1.1返回blobId)          |
| signerList           |        List | 是           | 签名者列表（注意：需要用户签名） |
| signerList.signBlob  | String(128) | 是           | 签名串                           |
| signerList.publicKey | String(128) | 是           | 签名者公钥                       |

返回数据：

| **字段名** | **类型** | **描述**     |
| :--------- | :------- | :----------- |
| applyNo    | String   | 申请编号     |
| txHash     | String   | 链上交易hash |

示例：

（1）请求示例：

```
http请求方式：POST
https://{url}/v1/gpds/service/provider/submit
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjE5MzQyNDIyLCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.3gcQQBqvqtZH1q-TfUiGne68R1TnBcvLAA6nIJ8qDGU",
  "params": {
    "blobId": "3",
    "signerList": [{
      "signBlob": "4D47D46AA467C8DF8FD1D60374D9D2378AC536612C3B84E1AC9BA4EA55E10810482EC634EADED63A389F82573AB1E055698189F7497DF23C5E32CF26A7D8270F",
      "publicKey": "b06566086febd16adf6553cc9c000dba5f9f410d34f8541392303561dddc045b15ef4c"
    }]
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```
{
    "errorCode": "0",
    "message": "操作成功",
      "data": {
      "applyNo":"",
      "txHash": "083b797ea00204d2c17a9e1bd77cab11385f475da756f76a96b922e4c7b8f7c7"
    }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

#### 5.1.3 查询服务商认证结果

接口说明：

查询服务商认证申请结果

请求参数：

| **字段名** | **类型** | **是否必填** | **描述** |
| :--------- | -------: | :----------- | :------- |
| applyNo    |   String | 是           | 申请编号 |


返回数据：

| **字段名**  | **类型** | **描述**                      |
| :---------- | :------- | :---------------------------- |
| applyStatus | String   | 1待审核 2审核不通过 3审核通过 |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/gpds/service/provider/authStatus
{
  "accessToken":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
  "params":{
    "applyNo":"efNY98KmDLXQFvJuJEtMUvFX3fTEvxVS"
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
    "errorCode": 0,
    "message": "操作成功",
    "data": {
        "applyStatus":"1"
    }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 941089,
    "message": "申请编号不存在"
}
```

### 5.2服务上架

用户可以在星火·链网平台申请成为服务商，审核通过后，可以在星火·链网平台上服务市场上架自己的服务（业务管理平台上操作，API接口）

### 5.3 查询应用服务订单数据

接口说明：

针对星火·链网公共数据服平台上架的应用服务，服务商使用此接口可以向主链查询服务商品的售卖情订单数据，服务商需根据订单数据为用户提供对应服务。

请求参数：

| **字段名** | **类型** | **是否必填** | **描述**                                      |
| :--------- | -------: | :----------- | :-------------------------------------------- |
| spu        |   String | 否           | 服务商品ID                                    |
| startTime  |   String | 否           | 订单查询开始时间（格式 2020-12-24 12:18:22）  |
| endTime    |   String | 否           | 订单查询结束时间 （格式 2020-12-25 12:18:22） |
| pageStart  |  Integer | 否           | 开始页 默认1                                  |
| pageSize   |  Integer | 否           | 每页条数 默认100条                            |

返回数据：

| **字段名**              | **类型** | **描述**                                  |
| :---------------------- | :------- | :---------------------------------------- |
| orderList               | List     | 订单列表                                  |
| orderList.orderNo       | String   | 订单编号                                  |
| orderList.buyerBid      | String   | 购买者主链bid                             |
| orderList.spu           | String   | 服务商品ID                                |
| orderList.sku           | String   | 服务商品规格ID                            |
| orderList.skuDescribe   | String   | 商品规格描述                              |
| orderList.orderType     | String   | 订单类型 0首次购买 1续费购买              |
| orderList.serviceName   | String   | 服务名称                                  |
| orderList.serviceRatio  | String   | 服务费率                                  |
| orderList.orderAmount   | String   | 订单总额                                  |
| orderList.arrivalAmount | String   | 到账金额                                  |
| orderList.orderTime     | Long     | 下单时间（时间戳 单位：毫秒）             |
| orderList.paymentTime   | Long     | 支付完成时间时间戳 单位：毫秒）           |
| orderList.orderStatus   | String   | 0支付中，1支付成功，2支付失败，3支付超时' |
| orderList.remark        | String   | 备注                                      |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/gpds/order/list
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params": {
        "spu": "",
        "startTime": "",
        "endTime": "",
        "pageStart": 1,
        "pageSize": 10
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "page": {
      "pageSize": 10,
      "pageStart": 1,
      "pageTotal": 1
    },
    "orderList": [
      {
        "orderNo": "4c8572f4b83a74ec7c0f7a0550f6e713",
        "buyerBid": "did:bid:efiuyquGWTvREsY9mrVoKVhFe5ZJxZdR",
        "serviceNo": "cb5eb6ae5b8c08b75cd1c3a762641868",
        "serviceName": "by01-服务",
        "serviceRatio": "0.22",
        "orderAmount": "12.00",
        "arrivalAmount": "9.36",
        "orderTime": "1610004089000",
        "paymentTime": "1610004109000",
        "orderStatus": "1",
        "remark": ""
      }
    ]
  }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 5.4 同步API类服务商品的授权信息

接口说明：

对于API类服务服务商根据订单号，需要为购买者提供调用API服务的appId、appSecret等相关使用权限的凭证，并将凭证的信息推送给平台，平台会将该信息推给对应购买者。

请求参数：

| **字段名** |    **类型** | **是否必填** | **描述**            |
| :--------- | ----------: | :----------- | :------------------ |
| orderNo    | String(128) | 是           | 订单号              |
| appId      | String(128) | 是           | 访问应用服务的appId |
| appSecret  | String(128) | 是           | appSecret           |


返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | Integer  | 错误码   |
| message    | String   | 错误描述 |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/gpds/sync/appInfo
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params": {
        "orderNo": "4c8572f4b83a74ec7c0f7a0550f6e713",
        "appId": "avCfp7lqMzH5mNNd",
        "appSecret": "68bdc38618c6f8832b9ba1beac0ff763d0f32763"
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
    "errorCode": 0,
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

PS：对后台推送数据时，如果收到平台的应答不是成功，认为推送失败，服务提供商需要通过一定的策略定期重新发起通知，尽可能提高通知的成功率。 （推送频率为15/15/30/180/1800/1800/1800/1800/3600，单位：秒）

### 5.5 查询API类服务商品调用余量

接口说明：

需要服务提供商API调用余量给平台查询

请求参数：

| **字段名** |    **类型** | **是否必填** | **描述**                                                     |
| :--------- | ----------: | :----------- | :----------------------------------------------------------- |
| appId      | String(128) | 是           | 访问API类服务的appId                                         |
| salt       |  String(32) | 是           | 随机串32位                                                   |
| sign       |  String(64) | 是           | 对请求数据的签名串，签名规则如下：appId、salt和secretKey拼接起来的字符串进行sha256后得到的字符串。其中secretKey是在星火·链网网平台录入的 |

返回数据：

| **字段名** | **类型** | **描述**                  |
| :--------- | :------- | :------------------------ |
| allowance  | Long     | 余量（次数、时长-单位秒） |

示例：

（1）请求示例：

```json
http请求方式：POST
https://{url}/api/allowance
{
  "appId": "avCfp7lqMzH5mNNd",
  "salt":"2be9bd7a3434f7038ca27d1918de58bd",
  "sign":"73376126e735e645d555ed111a4990c2" 
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```json
{
  "data":{
    "allowance":20
  }, 
  "errorCode": 0,
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```json
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 5.6 用户统一授权认证（暂缓开通）

统一认证OAuth2.0授权登录让统一认证用户使用统一认证身份安全登录第三方应用或网站，在统一认证用户授权登录已接入统一认证OAuth2.0的第三方应用后，第三方可以获取到用户的接口调用凭证（access_token），通过access_token可以进行统一认证开放平台授权关系接口调用，从而可实现获取统一认证用户基本开放信息和帮助用户实现基础开放功能等

**统一认证流程:**

1. 第三方发起统一认证授权登录请求，统一认证用户允许授权第三方应用后，统一认证会拉起应用或重定向到第三方网站，并且带上授权临时票据code参数；

2. 通过code参数加上api_key和api_secret等，通过API换取bidAccessToken；

3. 通过bidAccessToken进行接口调用，获取用户基本数据资源或帮助用户实现基本操作。

#### 5.6.1请求认证

接口说明：

```plain
http请求方式：GET
https://{url}/oAuth/code?api_key=${appi_key}&redirect_uri=REDIRECT_URI&scope=web&response_type=code
```

请求参数：

| **字段名**    | **类型** | **是否必填** | **描述**                      |
| :------------ | -------: | :----------- | :---------------------------- |
| api_key       |   String | 是           | api_key                       |
| redirect_uri  |   String | 是           | 重定向地址，需要进行UrlEncode |
| response_type |   String | 是           | 返回类型,code                 |
| scope         |   String | 是           | 用户授权的作用域 web          |


返回数据：

用户登录授权后，将会重定向到redirect_uri的网址上，并且带上code参数

返回示例：

```plain
redirect_uri?code=CODE
```

#### 5.6.2通过code获取bidAccessToken

接口说明：

​		通过code获取bidAccessToken

```plain
http请求方式：POST
https://{url}/oAuth/accessToken

Content-Type:application/x-www-form-urlencoded

```

请求参数：

| **字段名** | **类型** | **是否必填** | **描述**             |
| :--------- | -------: | :----------- | :------------------- |
| api_key    |   String | 是           | api_key              |
| api_secret |   String | 是           | api密钥AppSecret     |
| code       |   String | 是           | 5.4.1获取的code      |
| grant_type |   String | 是           | 填authorization_code |


返回数据：

| **字段名**      | **类型** | **描述**                            |
| :-------------- | :------- | :---------------------------------- |
| bidAccessToken  | String   | bid令牌                             |
| refreshBidToken | String   | 刷新票据                            |
| expireIn        | String   | 过期时间 （单位秒 有效期为 7200 s） |
| scope           | String   | 用户授权的作用域                    |

返回示例：

```plain
{
    "data": {
        "bidAccessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhcHBJZCI6IkZDMjFnN2JzMVI2TjJGWjMiLCJpc3MiOiJidW1vIiwiZXhwIjoxNTYxMDAyODIyfQ.hgVH0T6fLxk973U2fIj_ejDx5aJuzRFlg1VAUA2RgzM",
        "expireIn": 7200,
        "refreshBidToken":"",
        "scope":""
    },
    "errorCode": 0,
    "message": "操作成功"
}
```

#### 5.6.3 刷新bidAccessToken

接口说明：

​		由于bidAccessToken拥有较短的有效期，当bidAccessToken超时后，可以使用refreshBidToken进行刷新，当refreshBidToken失效之后，需要用户重新授权。

```plain
http请求方式：POST
https://{url}/oAuth/refreshToken

Content-Type:application/x-www-form-urlencoded

```

请求参数：

| **字段名**      | **类型** | **是否必填** | **描述**             |
| :-------------- | -------: | :----------- | :------------------- |
| api_key         |   String | 是           | api_key              |
| refreshBidToken |   String | 是           | 刷新token票据        |
| grant_type      |   String | 是           | 填authorization_code |


返回数据：

| **字段名**      | **类型** | **描述**                            |
| :-------------- | :------- | :---------------------------------- |
| bidAccessToken  | String   | 接口调用令牌                        |
| refreshBidToken | String   | 刷新票据                            |
| expireIn        | String   | 过期时间 （单位秒 有效期为 7200 s） |
| scope           | String   | 用户授权的作用域                    |

返回示例：

```plain
{
    "data": {
        "bidAccessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhcHBJZCI6IkZDMjFnN2JzMVI2TjJGWjMiLCJpc3MiOiJidW1vIiwiZXhwIjoxNTYxMDAyODIyfQ.hgVH0T6fLxk973U2fIj_ejDx5aJuzRFlg1VAUA2RgzM",
        "expireIn": 7200,
        "refreshBidToken":"",
        "scope":""
    },
    "errorCode": 0,
    "message": "操作成功"
}
```

#### 5.6.4 获取用户信息

接口说明：

​		使用bidAccessToken来获取用户的信息

```plain
http请求方式：POST
https://{url}/oAuth/getUserInfo

{
	"accessToken":"",
	"params":{
		"bidAccessToken":""
	}
}

```

请求参数：

| **字段名**     | **类型** | **是否必填** | **描述**    |
| :------------- | -------: | :----------- | :---------- |
| bidAccessToken |   String | 是           | bid授权令牌 |

返回数据：

| **字段名**  | **类型** | **描述** |
| :---------- | :------- | :------- |
| userBid     | String   | 企业bid  |
| companyName | String   | 企业名称 |

返回示例：

```plain
{
    "data": {
        "userBid": "did:bid:efv7DynieC5sF1UoKAthKTfUCkuzaBfR",
        "companyName":"信通院"
    },
    "errorCode": 0,
    "message": "操作成功"
}
```

## 6 资源数据共享

本接口定义了一个标准的资源数据开放方式，通过将资源数据作为公共数据开放到主链开放资源库，实现资源数据可以被星火·链网生态全体用户按需检索和在线交易。 骨干节点需具备调用该接口的功能，子链可按需将资源数据在主链进行开放、交易。资源数据上传至主链，其BID标识无自主交互需求时，无需为其生成主链BID。

### 6.1 数据资源上传

接口说明：

将子链资源数据上传至主链，便于后续托管、共享、开放等操作，限制大小为100M。

请求参数：

form表单格式

| **字段名**  | **类型** | **是否必填** | **描述**                 |
| :---------- | -------: | :----------- | :----------------------- |
| accessToken |   String | 是           | API令牌                  |
| file        |     file | 是           | 资源文件                 |
| name        |   String | 是           | 资源名称                 |
| industry    |   String | 是           | 行业类型**见行业类型表** |
| encrypt     |   String | 是           | 0不加密,1加密            |
| ownerBid    |   String | 是           | 资源拥有人BID            |


返回数据：

| **字段名**  | **类型** | **描述**             |
| :---------- | :------- | :------------------- |
| resourceBid | String   | 资源bid              |
| errorCode   | int      | 错误码 0成功 非0失败 |
| message     | String   | 错误描述             |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/rdm/property/upload
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "file":a.img,
    "name":"",
    "industry":"",
    "encrypt":"",
    "ownerBid":""
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
	"data":{
		"resourceBid":"did:bid:efhbk6Y1kF7SuuvT2389JExuQtUVEX4n"
	},
    "errorCode": 0,
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 6.2 创建共享群组

接口说明：

创建一个共享的群组，每个群组可以设置多个账户，用户可将上传至主链的资源数据共享给该群组，实现资源数据的共享操作。

请求参数：

| **字段名** |     **类型** | **是否必填** | **描述**  |
| :--------- | -----------: | :----------- | :-------- |
| creatorBid |       String | 是           | 创建者BID |
| name       |       String | 是           | 群组名称  |
| memberList | List<String> | 是           | 群组成员  |


返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| groupId    | Integer  | 群组id   |
| errorCode  | Integer  | 错误码   |
| message    | String   | 错误描述 |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/rdm/group/add
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params": {
        "name": "我的群组1",
        "creatorBid":"did:bid:efhbk6Y1kF7SuuvT2389JExuQtUVEX4n",
        "memberList": [
        	"did:bid:2Bsqo7DDFycSf7eNHdxUTCHZopZy4YC",
        	"did:bid:efhbk6Y1kF7SuuvT2389JExuQtUVEX4n"
        ]
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
	"data": {
        "groupId": 11
    },
    "errorCode": 0,
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 6.3 数据资源共享

接口说明：

将资源数据共享给群组，群组的相关人员可以查询数据资源。

请求参数：

| **字段名**  |      **类型** | **是否必填** | **描述**           |
| :---------- | ------------: | :----------- | :----------------- |
| ownerBid    |        String | 是           | 资源拥有人bid      |
| resourceBid |        String | 是           | 数据资源bid        |
| groupIds    | List<Integer> | 是           | 共享的目标群组列表 |


返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| errorCode  | Integer  | 错误码   |
| message    | String   | 错误描述 |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/rdm/property/share
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params": {
        "ownerBid":"did:bid:ef97XGDXwY3VF6Pe6hkSPkawwQ4JJaHX",
        "resourceBid":"did:bid:efhbk6Y1kF7SuuvT2389JExuQtUVEX4n",
        "groupIds": [11]
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
    "errorCode": 0,
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 6.4 开放资源数据申请

接口说明：

将子链存证数据等公共资源数据同步至主链，在整个链网中开放共享，其他用户可以在主链的开放资源库上查看该资源数据，并有条件地下载。

请求参数：

| **字段名**  | **类型** | **是否必填** | **描述**                  |
| :---------- | -------: | :----------- | :------------------------ |
| ownerBid    |   String | 是           | 资源拥有人bid             |
| resourceBid |   String | 是           | 数据资源BID               |
| xhToken     |   String | 否           | 下载资源数据 所需星火积分 |


返回数据：

| **字段名** | **类型** | **描述**     |
| :--------- | :------- | :----------- |
| applyNo    | String   | 开放申请编号 |


示例：

（1）请求示例：

```plain
http请求方式：POST 
https://{url}/v1/rdm/open/resource
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params":{
      "ownerBid":"did:bid:ef97XGDXwY3VF6Pe6hkSPkawwQ4JJaHX",
      "resourceBid": "did:bid:efhbk6Y1kF7SuuvT2389JExuQtUVEX4n",
      "xhToken": "20"
    }   
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
    "data": {
        "applyNo": "d65811d227d8308f"
    },
    "errorCode": 0,
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 6.5 查询开放资源申请状态

接口说明：

查询开放资源申请状态

请求参数：

| **字段名** | **类型** | **是否必填** | **描述** |
| :--------- | -------: | :----------- | :------- |
| applyNo    |   String | 是           | 申请编号 |


返回数据：

| **字段名**  | **类型** | **描述**                      |
| :---------- | :------- | :---------------------------- |
| auditStatus | String   | 0待审核 1审核通过 2审核不通过 |
| auditRemark | String   | 审核备注                      |


示例：

（1）请求示例：

```plain
http请求方式：POST 
https://{url}/v1/rdm/open/resource/status
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params":{
       "applyNo":""
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
    "data": {
        "auditStatus": "1",
        "auditRemark": ""
    },
    "errorCode": 0,
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

## 7 监管服务

骨干节点按监管节点的监管规则对其下属子链执行穿透式监管，并将监管报告周期性上报至监管节点。

### 7.1 违禁词查询

接口说明：

违禁词列表是由监管节点制定，骨干节点可通过该接口查询违禁字典列表信息，并在其骨干节点及其子链系统中按照违禁词列表要求执行筛查工作，避免违禁内容上链。

请求参数：

| **字段名** | **类型** | **是否必填** | **描述**           |
| :--------- | -------: | :----------- | :----------------- |
| pageStart  |  Integer | 是           | 开始页 默认1       |
| pageSize   |  Integer | 是           | 每页条数 默认100条 |

返回数据：

| 字段名                  | 类型    | 描述          |
| :---------------------- | :------ | :------------ |
| forbiddenWords.bid      | string  | 违禁词文件bid |
| forbiddenWords.fileName | string  | 文件名称      |
| forbiddenWords.fileSize | string  | 文件大小      |
| forbiddenWords.hash     | string  | 文件hash      |
| forbiddenWords.showUrl  | string  | 下载链接      |
| forbiddenWords.time     | string  | 时间戳        |
| page                    | Object  | 分页对象      |
| page.pageSize           | Integer | 每页条数      |
| page.pageStart          | Integer | 开始页        |
| page.pageTotal          | Integer | 总页数        |

示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/sam/forbiddenWords/list
{
"accessToken":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
 "params":{
 		"pageStart":1,
 		"pageSize":"10"
 }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
   "page": {
      "pageSize": 10,
      "pageStart": 1,
      "pageTotal": 1
    },
    "forbiddenWords": [
      {
        "bid": "did:bid:ef28p1tiudukKhrm55uLXYYEAz8aSSykc",
        "fileName": "建筑业",
        "fileSize": 351543,
        "hash": "QmdPQcsYwswHTDBsvcdFhHAkyZujpcJcNEsHCJ3BwqaV88",
        "showUrl": "/oss/file/get/QmdPQcsYwswHTDBsvcdFhHAkyZujpcJcNEsHCJ3BwqaV88",
        "time": "1614838039000"
      }
    ]
  }
}

```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 7.2 监管报告

骨干节点向监管节点周期性同步监管报告

请求参数：

| 字段名     | 类型   | 是否必填 | 描述                        |
| ---------- | ------ | -------- | --------------------------- |
| chainCode  | String | 是       | AC号                        |
| reportTime | Long   | 是       | 报告时间（时间戳 单位毫秒） |
| fileName   | String | 是       | 文件名                      |
| fileSize   | String | 是       | 文件大小                    |
| hash       | String | 是       | 文件存储的hash              |
| showUrl    | String | 是       | 获取文件的链接              |


返回数据：

| 字段名    | 类型   | 描述    |
| :-------- | :----- | :------ |
| reportBid | string | 报告BID |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/sam/report/syn
{

"accessToken":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhcHBJZCI6IkZDMjFnN2JzMVI2TjJGWjMiLCJpc3MiOiJidW1vIiwiZXhwIjoxNTYxMDAyODIyfQ.hgVH0T6fLxk973U2fIj_ejDx5aJuzRFlg1VAUA2RgzM",
  "params":{
  		"chainCode":"",
			"reportTime":"",
			"fileName":"",
			"fileSize":"",
			"hash":"",
			"showUrl":""
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "reportBid":""
  }
}

```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 7.3 查询工单列表(暂缓开通)

骨干节点可通过该接口在询监管节点查询跟其相关的工单列表。

请求参数：

| **字段名** | **类型** | **是否必填** | **描述**           |
| :--------- | :------- | :----------- | :----------------- |
| pageStart  | Integer  | 是           | 开始页 默认1       |
| pageSize   | Integer  | 是           | 每页条数 默认100条 |


返回数据：

| **字段名**                  | **类型** | **描述**                                             |
| :-------------------------- | :------- | :--------------------------------------------------- |
| workOrderList               | List     | 工单列表                                             |
| workOrderList.workOrderBid  | String   | 工单BID                                              |
| workOrderList.workOrderType | String   | 工单类型（1合约违规、2大额交易、3违禁词违规、4其他） |
| workOrderList.title         | String   | 工单标题                                             |
| workOrderList.subjectBid    | String   | 违规主体BID                                          |
| workOrderList.remark        | String   | 工单备注                                             |
| workOrderList.createTime    | Long     | 工单创建时间                                         |
| workOrderList.files         | String   | 附件下载地址                                         |
| Page                        | Object   | 分页对象                                             |
| page.pageSize               | Integer  | 每页条数                                             |
| page.pageStart              | Integer  | 开始页                                               |
| page.pageTotal              | Integer  | 总页数                                               |

示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/sam/order/list
{
  "accessToken":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
  "params":{
      "pageStart":1,
      "pageSize":10
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
    "errorCode": 0,
    "message": "操作成功",
    "data": {
        "workOrderList": [{
            "workOrderBid": "",
            "workOrderType": "1",
            "chainCode": "by01",
            "title": "",
            "subjecBid": "",
            "crateTime": 1627550601505,
            "remark": ""
        }],
        "page": {
            "pageSize": 100,
            "pageStart": 1,
            "pageTotal": 1
        }
    }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 7.4 工单处理反馈(暂缓开通)

星火·链网中违法违规事件通过工单的形式体现，骨干节点需要按照工单规范将违法违规事件同步给监管节点，便于审查。（骨干节点有工单处理需实时同步给监管节点）

请求参数：

| **字段名**     | **类型** | **是否必填** | **描述**                                                   |
| :------------- | :------- | :----------- | :--------------------------------------------------------- |
| chainCode      | String   | 是           | AC号                                                       |
| workOrderBid   | String   | 是           | 工单BID                                                    |
| subjectBid     | String   | 否           | 主体bid(处置方式为忽略，此参数不传)                        |
| subjectType    | String   | 否           | 主体类型 1业务节点 2企业 3 合约处置方法为忽略，此参数不传) |
| disposalMethod | String   | 是           | 处置方式（0忽略 1启用 2停用 ）                             |
| status         | String   | 是           | 工单状态（2已处置 3已忽略）                                |
| remarks        | String   | 否           | 处置备注                                                   |


返回数据：

| **字段名** | **类型** | **描述**                       |
| :--------- | :------- | :----------------------------- |
| bid        | String   | 处置记录Bid(忽略时 该值不返回) |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/sam/order/syn
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
   "params": {
        "chainCode": "by01",
        "workOrderBid":"did:bid:efRiWFH15Kv8sHH4iurnKA8V9VZan2VW",
        "subjectBid": "did:bid:ef28pM9MG3TGXGyWAW4JpWCFsJDd5MBnc",
        "subjectType": "1",
        "disposalMethod": "1",
        "remarks": "1",
        "status": "2"
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
    "errorCode": 0,
    "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

## 8 跨链服务接入

### 8.1通用查询操作

#### 8.1.1查询跨链交易列表

接口说明：

骨干节点可以查询主链上的跨链交易列表

请求参数说明：

| **字段名**    | **类型** | **是否必填** | **描述**                                                     |
| ------------- | -------: | ------------ | ------------------------------------------------------------ |
| chainCode     |   String | 否           | 被查询链的链码                                               |
| srcChainCode  |   String | 否           | 源AC码                                                       |
| destChainCode |   String | 否           | 目标AC码                                                     |
| srcAddress    |   String | 否           | 源用户                                                       |
| destAddress   |   String | 否           | 目标用户                                                     |
| payloadType   |   String | 否           | 跨链交易类型。"0"主链积分转移，"1"子链积分兑换，"2"合约互操作，3数据传递 |
| result        |   String | 否           | 跨链交易的状态。"0":初始化，"1":已确认成功,  "2":已确认失败, "3":已超时。 |
| refunded      |   String | 否           | 星火积分交易失败后的退款信息。"0"无需退款，"1"是待退款，"2"是已退款。 |
| useAsc        |   Boolen | 否           | 是否使用升序排列。默认不填为否，即降序排列                   |
| pageStart     |      int | 否           | 开始页                                                       |
| pageSize      |      int | 否           | 每页条数                                                     |

响应参数说明：

| **字段名**       | **类型** | **描述**                                                     |
| ---------------- | -------- | ------------------------------------------------------------ |
| crossTxNo        | String   | 跨链编号。"A:B:XXXXXXXX"，A为源AC码，B为目标AC码，XXXX为随机编码 |
| srcChainCode     | String   | 源链的AC码                                                   |
| destChainCode    | String   | 目标链的AC码                                                 |
| srcAddress       | String   | 源地址                                                       |
| destAddress      | String   | 目标地址                                                     |
| payloadType      | String   | 跨链交易类型。"0"主链积分转移，"1"子链积分兑换，"2"合约互操作，3数据传递 |
| remark           | String   | 用户备注信息                                                 |
| result           | String   | 跨链交易的状态。"0":初始化，"1":已确认成功,  "2":已确认失败, "3":已超时。非0是终止态，不会发生变化。 |
| refunded         | String   | 星火积分交易失败后的退款信息。0无需退款，1是待退款，2是已退款。 |
| extension        | String   | 扩展消息。用户自定义消息                                     |
| version          | String   | 协议版本号.如果版本号不兼容，则跨链交易失败                  |
| lastUpdateSeqNum | String   | 最新更新的区块号                                             |
| page             | obj      | 分页对象                                                     |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/crosschain/query/tx/list

{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params": {
        "chainCode": "0",
        "srcChainCode": "",
        "destChainCode": "",
        "srcAddress": "",
        "destAddress": "",
        "payloadType": "",
        "result": "",
        "refunded": "",
        "useAsc": true,
        "pageStart": 1,
        "pageSize": 2
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```json
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "page": {
      "pageSize": 2,
      "pageStart": 1,
      "pageTotal": 25
    },
    "list": [
      {
        "chainCode": "0",
        "crossTxNo": "0:by01:83e951266fc90b4fb0a9c35f01b072b9",
        "srcChainCode": "0",
        "destChainCode": "by01",
        "srcAddress": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "destAddress": "did:bid:by01:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "payloadType": "1",
        "remark": "remark",
        "result": "1",
        "refunded": "0",
        "lastUpdateSeqNum": "699434"
      },
      {
        "chainCode": "0",
        "crossTxNo": "by01:0:2e51d896fd392a4aca439f530e9d62e7",
        "srcChainCode": "by01",
        "destChainCode": "0",
        "srcAddress": "did:bid:by01:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "destAddress": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "payloadType": "2",
        "remark": "remark",
        "result": "1",
        "refunded": "0",
        "lastUpdateSeqNum": "699429"
      }
    ]
  }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

#### 8.1.2查询跨链交易详情

接口说明：

骨干节点可以查询主链上的跨链交易详情

请求参数：

| **字段名** | **类型** | **是否必填** | **描述**     |
| ---------- | -------: | ------------ | ------------ |
| chainCode  |   String | 是           | 链AC号       |
| crossTxNo  |   String | 是           | 跨链交易编号 |

返回数据：

| **字段名**                      | **类型** | **描述**                                                     |
| ------------------------------- | -------- | ------------------------------------------------------------ |
| crossTxNo                       | String   | 跨链编号。"A:B:XXXXXXXX"，A为源AC码，B为目标AC码，XXXX为随机编码 |
| srcChainCode                    | String   | 源链的AC码                                                   |
| destChainCode                   | String   | 目标链的AC码                                                 |
| srcAddress                      | String   | 源地址                                                       |
| destAddress                     | String   | 目标地址                                                     |
| remark                          | String   | 用户备注信息                                                 |
| result                          | String   | 跨链交易的状态。"0":初始化，"1":已确认成功,  "2":已确认失败, "3":已超时。非0是终止态，不会再次发生变化。 |
| refunded                        | String   | 星火积分交易失败后的退款信息。0无需退款，1是待退款，2是已退款。 |
| extension                       | String   | 扩展消息。用户自定义消息                                     |
| version                         | String   | 协议版本号.如果版本号不兼容，则跨链交易失败                  |
| lastUpdateSeqNum                | String   | 最新更新的区块号                                             |
| payloadType                     | String   | 交易类型。0主链积分转移，1子链积分兑换，2合约互操作，3数据传递 |
| payloadGas                      | Object   | 星火链积分对象，当payloadType=0时候有值                      |
| payloadGas.amount               | String   | 转移的积分                                                   |
| payloadSgas                     | Object   | 子链积分对象，当payloadType=1时候有值                        |
| payloadSgas.masterAmount        | String   | 主链积分数                                                   |
| payloadSgas.srcAmount           | String   | 源链积分数                                                   |
| payloadSgas.srcTokenRate        | String   | 源链积分汇率                                                 |
| payloadSgas.destAmount          | String   | 目标链积分数                                                 |
| payloadSgas.destTokenRate       | String   | 目标链积分汇率                                               |
| payloadCall                     | Object   | 调用合约对象，当payloadType=2时候有值                        |
| payloadCall.contractMethod      | String   | 调用方法                                                     |
| payloadCall.contractInput[]     | Array    | 参数数组[{"key-param1":"value-param1"},{"key-param2":"value-param2"}] |
| payloadCall.token               | Object   | 合约调用积分对象                                             |
| payloadCall.token.masterAmount  | String   | 主链积分数                                                   |
| payloadCall.token.srcAmount     | String   | 源链积分数                                                   |
| payloadCall.token.srcTokenRate  | String   | 源链积分汇率                                                 |
| payloadCall.token.destAmount    | String   | 目标链积分数                                                 |
| payloadCall.token.destTokenRate | String   | 目标链积分汇率                                               |
| payloadData                     | Object   | 跨链转移数据，当payloadType=3时有值                          |
| payloadData.data                | Object   | 参考DDO合约的定义                                            |
| flowList                        | Array    | 跨链事务流程的数组                                           |
| flowList[i].ChainCode           | String   | 交易所在的链                                                 |
| flowList[i].type                | String   | 事务类型。"0"用户发起，"1"子链跨链网关转发跨链,"2"子链跨链网关反馈,"3"用户取出 |
| flowList[i].txSender            | String   | 交易发起人                                                   |
| flowList[i].txHash              | String   | 交易hash                                                     |
| flowList[i].lastUpdateSeqNum    | String   | 交易的区块号                                                 |
| flowList[i].txErrorCode         | String   | 交易错误码                                                   |
| flowList[i].txTime              | String   | 交易时间                                                     |
| flowList[i].ack                 | Object   | 当flowList[i].type=2时候，有值                               |
| flowList[i].ack.ackResult       | String   | 跨链交易结果。"1"成功，"2"失败，"3":"超时"                   |

示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/crosschain/query/tx/detail
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params": {
        "chainCode": "0",
        "crossTxNo": "0:by01:83e951266fc90b4fb0a9c35f01b072b9"
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```json
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "chainCode": "0",
    "crossTxNo": "0:by01:83e951266fc90b4fb0a9c35f01b072b9",
    "srcChainCode": "0",
    "destChainCode": "by01",
    "srcAddress": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
    "destAddress": "did:bid:by01:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
    "remark": "remark",
    "result": "1",
    "refunded": "0",
    "lastUpdateSeqNum": "699434",
    "payloadType": "1",
    "payloadGas": null,
    "payloadContractCall": null,
    "payloadSgas": {
      "masterAmount": "1000",
      "srcAmount": "1000",
      "srcTokenRate": "1",
      "destAmount": "729",
      "destTokenRate": "0.7291"
    },
    "payloadData": null,
    "flowList": [
      {
        "chainCode": "0",
        "extension": "startTx, extension",
        "version": "1000",
        "type": "0",
        "txSender": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "txHash": "d4da61d82afe8b54f7cdf223889dc1dcb0833dc5ac487085ff177e2c3c94a747",
        "lastUpdateSeqNum": "699430",
        "txErrorCode": "0",
        "txTime": "1627999296000",
        "ack": null
      },
      {
        "chainCode": "0",
        "extension": "extension",
        "version": "1000",
        "type": "2",
        "txSender": "did:bid:efb8Ehu9SGVCTm2BbvpPiQjzNWo4LQAA",
        "txHash": "05b04091cd5b53e8ac64e30fb02d615f0d1ccb6e042321cdce6ea32edbab64fe",
        "lastUpdateSeqNum": "699434",
        "txErrorCode": "0",
        "txTime": "1627999333000",
        "ack": {
          "ackResult": "1"
        }
      }
    ]
  }
}

```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

#### 8.1.3查询跨链网关BID地址

接口说明：

查询当前子链子链网关BID地址

请求参数说明：

| **字段名** | **类型** | **是否必填** | **描述** |
| ---------- | -------: | ------------ | -------- |
| chainCode  |   String | 是           | 目标链码 |

响应参数说明：

| **字段名**        | **类型** | **描述**   |
| ----------------- | -------- | ---------- |
| list              | Array    | 数组       |
| list[i].chainCode | String   | 链AC号     |
| list[i].address   | String   | 公证人地址 |

示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/crosschain/query/gateway/list

{
	"accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params":{
    	"chainCode": "by02"
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```json
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "list": [
      {
        "address": "did:bid:efb8Ehu9SGVCTm2BbvpPiQjzNWo4LQAA",
        "chainCode": "by02"
      }
    ]
  }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

#### 8.1.4查询子链兑换比例

接口说明：

查询主链积分兑换子链积分的兑换比例

请求参数说明：

| **字段名** | **类型** | **是否必填** | **描述** |
| ---------- | -------: | ------------ | -------- |
| chainCode  |   String | 是           | 子链AC码 |

响应参数说明：

| **字段名** | **类型** | **描述**     |
| ---------- | -------- | ------------ |
| chainCode  | String   | 子链AC码     |
| rate       | String   | 子链兑换比率 |
| remark     | String   | 备注         |

示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/crosschain/query/token/rate
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjI4MDcwODc0LCJiaWQiOiJkaWQ6YmlkOmVmQlpxY2lYc3UydG5nOWNFVlZXU0VQU3dFMlBod210In0.UVXqQaCF75hTjxpZYXk9gH2a5cKiGMr0CxweiCVR-X4",
  "params":{
    "chainCode": "by02"
  }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```json
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "chainCode": "by02",
    "rate": "6.5893",
    "remark": "remark"
  }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

#### 8.1.5查询链积分精度

接口说明：

查询区块链积分的精度

请求参数说明：

| **字段名** | **类型** | **是否必填** | **描述** |
| ---------- | -------: | ------------ | -------- |
| chainCode  |   String | 是           | 链AC码   |

响应参数说明：

| **字段名** | **类型** | **描述** |
| ---------- | -------- | -------- |
| chainCode  | String   | 子链AC码 |
| precision  | String   | 精度     |

示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/crosschain/query/chain/precision
{
	"accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params":{
    	"chainCode": "by01"
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```json
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "chainCode": "by01",
    "precision": "8"
  }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

#### 8.1.6查询跨链合约版本信息

接口说明：

查询主链或者子链其他子链版本信息

请求参数说明：

| **字段名** | **类型** | **是否必填** | **描述** |
| ---------- | -------: | ------------ | -------- |
| chainCode  |   String | 是           | 链AC码   |

响应参数说明：

| **字段名** | **类型** | **描述** |
| ---------- | -------- | -------- |
| version    | String   | 版本信息 |

示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/crosschain/query/base/version
{
	"accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params":{
    	"chainCode": "by01"
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```json
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "version": "1000"
  }
}

```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 8.2 用户发起跨链交易

#### 8.2.1 主链积分

##### A 获取交易blob

接口说明：

用户发起跨链交易，该接口用于构建交易的Blob内容。

```json
http请求方式：POST
https://{url}/crosschain/user/start/tx/maingas/blob
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params": {
        "userBid": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "srcAddress": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "destAddress": "did:bid:by02:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "destChainCode": "by02",
        "payload": {
            "amount": "10"
        },
        "remark": "Iam from by01 for start main gas",
        "extension": "extension",
        "version": "1000"
    }
}
```



请求参数说明：

| 变量           | 类型   | 描述                                                         |
| -------------- | ------ | ------------------------------------------------------------ |
| userBid        | String | 发起交易者BID，必须与srcAddress保持一致                      |
| srcAddress     | String | 源地址                                                       |
| destAddress    | String | 目标地址                                                     |
| destChainCode  | String | 目标链的AC码                                                 |
| payload        | Object | 主链积分信息                                                 |
| payload.amount | String | 积分数量                                                     |
| extension      | Object | 用户扩展信息                                                 |
| version        | String | 版本信息，由原始用户发起交易携带，填写源链跨链合约的版本信息 |
| remark         | String | 备注信息                                                     |



接口调用成功，则返回JSON数据示例为：

```json
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "blobId": "289",
    "txHash": "13ef98abcfcef7d68cf7459d34a3ed472c1c8a8148314666bd94f7b384f3f3d9",
    "blob": "0A286469643A6269643A6566766F354D4B7842314B676671787A57757A64464B34756D59776338476A471026228303080712286469643A6269643A6566766F354D4B7842314B676671787A57757A64464B34756D59776338476A4752D4020A286469643A6269643A656634354C437344614A53385272576A58737A7031317066626F76665A744C41100A1AA5027B226D6574686F64223A2273746172745478222C22706172616D73223A7B22657874656E73696F6E223A22657874656E73696F6E222C227061796C6F616454797065223A2230222C227061796C6F6164223A7B22616D6F756E74223A223130227D2C22737263426964223A226469643A6269643A6566766F354D4B7842314B676671787A57757A64464B34756D59776338476A47222C2264657374426964223A226469643A6269643A627930323A6566766F354D4B7842314B676671787A57757A64464B34756D59776338476A47222C2272656D61726B223A2249616D2066726F6D206279303120666F72207374617274206D61696E20676173222C2276657273696F6E223A2231303030222C2264657374436861696E436F6465223A2262793032227D7D3098B3870538E807"
  }
}
```

返回数据：

| **字段名** | **类型** | **描述**     |
| :--------- | :------- | :----------- |
| blobId     | String   | blodId       |
| txHash     | String   | 链上交易hash |
| blod       | String   | blod串       |

##### B 提交交易（上链）

接口说明：

子链跨链网关监听到当前子链发起的跨链交易后，将该跨链交易转发到主链，该接口用于提交交易上链。

```json
http请求方式：POST
https://{url}/crosschain/user/start/tx/maingas/submit
{
    	"accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params":{
      "blobId": "3",
    "signerList": [{
        "signBlob": "4D47D46AA467C8DF8FD1D60374D9D2378AC536612C3B84E1AC9BA4EA55E10810482EC634EADED63A389F82573AB1E055698189F7497DF23C5E32CF26A7D8270F",
        "publicKey": "b06566086febd16adf6553cc9c000dba5f9f410d34f8541392303561dddc045b15ef4c"
    }]
   }
}
```

请求参数：

| **字段名**           |    **类型** | **是否必填** | **描述**                           |
| :------------------- | ----------: | :----------- | :--------------------------------- |
| blobId               |  String(20) | 是           | blobId(上一小节返回的blobId)       |
| signerList           |        List | 是           | 签名者列表（注意：需要源用户签名） |
| signerList.signBlob  | String(128) | 是           | 签名串                             |
| signerList.publicKey | String(128) | 是           | 签名者公钥                         |

返回成功的JSON：

```json
{
    "errorCode": "0",
    "message": "操作成功",
    "data": {
      	"txHash": "083b797ea00204d2c17a9e1bd77cab11385f475da756f76a96b922e4c7b8f7c7"
    }
}
```

返回数据：

| **字段名** | **类型** | **描述**                                    |
| :--------- | :------- | :------------------------------------------ |
| txHash     | String   | 链上交易hash,（可用于查询上链交易最终状态） |

#### 8.2.2 子链积分兑换

##### A  获取交易blob

接口说明：

用户发起跨链交易，该接口用于构建交易的Blob内容。

```json
http请求方式：POST
https://{url}/crosschain/user/start/tx/subgas/blob

{
    "accessToken": "{{accessToken}}",
    "params": {
        "userBid": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "srcAddress": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "destAddress": "did:bid:by02:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "destChainCode": "by02",
        "payload": {
            "masterAmount": "10",
            "srcAmount": "10",
            "srcTokenRate": "1",
            "destAmount": "10",
            "destTokenRate": "6.5893"
        },
        "remark": "remark",
        "extension": "extension",
        "version": "1000"
    }
}
```

请求参数说明：

| 变量                  | 类型   | 描述                                                         |
| --------------------- | ------ | ------------------------------------------------------------ |
| userBid               | String | 发起交易者BID，必须与srcAddress保持一致                      |
| srcAddress            | String | 源地址                                                       |
| destAddress           | String | 目标地址                                                     |
| destChainCode         | String | 目标链的AC码                                                 |
| payload               | Object |                                                              |
| payload.masterAmount  | String | 主链星火令数量                                               |
| payload.srcAmount     | String | 源链积分数量                                                 |
| payload.srcTokenRate  | String | 源链积分的汇率                                               |
| payload.destAmount    | String | 目标链积分数量                                               |
| payload.destTokenRate | String | 目标链积分的汇率（从主链获取）                               |
| extension             | Object | 用户扩展信息                                                 |
| version               | String | 版本信息，由原始用户发起交易携带，填写源链跨链合约的版本信息 |
| remark                | String | 备注信息                                                     |



接口调用成功，则返回JSON数据示例为：

```json
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "blobId": "15",
    "txHash": "d60c15bacbb7f98f00921d5ad17d49fafe0c65ea706d5d4ced25e490ac35e9e0",
    "blob": "0A286469643A6269643A65663477776B727A53564650545A4A627257433439446B416554535169377241101E228304080712286469643A6269643A656662384568753953475643546D32426276705069516A7A4E576F344C51414152D4030A286469643A6269643A6566427070397753676564315A5A72437566426D36374A4C55635255396375321AA7037B226D6574686F64223A227375626D6974426C6F636B486561646572222C22706172616D73223A7B22626C6F636B486561646572223A7B226163636F756E745F747265655F68617368223A22222C22636C6F73655F74696D65223A2231353933333338343030303032303030222C22636F6E73656E7375735F76616C75655F68617368223A22222C22666565735F68617368223A22222C2268617368223A2235323134303538363966626533633132396237383434383232333566616365633435656438653432313666633834653962623665336136386661653430636232222C2270726576696F75735F68617368223A2231323334363966626533633132396237383434383232333566616365633435656438653233313435363537646161313233343131326136386661653430636232222C22736571223A223938222C2274785F636F756E74223A223132222C2276616C696461746F72735F68617368223A22696E7465726E616C436861696E496432222C2276657273696F6E223A22227D2C22636861696E436F6465223A2262793031222C2264796E616D6963496E666F223A7B7D7D7D3080EAADE907"
  }
}
```

返回数据：

| **字段名** | **类型** | **描述**     |
| :--------- | :------- | :----------- |
| blobId     | String   | blodId       |
| txHash     | String   | 链上交易hash |
| blod       | String   | blod串       |

##### B 提交交易（上链）

接口说明：

子链跨链网关监听到当前子链发起的跨链交易后，将该跨链交易转发到主链，该接口用于提交交易上链。

```json
http请求方式：POST
https://{url}/crosschain/user/start/tx/subgas/submit
{
    	"accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params":{
      "blobId": "3",
    "signerList": [{
        "signBlob": "4D47D46AA467C8DF8FD1D60374D9D2378AC536612C3B84E1AC9BA4EA55E10810482EC634EADED63A389F82573AB1E055698189F7497DF23C5E32CF26A7D8270F",
        "publicKey": "b06566086febd16adf6553cc9c000dba5f9f410d34f8541392303561dddc045b15ef4c"
    }]
   }
}
```

请求参数：

| **字段名**           |    **类型** | **是否必填** | **描述**                           |
| :------------------- | ----------: | :----------- | :--------------------------------- |
| blobId               |  String(20) | 是           | blobId(上一小节返回的blobId)       |
| signerList           |        List | 是           | 签名者列表（注意：需要源用户签名） |
| signerList.signBlob  | String(128) | 是           | 签名串                             |
| signerList.publicKey | String(128) | 是           | 签名者公钥                         |

返回成功的JSON：

```json
{
    "errorCode": "0",
    "message": "操作成功",
    "data": {
      	"txHash": "083b797ea00204d2c17a9e1bd77cab11385f475da756f76a96b922e4c7b8f7c7"
    }
}
```

返回数据：

| **字段名** | **类型** | **描述**                                    |
| :--------- | :------- | :------------------------------------------ |
| txHash     | String   | 链上交易hash,（可用于查询上链交易最终状态） |

#### 8.2.4 合约互操作

##### A  获取交易blob

接口说明：

用户发起跨链交易，该接口用于构建交易的Blob内容。

```json
http请求方式：POST
https://{url}/crosschain/user/start/tx/contractcall/blob

{
    "accessToken": "{{accessToken}}",
    "params": {
        "userBid": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "srcAddress": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "destAddress": "did:bid:by02:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "destChainCode": "by02",
        "payload": {
            "contractMethod": "helloMethord",
            "contractInput": [{
                "key": "key",
                "value": "value"
            }],
            "token": {
                "masterAmount": "10",
                "srcAmount": "10",
                "srcTokenRate": "1",
                "destAmount": "10",
                "destTokenRate": "6.5893"
            }
        },
        "remark": "remark",
        "extension": "extension",
        "version": "1000"
    }
}
```

请求参数说明：

| 变量                        | 类型                | 描述                                                         |
| --------------------------- | ------------------- | ------------------------------------------------------------ |
| userBid                     | String              | 发起交易者BID，必须与srcAddress保持一致                      |
| srcAddress                  | String              | 源地址                                                       |
| destAddress                 | String              | 目标地址                                                     |
| destChainCode               | String              | 目标链的AC码                                                 |
| payload                     | Object              | 自定义消息                                                   |
| payload.contractMethod      | String              | 目标合约方法                                                 |
| payload.contractInput       | Array               | 合约参数。用户自定义                                         |
| payload.contractInput[n]    | Object/String/Array | 自定义参数N                                                  |
| payload.token               | Object              | 可以为空，源链积分信息当涉及到需要支付资产时候，可以使用子链积分兑换的协议进行扩展 |
| payload.token.masterAmount  | String              | 主链积分数量                                                 |
| payload.token.srcAmount     | String              | 源链积分数量                                                 |
| payload.token.srcTokenRate  | String              | 源链积分的汇率                                               |
| payload.token.destAmount    | String              | 目标链积分数量                                               |
| payload.token.destTokenRate | String              | 目标链积分的汇率                                             |
| extension                   | String              | 用户扩展信息                                                 |
| version                     | String              | 版本信息，由原始用户发起交易携带，填写源链跨链合约的版本信息 |
| remark                      | String              | 备注信息                                                     |



接口调用成功，则返回JSON数据示例为：

```json
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "blobId": "15",
    "txHash": "d60c15bacbb7f98f00921d5ad17d49fafe0c65ea706d5d4ced25e490ac35e9e0",
    "blob": "0A286469643A6269643A65663477776B727A53564650545A4A627257433439446B416554535169377241101E228304080712286469643A6269643A656662384568753953475643546D32426276705069516A7A4E576F344C51414152D4030A286469643A6269643A6566427070397753676564315A5A72437566426D36374A4C55635255396375321AA7037B226D6574686F64223A227375626D6974426C6F636B486561646572222C22706172616D73223A7B22626C6F636B486561646572223A7B226163636F756E745F747265655F68617368223A22222C22636C6F73655F74696D65223A2231353933333338343030303032303030222C22636F6E73656E7375735F76616C75655F68617368223A22222C22666565735F68617368223A22222C2268617368223A2235323134303538363966626533633132396237383434383232333566616365633435656438653432313666633834653962623665336136386661653430636232222C2270726576696F75735F68617368223A2231323334363966626533633132396237383434383232333566616365633435656438653233313435363537646161313233343131326136386661653430636232222C22736571223A223938222C2274785F636F756E74223A223132222C2276616C696461746F72735F68617368223A22696E7465726E616C436861696E496432222C2276657273696F6E223A22227D2C22636861696E436F6465223A2262793031222C2264796E616D6963496E666F223A7B7D7D7D3080EAADE907"
  }
}
```

返回数据：

| **字段名** | **类型** | **描述**     |
| :--------- | :------- | :----------- |
| blobId     | String   | blodId       |
| txHash     | String   | 链上交易hash |
| blod       | String   | blod串       |

##### B 提交交易（上链）

接口说明：

子链网关监听到当前子链发起的跨链交易后，将该跨链交易转发到主链，该接口用于提交交易上链。

```json
http请求方式：POST
https://{url}/crosschain/user/start/tx/contractcall/submit
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params":{"blobId": "3",
    "signerList": [{
        "signBlob": "4D47D46AA467C8DF8FD1D60374D9D2378AC536612C3B84E1AC9BA4EA55E10810482EC634EADED63A389F82573AB1E055698189F7497DF23C5E32CF26A7D8270F",
        "publicKey": "b06566086febd16adf6553cc9c000dba5f9f410d34f8541392303561dddc045b15ef4c"
    }]
  }
}
```

请求参数：

| **字段名**           |    **类型** | **是否必填** | **描述**                           |
| :------------------- | ----------: | :----------- | :--------------------------------- |
| blobId               |  String(20) | 是           | blobId(上一小节返回的blobId)       |
| signerList           |        List | 是           | 签名者列表（注意：需要源用户签名） |
| signerList.signBlob  | String(128) | 是           | 签名串                             |
| signerList.publicKey | String(128) | 是           | 签名者公钥                         |

返回成功的JSON：

```json
{
    "errorCode": "0",
    "message": "操作成功",
    "data": {
      	"txHash": "083b797ea00204d2c17a9e1bd77cab11385f475da756f76a96b922e4c7b8f7c7"
    }
}
```

返回数据：

| **字段名** | **类型** | **描述**                                    |
| :--------- | :------- | :------------------------------------------ |
| txHash     | String   | 链上交易hash,（可用于查询上链交易最终状态） |

#### 8.2.4 数据传递

##### A  获取交易blob

接口说明：

用户发起跨链交易，该接口用于构建交易的Blob内容。

```json
http请求方式：POST
https://{url}/crosschain/user/start/tx/docdata/blob

{
    "accessToken": "{{accessToken}}",
    "params": {
        "userBid": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "srcAddress": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "destAddress": "did:bid:by02:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "destChainCode": "by02",
        "payload": {
            "data": "{\"document\":{\"@context\":\"context\",\"attributes\":[{\"encrypt\":\"false\",\"key\":\"realName\",\"label\":\"真实名称\",\"type\":\"text\",\"value\":\"白晶晶\"},{\"encrypt\":\"false\",\"key\":\"idType\",\"label\":\"证件类型\",\"type\":\"text\",\"value\":\"6\"},{\"encrypt\":\"false\",\"key\":\"idNumber\",\"label\":\"证件号\",\"type\":\"text\",\"value\":\"800001198209289982\"},{\"encrypt\":\"false\",\"key\":\"validateStart\",\"label\":\"证件有效期\",\"type\":\"text\",\"value\":\"2020-11-04\"},{\"encrypt\":\"false\",\"key\":\"validateEnd\",\"label\":\"证件有效期\",\"type\":\"text\",\"value\":\"\"},{\"encrypt\":\"false\",\"key\":\"idPic\",\"label\":\"证件照片\",\"type\":\"image\",\"value\":\"ipfs://QmQmWxAMKAUFY9VHLYgeWqQo9qFYZvsUz22npeBkaAes24\"},{\"encrypt\":\"false\",\"key\":\"idBackPic\",\"label\":\"证件背面\",\"type\":\"image\",\"value\":\"ipfs://QmW4AXVbV2PTBpRPgQZVjiqFAqoUJUY1mvqz4BqC1CSQcv\"}],\"context\":\" \",\"created\":\"2020-11-25 18:33:59\",\"encryptEndpoint\":\" \",\"id\":\"did:bid:A:efhnDtNokTM7hobc3iK2nVWVf5qAyqX5\",\"publicKey\":[{\"id\":\" \",\"publicKeyHex\":\" \",\"type\":\"Ed25519\"}],\"recovery\":[\" \"],\"service\":[{\"id\":\"did:bid:A:efhnDtNokTM7hobc3iK2nVWVf5qAyqX5#resolver\",\"serviceEndpoint\":\" \",\"type\":\" \"}],\"type\":\"mixture\",\"updated\":\"2020-11-25 18:33:59\",\"version\":\"1\"},\"proof\":{\"creator\":\" \",\"signatureValue\":\" \",\"type\":\" \"}}"
        },
        "remark": "remark",
        "extension": "extension",
        "version": "1000"
    }
}
```

请求参数说明：

| 变量          | 类型   | 描述                                                         |
| ------------- | ------ | ------------------------------------------------------------ |
| userBid       | String | 发起交易者BID，必须与srcAddress保持一致                      |
| srcAddress    | String | 源地址                                                       |
| destAddress   | String | 目标地址                                                     |
| destChainCode | String | 目标链的AC码                                                 |
| payload       | Object | 自定义消息                                                   |
| payload.data  | String | BID DOC对象，参考《BID协议》元数据章节，为json字符串         |
| extension     | String | 用户扩展信息                                                 |
| version       | String | 版本信息，由原始用户发起交易携带，填写源链跨链合约的版本信息 |
| remark        | String | 备注信息                                                     |



接口调用成功，则返回JSON数据示例为：

```json
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "blobId": "15",
    "txHash": "d60c15bacbb7f98f00921d5ad17d49fafe0c65ea706d5d4ced25e490ac35e9e0",
    "blob": "0A286469643A6269643A65663477776B727A53564650545A4A627257433439446B416554535169377241101E228304080712286469643A6269643A656662384568753953475643546D32426276705069516A7A4E576F344C51414152D4030A286469643A6269643A6566427070397753676564315A5A72437566426D36374A4C55635255396375321AA7037B226D6574686F64223A227375626D6974426C6F636B486561646572222C22706172616D73223A7B22626C6F636B486561646572223A7B226163636F756E745F747265655F68617368223A22222C22636C6F73655F74696D65223A2231353933333338343030303032303030222C22636F6E73656E7375735F76616C75655F68617368223A22222C22666565735F68617368223A22222C2268617368223A2235323134303538363966626533633132396237383434383232333566616365633435656438653432313666633834653962623665336136386661653430636232222C2270726576696F75735F68617368223A2231323334363966626533633132396237383434383232333566616365633435656438653233313435363537646161313233343131326136386661653430636232222C22736571223A223938222C2274785F636F756E74223A223132222C2276616C696461746F72735F68617368223A22696E7465726E616C436861696E496432222C2276657273696F6E223A22227D2C22636861696E436F6465223A2262793031222C2264796E616D6963496E666F223A7B7D7D7D3080EAADE907"
  }
}
```

返回数据：

| **字段名** | **类型** | **描述**     |
| :--------- | :------- | :----------- |
| blobId     | String   | blodId       |
| txHash     | String   | 链上交易hash |
| blod       | String   | blod串       |

##### B 提交交易（上链）

接口说明：

子链跨链网关监听到当前子链发起的跨链交易后，将该跨链交易转发到主链，该接口用于提交交易上链。

```json
http请求方式：POST
https://{url}/crosschain/user/start/tx/docdata/submit
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params":{
      "blobId": "3",
    "signerList": [{
        "signBlob": "4D47D46AA467C8DF8FD1D60374D9D2378AC536612C3B84E1AC9BA4EA55E10810482EC634EADED63A389F82573AB1E055698189F7497DF23C5E32CF26A7D8270F",
        "publicKey": "b06566086febd16adf6553cc9c000dba5f9f410d34f8541392303561dddc045b15ef4c"
    }]
 }
}
```

请求参数：

| **字段名**           |    **类型** | **是否必填** | **描述**                           |
| :------------------- | ----------: | :----------- | :--------------------------------- |
| blobId               |  String(20) | 是           | blobId(上一小节返回的blobId)       |
| signerList           |        List | 是           | 签名者列表（注意：需要源用户签名） |
| signerList.signBlob  | String(128) | 是           | 签名串                             |
| signerList.publicKey | String(128) | 是           | 签名者公钥                         |



返回成功的JSON：

```json
{
    "errorCode": "0",
    "message": "操作成功",
    "data": {
      	"txHash": "083b797ea00204d2c17a9e1bd77cab11385f475da756f76a96b922e4c7b8f7c7"
    }
}
```

返回数据：

| **字段名** | **类型** | **描述**                                    |
| :--------- | :------- | :------------------------------------------ |
| txHash     | String   | 链上交易hash,（可用于查询上链交易最终状态） |

### 8. 3 骨干节点的跨链网关操作

#### 8.3.1 跨链网关转发跨链交易

##### 8.3.1.1 主链积分

###### A 获取交易blob

接口说明：

子链跨链网关监听到当前子链发起的跨链交易后，将该跨链交易转发到主链，该接口用于构建交易的Blob内容。

```json
http请求方式：POST
https://{url}/crosschain/backbone/send/tx/maingas/blob

{
    "accessToken": "{{accessToken}}",
    "params": {
        "gatewayAddress": "did:bid:efb8Ehu9SGVCTm2BbvpPiQjzNWo4LQAA",
        "crossTxNo": "by01:0:96c1a5e14db0a1cc676931874ba9849c",
        "srcAddress": "did:bid:by01:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "destAddress": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "srcChainCode": "by01",
        "destChainCode": "0",
        "payload": {
            "amount": "200"
        },
        "remark": "remark",
        "extension": "extension",
        "version": "1000",
        "proof": {
            "ledgerSeq": "ledgerSeq",
            "txHash": "txHash"
        }
    }
}
```

请求参数说明：

| 变量            | 类型       | 描述                                                         |
| --------------- | ---------- | ------------------------------------------------------------ |
| gatewayAddress  | String(64) | 跨链网关BID地址（在平台链详情中查看该地址），如：**did:bid:eft4DztdgVgph2bmij93REU7kzrET7Do** |
| crossTxNo       | String     | 跨链交易编号                                                 |
| srcAddress      | String     | 源地址                                                       |
| destAddress     | String     | 目标地址                                                     |
| destChainCode   | String     | 目标链的AC码                                                 |
| payload         | Object     | 主链积分信息                                                 |
| payload.amount  | String     | 积分数量                                                     |
| extension       | Object     | 用户扩展信息                                                 |
| version         | String     | 版本信息，由原始用户发起交易携带，填写源链跨链合约的版本信息 |
| remark          | String     | 备注信息                                                     |
| proof           | Object     | 子链的跨链存证对象                                           |
| proof.ledgerSeq | String     | 子链上跨链交易的区块号                                       |
| proof.txHash    | String     | 子链上跨链交易的哈希值                                       |

接口调用成功，则返回JSON数据示例为：

```json
{
    "errorCode": 0,
    "message": "操作成功",
    "data": {
        "blobId": "15",
        "txHash": "d60c15bacbb7f98f00921d5ad17d49fafe0c65ea706d5d4ced25e490ac35e9e0",
        "blob": "0A286469643A6269643A65663477776B727A53564650545A4A627257433439446B416554535169377241101E228304080712286469643A6269643A656662384568753953475643546D32426276705069516A7A4E576F344C51414152D4030A286469643A6269643A6566427070397753676564315A5A72437566426D36374A4C55635255396375321AA7037B226D6574686F64223A227375626D6974426C6F636B486561646572222C22706172616D73223A7B22626C6F636B486561646572223A7B226163636F756E745F747265655F68617368223A22222C22636C6F73655F74696D65223A2231353933333338343030303032303030222C22636F6E73656E7375735F76616C75655F68617368223A22222C22666565735F68617368223A22222C2268617368223A2235323134303538363966626533633132396237383434383232333566616365633435656438653432313666633834653962623665336136386661653430636232222C2270726576696F75735F68617368223A2231323334363966626533633132396237383434383232333566616365633435656438653233313435363537646161313233343131326136386661653430636232222C22736571223A223938222C2274785F636F756E74223A223132222C2276616C696461746F72735F68617368223A22696E7465726E616C436861696E496432222C2276657273696F6E223A22227D2C22636861696E436F6465223A2262793031222C2264796E616D6963496E666F223A7B7D7D7D3080EAADE907"
    }
}
```

返回数据：

| **字段名** | **类型** | **描述**     |
| :--------- | :------- | :----------- |
| blobId     | String   | blodId       |
| txHash     | String   | 链上交易hash |
| blod       | String   | blod串       |

###### B 提交交易（上链）

接口说明：

子链跨链网关监听到当前子链发起的跨链交易后，将该跨链交易转发到主链，该接口用于提交交易上链。

```json
http请求方式：POST
https://{url}/crosschain/backbone/send/tx/maingas/submit
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params":{
      "blobId": "3",
    "signerList": [{
        "signBlob": "4D47D46AA467C8DF8FD1D60374D9D2378AC536612C3B84E1AC9BA4EA55E10810482EC634EADED63A389F82573AB1E055698189F7497DF23C5E32CF26A7D8270F",
        "publicKey": "b06566086febd16adf6553cc9c000dba5f9f410d34f8541392303561dddc045b15ef4c"
    }]
    }
}
```

请求参数：

| **字段名**           |    **类型** | **是否必填** | **描述**                           |
| :------------------- | ----------: | :----------- | :--------------------------------- |
| blobId               |  String(20) | 是           | blobId(上一小节返回的blobId)       |
| signerList           |        List | 是           | 签名者列表（注意：需要源用户签名） |
| signerList.signBlob  | String(128) | 是           | 签名串                             |
| signerList.publicKey | String(128) | 是           | 签名者公钥                         |

返回成功的JSON：

```json
{
    "errorCode": "0",
    "message": "操作成功",
    "data": {
      	"txHash": "083b797ea00204d2c17a9e1bd77cab11385f475da756f76a96b922e4c7b8f7c7"
    }
}
```

返回数据：

| **字段名** | **类型** | **描述**                                    |
| :--------- | :------- | :------------------------------------------ |
| txHash     | String   | 链上交易hash,（可用于查询上链交易最终状态） |

##### 8.3.1.2 子链积分兑换

###### A  获取交易blob

接口说明：

子链网关监听到当前子链发起的跨链交易后，将该跨链交易转发到主链，该接口用于构建交易的Blob内容。

```json
http请求方式：POST
https://{url}/crosschain/backbone/send/tx/subgas/blob

{
    "accessToken": "{{accessToken}}",
    "params": {
        "gatewayAddress": "did:bid:efb8Ehu9SGVCTm2BbvpPiQjzNWo4LQAA",
        "crossTxNo": "by01:0:55bee90c7ce3e658a1729a7cc5ffb773",
        "srcAddress": "did:bid:by01:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "destAddress": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "srcChainCode": "by01",
        "destChainCode": "0",
        "payload": {
            "masterAmount": "1371",
            "srcAmount": "1000",
            "srcTokenRate": "0.7291",
            "destAmount": "1371",
            "destTokenRate": "1"
        },
        "remark": "remark",
        "extension": "extension",
        "version": "1000",
        "proof": {
            "ledgerSeq": "ledgerSeq",
            "txHash": "txHash"
        }
    }
}
```



请求参数说明：

| 变量                  | 类型       | 描述                                                         |
| --------------------- | ---------- | ------------------------------------------------------------ |
| gatewayAddress        | String(64) | 跨链网关BID地址（在平台链详情中查看该地址），如：**did:bid:eft4DztdgVgph2bmij93REU7kzrET7Do** |
| crossTxNo             | String     | 跨链交易编号                                                 |
| srcAddress            | String     | 源地址                                                       |
| destAddress           | String     | 目标地址                                                     |
| destChainCode         | String     | 目标链的AC码                                                 |
| payload               | Object     |                                                              |
| payload.masterAmount  | String     | 主链星火令数量                                               |
| payload.srcAmount     | String     | 源链积分数量                                                 |
| payload.srcTokenRate  | String     | 源链积分的汇率                                               |
| payload.destAmount    | String     | 目标链积分数量                                               |
| payload.destTokenRate | String     | 目标链积分的汇率（从主链获取）                               |
| extension             | Object     | 用户扩展信息                                                 |
| version               | String     | 版本信息，由原始用户发起交易携带，填写源链跨链合约的版本信息 |
| remark                | String     | 备注信息                                                     |
| proof                 | Object     | 子链的跨链存证对象                                           |
| proof.ledgerSeq       | String     | 子链上跨链交易的区块号                                       |
| proof.txHash          | String     | 子链上跨链交易的哈希值                                       |



接口调用成功，则返回JSON数据示例为：

```json
{
    "errorCode": 0,
    "message": "操作成功",
    "data": {
        "blobId": "15",
        "txHash": "d60c15bacbb7f98f00921d5ad17d49fafe0c65ea706d5d4ced25e490ac35e9e0",
        "blob": "0A286469643A6269643A65663477776B727A53564650545A4A627257433439446B416554535169377241101E228304080712286469643A6269643A656662384568753953475643546D32426276705069516A7A4E576F344C51414152D4030A286469643A6269643A6566427070397753676564315A5A72437566426D36374A4C55635255396375321AA7037B226D6574686F64223A227375626D6974426C6F636B486561646572222C22706172616D73223A7B22626C6F636B486561646572223A7B226163636F756E745F747265655F68617368223A22222C22636C6F73655F74696D65223A2231353933333338343030303032303030222C22636F6E73656E7375735F76616C75655F68617368223A22222C22666565735F68617368223A22222C2268617368223A2235323134303538363966626533633132396237383434383232333566616365633435656438653432313666633834653962623665336136386661653430636232222C2270726576696F75735F68617368223A2231323334363966626533633132396237383434383232333566616365633435656438653233313435363537646161313233343131326136386661653430636232222C22736571223A223938222C2274785F636F756E74223A223132222C2276616C696461746F72735F68617368223A22696E7465726E616C436861696E496432222C2276657273696F6E223A22227D2C22636861696E436F6465223A2262793031222C2264796E616D6963496E666F223A7B7D7D7D3080EAADE907"
    }
}
```

返回数据：

| **字段名** | **类型** | **描述**     |
| :--------- | :------- | :----------- |
| blobId     | String   | blodId       |
| txHash     | String   | 链上交易hash |
| blod       | String   | blod串       |

###### B 提交交易（上链）

接口说明：

子链网关监听到当前子链发起的跨链交易后，将该跨链交易转发到主链，该接口用于提交交易上链。

```json
http请求方式：POST
https://{url}/crosschain/backbone/send/tx/subgas/submit
{
     "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params":{
      "blobId": "3",
    "signerList": [{
        "signBlob": "4D47D46AA467C8DF8FD1D60374D9D2378AC536612C3B84E1AC9BA4EA55E10810482EC634EADED63A389F82573AB1E055698189F7497DF23C5E32CF26A7D8270F",
        "publicKey": "b06566086febd16adf6553cc9c000dba5f9f410d34f8541392303561dddc045b15ef4c"
    }]
    }
}
```

请求参数：

| **字段名**           |    **类型** | **是否必填** | **描述**                           |
| :------------------- | ----------: | :----------- | :--------------------------------- |
| blobId               |  String(20) | 是           | blobId(上一小节返回的blobId)       |
| signerList           |        List | 是           | 签名者列表（注意：需要源用户签名） |
| signerList.signBlob  | String(128) | 是           | 签名串                             |
| signerList.publicKey | String(128) | 是           | 签名者公钥                         |



返回成功的JSON：

```json
{
    "errorCode": "0",
    "message": "操作成功",
    "data": {
        "txHash": "083b797ea00204d2c17a9e1bd77cab11385f475da756f76a96b922e4c7b8f7c7"
    }
}
```

返回数据：

| **字段名** | **类型** | **描述**                                    |
| :--------- | :------- | :------------------------------------------ |
| txHash     | String   | 链上交易hash,（可用于查询上链交易最终状态） |

##### 8.3.1.3 合约互操作

###### A  获取交易blob

接口说明：

子链跨链网关监听到当前子链发起的跨链交易后，将该跨链交易转发到主链，该接口用于构建交易的Blob内容。

```json
http请求方式：POST
https://{url}/crosschain/backbone/send/tx/contractcall/blob

{
    "accessToken": "{{accessToken}}",
    "params": {
        "gatewayAddress": "did:bid:efb8Ehu9SGVCTm2BbvpPiQjzNWo4LQAA",
        "crossTxNo": "by01:0:2e51d896fd392a4aca439f530e9d62e7",
        "srcAddress": "did:bid:by01:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "destAddress": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "srcChainCode": "by01",
        "destChainCode": "0",
        "payload": {
            "contractMethod": "helloMethord",
            "contractInput": [{
                "key": "key",
                "value": "value"
            }],
            "token": {
                "masterAmount": "10",
                "srcAmount": "10",
                "srcTokenRate": "1",
                "destAmount": "10",
                "destTokenRate": "6.5893"
            }
        },
        "remark": "remark",
        "extension": "extension",
        "version": "1000",
        "proof": {
            "ledgerSeq": "ledgerSeq",
            "txHash": "txHash"
        }
    }
}
```



请求参数说明：

| 变量                        | 类型                | 描述                                                         |
| --------------------------- | ------------------- | ------------------------------------------------------------ |
| gatewayAddress              | String(64)          | 链网关地址（在平台链详情中查看该地址），如：**did:bid:eft4DztdgVgph2bmij93REU7kzrET7Do** |
| crossTxNo                   | String              | 跨链交易编号                                                 |
| srcAddress                  | String              | 源地址                                                       |
| destAddress                 | String              | 目标地址                                                     |
| destChainCode               | String              | 目标链的AC码                                                 |
| payload                     | Object              | 自定义消息                                                   |
| payload.contractMethod      | String              | 目标合约方法                                                 |
| payload.contractInput       | Array               | 合约参数。用户自定义                                         |
| payload.contractInput[n]    | Object/String/Array | 自定义参数N                                                  |
| payload.token               | Object              | 可以为空，源链积分信息当涉及到需要支付资产时候，可以使用子链积分兑换的协议进行扩展 |
| payload.token.masterAmount  | String              | 主链积分数量                                                 |
| payload.token.srcAmount     | String              | 源链积分数量                                                 |
| payload.token.srcTokenRate  | String              | 源链积分的汇率                                               |
| payload.token.destAmount    | String              | 目标链积分数量                                               |
| payload.token.destTokenRate | String              | 目标链积分的汇率                                             |
| extension                   | String              | 用户扩展信息                                                 |
| version                     | String              | 版本信息，由原始用户发起交易携带，填写源链跨链合约的版本信息 |
| remark                      | String              | 备注信息                                                     |
| proof                       | Object              | 子链的跨链存证对象                                           |
| proof.ledgerSeq             | String              | 子链上跨链交易的区块号                                       |
| proof.txHash                | String              | 子链上跨链交易的哈希值                                       |



接口调用成功，则返回JSON数据示例为：

```json
{
    "errorCode": 0,
    "message": "操作成功",
    "data": {
        "blobId": "15",
        "txHash": "d60c15bacbb7f98f00921d5ad17d49fafe0c65ea706d5d4ced25e490ac35e9e0",
        "blob": "0A286469643A6269643A65663477776B727A53564650545A4A627257433439446B416554535169377241101E228304080712286469643A6269643A656662384568753953475643546D32426276705069516A7A4E576F344C51414152D4030A286469643A6269643A6566427070397753676564315A5A72437566426D36374A4C55635255396375321AA7037B226D6574686F64223A227375626D6974426C6F636B486561646572222C22706172616D73223A7B22626C6F636B486561646572223A7B226163636F756E745F747265655F68617368223A22222C22636C6F73655F74696D65223A2231353933333338343030303032303030222C22636F6E73656E7375735F76616C75655F68617368223A22222C22666565735F68617368223A22222C2268617368223A2235323134303538363966626533633132396237383434383232333566616365633435656438653432313666633834653962623665336136386661653430636232222C2270726576696F75735F68617368223A2231323334363966626533633132396237383434383232333566616365633435656438653233313435363537646161313233343131326136386661653430636232222C22736571223A223938222C2274785F636F756E74223A223132222C2276616C696461746F72735F68617368223A22696E7465726E616C436861696E496432222C2276657273696F6E223A22227D2C22636861696E436F6465223A2262793031222C2264796E616D6963496E666F223A7B7D7D7D3080EAADE907"
    }
}
```

返回数据：

| **字段名** | **类型** | **描述**     |
| :--------- | :------- | :----------- |
| blobId     | String   | blodId       |
| txHash     | String   | 链上交易hash |
| blod       | String   | blod串       |

###### B 提交交易（上链）

接口说明：

子链跨链网关监听到当前子链发起的跨链交易后，将该跨链交易转发到主链，该接口用于提交交易上链。

```json
http请求方式：POST
https://{url}/crosschain/backbone/send/tx/contractcall/submit
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjE5MzQyNDIyLCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.3gcQQBqvqtZH1q-TfUiGne68R1TnBcvLAA6nIJ8qDGU",
    "params": {
        "blobId": "3",
        "signerList": [{
            "signBlob": "4D47D46AA467C8DF8FD1D60374D9D2378AC536612C3B84E1AC9BA4EA55E10810482EC634EADED63A389F82573AB1E055698189F7497DF23C5E32CF26A7D8270F",
            "publicKey": "b06566086febd16adf6553cc9c000dba5f9f410d34f8541392303561dddc045b15ef4c"
        }]
    }
}
```

请求参数：

| **字段名**           |    **类型** | **是否必填** | **描述**                           |
| :------------------- | ----------: | :----------- | :--------------------------------- |
| blobId               |  String(20) | 是           | blobId(上一小节返回的blobId)       |
| signerList           |        List | 是           | 签名者列表（注意：需要源用户签名） |
| signerList.signBlob  | String(128) | 是           | 签名串                             |
| signerList.publicKey | String(128) | 是           | 签名者公钥                         |



返回成功的JSON：

```json
{
    "errorCode": "0",
    "message": "操作成功",
    "data": {
      	"txHash": "083b797ea00204d2c17a9e1bd77cab11385f475da756f76a96b922e4c7b8f7c7"
    }
}
```

返回数据：

| **字段名** | **类型** | **描述**                                    |
| :--------- | :------- | :------------------------------------------ |
| txHash     | String   | 链上交易hash,（可用于查询上链交易最终状态） |

##### 8.3.1.4 数据传递

###### A  获取交易blob

接口说明：

子链跨链网关监听到当前子链发起的跨链交易后，将该跨链交易转发到主链，该接口用于构建交易的Blob内容。

```json
http请求方式：POST
https://{url}/crosschain/backbone/send/tx/docdata/blob

{
    "accessToken": "{{accessToken}}",
    "params": {
        "gatewayAddress": "did:bid:efb8Ehu9SGVCTm2BbvpPiQjzNWo4LQAA",
        "crossTxNo": "by01:0:da4c3a53f357b9096667d2333e103d3a",
        "srcAddress": "did:bid:by01:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "destAddress": "did:bid:efvo5MKxB1KgfqxzWuzdFK4umYwc8GjG",
        "srcChainCode": "by01",
        "destChainCode": "0",
        "payload": {
            "data": "{\"document\":{\"@context\":\"context\",\"attributes\":[{\"encrypt\":\"false\",\"key\":\"realName\",\"label\":\"真实名称\",\"type\":\"text\",\"value\":\"白晶晶\"},{\"encrypt\":\"false\",\"key\":\"idType\",\"label\":\"证件类型\",\"type\":\"text\",\"value\":\"6\"},{\"encrypt\":\"false\",\"key\":\"idNumber\",\"label\":\"证件号\",\"type\":\"text\",\"value\":\"800001198209289982\"},{\"encrypt\":\"false\",\"key\":\"validateStart\",\"label\":\"证件有效期\",\"type\":\"text\",\"value\":\"2020-11-04\"},{\"encrypt\":\"false\",\"key\":\"validateEnd\",\"label\":\"证件有效期\",\"type\":\"text\",\"value\":\"\"},{\"encrypt\":\"false\",\"key\":\"idPic\",\"label\":\"证件照片\",\"type\":\"image\",\"value\":\"ipfs://QmQmWxAMKAUFY9VHLYgeWqQo9qFYZvsUz22npeBkaAes24\"},{\"encrypt\":\"false\",\"key\":\"idBackPic\",\"label\":\"证件背面\",\"type\":\"image\",\"value\":\"ipfs://QmW4AXVbV2PTBpRPgQZVjiqFAqoUJUY1mvqz4BqC1CSQcv\"}],\"context\":\" \",\"created\":\"2020-11-25 18:33:59\",\"encryptEndpoint\":\" \",\"id\":\"did:bid:A:efhnDtNokTM7hobc3iK2nVWVf5qAyqX5\",\"publicKey\":[{\"id\":\" \",\"publicKeyHex\":\" \",\"type\":\"Ed25519\"}],\"recovery\":[\" \"],\"service\":[{\"id\":\"did:bid:A:efhnDtNokTM7hobc3iK2nVWVf5qAyqX5#resolver\",\"serviceEndpoint\":\" \",\"type\":\" \"}],\"type\":\"mixture\",\"updated\":\"2020-11-25 18:33:59\",\"version\":\"1\"},\"proof\":{\"creator\":\" \",\"signatureValue\":\" \",\"type\":\" \"}}"
        },
        "remark": "remark",
        "extension": "extension",
        "version": "1000",
        "proof": {
            "ledgerSeq": "ledgerSeq",
            "txHash": "txHash"
        }
    }
}
```



请求参数说明：

| 变量             | 类型       | 描述                                                         |
| ---------------- | ---------- | ------------------------------------------------------------ |
| gatewayAddress   | String(64) | 跨链网关BID地址（在平台链详情中查看该地址），如：**did:bid:eft4DztdgVgph2bmij93REU7kzrET7Do** |
| crossTxNo        | String     | 跨链交易编号                                                 |
| srcAddress       | String     | 源地址                                                       |
| destAddress      | String     | 目标地址                                                     |
| destChainCode    | String     | 目标链的AC码                                                 |
| payload          | Object     | 自定义消息                                                   |
| payload.document | String     | BID DOC对象，参考《BID协议》元数据章节                       |
| extension        | String     | 用户扩展信息                                                 |
| version          | String     | 版本信息，由原始用户发起交易携带，填写源链跨链合约的版本信息 |
| remark           | String     | 备注信息                                                     |
| proof            | Object     | 子链的跨链存证对象                                           |
| proof.ledgerSeq  | String     | 子链上跨链交易的区块号                                       |
| proof.txHash     | String     | 子链上跨链交易的哈希值                                       |



接口调用成功，则返回JSON数据示例为：

```json
{
    "errorCode": 0,
    "message": "操作成功",
    "data": {
        "blobId": "15",
        "txHash": "d60c15bacbb7f98f00921d5ad17d49fafe0c65ea706d5d4ced25e490ac35e9e0",
        "blob": "0A286469643A6269643A65663477776B727A53564650545A4A627257433439446B416554535169377241101E228304080712286469643A6269643A656662384568753953475643546D32426276705069516A7A4E576F344C51414152D4030A286469643A6269643A6566427070397753676564315A5A72437566426D36374A4C55635255396375321AA7037B226D6574686F64223A227375626D6974426C6F636B486561646572222C22706172616D73223A7B22626C6F636B486561646572223A7B226163636F756E745F747265655F68617368223A22222C22636C6F73655F74696D65223A2231353933333338343030303032303030222C22636F6E73656E7375735F76616C75655F68617368223A22222C22666565735F68617368223A22222C2268617368223A2235323134303538363966626533633132396237383434383232333566616365633435656438653432313666633834653962623665336136386661653430636232222C2270726576696F75735F68617368223A2231323334363966626533633132396237383434383232333566616365633435656438653233313435363537646161313233343131326136386661653430636232222C22736571223A223938222C2274785F636F756E74223A223132222C2276616C696461746F72735F68617368223A22696E7465726E616C436861696E496432222C2276657273696F6E223A22227D2C22636861696E436F6465223A2262793031222C2264796E616D6963496E666F223A7B7D7D7D3080EAADE907"
    }
}
```

返回数据：

| **字段名** | **类型** | **描述**     |
| :--------- | :------- | :----------- |
| blobId     | String   | blodId       |
| txHash     | String   | 链上交易hash |
| blod       | String   | blod串       |

###### B 提交交易（上链）

接口说明：

子链网关监听到当前子链发起的跨链交易后，将该跨链交易转发到主链，该接口用于提交交易上链。

```json
http请求方式：POST
https://{url}/crosschain/backbone/send/tx/docdata/submit
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjE5MzQyNDIyLCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.3gcQQBqvqtZH1q-TfUiGne68R1TnBcvLAA6nIJ8qDGU",
  "params": {
    "blobId": "3",
    "signerList": [{
      "signBlob": "4D47D46AA467C8DF8FD1D60374D9D2378AC536612C3B84E1AC9BA4EA55E10810482EC634EADED63A389F82573AB1E055698189F7497DF23C5E32CF26A7D8270F",
      "publicKey": "b06566086febd16adf6553cc9c000dba5f9f410d34f8541392303561dddc045b15ef4c"
    }]
  }
}
```

请求参数：

| **字段名**           |    **类型** | **是否必填** | **描述**                           |
| :------------------- | ----------: | :----------- | :--------------------------------- |
| blobId               |  String(20) | 是           | blobId(上一小节返回的blobId)       |
| signerList           |        List | 是           | 签名者列表（注意：需要源用户签名） |
| signerList.signBlob  | String(128) | 是           | 签名串                             |
| signerList.publicKey | String(128) | 是           | 签名者公钥                         |



返回成功的JSON：

```json
{
    "errorCode": "0",
    "message": "操作成功",
    "data": {
      	"txHash": "083b797ea00204d2c17a9e1bd77cab11385f475da756f76a96b922e4c7b8f7c7"
    }
}
```

返回数据：

| **字段名** | **类型** | **描述**                                    |
| :--------- | :------- | :------------------------------------------ |
| txHash     | String   | 链上交易hash,（可用于查询上链交易最终状态） |

#### 8.3.2 骨干节点反馈跨链交易

##### 8.3.2.1  获取交易blob

接口说明：

子链跨链网关监听到当前子链的跨链交易结束后，将构建反馈交易发送至主链，该接口用于构建交易的Blob内容。

```json
http请求方式：POST
https://{url}/crosschain/backbone/acked/tx/blob

{
    "accessToken": "{{accessToken}}",
    "params": {
        "gatewayAddress": "did:bid:efb8Ehu9SGVCTm2BbvpPiQjzNWo4LQAA",
        "crossTxNo": "by01:0:55bee90c7ce3e658a1729a7cc5ffb773",
        "result": "1",
        "extension": "extension",
        "version": "1000",
        "proof": {
            "ledgerSeq": "ledgerSeq",
            "txHash": "txHash"
        }
    }
}
```

请求参数说明：

| 变量            | 类型       | 描述                                                         |
| --------------- | ---------- | ------------------------------------------------------------ |
| gatewayAddress  | String(64) | 跨链网关BID地址（在平台链详情中查看该地址），如：**did:bid:eft4DztdgVgph2bmij93REU7kzrET7Do** |
| crossTxNo       | String     | 跨链交易编号                                                 |
| result          | String     | 跨链交易结果。"0"成功，"1"失败，"2":"超时"                   |
| extension       | String     | 扩展信息。可以不填                                           |
| version         | String     | 版本信息，由原始用户发起交易携带，填写源链跨链合约的版本信息 |
| proof           | Object     | 子链的跨链存证对象                                           |
| proof.ledgerSeq | String     | 子链上跨链交易的区块号                                       |
| proof.txHash    | String     | 子链上跨链交易的哈希值                                       |

接口调用成功，则返回JSON数据示例为：

```json
{
    "errorCode": 0,
    "message": "操作成功",
    "data": {
        "blobId": "15",
        "txHash": "d60c15bacbb7f98f00921d5ad17d49fafe0c65ea706d5d4ced25e490ac35e9e0",
        "blob": "0A286469643A6269643A65663477776B727A53564650545A4A627257433439446B416554535169377241101E228304080712286469643A6269643A656662384568753953475643546D32426276705069516A7A4E576F344C51414152D4030A286469643A6269643A6566427070397753676564315A5A72437566426D36374A4C55635255396375321AA7037B226D6574686F64223A227375626D6974426C6F636B486561646572222C22706172616D73223A7B22626C6F636B486561646572223A7B226163636F756E745F747265655F68617368223A22222C22636C6F73655F74696D65223A2231353933333338343030303032303030222C22636F6E73656E7375735F76616C75655F68617368223A22222C22666565735F68617368223A22222C2268617368223A2235323134303538363966626533633132396237383434383232333566616365633435656438653432313666633834653962623665336136386661653430636232222C2270726576696F75735F68617368223A2231323334363966626533633132396237383434383232333566616365633435656438653233313435363537646161313233343131326136386661653430636232222C22736571223A223938222C2274785F636F756E74223A223132222C2276616C696461746F72735F68617368223A22696E7465726E616C436861696E496432222C2276657273696F6E223A22227D2C22636861696E436F6465223A2262793031222C2264796E616D6963496E666F223A7B7D7D7D3080EAADE907"
    }
}
```

返回数据：

| **字段名** | **类型** | **描述**     |
| :--------- | :------- | :----------- |
| blobId     | String   | blodId       |
| txHash     | String   | 链上交易hash |
| blod       | String   | blod串       |

##### 8.3.2.2 提交交易（上链）

接口说明：

子链网关监听到当前子链的跨链交易结束后，将构建反馈交易发送至主链，该接口用于提交交易上链。

```json
http请求方式：POST
https://{url}/crosschain/backbone/acked/tx/submit
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJDcHB0NHRuMFlFZkIyMHJXIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjE5MzQyNDIyLCJiaWQiOiJkaWQ6YmlkOmVmMjhwTTlNRzNUR1hHeVdBVzRKcFdDRnNKRGQ1TUJuYyJ9.3gcQQBqvqtZH1q-TfUiGne68R1TnBcvLAA6nIJ8qDGU",
  "params": {
    "blobId": "3",
    "signerList": [{
      "signBlob": "4D47D46AA467C8DF8FD1D60374D9D2378AC536612C3B84E1AC9BA4EA55E10810482EC634EADED63A389F82573AB1E055698189F7497DF23C5E32CF26A7D8270F",
      "publicKey": "b06566086febd16adf6553cc9c000dba5f9f410d34f8541392303561dddc045b15ef4c"
    }]
  }
}
```

请求参数：

| **字段名**           |    **类型** | **是否必填** | **描述**                           |
| :------------------- | ----------: | :----------- | :--------------------------------- |
| blobId               |  String(20) | 是           | blobId(5.4.1返回blobId)            |
| signerList           |        List | 是           | 签名者列表（注意：需要源用户签名） |
| signerList.signBlob  | String(128) | 是           | 签名串                             |
| signerList.publicKey | String(128) | 是           | 签名者公钥                         |



返回JSON：

```json
{
    "errorCode": "0",
    "message": "操作成功",
      "data": {
      "txHash": "083b797ea00204d2c17a9e1bd77cab11385f475da756f76a96b922e4c7b8f7c7"
    }
}
```

返回数据：

| **字段名** | **类型** | **描述**                                    |
| :--------- | :------- | :------------------------------------------ |
| txHash     | String   | 链上交易hash,（可用于查询上链交易最终状态） |

### 8.4 跨链交易类型及结构体定义

#### 8.4.1  主链积分转移（payloadType="0"）

| 变量           | 类型   | 描述         |
| -------------- | ------ | ------------ |
| payload        | Object | 主链积分信息 |
| payload.amount | String | 积分数量     |

#### 8.4.2 子链积分兑换（payloadType="1"）

场景使用：

-   子链积分兑换成主链星火令
-   子链积分兑换成其他子链积分
-   主链星火令兑换成子链积分

骨干节点的跨链网关需要承担兑换子链积分和转移主链积分的功能。

参数说明：

| 变量                  | 类型   | 描述             |
| --------------------- | ------ | ---------------- |
| payload               | Object | 源链积分信息     |
| payload.masterAmount  | String | 主链积分数量     |
| payload.srcAmount     | String | 源链积分数量     |
| payload.srcTokenRate  | String | 源链积分的汇率   |
| payload.destAmount    | String | 目标链积分数量   |
| payload.destTokenRate | String | 目标链积分的汇率 |

| **变量**              | **类型** | **描述**                       |
| --------------------- | -------- | ------------------------------ |
| payload               | Object   | 源链积分信息                   |
| payload.masterAmount  | String   | 主链星火令数量                 |
| payload.srcAmount     | String   | 源链积分数量                   |
| payload.srcTokenRate  | String   | 源链积分的汇率（从主链获取）   |
| payload.destAmount    | String   | 目标链积分数量                 |
| payload.destTokenRate | String   | 目标链积分的汇率（从主链获取） |

兑换汇率换算方式如下：

```json
masterAmount = srcAmount/srcTokenRate;  主链积分=源链积分/源链汇率

masterAmount = destAmount/destTokenRate;  主链积分=目标链积分/目标链汇率

srcAmount/srcTokenRate = destAmount/destTokenRate;  源链积分/源链汇率 = 目标链积分/目标链汇率
```

#### 8.4.3 合约互操作（payloadType="2"）

| 变量                        | 类型                | 描述                                                         |
| --------------------------- | ------------------- | ------------------------------------------------------------ |
| payload                     | Object              | 自定义消息                                                   |
| payload.contractMethod      | String              | 目标合约方法                                                 |
| payload.contractInput       | Array               | 合约参数。用户自定义                                         |
| payload.contractInput[0]    | Object/String/Array | 自定义参数一                                                 |
| payload.contractInput[n]    | Object/String/Array | 自定义参数N                                                  |
| payload.token               | Object              | 可以为空，源链积分信息当涉及到需要支付资产时候，可以使用子链积分兑换的协议进行扩展 |
| payload.token.masterAmount  | String              | 主链积分数量                                                 |
| payload.token.srcAmount     | String              | 源链积分数量                                                 |
| payload.token.srcTokenRate  | String              | 源链积分的汇率                                               |
| payload.token.destAmount    | String              | 目标链积分数量                                               |
| payload.token.destTokenRate | String              | 目标链积分的汇率                                             |

#### 8.4.4 数据传递（payloadType="3"）

源链用户发起，传递到目标链，最终以BID DOC方式存储

| 变量             | 类型   | 描述                                   |
| ---------------- | ------ | -------------------------------------- |
| payload          | Object | 数据对象                               |
| payload.document | Object | BID DOC对象，参考《BID协议》元数据章节 |



## 9 星火令

星火令转移、星火令查询、星火令申请等，暂缓开通。

### 9.1 获取转移星火令blob

接口说明：

星火·链网中的星火令资产进行转移。

请求参数：

| **字段名** | **类型**   | **是否必填** | **描述**                                                     |
| :--------- | :--------- | :----------- | :----------------------------------------------------------- |
| from       | String(64) | 是           | 源地址                                                       |
| to         | String(64) | 是           | 目的地址                                                     |
| amount     | String(30) | 是           | 数量（底层最小单位 需要乘以10的8次方；比如 转1个星火令 此时这块需要传 100000000） |

返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| blob       | String   | Blob串   |
| txHash     | String   | 交易Hash |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/xht/transfer/blob
{
    "accessToken": "",
    "params": {
        "from": "did:bid:ef29SVUnVVUfToJEKmeAVd9TY6fsDYBHi",
        "to": "did:bid:ef24NBA7au48UTZrUNRHj2p3bnRzF3YCH",
        "amount":123
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
  "data":{
"blob":"A296469643A6269643A6652665770673644675A6B3232346F7A5570644343654C5A6231394635705238511001225A080712296469643A6269643A666D6E7A42315A724156375966727933665759795071646B6A57776F7458635258622B0A296469643A6269643A6841706E5959474C4550614678467A35543946383374485A6242474338483776513080DAC40938E807"
  }, 
  "errorCode": 0,
  "message": "操作成功"
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

### 9.2 转移星火令提交

接口说明：

交易签名后，通过该接口提交转移星火令的交易。

请求参数：

| **字段名**           |    **类型** | **是否必填** | **描述**   |
| :------------------- | ----------: | :----------- | :--------- |
| blob                 |      String | 是           | 交易blob串 |
| txHash               |  String(64) | 是           | 交易hash   |
| signerList           |        List | 是           | 签名列表   |
| signerList.signBlob  | String(128) | 是           | 签名串     |
| signerList.publicKey | String(128) | 是           | 签名者公钥 |


返回数据：

| **字段名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| txHash     | String   | 交易hash |


示例：

（1）请求示例：

```plain
http请求方式：POST
https://{url}/v1/xht/transfer/submit
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcGlLZXkiOiJhdkNmcDdscU16SDVtTk5kIiwiaXNzIjoiQklGLUNIQUlOIiwiZXhwIjoxNjExNzQ0NjIwLCJiaWQiOiJkaWQ6YmlkOmVmejNvVFFHN0xZZktKRWlDeU1nNThOOVREZjl2cFd5In0.sCm7gaWX_nmIasUyo64tn5FeAqDaxxDn2Kb9Jixk2YI",
    "params": {
        "blob": "0A2D6469643A6269643A627930313A6566544268396A53374B68366D69754736537A5A434E5A554534314B6A62353410032280020807122D6469643A6269643A627930313A6566554E4370633174417238483463434C66617577736148654532425776453252CC010A2E6469643A6269643A627930313A6566323369775A5434576A317A41594469355237434E70323534744666444370721A99017B226D6574686F64223A227365744163636F756E74537461747573222C22706172616D73223A7B227375626D697454696D65223A313630373037303633393835372C226465737441646472657373223A226469643A6269643A627930313A656662504A34697732315755365A43687576374E6A587531374466484D765176222C2272656D61726B73223A22222C22737461747573223A307D7D3080EAADE907",
        "txHash": "6592488eba4c85566cf853285fa98d840e83734d74d0e4fa336af45a75581496",
         "signerList": [{
            "signBlob": "B91AB8D815D3230AC678AE560351A10CC536470F80C6B0B89498BB0DA2811A1A5500AAE1AAE25EF05FBC6FB0F9CBE919779C28F424629E7B324E9EA81924550D",
            "publicKey": "b065667cc1e4584bc9ddb6806c455bdea9f8390724a77a6ed2f6369c830043418c0745"
        }]
    }
}
```

（2）返回结果示例：

a. 接口调用成功，则返回JSON数据示例为：

```plain
{
  "errorCode": 0,
  "message": "操作成功",
  "data": {
    "txHash": "6592488eba4c85566cf853285fa98d840e83734d74d0e4fa336af45a75581496"
  }
}
```

b. 接口调用失败，则返回JSON数据示例为：

```plain
{
    "errorCode": 940000,
    "message": "系统内部错误"
}
```

## 10 证件及行业类型表

（1）证件类型表

|**值**|**描述**|
|:----|:----|
|1|身份证|
|2|护照|
|3|港澳居民来往内地通行证|
|4|台湾居民来往内地通行证|
|5|外国人永久居留身份证|
|6|港澳居民居住证|
|7|台湾居民居住证|
|8|工商营业执照|


（2）行业类型表

|**值**|**描述**|
|:----|:----|
|A|农、林、牧、渔业|
|B|采矿业|
|C|制造业|
|D|电力、热力、燃气及水生产和供应业|
|E|建筑业|
|F|批发和零售业|
|G|交通运输、仓储和邮政业|
|H|住宿和餐饮业住宿和餐饮业|
|I|信息传输、软件和信息技术服务业|
|J|金融业|
|K|房地产业|
|L|租赁和商务服务业|
|M|科学研究和技术服务业|
|N|水利、环境和公共设施管理业|
|O|居民服务、修理和其他服务业|
|P|教育|
|Q|卫生和社会工作|
|R|文化、体育和娱乐业|
|S|公共管理、社会保障和社会组织|
|T|国际组织|


## 11 区块链与节点类型表

（1）区块链套餐类型表：

|**值**|**描述**|
|:----|:----|
|0|主链|
|1|骨干链|
|2|基础子链|
|3|行业子链|
|4|区域子链|


（2）星火·链网节点类型表：

|**值**|**描述**|
|:----|:----|
|0|超级节点（在主链）|
|1|监管节点（在主链）|
|2|骨干节点（在骨干链、子链或独立节点）|
|3|业务节点（在子链）|
|4|服务节点（在主链）|
|5|骨干链节点（在骨干链）|


## 12 BID对象类型表

数字身份适用于标识对象需要自主控制私钥的应用场景，资源数据应用于标识对象不需自主控制私钥的场景，分类详情可根据业务实际拓展。

|分类|**值**|**描述**|
|:----|:----|:----|
|数字身份|101|人|
|数字身份|102|企业|
|数字身份|103|节点|
|数字身份|104|智能设备|
|数字身份|105|智能合约|
|资源数据|201|图片|
|资源数据|202|视频|
|资源数据|203|文档|
|资源数据|204|资源数据|
|凭证数据|205|凭证|
|AC号|206|AC号|
|其他|999|其他|


## 13 bid数字身份生成算法

使用bid-sdk-java,目前已开源

github地址 ：https://github.com/CAICT-DEV/BID-SDK-JAVA

```plain
SDK bidSdk = new SDK();
KeyPairEntity kaypairEntity = bidSdk.getBidAndKeyPair("abcd");//abcd为子链的AC号，可选
String publicKey = kaypairEntity.getPublicKey();
String privateKey = kaypairEntity.getPrivateKey();
String bid = kaypairEntity.getBid();
```
## 14 签名算法

使用bid-sdk-java

```plain
SDK bidSdk = new SDK();
byte[] signByte = bidSdk.signBlob(privateKey, HexFormatUtil.hexStringToBytes(blob));//privateKey为上一步数字身份生成算法生成的privateKey
String signBlob = HexFormat.byteToHex(signByte);
```
## 15 凭证类型表

| **值** | **描述** |
| :----- | :------- |
| 201    | 可信认证 |
| 202    | 学历认证 |
| 203    | 资质认证 |
| 204    | 授权认证 |
| ...    | 待扩展   |

## 16 服务类型表

| **值**         | **描述**     |
| :------------- | :----------- |
| DIDDecrypt     | 加解密服务   |
| DIDStorage     | 存储服务     |
| DIDRevocation  | 凭证吊销服务 |
| DIDResolver    | 解析服务     |
| DIDSubResolver | 子链解析服务 |
| ...            | 待扩展       |

## 17 服务分类表

| **值** | **描述**   |
| :----- | :--------- |
| 10000  | 数字资产化 |
| 10001  | 供应链管理 |
| 10002  | 供应链金融 |
| 10003  | 电子政务   |
| 10004  | 防伪溯源   |
| 10005  | 采购招标   |
| 10006  | 公益慈善   |
| 10007  | 版权保护   |
| 10008  | 电子存证   |
| 10009  | 工具       |
| 10010  | 医疗健康   |
| 10011  | 其他       |

## 18 错误码

|**值**|**描述**|
|:----|:----|
|0|成功|
|940000|系统内部错误|
|940001|无效请求参数|
|940002|apiKey不存在|
|940003|apiSecret错误|
|940004|无效的accessToken|
|940005|上传文件异常|
|940006|申请编号不存在|
|940012|激活数字身份异常|
|940014|bid地址格式错误|
|940016|没有访问权限|
|940017|申请链blob异常|
|940018|申请链提交交易异常|
|940019|子链提交交易参数错误|
|940020|签名串错误|
|940021|交易重复提交，请重新申请blob|
|940022|查询申请子链状态异常|
|940023|申请子链交易不存在|
|940024|查询节点信息异常|
|940025|骨干节点地址不存在|
|940026|认证已申请完成或申请中|
|940027|数字身份服务申请可信认证blob异常|
|940028|数字身份服务申请可信认证提交交易异常|
|940029|申请可信提交交易参数错误|
|940030|数字身份已经激活无需再激活|
|940031|数字身份未激活|
|940032|数字身份未可信|
|940033|链码不存在|

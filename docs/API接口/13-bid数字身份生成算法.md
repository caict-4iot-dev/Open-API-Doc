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


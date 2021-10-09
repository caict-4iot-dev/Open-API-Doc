# 二.获取API令牌

令牌（accessToken）是调用API接口的凭据。令牌的获取是有限制的，每个令牌的有效期为 2 小时，请自行做好令牌的管理，在令牌快过期时（比如1小时50分），重新调用接口获取。

```plain
http请求方式：POST
https://{url}/auth/getAccessToken
{
  "api_key":"did:bid:efVhH327qkpqfo9NpBks5tU1qJjw81Wn",
  "api_secret":"f174bf944fa9c57e1f452ec69df5a84c49caec90"
}
```

请求参数说明：

| **字段名** | **类型** | **是否必填** | **描述**   |
| :--------- | -------: | :----------- | :--------- |
| api_key    |   String | 是           | api_key    |
| api_secret |   String | 是           | api_secret |


成功的返回JSON数据：

```plain
{
    "data": {
        "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhcHBJZCI6IkZDMjFnN2JzMVI2TjJGWjMiLCJpc3MiOiJidW1vIiwiZXhwIjoxNTYxMDAyODIyfQ.hgVH0T6fLxk973U2fIj_ejDx5aJuzRFlg1VAUA2RgzM",
        "expireIn": 7200
    },
    "errorCode": 0,
    "message": "操作成功"
}
```

响应参数说明：

| **字段名**  | **类型** | **描述**            |
| :---------- | :------- | :------------------ |
| accessToken | String   | 请求API令牌         |
| expireIn    | Long     | 有效期 （单位：秒） |

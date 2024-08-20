# Gaker PaaS API

集客云 PaaS 平台目前仅开放以下接口，如需开放更多数据，请联系客服反馈给我们

* [通讯录](organization.md)
* [应用信息](object.md)
* [应用数据](data.md)
* [数据字典](dict.md)

### 简要规则

* OpenAPI的统一访问地址为：`https://openapi.gaker.com`
* 所有请求都必须通过HTTPS发起
* 所有请求均采用POST请求的方式，且格式均为JSON格式
* 数据传输编码统一为UTF-8
* 接口的请求频率限制：5次/秒/URL，即一个接口1秒钟最多请求5次

### 鉴权方式

为方便快速地与租户的系统的集成，目前集客云采用基于简单令牌的身份验证，租户使用 **租户ID** + **唯一凭证** 来获取访问令牌，访问令牌的有效期为7天 ( 基于实际情况，随时可能变更 )，过期后需要重新获取访问令牌。你在开发过程中无需关注访问令牌的有效期，如果令牌失效，接口会响应“令牌已失效”，这时候重新获取即可，具体的请求参数如下：

获取访问令牌的接口：

`/v1/open/authentication/getAccessToken`

请求参数：

```JSON
{
    "tenantId":"T0000001",                              // 租户ID
    "tenantSecret":"d724d609cd22448dba63262b3999a77a"   // 唯一凭证
}
```

取得访问令牌后，后续在访问其他接口时，需要在 HTTP Headers 中设置 Authorization 的值为访问令牌，以此来进行身份认证，与此同时，需要将租户ID一同放到Headers，即在后续的所有请求需要在Header中设置两个值：

```SQL
Authorization: bad1b2cd36e543bf845ea16d1698d489
x-api-tenantId: T0000001
```

需要注意的是，<mark style="color:orange;">**如果你在管理后台中重置了租户唯一凭证，将导致访问令牌失效，这时你需要使用新的唯一凭证来获取新的访问令牌**</mark>。前文已经提到，当令牌失效时，访问接口会响应“令牌已失效”，这时需要再次获取新的令牌，再重新访问接口，这是我们建议的处理方式。如果你觉得这种方式稍显繁琐，可以在程序中定时刷新访问令牌，这样可以保证访问接口时，拿到的是最新的访问令牌。你只需要在请求的 Body中，增加 isRefresh 参数，即：

```JSON
{
    "tenantId":"T0000001",                               // 租户ID
    "tenantSecret":"d724d609cd22448dba63262b3999a77a",   // 唯一凭证
    "isRefresh":true
}
```

请求成功后，旧的访问立牌会立即失效，所有请求均需要使用新的访问令牌，新的访问令牌有效期仍为7天。由于令牌的有效期较长，定时任务的时间间隔可以尽可能长一些，我们建议的时间间隔是1天。

### 对照表

接口使用 **状态码** + **错误码** 的响应方式来表示接口是否正常响应以及错误原因。调用接口时，接口将返回响应状态码：

<table data-header-hidden><thead><tr><th width="100"></th><th width="145"></th><th></th></tr></thead><tbody><tr><td>状态码</td><td>消息</td><td>说明</td></tr><tr><td>2xx</td><td>OK</td><td>响应成功</td></tr><tr><td>400</td><td>Bad Request</td><td>请求错误，响应数据中会返回详细的错误信息</td></tr><tr><td>500</td><td>Internal Server</td><td>内部错误，当程序出现未处理异常时，会响应此状态码，这时候请联系客服协助排查</td></tr></tbody></table>

当接口响应的状态码是400时，将返回下述详细的错误信息

<table data-header-hidden><thead><tr><th width="100"></th><th width="115"></th><th></th></tr></thead><tbody><tr><td>错误码</td><td>场景</td><td>说明</td></tr><tr><td>40001</td><td>所有接口</td><td>访问令牌已失效，请重新获取</td></tr><tr><td>40002</td><td>所有接口</td><td>未知异常，比如HTTP Header中没有授权字段等，请联系客服协助排查</td></tr><tr><td>40003</td><td>获取令牌时</td><td>租户还未生成唯一凭证，请在管理后台中进行相关操作</td></tr><tr><td>40004</td><td>查询数据时</td><td>查询数据出现异常，具体的错误信息请参考响应数据中的 msg 字段</td></tr><tr><td>40005</td><td>删除数据时</td><td>删除数据出现异常，具体的错误信息请参考响应数据中的 msg 字段</td></tr><tr><td>40006</td><td>编辑数据时</td><td>编辑数据出现异常，具体的错误信息请参考响应数据中的 msg 字段</td></tr><tr><td>40007</td><td>新增数据时</td><td>新增数据出现异常，具体的错误信息请参考响应数据中的 msg 字段</td></tr><tr><td>40008</td><td>所有接口</td><td>接口调用频率超限</td></tr><tr><td>40011</td><td>新增数据时</td><td>根据查重规则判断当前添加的数据为重复数据</td></tr></tbody></table>

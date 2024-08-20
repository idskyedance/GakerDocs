# 通讯录

### 获取用户列表

请求接口：`/v1/open/contact/users`

请求方法：`POST`

请求参数：

```json
{
    "pageNum":1,   // 页码
    "pageSize":20  // 每页记录数
}
```

响应数据：

```json
{
    "code": "0",
    "data": {
        "content": [                            // 分页数据，类型为：数组
            {
                "userId": "U000000000001224",   // 用户ID
                "userName": "周礼",              // 用户名称
                "userNamePhonetic": "zhouli"    // 用户名称拼音
            },
            {
                "userId": "U000000000001185",
                "userName": "kaixin",
                "userNamePhonetic": "kaixin"
            }
        ],
        "total": 10,                           // 数据总量，满足条件的总记录数
        "totalElem": 10                        // 当前分页数量，小于等于pageSize
    },
    "msg": "success"
}
```

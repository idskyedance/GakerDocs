# 数据字典

### 获取国家区号

请求接口：`/v1/open/base/countryCode`

请求方法：`GET`

请求参数：无

响应数据：

```json
{
    "code": "0",
    "data": [
        {
            "countryCode": "+86",
            "countryName": "中国大陆"
        },
        {
            "countryCode": "+886",
            "countryName": "中国台湾"
        },
        {
            "countryCode": "+852",
            "countryName": "中国香港"
        },
        ......
        {
            "countryCode": "+236",
            "countryName": "中非共和国"
        }
    ],
    "msg": "success"
}
```

### 获取行政区划

请求接口：`/v1/open/base/administrativeDivision`

请求方法：`GET`

请求参数：

```JSON
目前仅支持查询中国的行政区划
```

响应数据：返回数据是树形结构，比如下面展示的是四川省的行政区划

<pre class="language-json"><code class="lang-json">{
<strong>    "code":"0",
</strong><strong>    "data":[
</strong>        {
<strong>            "cityList":Array[22],
</strong><strong>            "id":"2284",
</strong><strong>            "pid":"2283",
</strong><strong>            "value":"成都市"
</strong>        },
        {
<strong>            "cityList":Array[6],
</strong><strong>            "id":"2304",
</strong><strong>            "pid":"2283",
</strong><strong>            "value":"自贡市"
</strong>        }
    ],
    "msg": "success"
}
</code></pre>

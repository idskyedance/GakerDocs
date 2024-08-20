# 应用信息

### 获取应用列表

请求接口：`/v1/open/object/list`

请求方法：`POST`

请求参数：无

响应数据：

```json
{
    "code": "0",
    "data": [
        {
            "objectApiName": "salesLeadObject",               // 应用ApiName
            "objectType": "SYSTEM_PRESET_OBJECT",             // 应用类型
            "objectDesc": "线索是与客户初次接触获得的原始信息...", // 应用描述
            "objectIcon": "图片链接",                          // 应用Icon地址
            "objectName": "线索"                              // 应用名称
        },
        {
            "objectApiName": "customerObject",
            "objectType": "SYSTEM_PRESET_OBJECT",
            "objectDesc": "客户",
            "objectIcon": "crm3-static.oss-cn-hangzhou.aliyuncs.com/public/object/salesLead.png",
            "objectName": "客户"
        },
        {
            "objectApiName": "S0000420SubTable",
            "objectType": "SUB_TABLE_OBJECT",
            "objectDesc": "客户联系人",
            "objectName": "O0000554Object_客户联系人"
        }
    ],
    "msg": "success"
}
```

应用类型objectType的取值如下：

* `SYSTEM_PRESET_OBJECT`：系统预设应用
* `CUSTOM_OBJECT`：租户自定义应用
* `SUB_TABLE_OBJECT`：子表单

### 获取应用字段列表

请求接口：`/v1/open/object/{objectApiName}/fields`

请求方法：`POST`

请求路径参数：

```JSON
objectApiName  应用的ApiName，比如：customerObject
```

响应数据：

目前系统的主要字段类型如下所示，当字段为 REF、SUMMARY、CALCULATE 类型的字段时，返回的数据中包含 valueType 属性，表示该计算或引用字段返回的实际字段类型，当字段为其他类型时，valueType的值与 fieldType 一致。

比如，字段类型为 汇总(SUMMARY) 和 计算(CALCULATE) 字段类型时，大概率的使用场景是用来计算某些字段的值，比如合同.合同金额 = SUM(订单.总额)，这时，valueType = 数字(NUMBER)或金额(MONEY)；

再比如，字段类型为 引用(REF)字段类型时，valueType 就等于这个字段所引用的那个字段的字段类型，比如，合同“单位”字段中引用了客户的“客户名称”字段，那么“单位”是引用字段，其实际的 valueType 因为文本类型。

返回的数据中，如果发现有 valueType = REF，那么大概率是因为这些字段关联的字段已经被删除，系统已经不能确认这个字段引用的实际类型了。

```Plain
 文本类型字段：
         NUMBER_AUTO：自增编号
         TEXT_SINGLE：单行文本
         TEXT_MULTI：多行文本
         TELEPHONE：手机
         EMAIL：邮箱
         WEBSITE：网址
         ADDRESS：地址
         
 数字类型字段：
         MONEY：金额
         NUMBER：数字
         PERCENT：百分比
         
 选项类型字段：
         SELECT_SINGLE：单选
         SELECT_MULTI：多选
         RADIO：单选按钮
         CHECK_BOX：复选框
         BOOLEAN：布尔值(已废弃)
         
时间类型字段：
        DATE：日期
        TIME：时间
        DATE_TIME：日期时间
         
地址类型字段：
        COUNTRY：国家
        COUNTRY_CITY：国家城市
        
用户类型字段：
        USER：单个用户
        USER_LIST：用户列表
        
特殊字段：
        LAYOUT：布局
        POOL：公海池
        LOCATION：定位
        RICH_TEXT：富文本
        PICTURE：图片
        FILE：文件
        BUSINESS_PROCESS：商机阶段(已废弃)
        
计算字段：
    REF：引用字段
    SUMMARY：汇总字段
    CALCULATE： 计算字段
```

<pre class="language-json"><code class="lang-json">{
<strong>    "code":"0",
</strong><strong>    "data":[
</strong>        {
<strong>            "fieldApiName":"customerLevel", // 字段ApiName
</strong><strong>            "fieldName":"客户级别",          // 字段名称
</strong><strong>            "fieldType":"SELECT_SINGLE",    // 字段类型
</strong><strong>            "valueType":"SELECT_SINGLE",    // 字段实际类型
</strong><strong>            "optionList":[                  // 如果字段是选项字段，将返回选项值 
</strong>                {
<strong>                    "optionKey":"O0000000",
</strong><strong>                    "optionValue":"重要客户"
</strong>                },
                {
<strong>                    "optionKey":"O0000001",
</strong><strong>                    "optionValue":"一般客户"
</strong>                },
                {
<strong>                    "optionKey":"O0000002",
</strong><strong>                    "optionValue":"普通客户"
</strong>                }
            ]
        },
        {
<strong>            "fieldApiName":"addressField",
</strong><strong>            "fieldName":"详细地址",
</strong><strong>            "fieldType":"TEXT_MULTI",
</strong><strong>            "valueType":"TEXT_MULTI"
</strong>        },
        {
<strong>            "fieldApiName":"emailField",
</strong><strong>            "fieldName":"邮箱",
</strong><strong>            "fieldType":"EMAIL",
</strong><strong>            "valueType":"EMAIL"
</strong>        }
    ],
<strong>    "msg":"success"
</strong>}
</code></pre>

### 获取应用公海池列表

请求接口：`/v1/open/object/{objectApiName}/pools`

请求方法：`POST`

请求路径参数：

```undefined
objectApiName  应用的ApiName，比如：customerObject
```

响应数据：

```json
{
    "code": "0",
    "data": [
        {
            "poolId": "61a450d736329c5e3b5cfca0", // 公海池ID
            "poolName": "公海A"                    // 公海池名称
        },
        {
            "poolId": "626643543ff9146055525c4c",
            "poolName": "公海B"
        }
    ],
    "msg": "success"
}
```

### 获取应用布局列表

请求接口：`/v1/open/object/{objectApiName}/layouts?layoutType=CREATE_LAYOUT`

请求方法：`POST`

请求路径参数：

```JSON
objectApiName  应用的ApiName，比如：customerObject
```

请求查询参数：

系统约定，新建数据时必须传布局ID，这时候可以传 layoutType = CREATE\_LAYOUT 来获取该应用下所有新建布局 ( 你可以在管理后台为同一个应用配置不同的布局，不同布局上新建的字段可以不一致，必填字段也可以不同，比如你所在单位有销售和市场两个团队，这两个团队创建客户时需要填写的字段不一样，那么你就可以配置两个布局来满足这种业务场景 )

大多数使用接口的场景，都是为了新建数据来查询布局的，因此 layoutType 传 CREATE\_LAYOUT 即可

```JSON
layoutType 布局类型，其取值有：
CREATE_LAYOUT         新建页布局
DETAIL_LAYOUT         详情页布局
LIST_LAYOUT           列表页布局
CREATE_DETAIL_LAYOUT  新建,详情页布局
TABLE_HEADER_LAYOUT   Web端表头布局
ALL                   不返回布局详情
```

响应数据：

```json
{
    "code": "0",
    "data": [
        {
            "layoutId": "62451bdaa11bb70f823b6b70",  // 布局ID
            "layoutName": "Web端列表页",              // 布局名称
            "layoutType": "TABLE_HEADER_LAYOUT"      // 布局类型
        },
        {
            "layoutId": "PL000021",
            "layoutName": "移动端列表页",
            "layoutType": "LIST_LAYOUT"
        },
        {
            "layoutId": "60e552d954eb75428fca982c",
            "layoutName": "默认布局",
            "layoutType": "CREATE_DETAIL_LAYOUT"
        }
    ],
    "msg": "success"
}
```

### 获取应用布局字段列表

请求接口：`/v1/open/object/layout/field/{layoutId}`

请求方法：`POST`

请求路径参数：

```JSON
layoutId  布局ID，比如：60e552d954eb75428fca982c
```

响应数据：接口返回的数据与接口2一致，字段说明请移步到接口2，下面仅展示数据示例

```json
{
    "code": "0",
    "data": [
        {
            "fieldApiName": "mainField",
            "fieldName": "客户名称",
            "fieldType": "TEXT_SINGLE",
            "valueType": "TEXT_SINGLE"
        },
        {
            "fieldApiName": "customerSourceField",
            "fieldName": "客户来源",
            "fieldType": "SELECT_SINGLE",
            "valueType": "SELECT_SINGLE",
            "optionList": [
                {
                    "optionKey": "O0000000",
                    "optionValue": "SEO"
                },
                {
                    "optionKey": "O0000001",
                    "optionValue": "自媒体"
                }
            ]
        }
        {
            "fieldApiName": "ownerField",
            "fieldName": "归属人",
            "fieldType": "USER",
            "valueType": "USER"
        }
    ],
    "msg": "success"
}
```

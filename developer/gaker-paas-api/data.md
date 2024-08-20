# 应用数据

数据相关的许多接口涉及到系统底层字段的相关设计，建议简单浏览一下附录1，对系统所有字段类型的数据格式有一个简单的了解后再进行对接。在查看接口文档的时候，请着重关注橙色标注的文字，可能对你的开发有帮助。

### 分页获取指定应用数据

请求接口：`/v1/open/data/query`

请求方法：`POST`

请求参数：

<pre class="language-json"><code class="lang-json">{
<strong>    "objectApiName":"customerObject",   // 应用ApiName，即要查询哪个应用的数据
</strong><strong>    "fieldApiNames":[                   // 返回的字段列表
</strong><strong>        "customerScaleField",
</strong><strong>        "websiteField"
</strong>    ],
<strong>    "customQueryList":[                 // 特殊字段过滤条件，具体请参考附录2
</strong>        {
<strong>            "keyName":"associateField",
</strong><strong>            "keyValue":{
</strong><strong>                "fieldApiName":"associateCustomerField",
</strong><strong>                "itemQueryType":"REGEX_CONTAIN_IGNORE",
</strong><strong>                "associateObjectApiName":"customerObject",
</strong><strong>                "keyword":"A"
</strong>            }
        }
    ],
<strong>    "itemQueryList":[                  // 常规字段过滤条件，具体请参考附录2
</strong>        {
<strong>            "itemName":"customerScaleField",
</strong><strong>            "itemQueryType":"IN",
</strong><strong>            "itemValue":[
</strong><strong>                "O0000001"
</strong>            ]
        }
    ],
<strong>    "pageNum":1,                      // 分页参数：页码
</strong><strong>    "pageSize":5                      // 分页参数：每页记录数
</strong>}
</code></pre>

对部分请求参数的说明：

* `fieldApiNames`可以不传，不传将仅返回`数据ID`、`创建人`、`创建时间`、`名称`4个字段
* `fieldApiNames`最多仅支持传20个字段，加上上一条中的4个字段，也就说，查询数据列表时最多支持返回24个字段，如果查询更多字段，请分别获取数据详情
* `pageSize`支持的最大值为200
* 目前接口不支持自定义排序
* 返回列表数据时，将只返回基本数据，比如：选项字段，将仅返回optionKey，如需查看字段的完整数据，请获取数据详情
* 如果该指定的查询字段在数据中没有值，将不返回相应字段，注意在代码中进行判断
* 过滤条件的传值，请参考附录2

响应数据：

```json
{
    "code": "0",
    "data": {
        "content": [
            {
                "dataId": "62ea0fab0b0d0c7ef92cc325",  // 数据ID
                "createUserField": {                   // 创建人
                    "stringValue": "U000000000101984"
                },
                "createTimeField": {                   // 创建时间
                    "timeValue": 1659506580000
                },
                
                "mainField": {                         // 标题字段(主属性)
                    "stringValue": "83-2"
                },
                "customerScaleField": {                // 客户规模(选项字段)
                    "optionKey": "O0000001"
                }
            }        
        ],
        "total": 1,
        "totalElem": 1
    },
    "msg": "success"
}
```

返回的列表数据中，不同的字段类型其数据格式略有差异，具体可参考附录1。

### 获取数据详情

请求接口：`/v1/open/data/detail/{objectApiName}/{dataId}`

请求方法：`POST`

请求路径参数：

```JSON
objectApiName  应用的ApiName，比如：customerObject
dataId         数据唯一ID，比如：62fb35c16879fb0aa6f18f26
```

响应数据：

```json
{
    "code": "0",
    "data": {
        "F0017278Field": {
            "associateObjectApiName": "productObject",
            "associateObjectName": "产品",
            "associateMainFieldValue": "707-1",
            "associateTitleFieldValue": "707-1",
            "associateDataId": "62c64fd5961d3e312ae59e3f",
            "layoutId": "PL000122"
        },
        "F0017910Field": {
            "countryCode": "+86",
            "telephone": "13222222222"
        },
        "F0017919Field": {
            "stringValue": "玖佰圆整"
        },
        "F0017097Field": {
            "optionValueList": [
                "2",
                "4"
            ],
            "optionKeyList": [
                "C0003514",
                "C0003516"
            ]
        },
        "F0017917Field": {
            "userNameList": [
                {
                    "photoUrl": "https://s1-imfile...",
                    "userName": "emilee-test",
                    "userId": "U000000000001173"
                }
            ],
            "stringValueList": [
                "U000000000001173"
            ]
        },
        "F0015718Field": {
            "doubleValue": "610",
            "numberValue": 610.0
        },
        "F0017903Field": {
            "timeValue": 1661307300000
        },
        "layoutField": {
            "optionKey": "PL000032",
            "layoutName": "默认布局"
        },
        "F0017912Field": {
            "optionKey": "O0000001",
            "optionValue": "小型企业(10-99)"
        }
    },
    "msg": "success"
}
```

### 新建数据

请求接口：`/v1/open/data/create`

请求方法：`POST`

**新建数据接口主要在于构建 dataDetail 属性的值，每个字段传值格式请参考附录1** 说明

1. `dataDetail` 中需要包含布局字段 `layoutField`，其格式为：

<pre class="language-json"><code class="lang-json"><strong>  "layoutField": {
</strong><strong>      "optionKey": "66bb0c133043bb1c09267ba2"
</strong>  }
</code></pre>

2. `isDuplicateCheck`表示在新建数据时，是否启用查重。需要注意的是，数据查重是一个比较耗时的操作，如果查重规则配置的比较复杂，甚至包含联合查重的话，耗时会更长一些。因此，我们一般建议不要开启此操作，如果开启，需要注意代码中超时时间的设置。

数据重复时，接口响应数据如下，代码中根据`code=40011`做相应的处理即可。

```json
{
    "code": "40011",
    "msg": "数据已存在，请勿重复添加"
}
```

请求参数：

```json
{
    "objectApiName":"customerObject",          // 应用ApiName
    "isDuplicateCheck": false,                 // 新建数据时是否启用查重
    "dataDetail":{
        "mainField":{                          // 文本字段
            "stringValue":"OpenAPI创建数据测试"
        },
        "customerScaleField":{                 // 选项字段
            "optionKey":"O0000001"
        },
        "telephoneField":{                      // 电话字段
            "countryCode":"+86",
            "telephone":"13456678345"
        },
        "emailField":{                         // 邮箱字段
            "stringValue":"aa@qq.com"
        },
        "countryCityField":{                   // 地址，行政区划编码
            "stringValueList":[                // 依次为：国家-省-市-区县-街道
                "1",
                "19",
                "21"
            ]
        },
        "addressField":{
            "stringValue":"详细地址"
        },
        "F0017839Field":{                      // 多选字段
            "optionKeyList":[
                "C0003583"
            ]
        },
        "participantField":{                   // 用户字段
            "stringValueList":[
                "U000000000001185"
            ]
        }
    }
}
```

响应数据：如果新建成功，会返回完整的数据对象，具体可直接测试查看；如果新建失败，会返回具体原因，比如

```json
{
    "code": "40007",                     // 错误码
    "msg": "客户联系电话:电话号码格式错误"   // 错误提示信息，冒号前面为字段名称
}
```

### 批量新建数据

请求接口：`/v1/open/data/batch/create`

请求方法：`POST`

请求参数：这里的“批量新建”指的是创建1条主应用数据的同时创建多条关联的从应用数据，比如：一个客户下有多个联系人，一个客户下也能会有多个商机，如果你想在创建客户的同时，创建多条联系人和商机数据，那么可以调用此接口。调用此接口需要注意：

1. 调用API接口创建的数据，如果没有指明创建人的话，创建人为空。建议数据中增加创建人字段或在管理后台设置默认的创建人；
2. 调用API接口创建的数据，请务必增加归属人字段(`ownerField`)，否则创建的数据除超级管理员外，无人有查看和编辑权限。如果不传的话，建议在管理后台设置默认的归属人；
3. 同时创建多个应用数据时，每条数据均需要传布局字段(`layoutField`)；
4. 系统在创建每条数据时，均需要对数据的字段格式、权限、必填等进行校验，创建的数据条数越多，耗时越长，请在代码中设置合理的超时时间。

请求参数的整体结构如下所示：

<pre class="language-json"><code class="lang-json">{
<strong>    "masterData": {                             // 新增的主应用数据
</strong><strong>        "objectApiName": "salesLeadObject",     // 主应用ApiName
</strong><strong>        "dataDetail": Object{...}               // 主应用数据详情
</strong>    },
<strong>    "associateDataMap": {                       // 新增的从应用数据，数据结构为字典
</strong><strong>        "O0000010Object": [                     // 字典的Key=从应用ApiName
</strong>            Object{...},                        // 字典的Value=从应用数据列表
            {//从应用列表中每条数据的格式都是 objectApiName+dataDetail 的格式，与masterData一致
<strong>                "dataDetail": Object{...},
</strong><strong>                "objectApiName": "O0000010Object"
</strong>            }
        ],
<strong>        "O0000011Object": [
</strong>            Object{...},
            Object{...}
        ]
    }
}
</code></pre>

`dataDetail`中属性如何传值，请参考：附录1。请求参数的完整示例：

```json
{
    "masterData": {
        "objectApiName": "salesLeadObject",
        "dataDetail": {
            "mainField": {
                "stringValue": "B公司"
            },
            "ownerField": {                     // 归属人，用户字段
                "stringValue": "U6530A3BED9611E7B50DDF747"
            },
            "participantField": {              // 参与人，用户列表字段
                "stringValueList": [
                    "U6530A3BED9611E7B50DDF747"
                ]
            },
            "sourceDetailField": {            // 选项字段
                "optionKey": "C0000004"
            },
            "areaField": {
                "addressCodes": [
                    "1"
                ]
            },
            "companyScaleField": {
                "optionKey": "OPC0000742"
            },
            "industryField": {
                "optionKey": "OPC0000745"
            },
            "layoutField": {                // 布局字段
                "optionKey": "PL000002"
            }
        }
    },
    "associateDataMap": {
        "O0000010Object": [
            {
                "dataDetail": {
                    "F0000484Field": {
                        "stringValue": "工程部"
                    },
                    "layoutField": {
                        "optionKey": "6621bf1f89b083468c2d70c5"
                    },
                    "F0000481Field": {
                        "countryCode": "+86",
                        "telephone": "13587654321"
                    },
                    "F0000482Field": {
                        "stringValue": "zhangsan@gaker.com"
                    },
                    "mainField": {
                        "stringValue": "张三"
                    },
                    "F0000483Field": {
                        "stringValue": "总工程师"
                    }
                },
                "objectApiName": "O0000010Object"
            },
            {
                "dataDetail": {
                    "F0000484Field": {
                        "stringValue": "监理部"
                    },
                    "layoutField": {
                        "optionKey": "6621bf1f89b083468c2d70c5"
                    },
                    "F0000481Field": {
                        "countryCode": "+86",
                        "telephone": "156987456321"
                    },
                    "F0000482Field": {
                        "stringValue": "lisi@gaker.com"
                    },
                    "mainField": {
                        "stringValue": "李四"
                    },
                    "F0000483Field": {
                        "stringValue": "总监工"
                    }
                },
                "objectApiName": "O0000010Object"
            }
        ],
        "O0000011Object": [
            {
                "dataDetail": {
                    "F0000508Field": {
                        "optionKey": "OTHKU56N"
                    },
                    "layoutField": {
                        "optionKey": "6621bfc989b083468c2d70c8"
                    },
                    "mainField": {
                        "stringValue": "张三"
                    },
                    "ownerField": {
                        "stringValue": "U6530A3BED9611E7B50DDF747"
                    },
                    "F0000510Field": {
                        "stringValue": "就想试试"
                    }
                },
                "objectApiName": "O0000011Object"
            },
            {
                "dataDetail": {
                    "F0000508Field": {
                        "optionKey": "OT4LBP8I"
                    },
                    "layoutField": {
                        "optionKey": "6621bfc989b083468c2d70c8"
                    },
                    "mainField": {
                        "stringValue": "王麻子"
                    },
                    "ownerField": {
                        "stringValue": "U6530A3BED9611E7B50DDF747"
                    },
                    "F0000510Field": {
                        "stringValue": "第二个产品也想试试"
                    }
                },
                "objectApiName": "O0000011Object"
            }
        ]
    }
}
```

### 修改指定数据

请求接口：`/v1/open/data/updateDataById`

请求方法：`POST`

请求参数：**修改数据时，需要修改哪个字段，就在dataDetail中传哪个值**

```json
{
    "objectApiName":"customerObject",           // 应用ApiName，必填
    "dataId":"631e8d2c8f2192675acd7920",        // 数据ID，必填
    "dataDetail":{
        "mainField":{                           // 修改标题字段
            "stringValue":"OpenAPI修改数据测试"
        }
    }
}
```

响应数据：如果新建成功，响应成功的错误码

```json
{
    "code": "0",
    "msg": "success"
}
```

如果新建失败，响应失败的错误码并返回错误提示信息，比如：

```json
{
    "code": "40006",
    "msg": "数据已被业务流程锁定，无法执行此操作"
}
```

### 根据条件批量修改数据

**由于通过OpenAPI修改数据时，权限较高，为保证数据安全，此接口有如下两个限制：**

1. 如果根据条件能够查询出多条数据，最多仅能修改前100条
2. 一次请求仅能修改1个字段

请求接口：`/v1/open/data/updateDataByCondition`

请求方法：`POST`

请求参数：

过滤条件参数有两个：`itemQueryList` 和 `customQueryList`，具体可参考接口1，也可以在电脑端使用浏览器访问对应应用，然后根据字段条件筛选，查看控制台，OpenAPI接口的参数与电脑端查询数据传的参数是一致的。

```json
{
    "objectApiName": "productObject",           // 应用ApiName
    "itemQueryList": [                          // 过滤条件
        {
            "itemName": "productPriceField",
            "itemQueryType": "BETWEEN",
            "itemValue": [
                1,
                100
            ]
        }
    ],
    "fieldApiName": "F0016175Field",            // 修改哪个字段
    "fieldValue": {                             // 字段的值
        "stringValue": "OpenAPI批量修改数据"
    }
    "isEmpty": true         // 如果想要清空某个字段，此值传true，fieldValue不传
}
```

响应数据：

```json
{
    "code": "0",
    "data": {
        "objectApiName": "productObject",
        "objectName": "产品",
        "optDataResultDTOS": [    // 这里返回每条数据修改的具体结果
            {
                "dataId": "631a94588f2192675acd78ec", // 修改的数据ID
                "isSucc": true,                       // 是否修改成功
                "mainFieldValue": "编辑器测试01",       // 修改数据的标题字段值
                "objectName": "产品",                  // 应用名称
                "resultMsg": "成功"                    // 提示信息，如果修改失败，这里展示错误描述
            },
            {
                "dataId": "630ec5906f8aa4397451abc2",
                "isSucc": true,
                "mainFieldValue": "831",
                "objectName": "产品",
                "resultMsg": "成功"
            }
        ],
        "optFail": 0,    // 修改失败的条数
        "optSucc": 8,    // 修改成功的条数
        "optTotal": 8    // 修改总条数
    },
    "msg": "success"
}
```

### 删除指定数据

如果是删除子表单数据，建议使用接口8

请求接口：`/v1/open/data/delete/{objectApiName}/{dataId}`

请求方法：`POST`

请求路径参数：

```JSON
objectApiName  应用的ApiName，比如：customerObject
dataId         数据唯一ID，比如：62fb35c16879fb0aa6f18f26
```

响应数据：如果删除成功，响应成功的错误码，删除失败，响应失败的错误码，比如：

```json
{
    "code": "0",
    "msg": "success"
}
```

### 创建子表单数据

请求接口：`/v1/open/data/createSubTableData`

请求方法：`POST`

请求参数：

```json
{
    "objectApiName": "customerObject",     // 应用ApiName，必填
    "dataId": "631e9d8b8f2192675acd7945",  // 数据ID，必填
    "fieldApiName": "F0016821Field",       // 子表单字段ApiName，必填
    "dataDetails": [                       // 子表单数据，支持多条数据同时添加
        {
            "F0017875Field": {
                "stringValue": "OpenAPI测试1"
            },
            "F0017924Field": {
                "stringValue": "XXX"
            }
        },
        {
            "F0017875Field": {
                "stringValue": "OpenAPI测试2"
            },
            "F0017924Field": {
                "stringValue": "YYY"
            }
        }
    ]
}
```

响应数据：如果创建成功，响应成功的错误码，创建失败，响应失败的错误码，比如：

```json
{
    "code": "0",
    "msg": "success"
}
```

### 删除子表单数据

请求接口：`/v1/open/data/deleteSubTableData`

请求方法：`POST`

请求参数：

```json
{
    "objectApiName": "customerObject",         // 应用ApiName，必填
    "dataId": "631e9d8b8f2192675acd7945",      // 数据ID，必填
    "fieldApiName": "F0016821Field",           // 子表单字段ApiName，必填
    "subDataIds": ["631e9ea48f2192675acd795b"] // 删除的子表单数据ID列表
}
```

响应数据：如果删除成功，响应成功的错误码，删除失败，响应失败的错误码，比如：

```json
{
    "code": "0",
    "msg": "success"
}
```

### 添加动态

动态是一个比较特殊的应用，每条动态都需要关联一条应用数据，暂时还不能独立存在。因此，在使用接口创建动态数据时，有些字段是必须要传的。另外，其使用的接口与新建数据一致，只是传递的参数略有不同。

请求接口：`/v1/open/data/create`

请求方法：`POST`

请求参数：

<mark style="color:orange;">**在编码时，如果动态中没有自定义字段，不需要使用接口获取应用的字段列表，直接传递下面的3个必填字段即可。**</mark>

以下字段是必填字段：

```json
"dynamicAssociateField":{                           // 动态关联的数据信息
    "associateObjectApiName":"businessObject",      // 动态关联的应用ApiName
    "associateDataId":"63fdb572d2997b711bd189a6"    // 动态关联的具体数据
},
"dynamicTypeField":{                                // 动态类型,folloUp表示跟进记录,写死即可
    "optionKey":"followUp"
},
"followUpField":{                                   // 跟进记录的具体内容
     "stringValue":"<p>这里是跟进记录的详细信息</p>",   // 跟进记录的文本内容，支持html标签，必填
     "reminders":[                                  // 要@的人，可以调用通讯录接口获取用户ID，非必填
         "U000000000101344",
         "U000000000101066"
     ],
    "location":{                                    // 定位信息，非必填
        "addressName":"四川省成都市武侯区桂溪街道吉泰路61号朗基天香",  // 定位的文本信息
        "coordinate":[104.060067,30.538388]         // 定位的经纬度，仅支持高德地图的经纬度
    }
}
```

如果你在动态应用中添加了自定义字段，请按照下面的示例传递即可。

```json
{
    "objectApiName":"dynamicObject",   // 应用ApiName
    "dataDetail":{                     // 动态的完整信息
        "F0022162Field":{              // 自定义选项字段
            "optionKey":"C0003866"
        },
        "F0022166Field":{              // 自定义文本字段
            "stringValue":"234"
        },
        "F0022161Field":{              // 自定义时间或日期字段
            "timeValue":1677571480106
        },
        ......
        "dynamicAssociateField":{      // 前述的几个必填字段
            "associateObjectApiName":"businessObject",
            "associateDataId":"63fdb572d2997b711bd189a6"
        },
        "dynamicTypeField":{
            "optionKey":"followUp"
        },
        "followUpField":{
            "stringValue":"<p>这里是跟进记录的详细信息</p>",
            "reminders":[
                "U000000000101344",
                "U000000000101066"
            ],
            "location":{
                "addressName":"四川省成都市武侯区桂溪街道吉泰路61号朗基天香",
                "coordinate":[
                    104.060067,
                    30.538388
                ]
            }
        }
    }
}
```

注意：暂不支持上传图片响应数据：如果新建成功，会返回完整的数据对象，具体可直接测试查看；如果新建失败，会返回具体原因，比如

```json
{
    "code": "40007",                     // 错误码
    "msg": "客户联系电话:电话号码格式错误"   // 错误提示信息，冒号前面为字段名称
}
```

### 获取最近删除数据ID

在CRM中删除数据后，系统默认会保留15天，这时候超级管理员可以在管理后台中恢复部分已删除数据，15天以后，系统会物理删除这些数据，届时无法恢复。

部分客户会同步CRM中的数据到外部系统，当用户在CRM中删除数据后，目前还无法做到自动通知三方外部系统，所以，临时提供此接口用于获取已删除的数据ID。

后期，规划中的Webhook功能实现后，将会有更合理的方式来处理这种情况。

请求接口：`/v1/open/data/recentDeletedDataIds`

请求方法：`POST`

请求参数：

```json
{
    "objectApiName": "orderObject",  // 对象ApiName，必填
    "start": 1678174192000,          // 起始时间，单位：ms，选填
    "end": 1677174192000             // 截止时间，单位：ms，选填
}
```

`start`和`end`参数要么都传，要么都不传，不传的时候，默认返回最近删除的2000条数据；有值的时候，仅返回该时间范围内的数据，但最多返回2000条。

由于系统仅保留15天内的已删除数据，如果`start`和`end`的时间范围超过了这个时间范围，系统会根据规则缩小查询的时间范围，规则：`(Max(start,当前时间-15天), Min(end,当前时间)]。`

此接口最多返回2000条数据，如果在某个范围内删除的数据太多，建议缩小查询的时间范围。根据实际的业务场景，每天删除的数据不会太多，如果是定时任务，建议每天执行一次，且不传start和end参数即可。

响应数据：如果查询成功，返回已删除的数据ID列表；如果查询失败，会返回具体原因，比如

```json
{
    "code": "0",
    "msg": "success",
    "data": [
        "63144db3b85ca967b849cc3b",
        "63144da0b85ca967b849cc38"
    ]
}
```

### **附录1 不同字段类型的数据格式**

注意：

* 画中线的字段类型已经计划逐步废弃，仅供包含历史数据的租户使用；
* <mark style="color:orange;">**列表类型的接口，返回的数据中仅包含基本信息，而详情类接口，除了包含基本信息外，还增加了许多额外的信息**</mark>**，**比如，详情类型接口中返回的选项字段类型数据，除了选项的Key，还有选项的Label；再比如地址类型的数据，除了包含行政区划的编码，还包含编码代表的具体城市；比如用户类型字段，除了用户编号，还包含用户名和头像等，更多的字段请参考实际返回的数据。

<pre class="language-json"><code class="lang-json"><strong>// optionValueList、codeNameMap、userInfo等字段的信息仅在详情类接口返回
</strong><strong>"F0018070Field": {
</strong><strong>    "optionKeyList":["C0003610","C0003611"],
</strong><strong>    "optionValueList":["BB","CC"]
</strong>}
<strong>"F0018079Field":{
</strong><strong>    "addressCodes":["1","2283","2284","2288"],
</strong><strong>    "codeNameMap":{"1":"中国","2288":"武侯区","2284":"成都市","2283":"四川"},
</strong><strong>    "detail":"天府五街200号"
</strong> }
<strong> "ownerField":{
</strong><strong>     "stringValue":"U000000000001185",
</strong><strong>     "userInfo":{
</strong><strong>         "userId":"U000000000001185",
</strong><strong>         "photoUrl":"...",
</strong><strong>         "userName":"kaixin"
</strong>     }
}
</code></pre>

以下是具体的字段介绍：

**文本类型字段**

包含：单行文本(<mark style="color:red;">**TEXT\_SINGLE**</mark>)、多行文本(<mark style="color:red;">**TEXT\_MULTI**</mark><mark style="color:red;">)</mark>、自增编号(<mark style="color:red;">**NUMBER\_AUTO**</mark>)、用户(<mark style="color:red;">**USER**</mark>)、邮箱(<mark style="color:red;">**EMAIL**</mark>)、网址(<mark style="color:red;">**WEBSITE**</mark>)、国家(<mark style="color:red;">**COUNTRY**</mark>)，返回的数据格式如下：

```json
"createUserField": {
    "stringValue": "U000000000101984"
 }
```

**数值类型字段**

包含：金额(<mark style="color:red;">**MONEY**</mark>)、数字(<mark style="color:red;">**NUMBER**</mark>)、百分比(<mark style="color:red;">**PERCENT**</mark>)，返回的数据格式如下：

<pre class="language-json"><code class="lang-json"><strong>"F0017942Field":{
</strong><strong>    "doubleValue":"24.00",
</strong><strong>    "numberValue":24
</strong>}
</code></pre>

**时间类型字段**

包含：日期时间(<mark style="color:red;">**DATE\_TIME**</mark>)、日期(<mark style="color:red;">**DATE**</mark>)、~~时间(TIME)~~，返回的数据格式如下：

<pre class="language-json"><code class="lang-json"><strong> "createTimeField":{
</strong><strong>     "timeValue":1660535820000
</strong>}
</code></pre>

**文本数组字段**

包含：国家城市(<mark style="color:red;">**COUNTRY\_CITY**</mark>)、用户列表(<mark style="color:red;">**USER\_LIST**</mark>)，返回的数据格式如下：

<pre class="language-json"><code class="lang-json"><strong>"F0018078Field":{
</strong><strong>     "stringValueList":[
</strong><strong>         "U000000000001190",
</strong><strong>         "U000000000001196"
</strong>    ]
}
</code></pre>

**单选类型字段**

包含：下拉单选(<mark style="color:red;">**SELECT\_SINGLE**</mark>)、单选按钮(<mark style="color:red;">**RADIO**</mark>)、布局(<mark style="color:red;">**LAYOUT**</mark>)、~~布尔(BOOLEAN)、单选复合文本(TEXT\_COMPOSITE)~~，返回的数据格式如下：

<pre class="language-json"><code class="lang-json"><strong> "F0018069Field":{
</strong><strong>     "optionKey":"C0003605"
</strong>}
</code></pre>

**多选类型字段**

包含：下拉多选(<mark style="color:red;">**SELECT\_MULTI**</mark>)、多选按钮(<mark style="color:red;">**CHECK\_BOX**</mark>)，返回的数据格式如下：

<pre class="language-json"><code class="lang-json">"F0018078Field":{
<strong>    "optionKeyList":[
</strong><strong>        "C0003610",
</strong><strong>        "C0003611"
</strong>    ]
}
</code></pre>

**地址字段**

其类型为 <mark style="color:red;">**ADDRESS**</mark>**，**其中addressCodes为行政区划编码，detail为详细地址，详细地址可能为空，返回的数据格式如下：

<pre class="language-json"><code class="lang-json">"F0018079Field":{
<strong>    "addressCodes":[
</strong><strong>        "1",
</strong><strong>        "2283",
</strong><strong>        "2284",
</strong><strong>        "2288"
</strong>    ],
<strong>    "detail":"天府五街200号"
</strong>}
</code></pre>

**手机字段**

其类型为 <mark style="color:red;">**TELEPHONE**</mark>**，**其中countryCode为区号，telephone为电话号码，返回的数据格式如下：

```json
"F0018090Field":{
    "countryCode": "+86",
    "telephone": "16698745632"
}
```

**图片和文件字段**

包含：文件(<mark style="color:red;">**FILE**</mark>)、图片(<mark style="color:red;">**PICTURE**</mark>)，其中fileName为上传时文件的名称，fileSize为文件大小，fileUrl为下载地址，注意：**fileUrl根据资源的不同，其链接的有效期不同**，返回的数据格式如下：

<pre class="language-json"><code class="lang-json">"F0018091Field":{
<strong>    "fileInfoList":[
</strong>        {
<strong>            "fileName":"Xnip2022-08-08_14-10-44.jpg",
</strong><strong>            "upLoadTime":1660629903713,
</strong><strong>            "fileSize":160289,
</strong><strong>            "fileUrl":"swfile.lwork.com/test/DT000000/4ba61eba-df91-4fd2-912a-ea3cd461022e.jpg",
</strong><strong>            "suffix":"jpg"
</strong>        }
    ]
}
</code></pre>

**关联字段**

包含：关联应用(<mark style="color:red;">**OBJECT\_ASSOCIATE**</mark>)、~~主从关联(SLAVE\_ASSOCIATE)~~，associateObjectApiName为关联的应用ApiName，associateDataId为关联的数据ID，返回的数据格式如下：

```json
"F0018092Field":{
    "associateObjectApiName":"customerObject",
    "associateDataId":"62fafac66879fb0aa6f18f13"
}
```

**子表单字段**

其中subTableId为子表单应用的ApiName，subDataIds为子表单的数据ID列表，其返回的数据格式如下：

```json
"F0016821Field": {
    "subDataIds": ["62fb35c16879fb0aa6f18f26"],
    "subTableId": "S0000407SubTable"
}
```

**引用字段**

最后是引用字段(<mark style="color:red;">**REF**</mark>)，引用字段返回的数据，与其引用的具体字段一致，比如，字段A引用的是一个文本字段，那么A返回的数据，就是文本格式的数据。

### **附录2 过滤条件如何传值**

查询数据列表时，建议使用一些基础字段来过滤，这样能比较简单快速的获取结果，而且有些字段也不适合进行条件过滤，比如文件、富文本、多行文本、详细地址等等常见的适合进行条件过滤的字段类型有：日期时间类型、单选或多选类型、单行文本、数字类型等，下面仅介绍一些常见的使用场景，如果仍不能满足你的需求，请 [联系客服](https://applink.feishu.cn/Td5Qjh6v) 反馈给我们。在查询数据时，如果没有特殊说明，其传值的形式都是这样的：

```json
{
    "itemName": "字段名称",
    "itemQueryType": "EQ",
    "itemValue": "字段的值"
}
// 比如下面的示例表示：查询 客户名称 = "集客云" 的数据
{
    "itemName": "customerName",
    "itemQueryType": "EQ",
    "itemValue": "集客云
}
```

**基础类型字段过滤传值**

**文本类型**

* 精确匹配：`itemQueryType = EQ`
* 模糊匹配且忽略大小写：`itemQueryType = REGEX_CONTAIN_IGNORE`

**数值类型**

* 大于：`itemQueryType = GT`
* 小于：`itemQueryType = LT`
* 等于：`itemQueryType = EQ`
* 大于等于：`itemQueryType = GTE`
* 小于等于：`itemQueryType = LTE`
* 介于：`itemQueryType = BETWEEN`，此时，`itemValue传数组：[a,b] 其中 a < b`

**日期时间类型**

同数值类型，itemValue传数字，为时间戳，比如：

```json
{
    "itemName": "createTimeField", 
    "itemQueryType": "BETWEEN", 
    "itemValue": [1660406400000, 1661011199999]
}
```

**单选类型**

* 等于：`itemQueryType = EQ`
* 不等于：`itemQueryType = NEQ`
* 属于：`itemQueryType = IN`
* 不属于：`itemQueryType = NIN`

当itemType为EQ和NEQ时，itemValue传字符串，当itemType为IN和NIN时，itemValue传数组，比如：

```json
// 表示查询：客户规模 = 小客户(O0000001) 的数据
// itemQueryType = NEQ 与此类似
{
    "itemName": "customerScaleField",
    "itemQueryType": "EQ",
    "itemValue": "O0000001"
 }
// 表示查询：客户规模 = 小客户(O0000001)或中客户(O0000002) 的数据
// itemQueryType = NIN 与此类似
{
    "itemName": "customerScaleField",
    "itemQueryType": "IN",
    "itemValue": ["O0000001","O0000002"]
 }
```

**多选类型**

多选字段，仅建议使用`itemQueryType = IN`，只要该字段值的数组与所传的数组有交集，就会命中该条数据，比如

```json
// 如果 F0018070Field = ["A", "B"]              - 不会命中
// 如果 F0018070Field = ["C0003609"]            - 命中
// 如果 F0018070Field = ["C0003609","A"]        - 命中
// 如果 F0018070Field = ["C0003609","C0003611"] - 命中
{
    "itemName": "F0018070Field",
    "itemQueryType": "IN",
    "itemValue": ["C0003609","C0003611"]
 }
```

**特殊类型字段过滤传值**

如需对一些特殊的字段进行过滤，就不能按照常规的方式来传值，为此，在接口中增加了另外一个参数来专门处理：**customQueryList，**其格式为数组，数组的每个元素由`keyName`和`keyValue`组成，其中 keyName 用于标识按照什么类型来过滤，keyValue为字段以及条件。

**地址字段**

<pre class="language-json"><code class="lang-json"><strong>// addressCodesList 格式为数组的数组
</strong>// 其每个元素为1个数组，表示1个具体的行政区划编码，其格式为[国家、省、市、区县]
// 目前中国的行政区划为21年最新版本，其他国家的行政区划数据比较旧
// 行政区划编码为系统自用，非国家或国际标准
<strong>"customQueryList":[
</strong>    {
<strong>        "keyName":"addressField",            // 固定值，必传，表示地址字段过滤
</strong><strong>        "keyValue":{
</strong><strong>            "fieldApiName":"F0018079Field",  // 字段ApiName
</strong><strong>            "itemQueryType":"IN",            // 表示属于某地
</strong><strong>            "addressCodesList":[             // 行政区划的数组集合
</strong>                ["1", "220", "221"]，        // 表示中国,山西,太原市
                ["1", "792"]，               // 表示中国,上海市
                ["3291"]，                   // 表示阿尔及利亚
                ["3347"]                     // 表示阿根廷
             ]
         }
     }
 ]
</code></pre>

**查找关联**

比如有两个应用「客户」和「订单」，其中「订单」应用中有一个关联字段“关联客户”，这时候你想查询客户名称中包含“A”的客户的所有订单，那么你可以按照下面的示例来传值，注意下面的所有字段均必传。

```json
"customQueryList":[
    {
        "keyName":"associateField",     // 固定值，必传，表示关联字段过滤
        "keyValue":{
            "fieldApiName":"associateCustomerField",  // 字段ApiName
            "itemQueryType":"REGEX_CONTAIN_IGNORE",   // 模糊匹配，建议值
            "associateObjectApiName":"customerObject",// 关联应用
            "keyword":"A"                             // 过滤关键字
        } 
     }
  ]
```

注意，系统在查找客户时，并非只匹配“客户名称”字段，而是去匹配「客户」字段中所有可搜索的字段。

> 备注：在管理后台可以对每个应用参与搜索的字段进行配置，如下图所示。

<figure><img src="https://gakerteam.feishu.cn/space/api/box/stream/download/asynccode/?code=NTI0YmM4MjBkZDVjNDdlYWYxNGE2NDU3ODRmYmRiN2FfUnNNQ2V3YkNhQXh2a0FlNXNsUWY0VHBKQkdCeGoybE9fVG9rZW46Ym94Y25MbTNlYmNDTkE4ak54ZlhweEFhcU1jXzE3MjM3ODk5NzE6MTcyMzc5MzU3MV9WNA" alt=""><figcaption><p>搜索字段配置界面</p></figcaption></figure>

最后，OpenAPI 中分页查询的接口请求参数与应用端获取数据发起请求的参数大致一样，如果你觉得理解困难，可以直接打开浏览器控制台，然后在 Web 应用中查询对应应用的数据，观察发起请求的数据，可直接复制到代码中使用。\

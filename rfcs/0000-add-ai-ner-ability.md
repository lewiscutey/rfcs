- 提案时间: 2023-11-21
- 影响版本: 1400
- 相关 Issues: 无

## 【必填】概述

命名实体识别（Named-Entity Recognition, NER）是用于识别给定文本中具有特殊含义的子文本段（电话，电影，地名，人名等）。

## 【必填】动机

方便开发者更便利的使用AI相关一些能力，降低开发难度，从而可以开发更丰富的快应用；

## 【必填】API规格和使用案例

#### 接口声明
```javascript
{ "name": "service.ai.ner" }
```

#### 导入模块
```javascript
import ner from '@service.ai.ner' 或 const ner = require('@service.ai.ner')
```

#### 接口定义
---
##### ner.getProvider()
检查当前设备是否支持NER，获取服务提供商。

##### 参数：
无

##### 返回值：
字符串，服务提供商的代号，如厂商的英文品牌名称，假如无此服务则返回空字符串;

##### 代码案例：
```javascript
console.log(ner.getProvider())
```

#


##### ner.detect(object)
对给定文本进行NER识别。

##### 参数：
| 参数名 | 类型 | 是否必填 | 说明 |
| :----- | :---- | :--: |:---------------------------------------------------------- |
| data | Object | 是 | 输入的数据资源 |
| config | Object | 否 | NER能力配置 |
| success | Function | 否 | 成功回调函数 |
| fail | Function | 否 | 失败回调函数 |

###### 参数 - data值：
| 参数名 | 类型 | 是否必填 | 说明 |
| :----- | :---- | :--: |:---------------------------------------------------------- |
| type | String | 是 | 输入的数据资源类型，可以为uri、rawString； |
| content | String | 是 | 与type匹配的资源描述； |

###### 参数 - data - type值：
| type参数值  | 说明 |
| :-----|:---------------------------------------------------------- |
| uri | content表示数据从文件获取，提供文件的本地路径，类型为String； |
| rawString | content表示数据实际内容，类型为String，表示实际文本； |

###### 参数 - config值：
| 参数名 | 类型 | 是否必填 | 说明 |
| :-----: | :----: | :--: |:---------------------------------------------------------- |
| tags | String[] | 否 | 识别的目标实体类型，成员可取值：tel：电话号码；location：地名；car：车牌号码；id：身份证号码；train：列车班次； |


##### 返回值：
| 参数名 | 类型 | 是否必填 | 说明 |
| :-----: | :----: | :--: |:----------------------------------------------------------: |
| result | Array\<NERResult> | 是 | 识别出的实体； |

##### 返回值 NERResult值：
| 参数名 | 类型 | 是否必填 | 说明 |
| :----- | :---- | :--: |:---------------------------------------------------------- |
| type | String | 是 | 识别出的数据传递方式，与入参data中的type字段定义一致，这里一般为“rawString” |
| content | String | 是 | 识别出的数据描述，与入参data中的content字段含义一致，这里是识别出的实体文本； |
| start | Number | 是 | 实体文本在原文本的起始点； |
| end | Number | 是 | 实体文本在原文本的终止点； |
| tag | Number | 是 | 被识别出的实体类型； |

##### 代码案例：
```javascript
const text = "我的手机号码是18856321894"
ocr.detect({
    data: {
        type: 'rawString',
        content: text
    },
    config: {
        tags: ['tel']
    },
    success: (data) => {
        const entities = data.result
        for(const entity of entities){
            const {type, content, start, end, tag} = entity
        }
        console.log("NER success")
    }
})
```



## 【必填】提案人员是否愿意自行实现该功能

是：提案人员愿意在提案通过后自行实现该功能；

## 详细设计

可选，请向一个熟悉 hapjs内部实现的人讲解如何在 hapjs中实现这个功能，或讲解实现这一功能需要什么步骤。

## 缺陷

我们是不是可以不做这个功能，请考虑：

- 实现这个功能的投入：包括代码的复杂度、代码体积的增加、实现功能投入的人力
- 这个功能是不是不需要 hapjs提供，使用 hapjs的开发者也可以在应用层实现，甚至实现得更好
- 对 hapjs既有惯用开发习惯的影响
- 对已发布版本和现有功能的影响，以及用户进行迁移的成本
- 对其它未有代码实现的 RFC 提案的影响

## 替代选择

还有其他的方案也可以实现这个功能吗？

## 适配策略

如果我们实现了这个提案，有没有什么办法可以帮助开发者更好地适应这个改动？
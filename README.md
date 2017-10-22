# HiShop BaiChuan 2017

>***当面对企业的海量数据难题时你有优良的技术架构支撑吗？  
面对强大的竞争对手你是迎接挑战主动过招还是选择退缩？  
与凤凰同飞，必是俊鸟。  
与虎狼同行，必是猛兽！***

HiShop 2017 百川数据挑战赛给你一个尽情施展才华的舞台！

本次挑战赛重点考验参加团队应对海量零售基础业务数据的架构设计能力，对云计算设施资源的规划和合理使用能力。

## 赛制

赛事共分为三个阶段，以 **2 ~ 5人** 自发组成团队为单位参加比赛。

### 第一阶段（9月  日 - 9月  日）
由组委会将赛题公示，并接受参赛团队报名，并向参赛团队提供原始数据和验证程序，方便选手对结果进行纠错和校正。

### 第二阶段（9月  日 - 9月  日）
由参赛团队充分自由完成设计、实现和内部测试，共计一周时间。

### 第三阶段（9月  日）
进行现场公开评选，主办方向各参赛团队提供阿里云VPC环境内的三台同配置ECS供参赛团队部署测试自己的应用程序及相关依赖组件或服务，并要求所有团队在指定的时间内范围完成部署与联调。主办方使用评测程序依次对各团队的程序按照赛题要求中的技术规范进行自动测试并打分，最终由主办方宣部优胜团队并颁发相应奖项。

## 赛题
激增的品类数据和交易数据给系统的查询统计类功能的可用性形成了巨大的挑战，请设法在有限的计算资源前提下高效准确的完成以下查询统计接口，以基于HTTP协议的RESTful风格HTTP API对外暴露，计算结果以

```http
Content-Type:application/json;charset=UTF-8
```

JSON 数据格式返回

### 必须实现的HTTP API
#### 查询用户的个人交易记录
> GET /users/{***user_id***}/transactions

路径参数：
>
| 参数 | 说明 | 必填 | 示例 |
| :-- | :-- | :-- | :-- |
| user_id | 用户ID | 是 | 1001 |

请求参数：
>
| 参数 | 类型 | 说明 | 必填 | 示例 |
| :-- | :-- | :-- | :---- | :-- |
| date | string | 交易日期 | 是 | 201708 |

请求示例：
>
```HTTP
GET /users/123/transactions?date=201702（查询月度交易）
GET /users/123/transactions?date=20170228（查询指定日期交易）
```

响应示例：
>
```JSON
{
  "data": [
    {
      "order_id": "2017100113141566",
      "store_id": 2,
      "total_amount": 102400,
      "created_at": "2017-10-01T15:19:09.403CST",
      "items": [
        {
          "sku": 1,
          "price": 12800,
          "qty": 2,
          "amount": 25600
        },
        {
          "sku": 2,
          "price": 25600,
          "qty": 3,
          "amount": 76800
        }
      ]
    },
    {
      "order_id": "2017100913141581",
      "store_id": 2,
      "total_amount": 25600,
      "created_at": "2017-10-09T11:09:01.107CST",
      "items": [
        {
          "sku": 1,
          "price": 12800,
          "qty": 2,
          "amount": 25600
        }
      ]
    }
  ]
}
```

#### 门店业绩排名
> GET /store/rankings

路径参数：
> 无

请求参数：
>  
| 参数 | 类型 | 说明 | 必填 | 示例 |
| :-- | :-- | :-- | :-- | :-- |
| province | int | 省份 | 否 | 3 |
| by | string | 排名依据 | 是 | amount / count |
| from | string | 起始月份 | 是 | 201708 |
| to | string | 截止月份 | 是 | 201710 |

请求示例：
>
```HTTP
GET /sales/rankings?by=count&from=201708&to=201710（全国门店销量（订单总量）排名）
GET /sales/rankings?province=3&by=amount&from=201708&to=201710（某省门店销售额（订单金额）排名）
GET /sales/rankings?province=3&by=count&from=201708&to=201710（某省门店销销量（订单总量）排名）
```

返回结果示例：
>  
```JSON
{
  "data": [
    {
      "rank": 1,
      "store_id": 168,
      "total_count": 131072,
      "total_amount": 61872100
    },
    {
      "rank": 2,
      "store_id": 11,
      "total_count": 87112,
      "total_amount": 918712310
    },
    {
      "rank": 3,
      "store_id": 36,
      "total_count": 76232,
      "total_amount": 53246112
    }
  ]
}
```

#### 商品品类排名
> GET /categories/rankings

路径参数：
> 无

请求参数：
>  
| 参数 | 类型 | 说明 | 必填 | 示例 |
| :-- | :-- | :-- | :-- | :-- |
| by | string | 排名依据 | 是 | amount / count |
| from | string | 起始月份 | 是 | 201708 |
| to | string | 截止月份 | 是 | 201710 |

请求示例：
>
```HTTP
GET /categories/rankings?by=amount&from=201708&to=201710（基于销售额（订单项金额）排名）
GET /categories/rankings?by=count&from=201708&to=201710（基于销量（订单项商品数量）排名）
```

返回结果示例：
>  
```JSON
{
  "data": [
    {
      "rank": 1,
      "category_id": 23,
      "total_count": 131072,
      "total_amount": 61872100
    },
    {
      "rank": 2,
      "category_id": 53,
      "total_count": 87112,
      "total_amount": 918712310
    },
    {
      "rank": 3,
      "category_id": 19,
      "total_count": 76232,
      "total_amount": 53246112
    }
  ]
}
```

## 数据格式和样本

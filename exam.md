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
          "title": "iPhone X 128G 银灰"
          "price": 12800,
          "qty": 2,
          "amount": 25600
        },
        {
          "sku": 2,
          "title": "iPhone X 专用贴膜 - 透明"
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
          "title": "iPhone X 128G 银灰"
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
      "store_id": 3,
      "store_name": "静安寺店",
      "total_count": 131072,
      "total_amount": 61872100
    },
    {
      "rank": 2,
      "store_id": 4,
      "store_name": "陆家嘴店",
      "total_count": 87112,
      "total_amount": 918712310
    },
    {
      "rank": 3,
      "store_id": 1,
      "store_name": "王府井形象店",
      "total_count": 76232,
      "total_amount": 53246112
    }
  ]
}
```

#### 一级商品品类排名
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
      "category_id": 5,
      "category_name": "3C数码",
      "total_count": 131072,
      "total_amount": 61872100
    },
    {
      "rank": 2,
      "category_id": 1,
      "category_name": "服装",
      "total_count": 87112,
      "total_amount": 918712310
    },
    {
      "rank": 3,
      "category_id": 6,
      "category_name": "家具",
      "total_count": 76232,
      "total_amount": 53246112
    }
  ]
}
```

## 样本数据及格式
数据样本将以纯文本形式提供给所有参赛者，参考者在充分理解赛题的前提下自行选择数据库或缓存系统来存储这些数据，但不能改变内容本身。所有价格均以“人民币-分”为最小单位。

### 省份
>"ID","省份"  
"1","北京"  
"2","上海"  
"3","江苏"  
"4","浙江"  
"5","广东"  

### 门店
>"ID","店名","省属ID"  
"1","王府井形象店","1"  
"2","国贸店","1"  
"3","静安寺店","2"  
"4","陆家嘴店","2"  
"5","南京夫子庙店","3"  

### 品类
>"ID","类名","父类ID"  
"1","服装","-1"  
"2","男装","1"  
"3","女装","1"  
"4","连衣裙","3"  
"5","3C数码","-1"  
"6","家具","-1"  

### 商品规格
>"SKU","标题","售价","品类ID"  
"1","iPhone X 128G 银灰","12800","5"  
"2","iPhone X 专用贴膜 - 透明","25600","5"  
"3","【2017夏季热销】白色连衣裙","168800","3"  

### 订单项
>"订单ID","SKU","单价","数量","时间","用户ID","门店ID"  
"2017100113141566","1","12800","2","2017-10-01T15:19:09.403CST","1001","3"  
"2017100113141566","2","25600","3","2017-10-01T15:19:09.403CST","1001","3"  
"2017100913141581","1","12800","2","2017-10-09T11:09:01.107CST","1001","5"  
"2017102108432007","3","18500","1","2017-10-21T14:26:57.521CST","1013","1"  

## 完整数据样本下载地址：
（即将上线）

## 接口自测工具下载地址：
（即将上线）

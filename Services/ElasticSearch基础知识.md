# ElasticSearch 简单使用

### 创建与展示索引
```
$ PUT /customer?pretty
$ GET /_cat/indices?v
```
### 生成索引与查询
```
# 插入数据，
$ PUT /customer/_doc/1?pretty
{
  "name": "John Doe"
}
# 查询数据
$ GET /customer/_doc/1?pretty
# 索引查询
$ GET /customer/_search
# 文档查询
$ GET /customer/_doc/_search
# 分页
$ GET //customer/_doc/_search?size=5&from=10
```
### 删除索引
```
DELETE /customer?pretty
GET /_cat/indices?v
```

### 流程展示
```
$ PUT /customer
$ PUT /customer/_doc/1
{
  "name": "John Doe"
}
$ GET /customer/_doc/1
$ DELETE /customer
```
> 搜索地址如下:
`<REST Verb> /<Index>/<Type>/<ID>`

### 更新文档
```
# 插入数据
$ PUT /customer/_doc/1?pretty
{
  "name": "John Doe"
}
# 修改数据
$ PUT /customer/_doc/1?pretty
{
  "name": "new Jane Doe"
}
```
### 修改字段
```
# 修改信息
$ POST /customer/_doc/1/_update?pretty
{
  "doc": { "name": "Jane Doe" }
}
# 修改字段
$ POST /customer/_doc/1/_update?pretty
{
  "doc": { "name": "Jane Doe", "age": 20 }
}
# 脚本计算 
$ POST /customer/_doc/1/_update?pretty
{
  "script" : "ctx._source.age += 5"
}
```

### 批量处理
```
# 一次生成两条数据
$ POST /customer/_doc/_bulk?pretty
{"index":{"_id":"1"}}
{"name": "John Doe" }
{"index":{"_id":"2"}}
{"name": "Jane Doe" }
# 批量处理其他操作
$ POST /customer/_doc/_bulk?pretty
{"update":{"_id":"1"}}
{"doc": { "name": "John Doe becomes Jane Doe" } }
{"delete":{"_id":"2"}}
```

### 导入数据
```
# 导入当前目录下 accounts.json 文件中的数据
curl -H "Content-Type: application/json" -XPOST "localhost:9200/bank/_doc/_bulk?pretty&refresh" --data-binary "@accounts.json"
# 查看索引
curl "localhost:9200/_cat/indices?v"
```
> accounts.json 文件格式如下
`{"index":{"_id":"902"}}`
`{"account_number":938,"balance":9597,"firstname":"Sharron","lastname":"Santos","age":40,"gender":"F","address":"215 Matthews Place",             "employer":"Zenco","email":"sharronsantos@zenco.com","city":"Wattsville","state":"VT"}`

### 搜索接口
```
# 简写
$ GET /bank/_search?q=*&sort=account_number:asc&pretty
# 繁写
$ GET /bank/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ]
}
```
##### 偏移索引
```
# 搜索所有数据
$ GET /bank/_search
{
  "query": { "match_all": {} }
}
# 限制条数
$ GET /bank/_search
{
  "query": { "match_all": {} },
  "size": 1
}
# 设置偏移
$ GET /bank/_search
{
  "query": { "match_all": {} },
  "from": 10,
  "size": 10
}
# 设置排序
$ GET /bank/_search
{
  "query": { "match_all": {} },
  "sort": { "balance": { "order": "desc" } }
}
```
##### 关联搜索
```
# 设置返回字段
$ GET /bank/_search
{
  "query": { "match_all": {} },
  "_source": ["account_number", "balance"]
}
# 条件搜索
$ GET /bank/_search
{
  "query": { "match": { "account_number": 20 } }
}
# OR 匹配(匹配mill 或 lane)
$ GET /bank/_search
{
  "query": { "match": { "address": "mill lane" } }
}
# AND 匹配 (匹配 mill 且 lane)
$ GET /bank/_search
{
  "query": { "match_phrase": { "address": "mill lane" } }
}
# AND 匹配(包含mill 且 lane的数据)
$ GET /bank/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}
# OR 匹配(匹配 mill 或 lane 的数据)
$ GET /bank/_search
{
  "query": {
    "bool": {
      "should": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}
# NO AND 匹配(匹配 非mill 且 非 lane)
$ GET /bank/_search
{
  "query": {
    "bool": {
      "must_not": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}
# 例 : age=40 且 state!=ID
$ GET /bank/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "age": "40" } }
      ],
      "must_not": [
        { "match": { "state": "ID" } }
      ]
    }
  }
}
```
```
# 范围匹配 (balance 在2000:3000之间的数据)
$ GET /bank/_search
{
  "query": {
    "bool": {
      "must": { "match_all": {} },
      "filter": {
        "range": {
          "balance": {
            "gte": 20000,
            "lte": 30000
          }
        }
      }
    }
  }
}
```
##### 聚合索引
```bash
$ GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword"
      }
    }
  }
}
# 类似于
$ SELECT state, COUNT(*) FROM bank GROUP BY state ORDER BY COUNT(*) DESC
```
```
# 聚合嵌套
$ GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword"
      },
      "aggs": {
        "average_balance": {
          "avg": {
            "field": "balance"
          }
        }
      }
    }
  }
}
```
```
# 聚合排序
$ GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_state": {
      "terms": {
        "field": "state.keyword",
        "order": {
          "average_balance": "desc"
        }
      },
      "aggs": {
        "average_balance": {
          "avg": {
            "field": "balance"
          }
        }
      }
    }
  }
}
```
```
# 聚合范围分组
$ GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_age": {
      "range": {
        "field": "age",
        "ranges": [
          {
            "from": 20,
            "to": 30
          },
          {
            "from": 30,
            "to": 40
          },
          {
            "from": 40,
            "to": 50
          }
        ]
      },
      "aggs": {
        "group_by_gender": {
          "terms": {
            "field": "gender.keyword"
          },
          "aggs": {
            "average_balance": {
              "avg": {
                "field": "balance"
              }
            }
          }
        }
      }
    }
  }
}
```
### 04.ES中常用的搜索方法

#### 1. query string search
- 介绍：搜索全部数据  
took：耗费了几毫秒  
timed_out：是否超时   
_shards：数据拆成了1个分片，所以对于搜索请求，会打到所有的primary   shard（或者是它的某个replica shard也可以）。  
hits.total：查询结果的数量，4个document。  
hits.max_score：score的含义，就是document对于一个search的相关度的匹配分数，越相关，就越匹配，分数也越高。  
hits.hits：包含了匹配搜索的document的详细数据。
- 语法（生产环境很少用）：
```
GET /building/house/_search
```
- 结果：
```
{
  "took" : 3,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 4,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "2",
        "_score" : 1.0,
        "_source" : {
          "name" : "guang ming xiao qu",
          "finash" : 2010,
          "room" : 802,
          "sale" : 4000000,
          "tags" : [
            "lift",
            "south",
            "gas"
          ]
        }
      },
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "3",
        "_score" : 1.0,
        "_source" : {
          "name" : "shan shui xiao qu",
          "finash" : 2016,
          "room" : 902,
          "sale" : 4500000,
          "tags" : [
            "lift",
            "south",
            "gas"
          ]
        }
      },
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "4",
        "_score" : 1.0,
        "_source" : {
          "name" : "hao yue xiao qu",
          "finash" : 2018,
          "room" : 805,
          "sale" : 5200000,
          "tags" : [
            "lift",
            "south",
            "gas"
          ]
        }
      },
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "name" : "xin fu xiao qu",
          "finash" : 2015,
          "room" : 502,
          "sale" : 3000000,
          "tags" : [
            "lift",
            "south"
          ]
        }
      }
    ]
  }
}
```

#### 2.query DSL
- 介绍：DSL：Domain Specified Language，特定领域的语言。
http request body：请求体，可以用json的格式来构建查询语法，比较方便，可以构建各种复杂的语法，比query string search肯定强大多了
- 语法：
 - 查询所有房屋
```
GET /building/house/_search
{
  "query":{
    "match_all": {}
  }
}
# 结果：略
```
 - 查询包含电梯（lift），且售价降序排列
 ```
GET /building/house/_search
{
  "query":{
    "match": {
      "tags": "lift"
    }
  },
  "sort":[
    {"sale":"desc"}
    ]
}
 ```
 - 结果：
 ```
{
  "took" : 158,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 4,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "4",
        "_score" : null,
        "_source" : {
          "name" : "hao yue xiao qu",
          "finash" : 2018,
          "room" : 805,
          "sale" : 5200000,
          "tags" : [
            "lift",
            "south",
            "gas"
          ]
        },
        "sort" : [
          5200000
        ]
      },
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "3",
        "_score" : null,
        "_source" : {
          "name" : "shan shui xiao qu",
          "finash" : 2016,
          "room" : 902,
          "sale" : 4500000,
          "tags" : [
            "lift",
            "south",
            "gas"
          ]
        },
        "sort" : [
          4500000
        ]
      },
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "2",
        "_score" : null,
        "_source" : {
          "name" : "guang ming xiao qu",
          "finash" : 2010,
          "room" : 802,
          "sale" : 4000000,
          "tags" : [
            "lift",
            "south",
            "gas"
          ]
        },
        "sort" : [
          4000000
        ]
      },
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "1",
        "_score" : null,
        "_source" : {
          "name" : "xin fu xiao qu",
          "finash" : 2015,
          "room" : 502,
          "sale" : 3000000,
          "tags" : [
            "lift",
            "south"
          ]
        },
        "sort" : [
          3000000
        ]
      }
    ]
  }
}
 ```
- 分页查询，总共4套房子，每页显示2套房子，查询第二页的房子
```
GET /building/house/_search
{
  "query":{
    "match_all": {}
  },
  "from":1,
  "size":2
}
```
- 结果
```
#! Deprecation: [types removal] Specifying types in search requests is deprecated.
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 4,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "3",
        "_score" : 1.0,
        "_source" : {
          "name" : "shan shui xiao qu",
          "finash" : 2016,
          "room" : 902,
          "sale" : 4500000,
          "tags" : [
            "lift",
            "south",
            "gas"
          ]
        }
      },
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "4",
        "_score" : 1.0,
        "_source" : {
          "name" : "hao yue xiao qu",
          "finash" : 2018,
          "room" : 805,
          "sale" : 5200000,
          "tags" : [
            "lift",
            "south",
            "gas"
          ]
        }
      }
    ]
  }
}
```
- 只查询小区名称和售价(常用)
```
GET /building/house/_search
{
  "query":{
    "match_all": {}
  },
  "_source":["name", "sale"]
}
```
- 结果
```
#! Deprecation: [types removal] Specifying types in search requests is deprecated.
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 4,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "2",
        "_score" : 1.0,
        "_source" : {
          "sale" : 4000000,
          "name" : "guang ming xiao qu"
        }
      },
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "3",
        "_score" : 1.0,
        "_source" : {
          "sale" : 4500000,
          "name" : "shan shui xiao qu"
        }
      },
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "4",
        "_score" : 1.0,
        "_source" : {
          "sale" : 5200000,
          "name" : "hao yue xiao qu"
        }
      },
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "sale" : 3000000,
          "name" : "xin fu xiao qu"
        }
      }
    ]
  }
}
```

#### 3.query filter
- 要求：搜索小区名称包含xin fu,且售价（sale）大于2500000的房屋
- 语法：
```
GET /building/house/_search
{
  "query":{
    "bool":{
      "must":{
        "match":{
          "name":"xin fu"
        }
      },
      "filter": {
        "range": {
          "sale": {
            "gte": 2500000
          }
        }
      }
    }
  }
}
```
- 结果：
```
#! Deprecation: [types removal] Specifying types in search requests is deprecated.
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 2.4079456,
    "hits" : [
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "1",
        "_score" : 2.4079456,
        "_source" : {
          "name" : "xin fu xiao qu",
          "finash" : 2015,
          "room" : 502,
          "sale" : 3000000,
          "tags" : [
            "lift",
            "south"
          ]
        }
      }
    ]
  }
}
```

#### 4.full-text search(全文检索)
- 搜索包含shan shui的小区
```
GET /building/house/_search
{
  "query":{
    "match": {
      "name": "shan shui"
    }
  }
}
```
- 结果
```
#! Deprecation: [types removal] Specifying types in search requests is deprecated.
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 2,
      "relation" : "eq"
    },
    "max_score" : 1.420477,
    "hits" : [
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "3",
        "_score" : 1.420477,
        "_source" : {
          "name" : "shan shui xiao qu",
          "finash" : 2016,
          "room" : 902,
          "sale" : 4500000,
          "tags" : [
            "lift",
            "south",
            "gas"
          ]
        }
      },
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "1",
        "_score" : 1.2929528,
        "_source" : {
          "name" : "shan shui jia xiao qu",
          "finash" : 2016,
          "room" : 505,
          "sale" : 3000000,
          "tags" : [
            "lift",
            "south"
          ]
        }
      }
    ]
  }
}
```

#### 5.phrase search(短语搜索)
- 介绍：跟全文检索相对应，相反，全文检索会将输入的搜索串拆解开来，去倒排索引里面去一一匹配，只要能匹配上任意一个拆解后的单词，就可以作为结果返回。phrase search，要求输入的搜索串，必须在指定的字段文本中，完全包含一模一样的，才可以算匹配，才能作为结果返回。

- 语法
```
GET /building/house/_search
{
  "query":{
    "match_phrase": {
      "name": "hao yue"
    }
  }
}
```
- 结果

```
{
  "took" : 660,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 2,
      "relation" : "eq"
    },
    "max_score" : 1.8185701,
    "hits" : [
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "4",
        "_score" : 1.8185701,
        "_source" : {
          "name" : "hao yue xiao qu",
          "finash" : 2018,
          "room" : 805,
          "sale" : 5200000,
          "tags" : [
            "lift",
            "south",
            "gas"
          ]
        }
      },
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "5",
        "_score" : 1.658422,
        "_source" : {
          "name" : "hao yue fang xiao qu",
          "finash" : 2018,
          "room" : 603,
          "sale" : 4000000,
          "tags" : [
            "lift",
            "south"
          ]
        }
      }
    ]
  }
}
```
#### 6.highlight search(高亮搜索结果)
- 将小区名字中含有guang ming的字段，高亮显示。
- 语法
```
GET /building/house/_search
{
  "query":{
    "match_phrase": {
      "name": "guang ming"
    }
  },
  "highlight":{
    "fields": {
      "name": {}
    }
  }
}
```
- 结果
```
#! Deprecation: [types removal] Specifying types in search requests is deprecated.
{
  "took" : 382,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 2.8796844,
    "hits" : [
      {
        "_index" : "building",
        "_type" : "house",
        "_id" : "2",
        "_score" : 2.8796844,
        "_source" : {
          "name" : "guang ming xiao qu",
          "finash" : 2010,
          "room" : 802,
          "sale" : 4000000,
          "tags" : [
            "lift",
            "south",
            "gas"
          ]
        },
        "highlight" : {
          "name" : [
            "<em>guang</em> <em>ming</em> xiao qu"
          ]
        }
      }
    ]
  }
}
```


































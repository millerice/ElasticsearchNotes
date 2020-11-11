### 05.ES中的聚合分析

#### 1.计算每个tag下的房屋数量
- 语法：
```
# 聚合查询
GET /building/house/_search
{
  "size":0,
  "aggs":{
    "group_by_tags":{
      "terms": {
        "field": "tags.keyword"
      }
    }
  }
}
```
- 结果：
```
#! Deprecation: [types removal] Specifying types in search requests is deprecated.
{
  "took" : 5,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 6,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "group_by_tags" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 0,
      "buckets" : [
        {
          "key" : "gas",
          "doc_count" : 5
        },
        {
          "key" : "lift",
          "doc_count" : 5
        },
        {
          "key" : "south",
          "doc_count" : 5
        },
        {
          "key" : "hardcover",
          "doc_count" : 1
        },
        {
          "key" : "noth",
          "doc_count" : 1
        }
      ]
    }
  }
}
```

#### 2.对小区名称中含有shan shui的小区，计算每个tag下的房屋数量
- 语法：
```
GET /building/house/_search
{
  "size":0,
  "query":{
    "match":{
      "name":"shan shui"
    }
  },
  "aggs":{
    "group_by_tags":{
      "terms": {
        "field": "tags.keyword"
      }
    }
  }
}
```
- 结果：
```
#! Deprecation: [types removal] Specifying types in search requests is deprecated.
{
  "took" : 16,
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
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "group_by_tags" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 0,
      "buckets" : [
        {
          "key" : "lift",
          "doc_count" : 2
        },
        {
          "key" : "south",
          "doc_count" : 2
        },
        {
          "key" : "gas",
          "doc_count" : 1
        }
      ]
    }
  }
}
```

#### 3.先根据tag分组，再计算每组的平均售价
- 语法：
```
GET /building/house/_search
{
  "size":0,
  "aggs":{
    "group_by_tags":{
      "terms": {
        "field": "tags.keyword"
      },
      "aggs": {
        "avg_sale": {
          "avg": {
            "field": "sale"
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
  "took" : 8,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 6,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "group_by_tags" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 0,
      "buckets" : [
        {
          "key" : "gas",
          "doc_count" : 5,
          "avg_sale" : {
            "value" : 4400000.0
          }
        },
        {
          "key" : "lift",
          "doc_count" : 5,
          "avg_sale" : {
            "value" : 3900000.0
          }
        },
        {
          "key" : "south",
          "doc_count" : 5,
          "avg_sale" : {
            "value" : 4100000.0
          }
        },
        {
          "key" : "hardcover",
          "doc_count" : 1,
          "avg_sale" : {
            "value" : 6000000.0
          }
        },
        {
          "key" : "noth",
          "doc_count" : 1,
          "avg_sale" : {
            "value" : 4000000.0
          }
        }
      ]
    }
  }
}
```

#### 4.再3的基础上，按照售价降序排列
- 语法：
```
GET /building/house/_search
{
  "size":0,
  "aggs":{
    "group_by_tags":{
      "terms": {
        "field": "tags.keyword",
        "order": {
          "avg_sale": "desc"
        }
      },
      "aggs": {
        "avg_sale": {
          "avg": {
            "field": "sale"
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
  "took" : 6,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 6,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "group_by_tags" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 0,
      "buckets" : [
        {
          "key" : "hardcover",
          "doc_count" : 1,
          "avg_sale" : {
            "value" : 6000000.0
          }
        },
        {
          "key" : "gas",
          "doc_count" : 5,
          "avg_sale" : {
            "value" : 4400000.0
          }
        },
        {
          "key" : "south",
          "doc_count" : 5,
          "avg_sale" : {
            "value" : 4100000.0
          }
        },
        {
          "key" : "noth",
          "doc_count" : 1,
          "avg_sale" : {
            "value" : 4000000.0
          }
        },
        {
          "key" : "lift",
          "doc_count" : 5,
          "avg_sale" : {
            "value" : 3900000.0
          }
        }
      ]
    }
  }
}
```

#### 5.按照指定的售价区间进行分组，然后在每个分组内再按照tag分组，最后再计算每个tag组的平均售价
- 语法：
```
GET /building/house/_search
{
  "size":0,
  "aggs":{
    "group_by_sale":{
      "range": {
        "field": "sale",
        "ranges": [
          {
            "from": 1000000,
            "to":2000000
          },
          {
            "from": 2000000,
            "to": 4000000
          },
          {
            "from": 4000000,
            "to":6000000
          }
        ]
      },
      "aggs": {
        "group_by_tags": {
          "terms": {
            "field": "tags.keyword"
          },
          "aggs": {
            "average_sale": {
              "avg": {
                "field": "sale"
              }
            }
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
  "took" : 7,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 6,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "group_by_sale" : {
      "buckets" : [
        {
          "key" : "1000000.0-2000000.0",
          "from" : 1000000.0,
          "to" : 2000000.0,
          "doc_count" : 0,
          "group_by_tags" : {
            "doc_count_error_upper_bound" : 0,
            "sum_other_doc_count" : 0,
            "buckets" : [ ]
          }
        },
        {
          "key" : "2000000.0-4000000.0",
          "from" : 2000000.0,
          "to" : 4000000.0,
          "doc_count" : 2,
          "group_by_tags" : {
            "doc_count_error_upper_bound" : 0,
            "sum_other_doc_count" : 0,
            "buckets" : [
              {
                "key" : "lift",
                "doc_count" : 2,
                "average_sale" : {
                  "value" : 2750000.0
                }
              },
              {
                "key" : "south",
                "doc_count" : 2,
                "average_sale" : {
                  "value" : 2750000.0
                }
              },
              {
                "key" : "gas",
                "doc_count" : 1,
                "average_sale" : {
                  "value" : 3000000.0
                }
              }
            ]
          }
        },
        {
          "key" : "4000000.0-6000000.0",
          "from" : 4000000.0,
          "to" : 6000000.0,
          "doc_count" : 3,
          "group_by_tags" : {
            "doc_count_error_upper_bound" : 0,
            "sum_other_doc_count" : 0,
            "buckets" : [
              {
                "key" : "gas",
                "doc_count" : 3,
                "average_sale" : {
                  "value" : 4333333.333333333
                }
              },
              {
                "key" : "lift",
                "doc_count" : 2,
                "average_sale" : {
                  "value" : 4000000.0
                }
              },
              {
                "key" : "south",
                "doc_count" : 2,
                "average_sale" : {
                  "value" : 4500000.0
                }
              },
              {
                "key" : "noth",
                "doc_count" : 1,
                "average_sale" : {
                  "value" : 4000000.0
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```












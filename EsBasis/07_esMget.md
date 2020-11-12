### 07.ES中mget批量查询

__mget是很重要的，一般来说，在进行查询的时候，如果一次性要查询多条数据的话，那么一定要用batch批量操作的api，尽可能减少网络开销次数，可以将性能提升数倍，甚至数十倍。__

#### 1.mget批量查询
- 语法：
```
GET _mget
{
  "docs":[
      {
        "_index":"building",
        "_type":"house",
        "_id":1
      },
      {
        "_index":"building",
        "_type":"house",
        "_id":2
      }
    ]
}
```
- 结果
```
#! Deprecation: [types removal] Specifying types in multi get requests is deprecated.
{
  "docs" : [
    {
      "_index" : "building",
      "_type" : "house",
      "_id" : "1",
      "_version" : 7,
      "_seq_no" : 18,
      "_primary_term" : 2,
      "found" : true,
      "_source" : {
        "name" : "shan shui xiao qu",
        "finash" : 2016,
        "room" : 505,
        "sale" : 2500000,
        "tags" : [
          "lift",
          "south"
        ]
      }
    },
    {
      "_index" : "building",
      "_type" : "house",
      "_id" : "2",
      "_version" : 2,
      "_seq_no" : 11,
      "_primary_term" : 2,
      "found" : true,
      "_source" : {
        "name" : "shan shui xiao qu",
        "finash" : 2010,
        "room" : 802,
        "sale" : 3000000,
        "tags" : [
          "lift",
          "south",
          "gas"
        ]
      }
    }
  ]
}
```

#### 2.批量查询多个索引
- 语法：
```
GET _mget
{
  "docs":[
      {
        "_index":"building",
        "_type":"house",
        "_id":1
      },
      {
        "_index":"script_index",
        "_type":"script_type",
        "_id":1
      }
    ]
}
```
- 结果：
```
#! Deprecation: [types removal] Specifying types in multi get requests is deprecated.
{
  "docs" : [
    {
      "_index" : "building",
      "_type" : "house",
      "_id" : "1",
      "_version" : 7,
      "_seq_no" : 18,
      "_primary_term" : 2,
      "found" : true,
      "_source" : {
        "name" : "shan shui xiao qu",
        "finash" : 2016,
        "room" : 505,
        "sale" : 2500000,
        "tags" : [
          "lift",
          "south"
        ]
      }
    },
    {
      "_index" : "script_index",
      "_type" : "script_type",
      "_id" : "1",
      "_version" : 3,
      "_seq_no" : 2,
      "_primary_term" : 1,
      "found" : true,
      "_source" : {
        "num" : 3,
        "tags" : [ ]
      }
    }
  ]
}
```

#### 3.查询同一个index下的同一个type
- 语法：
```
GET /building/house/_mget
{
  "ids":[1, 2]
}
```
- 结果：
```
#! Deprecation: [types removal] Specifying types in multi get requests is deprecated.
{
  "docs" : [
    {
      "_index" : "building",
      "_type" : "house",
      "_id" : "1",
      "_version" : 7,
      "_seq_no" : 18,
      "_primary_term" : 2,
      "found" : true,
      "_source" : {
        "name" : "shan shui xiao qu",
        "finash" : 2016,
        "room" : 505,
        "sale" : 2500000,
        "tags" : [
          "lift",
          "south"
        ]
      }
    },
    {
      "_index" : "building",
      "_type" : "house",
      "_id" : "2",
      "_version" : 2,
      "_seq_no" : 11,
      "_primary_term" : 2,
      "found" : true,
      "_source" : {
        "name" : "shan shui xiao qu",
        "finash" : 2010,
        "room" : 802,
        "sale" : 3000000,
        "tags" : [
          "lift",
          "south",
          "gas"
        ]
      }
    }
  ]
}
```


















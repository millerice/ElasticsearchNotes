### 08.ES中bulk批量操作

__介绍：bulk api对json的语法，有严格的要求，每个json串不能换行，只能放一行，同时一个json串和一个json串之间，必须有一个换行。bulk操作中，任意一个操作失败，是不会影响其他的操作的，但是在返回结果里，会告诉你异常日志__
#### 1.bulk size最佳大小
__bulk request会加载到内存里，如果太大的话，性能反而会下降，因此需要反复尝试一个最佳的bulk size。一般从1000~5000条数据开始，尝试逐渐增加。另外，如果看大小的话，最好是在5~15MB之间。__

#### 2.可以执行bulk操作的类型
（1）delete：删除一个文档，只要1个json串就可以了  
（2）create：PUT /index/type/id/_create，强制创建  
（3）index：普通的put操作，可以是创建文档，也可以是全量替换文档  
（4）update：执行的partial update操作  

#### 3.bulk操作
- 要求：删除/building/house/ 下id为6的房屋，并创建一个id为7的房子
- 语法：
```
POST /building/house/_bulk
{"delete":{"_id":6}}
{"create":{"_id":7}}
```
- 结果：
```
#! Deprecation: [types removal] Specifying types in bulk requests is deprecated.
{
  "took" : 175,
  "errors" : false,
  "items" : [
    {
      "delete" : {
        "_index" : "building",
        "_type" : "house",
        "_id" : "6",
        "_version" : 2,
        "result" : "deleted",
        "_shards" : {
          "total" : 2,
          "successful" : 2,
          "failed" : 0
        },
        "_seq_no" : 19,
        "_primary_term" : 2,
        "status" : 200
      }
    }
  ]
}
```

#### 4.bulk批量插入数据
- 要求：插入id为8和id为9的两套房子
- 语法：
```
POST _bulk
{"index":{"_index":"building", "_type":"house", "_id":8}}
{"name":"shui yue xiao qu","finash":2020,"room":1001,"sale":7500000,"tags":["lift","south","gas"]}
{"index":{"_index":"building", "_type":"house", "_id":9}}
{"name":"jing yue xiao qu","finash":2020,"room":1101,"sale":8000000,"tags":["lift","south","gas"]}
```
- 结果：
```
#! Deprecation: [types removal] Specifying types in bulk requests is deprecated.
{
  "took" : 122,
  "errors" : false,
  "items" : [
    {
      "index" : {
        "_index" : "building",
        "_type" : "house",
        "_id" : "8",
        "_version" : 1,
        "result" : "created",
        "_shards" : {
          "total" : 2,
          "successful" : 2,
          "failed" : 0
        },
        "_seq_no" : 20,
        "_primary_term" : 2,
        "status" : 201
      }
    },
    {
      "index" : {
        "_index" : "building",
        "_type" : "house",
        "_id" : "9",
        "_version" : 1,
        "result" : "created",
        "_shards" : {
          "total" : 2,
          "successful" : 2,
          "failed" : 0
        },
        "_seq_no" : 21,
        "_primary_term" : 2,
        "status" : 201
      }
    }
  ]
}
```






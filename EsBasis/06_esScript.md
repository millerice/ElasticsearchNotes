###06.ES中的painless脚本操作
__了解即可，使用脚本会导致es性能降低。不常使用，在解决复杂业务问题时才使用.__

- 添加一条数据
- 语法
```
PUT /script_index/script_type/1
{
  "num":0,
  "tags":[]
}
# 结果：略
```

#### 1.使用script脚本实现加法操作
- 语法：
```
POST /script_index/script_type/1/_update
{
  "script":{
    "lang": "painless",
    "source": """ctx._source.num+=1"""
  }
}
```
- 结果：
```
#! Deprecation: [types removal] Specifying types in document update requests is deprecated, use the endpoint /{index}/_update/{id} instead.
{
  "_index" : "script_index",
  "_type" : "script_type",
  "_id" : "1",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 2,
    "failed" : 0
  },
  "_seq_no" : 2,
  "_primary_term" : 1
}
# 搜索一下
#! Deprecation: [types removal] Specifying types in document get requests is deprecated, use the /{index}/_doc/{id} endpoint instead.
{
  "_index" : "script_index",
  "_type" : "script_type",
  "_id" : "1",
  "_version" : 2,
  "_seq_no" : 2,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "num" : 2,
    "tags" : [ ]
  }
}
```
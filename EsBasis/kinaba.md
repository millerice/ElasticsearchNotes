### 02.初识kinaba

#### 1.打开kinaba
- 浏览器中打开此链接：127.0.0.1:5601

#### 2.使用kinaba进行简单的增、删、改、查操作
> 1.增加操作
- 增加4条房屋数据  
```
PUT /building/house/1
{
  "name":"xin fu xiao qu",
  "finash":2015,
  "room":502,
  "sale":3000000,
  "tags":["lift", "south"]
}
...
{
  "_index" : "building",
  "_type" : "house",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```
- 结果
```
{
  "_index" : "building",
  "_type" : "house",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
...
{
  "_index" : "building",
  "_type" : "house",
  "_id" : "4",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 2,
    "failed" : 0
  },
  "_seq_no" : 3,
  "_primary_term" : 1
}
```
> 2.查询操作
- 查询id为1的房屋
```
GET /building/house/1
```
- 结果
```
{
  "_index" : "building",
  "_type" : "house",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 0,
  "_primary_term" : 1,
  "found" : true,
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
```
> 3.修改操作
- 将id为1的房间售价改为2500000
```
POST /building/house/1/_update
{
  "doc":{
    "sale":2500000
  }
}
```
- 结果
```
{
  "_index" : "building",
  "_type" : "house",
  "_id" : "1",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 2,
    "failed" : 0
  },
  "_seq_no" : 4,
  "_primary_term" : 1
}
# ---------------------------------------
# 查询一下看看：
{
  "_index" : "building",
  "_type" : "house",
  "_id" : "1",
  "_version" : 2,
  "_seq_no" : 4,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "xin fu xiao qu",
    "finash" : 2015,
    "room" : 502,
    "sale" : 2500000,
    "tags" : [
      "lift",
      "south"
    ]
  }
}
```
> 4.删除操作
- 将id为1的数据删除
```
DELETE /building/house/1
```
- 结果
```
{
  "_index" : "building",
  "_type" : "house",
  "_id" : "1",
  "_version" : 3,
  "result" : "deleted",
  "_shards" : {
    "total" : 2,
    "successful" : 2,
    "failed" : 0
  },
  "_seq_no" : 5,
  "_primary_term" : 1
}
# -------------
# 查询一下看看
{
  "_index" : "building",
  "_type" : "house",
  "_id" : "1",
  "found" : false
}
```






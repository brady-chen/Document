#MongoDB语法
###1.url连接语法
###mongodb://[user:pwd@]host[:port][/dbname][?options]
- mongodb://：这是固定的格式，必须要指定
- username:password@：可选项，如果设置，在连接数据库服务器之后，驱动都会尝试登陆这个数据库
- host1：必须的指定至少一个host, host1 是这个URI唯一要填写的。它指定了要连接服务器的地址。
- port：可选的指定端口，如果不填，默认27017
- /database：如果指定username:password@，连接并验证登陆指定数据库。若不指定，默认打开 test 数据库。
- ?options：是连接选项。如果不使用/database，则前面需要加/。所有连接选项都是键值对name=value，键值对之间通过&或；隔开

### 2.mongo程序连接
### mongo host:port/dbname -u user -p pwd
### 3.增加文档
### db.coll.insert(document)
### 4.更新文档​
```
db.collection.update(
    query,
    update,
    {
    upsert: boolean,
    multi: boolean,
    writeConcern: document
    }
)
```
- query：update的查询条件，类似sql update查询内where后面的。  
- update：update的对象和一些更新的操作符（如$，$inc...）等，也可以理解为sql update查询内set后面的  
- upsert：可选，如果query的值不存在，true则插入这条数据，false则不进行任何操作，默认为false  
- multi：可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。  
- writeConcern：可选，抛出异常的级别。 
**db.coll.update({'key':'old_value'},{$set:{'key':'new_value'}}) 等于 update coll set title='new' where title='old'** 

### 5.删除文档
```
db.collection.remove(
    <font color=#090>query</font>,
    {
    justOne: <font color=#090>boolean</font>,
    writeConcern: <font color=#090>document</font>
    }
)
```
- query :（可选）删除的文档的条件。  
- justOne : （可选）如果设为 true 或1，则只删除一个文档。  
- writeConcern :（可选）抛出异常的级别。

<br>**推荐新用法**
```
# 删除集合下全部文档：
db.inventory.deleteMany({})
# 删除 status 等于 A 的全部文档：
db.inventory.deleteMany({status: "A"})
# 删除 status 等于 D 的一个文档：
db.inventory.deleteOne({status: "D"})
```

### 6.查询文档
```
db.collection.find(
    <font color=#090>query</font>, 
    <font color=#090>projection</font>
)
```
- query ：可选，使用查询操作符指定查询条件  
- projection ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值,只需省略该参数即可（默认省略）  
**db.collection.findOne()返回一个文档**

操作|格式|范例|类似sql语句
:-|:-|:-|:-
等于|{\<key>:\<value>}|db.col.find({"by":"菜鸟教程"}).pretty()|where by = '菜鸟教程'|
小于	|{\<key>:{$lt:\<value>}}|db.col.find({"likes":{$lt:50}}).pretty()|where likes < 50
小于或等于|{\<key>:{$lte:\<value>}}|db.col.find({"likes":{$lte:50}}).pretty()|where likes <= 50
大于|{\<key>:{$gt:\<value>}}|db.col.find({"likes":{$gt:50}}).pretty()|where likes > 50
大于或等于|{\<key>:{$gte:\<value>}}|db.col.find({"likes":{$gte:50}}).pretty()	|where likes >= 50
不等于|{\<key>:{$ne:\<value>}}|db.col.find({"likes":{$ne:50}}).pretty()|where likes != 50
数据类型|{"key":{$type : value_type}}|db.col.find({"title" : {$type : 'string'}})|where xtype='nvarchar'
or|{$or: [{key: value}, {key: value}]}|db.col.find({$or: [{a: 1}, {b: 2}]})|where a=1 or b=2
```
# 查询 title 包含"教"字的文档：
db.col.find({title:/教/})
# 查询 title 字段以"教"字开头的文档：
db.col.find({title:/^教/})
# 查询 titl e字段以"教"字结尾的文档：
db.col.find({title:/教$/})
```

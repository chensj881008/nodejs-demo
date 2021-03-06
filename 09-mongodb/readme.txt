#查询所有的数据库
show dbs;
#创建数据库
use demo
#删除数据库 在当前数据库执行如下命令
db.dropDatabase();
#查询数据库中的表
show collections;
#删除表
db.table_name.drop();
#创建表和插入数据
db.user.insert({username:"admin",age:27,password:"admin123"})
#查询数据
db.user.find()
#查询 distinct
db.user.distinct("name") #name 为列名
#查询age=22
db.user.find({age:22})
#查询age大于22
db.user.find({age:{$gt:22}})
#查询age小于22
db.user.find({age:{$lt:22}})
#查询age大于等于22
db.user.find({age:{$gte:22}})
#查询age小于等于22
db.user.find({age:{$lte:22}})
#模糊查询 name包含mongo
db.user.find({name:/mongo/})
#模糊查询 name以mongo开头
db.user.find({name:/^mongo/})
#查询指定列 name、age
db.user.find({}).({name:1,age:1}) #  1也可以使用true，false表示不显示
#组合查询 查询name/age，并且age>25
db.user.find({age:{$gt:25}}).({name:1,age:1})
#排序 1 升序  -1 降序
db.user.find().sort({age:1}) #按年龄升序
db.user.find().sort({age:-1}) #按年龄降序
#过滤查询
db.user.find({name:"admin",age:25}) #查询名称admin，age为25的数据
#查询前五条
db.user.find().limit(5)
#查询十条以后的数据
db.user.find().skip(5)
#查询5到10的数据
db.user.find().limit(10).skip(5)
#or 与查询
db.user.find({$or:[{name:"zhangsan"},{age:25}]}) #查询name=zhangsan或者age=25的数据
#查询第一条数据
db.user.findOne()
#统计数据
db.user.find({age:{$gt:25}}).count()

#修改数据
#修改姓名为小明的年龄为25
db.user.update({name:"小明"},{$set:{age:25}});
#全部替换 修改性别男的age全部为33
db.user.update({sex:"男"}，{$set:{age:33}},{multi true});
或者
db.user.update({sex:"男"}，{age:33});
db.users.update({name: 'Lisi'}, {$inc: {age: 50}}, false, true);
相当于：update users set age = age + 50 where name = ‘Lisi’;
db.users.update({name: 'Lisi'}, {$inc: {age: 50}, $set: {name: 'hoho'}}, false, true);
相当于：update users set age = age + 50, name = ‘hoho’ where name = ‘Lisi’;

#删除数据
db.users.remove({age: 132});
#只删除一条
db.users.remove( { "borough": "Queens" }, { justOne: true } )

#索引
#创建索引的命令：
db.user.ensureIndex({"username":1})
#获取当前集合的索引：
db.user.getIndexes()
#删除索引的命令是：
db.user.dropIndex({"username":1})
#创建复合索引  数字1表示username键的索引按升序存储，-1表示age键的索引按照降序方式存储
db.user.ensureIndex({"username":1, "age":-1})
#在创建索引时为其指定索引名，如：
db.user.ensureIndex({"username":1},{"name":"userindex"})
#唯一索引
db.user.ensureIndex({"userid":1},{"unique":true})
#复合唯一索引
db.user.ensureIndex({"userid":1,"age":1},{"unique":true})
#为已有数据的文档创建索引时，可以执行下面的命令，以使MongoDB在后台创建索引
db.user.ensureIndex({"username":1},{"background":true})

#使用explain
#  explain是非常有用的工具，会帮助你获得查询方面诸多有用的信息。
#  只要对游标调用该方法，就可以得到查询细节。explain会返回一个文档，而不是游标本身
#  explain会返回查询使用的索引情况，耗时和扫描文档数的统计信息

#explain executionStats查询具体的执行时间
db.tablename.find().explain( "executionStats" )
#mongodb 允许每条数据的数据列不一致


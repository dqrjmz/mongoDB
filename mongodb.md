mongodb
概念：
MongoDB
mongo
索引
集合
复制集
分片
数据均衡

mongodb的
        1.安装：文件下载
        2.配置：数据保存路径data/db 
        3.将mongodb设置为windows服务，可以随着windows启动而开启
         （也可以通过手工命令行启动）
        4.使用命令行链接mongodb数据库

查询数据库：show dbs
切换数据库：use test
删除当前数据库：db.DropDataBase()
创建数据库：use 数据库名 db.集合名（表名）.insert({x:1}) 即可

# mongdb中的数据集合就是关系数据库的表
mongodb的数据库操作：【增，删，改，查】
查询集合（表）：show collections/tables
删除集合：db.集合名.drop();
查询表中的数据：db.集合名（表名）.find()
                过滤查询db.集合名（表名）.find({x:1})
                查询表中数据条数：db.集合名（表名）.find().count()
                跳过多条数据从开始：db.集合名（表名）.find().skip(2)
                数据排序根据键：db.集合名（表名）.find().sort({x:1})
                数据限制为几条：db.集合名（表名）.find().limit(2)
插入数据到集合中：db.集合名.insert({x:1}) 可以使用js的语句进行操作
更新集合中的数据：db.集合名.update({x:1},{x:2})
                  更新部分字段：db.集合名（表名）.update({a:1},{$set:{b:2}});
                  更新一条不存在的数据（没有就创建）：db.集合名（表名）.update({a:1},{a:2},true); //第三个参数
                  更新多条数据：db.集合名（表名）.update({a:1},{$set:{a:2}},false,true); //第四个参数
删除集合中的数据：db.集合名（表名）.remove()  //参数必填
                  删除集合中所有条件数据：db.集合名（表名）.remove({x:1}) 


索引：
#数据量较小时使用索引查询较慢，数据量大时不使用索引又会变慢

获取当前集合中的索引：db.集合名（表名）.getIndexs()
创建当前集合的索引：db.集合名（表名）.ensureIndex({x:1})


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
        1.安装：文件下载，（如果data数据路径有错就会引起程序有错）
        2.配置：数据保存路径data/db 
        3.将mongodb设置为windows服务，可以随着windows启动而开启
         （也可以通过手工命令行启动）
        4.使用命令行链接mongodb数据库

查询数据库：show dbs
切换数据库：use test
删除当前数据库：db.DropDataBase()
创建数据库：use 数据库名 db.集合名（表名）.insert({x:1}) 即可

数据集：表
文档：行

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

获取当前集合中的索引：db.集合名（表名）.getIndexes()
创建当前集合的索引：db.集合名（表名）.ensureIndex({x:1})

我在dev分支上进行开发

查询的种类多种：对应的索引种类也多种
_id索引：自动生成
单键索引：最普通索引 使用单键索引进行查询db.my.find({x:1})
多键索引：db.集合名（表名）.ensureIndex({x:[1,2,3,4]})
复合索引：db.集合名（表名）.ensureIndex({x:1,y:2})
过期索引：在一段时间后会过期的索引，比如用户密码，过期删除，日志文件，过期删除
          db.集合名（表名）.ensureIndex({x:1},{expireAfterSeconds:10})
          注意点：必须是ISODate日期类型，或者是ISODate日期数组
                  如果指定数组则按最小的时间进行删除
                  不能是复合索引
  全文（文本）索引：
    创建全文索引：db.集合名（表名）.ensureIndex({key:'text'})
                  db.集合名（表名）.ensureIndex({key:'text',key:'text'})
                  key:对应的字段上
                  value:固定"text"
                  db.集合名（表名）.ensureIndex({'$**':'text'})创建整个集合的索引
    使用全文索引查找：db.集合名（表名）.find({$text:{$search:"aa bb -c"}) 
                      -c:不包含c
                      使用或查找
                      db.集合名（表名）.find({$text:{$search:\'a\' \'b\'}})
                      与搜索条件越相近越靠前：
                      db.集合名（表名）.find({$text:{$search:'aa bb'}},{$score:{$meta:'textScore'}})  $meta为每条记录添加textScore字段并且排除每条数据的相似度
                      使用sort()进行配合排序sort({$meta:{$search:'textSearch'}})
                      限制：
                      只能使用一次$text
                      不能使用$nor查询
                      不支持中文查询


  地理位置索引：
    索引属性：
          1.为索引指定名称：db.集合名（表名）.ensureIndex({key:'text'},{name:'index'})
          2.指定索引的唯一性：db.集合名（表名）.uniqueensureIndex({key:'text'},{unique:true/false})
          3.索引的稀疏性：
          db.集合名（表名）.uniqueensureIndex({key:'text'},{sparse:true/false})
# MongoDB 手册

## 安装

```bash
npm i mongodb
```



## 理解概念

database  =>  数据库(database)

collection  =>  数据集合(table)

document  =>  数据行记录(row)

field   =>  字段(field)

index  =>  索引(index)

table.json   =>  不支持表连接

primary key  =>  主键 MongoBD自动将 _id 设置为主键(primary key).



## 使用

```js
/* ES5 */
const mongodb = require('mongodb');

/* ES6 */
import mongodb from 'mongodb';

/* mongoDB 连接方式 1 */
mongodb.connect('monogdb://localhost:27017/db_name', {
  userNewUrlParser: true,
    useUnifiedTopology: true
});

/* mongoDB 连接方式 2 */
mongodb.createConnection('monogdb://localhost:27017/db_name', {
  userNewUrlParser: true,
    useUnifiedTopology: true
});


/* 监听数据库打开 only */
mongodb.on('open', () => { console.log('mongodb open success...'); });

/* 监听数据库报错 */
mongodb.on('error', err => console.log(err));
```



处理版本间的语法问题

```js
mongoose.set('useCreateIndex', true) /* solve problem: Use createIndexes instead. */
```



## MongoDB 数据类型

String, Number, Date, Boolean, Array, Mixed, ObjectId, Buffer, Decimal128

Mixed, ObjectId, Decimal128 需要通过 Schema.Types 来设置, 如下例子:

```
const doc_name = mongodb.Schema({
  user_name: String,
  age: {
    type: Number,
    min: 0,
    max: 200,
    default: 0
  },
  money: Schema.Types.Decimal128
});
```



## 数据库操作

```js
/* 其中 db_name 为数据库名 */
mongodb://localhost:27027/db_name

/* 创建或切换为指定数据库 */
use db_name;

/* 删除数据库 */
db_name.dropDatabase();

/* 显示所有数据库 */
show dbs;
```





## 集合(数据表)操作

```js
/* 创建集合 */
import mongodb from 'mongodb';
const doc_schema = new mongodb.Schema({
  field: {
   type: type_value,
   props: props_value,
   ...
  }
});

export default mongodb.model('Doc_name', doc_schema);

/* 删除集合 */
db.collection.drop();

/* 创建集合 */
db.collection.create(document);

/* 插入集合 */
db.collection.insert(document);
db.collection.save(document);

/* 查找集合 */
db.collection.find();
db.collection.findOne();

/* 删除集合 */
db.collection.remove(<query>, {
  justOne: boolean,
  writeConcern: <document>  /* 抛出异常 */
});
db.collection.delete(<query>);
db.collection.deleteOne(<query>);


/* 更新集合 */
db.collection.update(<query>, {
  upsert: <boolean>,   
  multi: <boolean>,  
  writeConcern: <document> /* 抛出异常 */
});
db.collection.save(<query>, {
  <document>,
  { writeConcern: <document> } /* 抛出异常 */
});

/* 集合排序 */
db.collection.find().sort({ key: 1 | -1 });

/* 指定集合排序 */
db.collection.find().limit(number); /* number 为记录条数*/

/* new_field 值为 0 | 1, 隐藏|显示字段 */
db.collection.find({ query_field }, { new_field... });

/* 集合索引 */
db.collection.getIndexes();
db.collection.totalIndexSize();
db.collection.dropIndexes(); /* 删除所有索引 */
db.collection.dropIndex(index_name);
```



## 集合聚合操作

```js
/* 聚合 aggregate， $num: 1 中的 1 为 true 的意思 */
db.collection.aggregate([
  { $group: { _id: '$group_field', new_field: { $num: 1 } } }
]);


$num  =>  计算总和
$avg  =>  计算平均值
$min  =>  获取文档对应值为最小值
$max  =>  获取文档对应值为最大值
$push  =>  将结果文档插入到数组中
$addToSet  =>  在结果文档中插入到数组中,但不创建副本
$first  =>  根据资源文档排序获取第一个文档对象
$last  =>  根据资源文档排序获取最后一个文档对象
```



## 集合管道操作

```js
/* 管道操作 aggregate */

db.collection.aggregate({
  $project: {
    title: 1,
    author: 1
  }
});

/* 先处理 $match, 再执行 $group, 再跳过前 5 个文档 */
db.collection.aggregate(
  { $match: { $score: { $gt: 70, $lt: 90 } }},
  { $group: { _id: null, count: { $num: 1 } }},
  { $skip: 5 }
);


$project：修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档。
$match：用于过滤数据，只输出符合条件的文档。$match使用MongoDB的标准查询操作。
$limit：用来限制MongoDB聚合管道返回的文档数。
$skip：在聚合管道中跳过指定数量的文档，并返回余下的文档。
$unwind：将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值。
$group：将集合中的文档分组，可用于统计结果。
$sort：将输入文档排序后输出。
$geoNear：输出接近某一地理位置的有序文档。
```






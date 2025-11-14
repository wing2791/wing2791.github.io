---
title: MongoDB的简单使用
---





#### 基本命令

| 命令             | 作用                   |
| ---------------- | ---------------------- |
| show databases   | 显示当前数据库         |
| show dbs         | 显示当前数据库         |
| use 数据库名     | 进入指定数据库         |
| db               | 显示当前所处数据库     |
| show collections | 显示数据库中所有的集合 |



#### CRUD操作（create read update delete）

| 命令/示例                                                    | 作用                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| db.\<collection\>.insert(doc)                                | 向集合中插入一个文档                                         |
| db.stus.insert({name:"孙悟空",age:18,gender:"男"})           | 向当前的数据库中的stus集合中插入一个学生对象{name:"孙悟空",age:18,gender:"男"} |
| db.stus.insert([{name:"孙悟空",age:18,gender:"男"},	{name:"沙和尚",age:22,gender:"男"},	{name:"猪八戒",age:24,gender:"男"}]) | 一次性插入多个                                               |
| db.stus.insert([{_id:"hello",name:"孙悟空",age:18,gender:"男"},]) | 会自定义id                                                   |
| db.stus.insertOne({name:"孙悟空",age:18,gender:"男"},)<br />db.stus.insertMany([{name:"孙悟空",age:18,gender:"男"},{name:"孙悟空",age:18,gender:"男"}]) | insertOne()和insertMany()分别是用来插入一个和多个数据        |
| db.\<collection\>.find()<br />-find()用来查看集合中所有符合条件的文档<br />-find()可以接收一个对象作为条件参数<br />{}表示查询集合中所有的文档<br />{属性：值}查询属性是指定值的文档 | 查询当前集合中的所有文档                                     |
| db.stus.find({_id:"hello"})<br />db.stus.find({age:28,name:"白骨精"}) | 查询id为hello的文档<br />查询年龄为28，name为白骨精的文档    |
| db.stus.find()<br />db.stus.find()[0]<br />db.stus.find()[0].name<br />db.stus.find({}).count()<br />db.stus.find().length() | 查询stus中所有文档<br />查询第一个文档<br />查询第一个文档的name属性<br />查询stus集合中文档的数量，find()中有{}效果是一样的<br />作用同count() |
| db.\<collectioin\>.findOne()<br />db.\<collection\>.findOne().name | 用来查询集合中符合条件的第一个<br />查询集合中第一个的name属性 |
| db.\<collection\>.findOne()                                  | 查询集合中符合条件的第一个文档                               |



| 命令/示例                                                    | 作用                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| db.collection.update(查询条件，新对象)                       | 修改查询到的对象为新对象<br />update()默认情况下会使用新对象替换旧的对象，默认替换修改一个符合条件的 |
| db.stus.update({"\_id":"hello"},{$set:{name:"沙和尚"}})<br />db.stus.update({"\_id":"hello"},{$set:{gender:"女",address:"流沙河"}}) | $set: 用来修改文档中的指定属性<br />修改\_id为hello的对象中name为沙和尚<br />修改\_id为hello的对象中的gender为女，添加address属性 |
| db.stus.update({"name":"孙悟空"},{$set:{address:"花果山2"}},{multi:true}) | 添加一个{multi:true}                                         |
| db.stus.update({"\_id":"hello"},{$unset:{address:{}}})       | $unset:用来删除文档中的指定属性<br />删除\_id为hello的对象中address属性，删除是根据属性删除，属性后面的值不管，可以用{}或""或者随便什么值（1也可以，简单就行）代替 |
| db.\<collection\>.updateMany()                               | 同时修改多个符合条件的文档                                   |
| db.stus.updateMany({"name":"孙悟空"},{$set:{address:"花果山"}}) | 同时将name属性为孙悟空的对象中添加一个address属性            |
| db.\<collection\>.updateOne()                                | 修改一个符合条件的文档                                       |
| db.\<collection\>.replaceOne()                               | 替换一个文档                                                 |



| 命令/示例                      | 作用                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| db.\<collection\>.remove()     | 可以根据条件来删除文档，传递的条件和find()一样，默认情况下删除符合条件的所有文档 |
| db.stus.remove({age:28},true)  | 可以根据条件来删除stus集合中的文档,只删除一个                |
| db.stus.remove({})             | 删除stus中的所有文档（性能略差），stus集合还在               |
| db.stus.drop()                 | 删除stus集合,如果stus集合是数据库中的最后一个，那么数据库也会被删除 |
| db.dropDatabase()              | 删除所在的数据库                                             |
| db.\<collection\>.deleteOne()  | 删除集合中的一个对象                                         |
| db.\<collection\>.deleteMany() | 删除集合中的多个对象                                         |

- 文档之间的关系

  - 一对一(one to one)

    - 夫妻（一个丈夫 对应 一个妻子）

    - 在MongoDB可以通过内嵌文档的形式来体现出一对一的关系

      ```sql
      use wifeAndHusband;
      db.wifeAndHusband.insert([
      	{
      		name:"黄蓉",
      		husband:
      		{
      			name:"郭靖",
      		}
      	},
      	{
      		name:"",
      		husband:
      		{
      			name:"武大郎",
      		}
      	}
      ]);
      
      db.wifeAndHusband.find();
      ```

      

  - 一对多(one to many)/多对一(many to one)

    - 也可以通过内嵌文档来映射一对多的关系

    - 父母 -- 孩子

    - 用户 -- 订单

    - 文章 -- 评论

      ```sql
      db.users.insert(
      	[
      		{
      			username:"swk",
      		},
      		{
      			username:"zbj",
      		}
      		
      	]
      );
      db.users.find();
      
      db.order.insert(
      	{
      		list:["苹果","香蕉","大鸭梨"],
      		user_id:ObjectId("6352bdd40a470000750024d8"),
      	}
      
      );
      db.order.find();
      
      db.order.insert(
      	{
      		list:["西瓜","葡萄","桃子"],
      		user_id:ObjectId("6352bdd40a470000750024d9"),
      	}
      
      );
      db.order.find();
      
      //查找用户swk的订单
      var user_id = db.users.findOne({username:"swk"})._id;
      db.order.find({user_id:user_id});
      ```

      

  - 多对多(many to many)

    - 分类 -- 商品

    - 老师 -- 学生

      ```sql
      //多对多
      db.teachers.insert(
      	[
      		{name:"洪七公"},
      		{name:"黄药师"},
      		{name:"龟仙人"},
      	]
      
      );
      db.teachers.find()
      
      
      db.stus.insert(
      	[
      		{
      			name:"郭靖",
      			tech_isd:
      				[
      					ObjectId("6352c0260a470000750024dc"),
      					ObjectId("6352c0260a470000750024dd"),
      				]
      		},
      		{
      			name:"孙悟空",
      			tech_isd:
      				[
      					ObjectId("6352c0260a470000750024dc"),
      					ObjectId("6352c0260a470000750024dd"),
      					ObjectId("6352c0260a470000750024de"),
      				]
      		},
      	]
      );
      db.stus.find()
      ```



#### 排序和投影

| 命令/示例                                    | 作用                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| db.\<collection\>.find({}).sort([属性:1/-1]) | 对集合中的数据进行排序<br />1表示升序，-1表示降序            |
| db.stus.find({}).sort({age:1,name:-1})       | 对stus集合中所有的数据按照年龄升序，name降序进行排序         |
| db.\<collection\>.find({},{属性:1/0})        | 对集合中的数据字段进行筛选<br />1表示显示，0表示不显示       |
| db.stus.find({},{name:1,_id:0,gender:1});    | 对stus集合中的数据，显示name和gender字段，不显示\_id字段，默认显示_id字段 |

```sql
//查询文档时，默认情况是按照创建的时间进行排序（升序），视频上说是按照_id，实际上我测试不是的，是根据创建时间进行排序
db.stus.find({});
//sort()可以用来指定文档的排序规则，sort()需要传递一个对象来指定排序规则，1表示升序，-1表示降序
//limit skip sort 可以以任意的顺序进行调用
db.stus.find({}).sort({age:1,name:-1})

//在查询时，可以在第二个参数的位置来设置查询结果的投影，1表示显示，0表示不显示
//显示name，不显示_id,显示gender
db.stus.find({},{name:1,_id:0,gender:1});
```



#### mongoose的使用

```javascript
/*
1.安装Mongoose
	准备：
	切换为淘宝镜像命令
	npm config set registry https://registry.npm.taobao.org
	查看当前使用的镜像地址命令
	npm config get registry
	如果返回 https://registry.npm.taobao.org，说明镜像配置成功。
	切换回原镜像（安装一些package不容易报错）
	npm config set registry https://registry.npmjs.org

	正式安装：
    cnpm i mongoose --save
2. 在项目中引入mongoose
    var mongoose = require("mongoose");
3. 连接MongoDB数据库
    mongoose.connect('mongodb://数据库Ip地址:端口号/数据库名称',{useMongoClient:true});
    - 如果端口号是默认端口号(27017)则可以省略不写

4. 断开数据库连接(一般不需要调用)
    - MongoDB数据库，一般情况下，只需要连接一次，连接一次后，除非项目停止，服务器关闭，否则连接一般不会断开
    mongoose.disconnect()
    - 监听MongoDB数据库的连接状态
        - 在Mongoose对象中，有一个属性叫做connection,该对象表示的就是数据库连接
            通过监视该对象的状态，可以来监听数据库的连接与断开

        数据库连接成功的事件
            mongoose.connection.once("open",function(){});

        数据库断开的事件
            mongoose.connection.once("close",function(){});
 */

//引入
const mongoose = require("mongoose");
//连接数据库
mongoose.connect('mongodb://localhost:27017/test');


mongoose.connection.once("open",function(){
    console.log("数据库连接成功");
});

mongoose.connection.once("close",function(){
    console.log("数据库连接已经断开");
});

//断开数据库连接
mongoose.disconnect();
```



##### Schema和Model

```javascript
//import mongoose from 'mongoose';

//引入
const mongoose = require("mongoose");
//连接数据库
mongoose.connect('mongodb://localhost:27017/test');
mongoose.connection.once("open",function () {
    console.log("数据库连接成功");
})

//创建Schema(模式)对象
const { Schema } = mongoose;

//创建Schema(模式)对象
const stuSchema = new Schema({
    title:  String,
    age:Number,
    gender:{
        //gender是个对象，类型是String,默认值是female
        type:String,
        default:"female"
    },
    address:String,
});

//通过Schema来创建Model
//Model代表的是数据库中的集合，通过Model才能鬼数据库进行操作
//mongoose.model(modelName, schema)
//modelName:就是要映射的集合名,实际映射的是students集合，mongoose会自动将集合名称变为负数
const StuModel = mongoose.model('student', stuSchema);

//向数据库中插入一个文档
//stuModel.create(doc,function(err){});
//doc:要插入的文档  function(err){}:回调函数
// StuModel.create({
//     name:"孙悟空",
//     age:18,
//     gender:"male",
//     address:"花果山",
// },function (err) {
//     if(!err){
//         console.log("插入成功~~~");
//     }
// });

//会自动将属性gender设置为female
StuModel.create({
    name:"白骨精",
    age:16,
    address:"白骨洞",
},function (err) {
    if(!err){
        console.log("插入成功~~~");
    }
});
```



#####  Model的方法

```javascript
//引入
const mongoose = require("mongoose");
//连接数据库
mongoose.connect('mongodb://localhost:27017/test');
mongoose.connection.once("open",function () {
    console.log("数据库连接成功");
})

//创建Schema(模式)对象
const { Schema } = mongoose;

//创建Schema(模式)对象
const stuSchema = new Schema({
    //原本这里是title:String,自己怎么插入name都无法插入，改成name就能够插入了
    name:  String,
    age:Number,
    gender:{
        //gender是个对象，类型是String,默认值是female
        type:String,
        default:"female"
    },
    address:String,
});

//通过Schema来创建Model
//Model代表的是数据库中的集合，通过Model才能鬼数据库进行操作
//mongoose.model(modelName, schema)
//modelName:就是要映射的集合名,实际映射的是students集合，mongoose会自动将集合名称变为负数
const StuModel = mongoose.model('students', stuSchema);

/**
 * - 有了Model，我们就可以来对数据库进行增删改查的操作
 *  添加
 *  Model.create(docs,[options],[callback])
 *  用来创建一个文档并添加到数据库中
 *  参数：
 *  docs:可以是一个文档对象，也可以是一个文档对象的数组
 *  callback:当操作完成以后调用的回调函数
 *
 *  查找
 *  Model.find(filter,[projection],[options],[callback])
 *      查询所有符合条件的文档,总会返回一个数组(即便是空数组)
 *      filter:查询的条件
 *      project:投影 需要获取到的字段
 *          - 两种方式
 *              {name:1,_id:0}
 *              "name -_id"
 *      options:查询选项(skip limit)
 *          跳过前三个，只显示后面的一个
 *          {skip:3,limit:1}
 *      callback:回调函数,查询结果会通过回调函数返回，回调函数必须传，如果不传回调函数,根本不会查询
 *  Model.findById(id,[projection],[options],[callback])
 *      根据文档的id属性查询文档 总会返回一个具体的文档对象
 *  Model.findOne([conditioins],[projection],[options],[callback])
 *      查询符合条件的第一个文档 总会返回一个具体的文档对象
 *
 *  修改
 *  Model.update(filter,update,[options],[callback])
 *  Model.updateMany(filter,update,[options],[callback])
 *  Model.updateOne(filter,update,[options],[callback])
 *      - 用来修改一个或多个文档
 *      - 参数
 *          filter 查询条件
 *          update 修改后的对象
 *          options 配置参数
 *          callback 回调函数
 *  Model.replaceOne(filter,doc,[options],[callback])
 *
 *  删除
 *  Model.remove([options],[fn])
 *  Model.deleteOne(conditions,[options],[callback])
 *  Model.deleteMany(conditions,[options],[callback])
 *
 *  统计文档的数量
 *  Model.count(filter,[callback])
 */

// StuModel.create([
//     {
//         name:"猪八戒",
//         age:28,
//         gender:"male",
//         address:"高老庄",
//
//     },
//     {
//         name:"唐僧",
//         age:16,
//         gender:"male",
//         address:"女儿国",
//
//     },
// ],function (err) {
//     if(!err){
//         console.log("插入成功~~~");
//     }
//
// });

// StuModel.create([
//     {
//         name:"沙僧",
//         age:36,
//         gender:"male",
//         address:"流沙河",
//     },
//
// ],function (err) {
//     if(!err){
//         console.log(arguments);
//     }
//
// });

// 运行后的输出结果
// 0:应该就是err参数
// 1:就是我们插入的文档
// [Arguments] {
//     '0': null,
//         '1': [
//         {
//             age: 36,
//             gender: 'male',
//             address: '流沙河',
//             _id: new ObjectId("6353a555d75ad742ba9e5e3d"),
//             __v: 0
//         }
//     ]
// }

// 只要name属性，不要_id属性
// StuModel.find({name:"唐僧"},{name:1,_id:0},function (err,docs) {
//     if(!err){
//         console.log(docs);
//         console.log(docs[0].name);
//     }
// });

//-_id指不要_id
// skip:1 指跳过第一个，显示后面的
// limit:1 指只显示一个
// StuModel.find({},'name age -_id',{skip:1,limit:1},function (err,docs) {
//     if(!err){
//         console.log(docs);
//         console.log(docs[0].name);
//     }
// });

//返回的是具体的对象
// StuModel.findOne({},function (err,doc) {
//     if(!err){
//         console.log(doc);
//         console.log(doc.name);
//     }
// });

//返回的是具体的对象
// StuModel.findById("6353ad5d5bd5fcedc0b7658b",function (err,doc) {
//     if(!err){
//         // console.log(doc);
//         // 通过find()查询的结果，返回的对象就是Document,文档对象
//         // Document对象是Model的实例，就是集合（StuModel）的实例
//         // 返回true，表示doc是StuModel的实例
//         console.log(doc instanceof StuModel)
//         // console.log(doc.name);
//     }
// });

// 修改唐僧的年龄为20
// StuModel.updateOne({name:"唐僧"},{$set:{age:20}},function (err) {
//     if(!err){
//         console.log("修改成功");
//     }
// });

// collection.remove is deprecated. Use deleteOne, deleteMany, or bulkWrite instead.
// StuModel.remove({name:"唐僧"},function (err) {
//     if(!err){
//         console.log("删除成功");
//     }
// });

统计集合中的文档数量
StuModel.count({},function (err,count) {
    if(!err){
        console.log(count);
    }
});
```



##### Document的方法

```javascript
//引入
const mongoose = require("mongoose");
//连接数据库
mongoose.connect('mongodb://localhost:27017/test');
mongoose.connection.once("open",function () {
    console.log("数据库连接成功");
})

//创建Schema(模式)对象
const { Schema } = mongoose;

//创建Schema(模式)对象
const stuSchema = new Schema({
    name:  String,
    age:Number,
    gender:{
        //gender是个对象，类型是String,默认值是female
        type:String,
        default:"female"
    },
    address:String,
});

//通过Schema来创建Model
//Model代表的是数据库中的集合，通过Model才能鬼数据库进行操作
//mongoose.model(modelName, schema)
//modelName:就是要映射的集合名,实际映射的是students集合，mongoose会自动将集合名称变为负数
const StuModel = mongoose.model('student', stuSchema);

/**
 * Document 和集合中的文档一一对应，Document是Model的实例
 * 通过Model查询到的结果都是Document
 *
 * document的方法
 *  Model#save({options},[fn])
 */
var stu = new StuModel({
    name:"奔波霸",
    age:48,
    gender:"male",
    address:"碧波谭",
});

//会将文档中的信息进行保存
// stu.save(function(err){
//     if(!err){
//         console.log("保存成功");
//     }
// });

StuModel.findOne({},function (err,doc) {
    if(!err){
        /**
         * update(update,[options],[callback])
         *  - 修改对象
         * remove([callback])
         *  - 删除对象
         */
        // console.log(doc);
        //这个doc就是指findOne()找到的那个对象，直接修改该对象
        // doc.update({$set:{age:38}},function (err) {
        //     if(!err){
        //         console.log("修改成功");
        //     }
        //
        // });
        //也可以直接修改findOne()获得的对象
        // doc.age = 18;
        // doc.save();
        //直接删除findOne()获得的对象
        // doc.remove(function (err) {
        //     if(!err){
        //         console.log("二师兄再见");
        //     }
        //
        // });
        /**
         * get(name)
         *  - 直接获取文档中指定属性值
         *
         *  set(name,value)
         *  - 设置文档的指定的属性值
         *
         *  id
         *      获取文档的_id属性值
         *
         *
         *  toObject()
         *      - 将Document对象转换为一个普通的js对象
         *          转换为普通的js对象以后，注意所有的Document对象的方法或属性都不能使用
         */
        // 下面两个的效果相同
        // console.log(doc.get("name"));
        // console.log(doc.name);

        // doc.set("name","猪小小");
        // doc.name = "猪小小";
        // console.log(doc);

        // console.log(doc._id);
        // new ObjectId("6353ee24148021db079f47db")

        // console.log(doc.id);
        // 6353ee24148021db079f47db

        //转换为一个普通的对象
        // var o = doc.toObject();
        // console.log(o);

        //转换为普通的Object后能够删除其中的address,否则不能使用delete删除数据
        doc = doc.toObject();
        delete doc.address;
        console.log(doc);
        console.log(doc.id);
        //undefined
        console.log(doc._id);
        // new ObjectId("6353ee24148021db079f47db")
    }

});
```



##### mongoose的模块化

可以在models包中创建模型，后面可以直接使用该模型，如index.js中的使用，tools包中主要是放连接mongoDB的代码，可以不用每次都重复写

[![xguig1.png](https://s1.ax1x.com/2022/10/22/xguig1.png)](https://imgse.com/i/xguig1)

conn_mongo.js

```javascript
/**
 * 定义一个模块，用来连接MongoDB数据库
 */
//引入
const mongoose = require("mongoose");
//连接数据库
mongoose.connect('mongodb://localhost:27017/test');
mongoose.connection.once("open",function () {
    console.log("数据库连接成功");
});
```

student.js

```javascript
/**
 * 用来定义Student的模型
 *
 */
//引入
const mongoose = require("mongoose");
//连接数据库
mongoose.connect('mongodb://localhost:27017/test');
mongoose.connection.once("open",function () {
    console.log("数据库连接成功");
})

//创建Schema(模式)对象
const { Schema } = mongoose;

//创建Schema(模式)对象
const stuSchema = new Schema({
    //原本这里是title:String,自己怎么插入name都无法插入，改成name就能够插入了
    name:  String,
    age:Number,
    gender:{
        //gender是个对象，类型是String,默认值是female
        type:String,
        default:"female"
    },
    address:String,
});

//定义模型
const StuModel = mongoose.model('students', stuSchema);

// exports.model = StuModel;
//使用该语句在index.js中就不需要.model了
module.exports = StuModel;
```

index.js

```javascript
//导入会直接执行该模块
require("./tools/conn_mongo");
// const Student = require("./models/student").model;
const Student = require("./models/student");

console.log(Student);

Student.find({},function (err,docs) {
    if(!err){
        console.log(docs);
    }
});
```



#### 参考博客

[npm相关配置](https://blog.csdn.net/weixin_45182409/article/details/117981169 "csdn博客npm相关使用")

#### 参考视频

[参考B站视频](https://www.bilibili.com/video/BV18s411E78K "尚硅谷MongoDB学习视频")
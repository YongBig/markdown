# MongoDB整理
 - [MongoDB概述](#Sketch)
 - [windows安装](#WindowsDL)
 - [初识MongoShell](#startMongoShell)
 - [MongoDB数据类型简述](#dataType)
 - [ 深入基础命令](#baseShell)
 - [索引](#index)
<h2 id="Sketch" style="color:blue">MongoDB 简述</h2>

### 1、计算机数据库的发展史

----------

 - 1951 
	 - -网状数据库模型（DBMS）
-	1970
	- -关系型数据库模型（RDBMS）
		- -1974 年 IBM的Ray和Don提出SQL语言
		- -1976年 Oracle1.0版本发布（后被sun公司收购）
		- -1984年 Sybase微软与IBM开发
		- -1985年 IBM的DB2发布
		- -1985年 MySQL瑞典AB团队开发（sun收购）
		- -1987年 SQLServer发布（收费）
- 1998
	- -非关系型数据库（NOSQL）
		- -纪元 1998年carlostrozzi开发的一个轻量开源，不提供SQL功能的关系数据库
		- -数据库 Cassandra、MongoDB、CouchDB、Readis、Riak、Membase、Neo4j、HBase .........

选择NOSQL的原因
	

 1. web2.0的诞生与发展，数据量庞大，需要大量的搜索功能。
 2. 快速读写，要求服务器的速度。
 
### 2、MongoDB发展史


----------


what？
> 是一个介于关系型数据库与非关系型数据库之间产品。

发展史

 - 2007年10月 MongoDB由10gen团队开发（后改名为MongoDB团队）
 - 2009年2月 首度推出
 - 2012年5月23日 MOngonDB2.1开发分支发布（正式版，稳定版本）
 - 截止本文撰写发布版本更新到 3.47版本
 -目前支持的语言：C、C++/.Net、Erlange、Haskell、Java、JavaScript、Lisp、nodeJs、Perl、PHP、Python、Uby、Scala、Golang..........
 
 <h2 id="WindowsDL" style="color:blue">Windows安装</h2>
 
###  1、[windows版本下载][1]


![enter description here][2]


###  2、==安装版本介绍==
  

  SSL：是MongoDB用于服务器与服务器之间传递的服务
  
  选择合适的安装包下载安装。
  
  默认安装：C:\Program Files\MongoDB\Server\3.4。
  
  自定义安装：可自定义安装路径，与选择性安装组件。
  
  ### 3、启动Mongo服务
  
  


----------
（1）、设置mongod 与 mongo 的全局变量（不做参数，不会百度）。
（2）、进入DOS窗口，设置数据保存目录（与端口）和日志输出目录。  
DOS命令：  
  

``` dsconfig
mongod --port <端口> --dbpath <数据路径> --logpath <日志路径> --logappend --directoryperdb  参数说明
```
指令：

 - --port    表示数据库端口，默认27017；  
-   --dbpath  表示数据文件存储路径，一般设置为%MONGODB_HOME%data；  
 -  --logpath 表示日志文件存储路径，一般设置为%MONGODB_HOME%logsmongodb.log；  
  - --logappend 表示日志追加，默认是覆盖；  
  - ---directoryperdb 表示每个db一个目录；
  
（3）、完成以上设置，MongoDB已经启动，新开启DOS窗口，执行“mongo.exe”，出现“MongoDB shell version: X.XX”表示安装成功了。  

（4）、目前是以无权限限制的方式启动的，你可以做任何操作。那么我们先切换到admin下，创建一个root用户吧。执行命令:  

``` elixir
"use admin" -> "db.addUser("root","root")" -> "db.auth("root","root")"
```
（5）、把MongoDB注册为Windows Service，让它开机自动启动；执行命令：
EX：

``` dsconfig
mongod --bind_ip 127.0.0.1 --logpath %MONGODB_HOME%logsmongodb.log --logappend --dbpath %MONGODB_HOME%data --directoryperdb --auth --install  
```

注意:
补充指令：

|参数|描叙|
| -------------------- | ----------------------------------------------------------------- |
| --bind_ip            | 绑定服务IP，若绑定127.0.0.1，则只能本机访问，不指定默认本地所有IP |
| --logpath            | 定MongoDB日志文件，注意是指定文件不是目录 |
| --logappend          | 使用追加的方式写日志                                              |
| --dbpath             | 指定数据库路径                                                    |
| --port               | 指定服务端口号，默认端口27017                                     |
| --serviceName        | 指定服务名称                                                      |
| --serviceDisplayName | 指定服务名称，有多个mongodb服务时执行。                           |
| --install            |            指定作为一个Windows服务安装。                                                       |

a.必须切换到bin目录下执行该条指令。  
    b.必须添加--auth用户权限才会生效。  
    c.除了“--auth”和“--install”两个参数，别的参数要跟你设置用户时启动服务的参数一致，尤其是“--directoryperdb”。  
    第一次配置完成后，一定要重启才会有效果 重启mongo客户端，不输入-u-p可以直接进入，但是不具有任何权限。正确的访问方式为：mongo 数据库名 -u 用户名 -p。另外设置用户  
	
  初识MongoShell
  
  


----------
### Mongoshell：在命令行的情况下操作MongoDB

1、启动MongoShell

出现三个警告（windows下只有两个）
	第一个：不安全，及没有设密码。
	第二个：你已经拥有火速据库的读和写的完全功能呢个。
	第三个：进程（只出现在Linux上）

==补充：==

支持一般的变量、函数。
支持 javaScript中的Math函数。
所有的mongoshell是小驼峰写法

2、显示所有的数据库

``` stylus
show dbs

#初始化数据库返回的是
admin
local
```
3、新建数据库

```stylus

use me

```

==注意：==
此时 输入 `show dbs`,发现没有me库。
原因：如果新建的库唯空，show dbs 则不显示。

4、插入数据

``` stylus
#声明变量
temp = {"name":"dy","age":"15"}

#进入me库插入数值
db.me.insert(temp)

#成功返回
WriteResult({"nInserted":1})

show dbs

#返回显示me库
admin 
local
me
```

5、查询数据

``` stylus
#显示me库所有值
show.me.find()

#查询第一条数据
db.me.findOne()
```
==注意：==
发现返回的结果中多出"_Id"的键，是mongo自动生成添加的唯一标识。

6、更新

``` stylus
#更新数据库中信息
db.me.update{{"name":"dy"},{"name":"ddy"}}
```
==注意：==
update()方法是找到谁更新谁，如果数据库原来数据是

``` stylus
{"_Id":ObjectId("123456789"),"name":"dy","age":"15"}
```
那么它只更新name的键为"ddy",那么之后的“age”更新狗会被删除。

7、删除数据

``` stylus
#先查询后删除该条数据
db.me.remove({"id":Object(xxxxx)})
```
8、删除库

``` stylus
db. 表名.dropDatabase();
```
==注意：==
如果是在当前me下则会孩子接删除
否则db.dropDatabase(me);

MongoDB数据类型简述


----------
### 1、简述
Mongo的数据类型类似于json，json是简单的数据局表达形式。

json的数据类型：

|  类型	   | 介绍     |
| --- | --- |
|    NULL | 空     |
|    Boolean |  true、false     |
|    String | 字符串    |
|  Array  | [1,2,3,4]    |
|   Object  | {"name":"dy"}     |
|    Number | 1 、2、3     |

  MongoDB数据类型
  

|   类型  | 介绍     |
| --- | --- |
|  Null   | 空    |
|    Boolean | true、false    |
|  String   | 字符串    |
|    Date 日期 | 使用的是JavaScript的new Date()    |
|    regular expression | 正则表达式    |
|   Array | 在Javascript中Array==Object    |
|    内嵌文档 | {a:{b:{c:“你的名字”}}}     |
|    Object |{name:"你的宝贝"}     |
|    二进制数组 |     |
|    Code | 代码（可将存储过程代码存储）    |
|   内部  | _Id 即ObjectId 自动生成的Id     |


<h2 id="baseShell" style="color:blue"> 深入基础命令</h2>


### 1、insert 插入

单条插入示例条件： Mongo数据库下，有log库，log库中有集合asdf.db表。插入数据

``` stylus
use log//使用log库

db.adsf.db.insert({"a":1})//插入数据

```

批量插入示例条件： Mongo数据库下，有log库，log库中有集合asdf表。批量插入数据
``` stylus
use log//使用log库

db.adsf.insert([//回车进入批量insert中
...{"_id":1}//回车
...{"_id":2},
...{"_id":3}
])//批量插入数据，回车
BulkWriteResult({//返回批处理结果
	"wirteError":[],//写入错误
	"writeConcernErrors":[],//写入连接错误
	"nInserted":3,//插入了多少数据
	"nUpserted":0,//更新了多少数据
	"nMatchet":0,
	"nModified":0,
	"nRemoved":0,
	"upserted":[]
})

```
==注意：批量插入必须是在数组中，批量插入速度大于单次一条一条插入的速度，如果mongoDB版本是在3.23以前,批量处理要使用batchInsert才能批量插入,"_id"是为唯一的值,如果插入有重复的值那么就会报错，insert批量插入，如果出错的，从出错的地方开始以后都不会执行==
拓展：如果写错了，只需三次回车就会推出.

mongoDB最大接受数据消息为48M。
### 2、删除文档
两种方法删除全部数据

``` stylus
db.asdf.drop();//删除asdf集合
db.asdf.remove({})//如果{}不写条件则删除全部数据，写入条件则查找到符合条件数据删除。

//删除全部数据 drop的速度是大于remove的速度
```



### 3、update修改器
外部操作
示例条件：
创建insert.js的文件
``` stylus
var user1={
	"name":"susan",
	"age":22,
	"sex":1,
	"del":
	{
		"province":"江苏"，
		"city":"南京",
		"address":"江苏省苏州市姑苏区道前街18号"
	},
	"regeditTime":new Date()
}
var user2={
	"name":"ddy",
	"age":22,
	"sex":1,
	"del":
	{
		"province":"江苏"，
		"city":"南京",
		"address":"江苏省苏州市姑苏区道前街18号"
	},
	"regeditTime":new Date()
}
var user3={
	"name":"xdy",
	"age":22,
	"sex":1,
	"del":
	{
		"province":"江苏"，
		"city":"南京",
		"address":"江苏省苏州市姑苏区道前街18号"
	},
	"regeditTime":new Date()
}

var db = connect('study');//连接study的库
db.user.insert([user1,user2,user3]);//在user的文档集合中插入三个值
print(''[ok] the data was inserted sucessfully')
```
在mogondb中载入这个js文件

``` stylus
//执行外部shell命令，这里的地址根据自己真实地址而填写
load("c:/data/shell/insert.js")
```
1>、updata 

这里我要修改 name = susan 中age为18

==（1）、注意第一种用法 写成关系型数据库的写法==
代码为：
新建update.js的文件
``` stylus
//连接数据库
var db = connect("study")

//关系型数据库写法
db.user.update({
	"name":"susan"
},{"age":18});
```
mogoshell中输入

``` stylus
//载入shell
load.user("c:/data/shell/update.js")

true //mongo返回的说明成功
```
结果我们发现我们的数据库却不是我们想要的结果那条数据变为：
{”_id“:ObjectId,"age":18}
其他数据消失了所以注意，文档型数据库我们的这样子的修改会覆盖整个文档。而不是更新其中的某一个字段。

==（2）、第二种用法==

新建update.js的文件
``` stylus
//连接数据库
var db = connect("study")

var user1={
	"name":"susan",
	"age":18,
	"sex":1,
	"del":
	{
		"province":"江苏"，
		"city":"南京",
		"address":"江苏省苏州市姑苏区道前街18号"
	},
	"regeditTime":new Date()
}

//直接写变量名
db.user.update({
	"name":"susan"
},{user1});
```
mogoshell中输入
``` stylus
//载入shell
load.user("c:/data/shell/update.js")

true //mongo返回的说明成功
```
结果该文档的结构却变为：
{”_id“:ObjectId,“user1”:{"name":"susan","age":18,"sex":1,"del":{"province":"江苏"，"city":"南京",“address”:"江苏省苏州市姑苏区道前街18号"},"regeditTime":new Date()}}

这样的修改会变为内嵌文档，变量名会变为其中的key字段，其他则变为他的内嵌文档。

==（3）、获取示例结果的方法==
新建update.js的文件
``` stylus
//连接数据库
var db = connect("study")

//直接写如完整的文档片段
db.user.update({
	"name":"susan"
},{
	"name":"susan",
	"age":18,
	"sex":1,
	"del":
	{
		"province":"江苏"，
		"city":"南京",
		"address":"江苏省苏州市姑苏区道前街18号"
	},
	"regeditTime":new Date()
});
```
mogoshell中输入
``` stylus
//载入shell
load.user("c:/data/shell/update.js")

true //mongo返回的说明成功
```
这样就会得到示例中想要的结果
{”_id“:ObjectId,"name":"susan","age":18,"sex":1,"del":{"province":"江苏"，"city":"南京",“address”:"江苏省苏州市姑苏区道前街18号"},"regeditTime":new Date()}

但是这样却很繁琐,所以就要用到我们的修改器$set拉，请接着看

 2>、update （$set）修改器
 ==what?修改一个指定的键值（key）==
举例：

``` stylus
db.user.update({
	"name":"susan"
},{
	"$set":
	{
		"sex":0
	}
})
```
mogonDB中susan的那条数据中只修改了sex那条数据，其它的保持不变。

``` stylus
{
	"name":"susan",
	"age":18,
	"sex":0,
	"del":
	{
		"province":"江苏"，
		"city":"南京",
		"address":"江苏省苏州市姑苏区道前街18号"
	},
	"regeditTime":new Date()
}
```
拓展：
如何修改内嵌文档
举例：修改del下的内嵌文档
``` stylus
{
	"name":"susan",
	"age":18,
	"sex":1,
	"del":
	{
		"province":"江苏"，
		"city":"南京",
		"address":"江苏省苏州市姑苏区道前街18号"
	},
	"regeditTime":new Date()
}
```
修改方法：

``` stylus
db.user.update({
	"name":"susan"
},{
	"$set":{
			"del.city":'南通'
	}
})
```
3>、update（$unset）
==what?用于将指定的key删除==
举例：删除susan 的age

``` stylus
db.user.update({
	"name":"susan"
},{
	"$unset":{
			"age":""//官方推荐删除的赋值直接写空就可以了
	}
})
```
删除后我们将数据恢复
如何恢复：
发现insert 并不合适，那么我们还是用$set来加入。
``` stylus
	db.user.update({
	"name":"susan"
},{
	"$set":{
			"age":18//$set是当没有找到指定的key时，则自动加入指定的key
	}
})
```
执行后 我们发现age的位置发生了变化变为
``` stylus
{
	"name":"susan",
	"sex":1,
	"del":
	{
		"province":"江苏"，
		"city":"南京",
		"address":"江苏省苏州市姑苏区道前街18号"
	},
	"regeditTime":new Date()，
	"age":"18"
}
```
age放在了最后，那是因为mongoDB是以文档片段形式存储的

4>、$inc 
==what?对指定的Key的数字类型的value进行计算==
举例：
假如susan的age变我们写错了，age为16。

``` stylus
	db.user.update({
	"name":"susan"
},{
	"$inc":{
			"age":-2//$inc  为复数则为减，为正数为加
	}
})
```
**注意！！！！执行后我们发现为错误**。

原因：我们在学习$unset时恢复age数据时，将age回复为字符串的"18"，$inc 只能对数子类型的进行增加修改。

在写mangodb脚本的时，外部执行不知道错误时，我们直接可以把脚本复制到终端，这样错误，就会显示出来。

5>、update 修改添加全部数据

举例：
假设我们要为每一条数据都要加上comment的数组的记录，那么我们写成

``` stylus
db.user.update({

},{
	"$set":{
		"comment":[]
	}
})
```
 执行后我们发现我们只给第一条数据加上了comment，其他后续都没有。
 ==注意，update只发现第一条数据，并执行相应的修改器后停止。==
 如何继续修改全部数据：
将update方法打印出来我们发现

``` stylus
//打印update方法
db.user.update //回车
//输出了 update的方法
function(query,obj,upsert,multi){
..............
}
```
	(1)、query参数时我们的查询条件 
	(2)、obj 修改器的一些方法
	(3)、multi  是对象 有true 和false 两个值，为true时查询所有数据并修改，默认值为false，修改查询到的第一条数据及停止
举例：

``` stylus
db.user.update({

},{
	"$set":{
		"comment":[]
	}
},{
	"multi":true
})
```
	(4)、upsert  对象两个参数 为true时，如果没有找到则新建，为false（默认）找不到则结束。

举例：

``` stylus
db.user.update({
    "name":"John"
},{
	"$set":{
		"comment":[]
	}
},{
	"upsert":true
})
```
6>、数组修改器
	（1）、$push 追加数据到数据最后。
	举例：
``` stylus
	db.user.update({
    "name":"John"
	},{
		"$push":{
			"comment":"100"
		}
	})
```
执行则john那条数据则变更为:
`{"_id":ObjectId("788768zdfzfzxfzXXXXXX"),"name":"john","comment":["100"]}`
再次执行
`{"_id":ObjectId("788768zdfzfzxfzXXXXXX"),"name":"john","comment":["100","100"]}`
==拓展：==
内联文档与数组，其实都时一样，那么我们可以在内联文档中使用数组的一些修改器。
举例：
给susan的del增加一条邮编上去。

``` stylus
	db.user.update({
    "name":"susan"
	},{
		"$push":{
			"del.zip":"10101010"
		}
	})
```
（2）、$ne
==what:查找是否存在，如果不存在，则执行修改器中的方法==
举例：
查找John 中 comment数据中是否有"200",没有的话加入"300"

``` stylus
db.user.update({
    "name":"John",
	"comment":{"$ne":"200"}
	},{
		"$push":{
			"comment":"300"
		}
	})
```
注意是没有

（3）、$addToSet
==what?查找是否存在，不存在添加（新版本替代$ne）==
举例：
``` stylus
db.user.update({
    "name":"John",
	},{
		"$addToSet":{
			"comment":"200"
		}
	})
```
（4）、$each批处理
==what？追加数组/内联文档 到 数组/内联文档==
举例：
为comment数组追加500，600，700
``` stylus
db.user.update({
    "name":"John",
},{
	"$addToSet":{
	"comment":{
		"$each":[500,600,700]
	}
}
})
```
执行后结果：
`{"_id":ObjectId("788768zdfzfzxfzXXXXXX"),"name":"john","comment":["100","100"，"300","200",500,600,700]}`

（5）、$pop 
==waht: 两个值  为1时 数组末端删除，为-1数组头部删除==?
举例：
``` stylus
db.user.update({
    "name":"John",
},{
	"$pop":{
	"comment":1//删除comment中的700
}
})
```
（6）、直接定位数组
举例：
修改comment下的第3个数值
``` stylus
db.user.update({
    "name":"John",
},{
	"$set":{
	"comment.2":"aaa"
}
})
```
（7）、不确定匹配数组
举例：

``` stylus
var friend = {
	"user":"Dy",
	"list":[
		{
			"name":"John",
			"e-mail":"John@gmail.com"
		},
		{
			"name":"susan",
			"e-mail":"susan@gmail.com"
		},
		{
			"name":"solid",
			"e-mail":"solid@gmail.com"
		}
	]
}

db.friend.insert(friend)
```
执行
 现在有一个需求 susan改了email 
 

``` stylus
db.friend.update({
	"name":"Dy",
	"list.$.name":"susan"//不确定的下标用$代替
},{
	"$set":{
		"list.$.email":"susan@qq.com"
	}
})
```
### 4、 findAndModify
mongodb的安全机制三个方面：
1、更新
2、插入
3、删除
==非应答与应该式写入：最简单的就是执行命令，有返回相应的数据。那么安全机制就是在返回的数据中，进行验证。==

为什么要有安全机制：现在的计算都是多进程的运行，mongo的实现数据的更新，插入等，时候按先到先得方式进行处理。所以如果我们有一条进程新建一条记录，另一条进程修改该条数据，mongoDB可能先接受到的是修改，这时候就会出错。所以出现安全

1>、db.Command()
what?
Command函数中可以执行所有的mongodb命令,相当于Windows当前注册表的接口，并且可以修改所有mogodbde配置。

``` stylus
db.listCommands()//可以显示所有MongoDB的命令
```
举例：

``` stylus
use study

db.user.update({"sex":1},{"$set":{"num":999}},false,false)//更新sex为1的文档片段中，设置num为999

var output = db.runCommand({getLastError:1})//如果有错误获取错误值

printjson(out)//打印为json格式
```
当然runCommand参数中不仅仅只有getLastError这个属性，我们可以通过其键来使用任何mongodb的方法。

2>、findAndModify
举例：

``` stylus
var tmp = db.runCommand({
	“findAndModify”:"user",//findAndModify这个命令使用早user库中
	"query":{"name":"dy"},//查询name为dy
	"update":{"$ser":{"age":16}},//更新他到16
	"new":true//位true，更新完后反回，false就是更新完后该干嘛干嘛。
})

printjson(temp)
```
这是用runCommand运行的，findAndModify模式到底有什么参数。

``` stylus
findAndModify({
	//query:<查询文档>,
	//sort:<"sex":1>，sex为1的排序,
	//remove:<boolean  表示是否删除文档>,
	//fields:,
	//update:<修改器文档>,
	//new:<boolean 返回更新前的文档还是更新后的文档>,
	//fields:<document 需要反回的字段>,
	//upsert:<boolean>,
	//bypassDocumentValidation:<boolean  文件验证旁路>,
	//writeConcern:<document 关注>,
	//collation:<document>
})
```
==注意remove与update必须要有一个存在，并且不能共存,findAndModify的命令效率低于其他直接命令，但是可以有效的保护我们进程，所以==

command可以修改mongodb的参数。
举例：

``` stylus
db.runCommand({
	"ping":1//和服务器连接是否正常
})
```
### 5、 find查找

1> 、查询条件（分点）

(1)、正常查找
``` stylus
db.user.find(
	{"del.city":"天津”}
)
```
返回全部

``` stylus
{ "_id" : ObjectId("59fafffc181d50551c2d4e9f"), "name" : "前进6", "age" : 22, "sex" : 1, "del" : { "province" : "天津", "city" : "天津", "address" : "天津南路11弄" }, "regedtTime" : ISODate("2017-11-02T11:22:36.252Z") }
```
有条件的查询，返回固定信息

``` stylus
db.user.find(
	{"del.city":"天津”}，
	{“name”:1,"age":1}//这里的1是true的意思
)
```
返回

``` stylus
{ "_id" : ObjectId("59fafffc181d50551c2d4e9f"), "name" : "前进6", "age" : 22 }
```
==我们发现，尼玛每次都有_id,我不想要怎么办==
那么这样做

``` stylus
db.user.find(
	{"del.city":"天津”}，
	{“name”:1,"age":1,"_id":0}//这样就好了
)
```
以上就是我们正常的查询，全部是==的查询。

(2)<、>、<=、>=、!=查询

``` stylus
// 小于 小于等于 $lt、 $lte
//大于 大于等于  $gt、$gte
//不等于	$ne

//举例x>=20,x<=30
db.user.find(
{"age":{"$lte":30,"$get":20}},//查询年龄小于等于30，大于等于20的数据
{"name":1,"age":1,"_id":0}
)

//查询时间
start = new Date(01/01/2017")
db.user.find(
{"regeditTime":{"$ge":start}},//查询时间大于start的值
{"name":1,"age":1,"_id":0}
)

```
(3)、$in,$nin查询
$in 查询一键多值
$nin 查询除了in以外的数据
举例
``` stylus
db.user.find(
{"age":{"$in":[22,38]}},//查询年龄是22和38的值
{"name":1,"age":1,"_id":0}
)

db.user.find(
{"age":{"$nin":[22,38]}},//查询除了年龄是22和38的值
{"name":1,"age":1,"_id":0}
)
```
(4)、$or $and $not多条件查询 

$or 是或的欢喜
$and 是且的关系

举例：
查询年龄小于等于30，或人在北京的宝宝
``` stylus
db.user.find(
	{"
		$or":[
				{"age":{"$lte":30}},
				{"del.city":"北京"}
			]
	},
	{"name":1,"age":1,"del.city":1,"_id":0}
)
```
返回

``` stylus
{ "name" : "前进", "age" : 15, "del" : { "city" : "上海" } }
{ "name" : "前进4", "age" : 12, "del" : { "city" : "兰州" } }
{ "name" : "前进5", "age" : 23, "del" : { "city" : "哈尔滨" } }
{ "name" : "前进6", "age" : 22, "del" : { "city" : "天津" } }
{ "name" : "前进7", "age" : 22, "del" : { "city" : "徐州" } }
{ "name" : "前进8", "age" : 25, "del" : { "city" : "广州" } }
{ "name" : "前进10", "age" : 27, "del" : { "city" : "法兰西" } }
```


$not 不匹配应用于条件之上，$not必须加在条件之上才可以

``` stylus
db.user.find(
	{"age":{
		"$not":{
			"$lte":30,
			"$gte":20
		}
	}},
	{"name":1,"age":1,"_id":0}
)
```
返回

``` stylus
{ "name" : "前进", "age" : 15 }
{ "name" : "前进2", "age" : 35 }
{ "name" : "前进3", "age" : 40 }
{ "name" : "前进4", "age" : 12 }
{ "name" : "前进9", "age" : 35 }
```
(5)、$sexists 色鬼
假设：文档数据中某个键的值为null，那么我们在查询时使用：

``` stylus
db.user.find({"login":null})
```
==查询的数据中记录中，起是包含了没有login字段的数据，并不是我们想要的结果，那么我需要用色鬼模式查询==

$sexists 色鬼模式
两种情况：
当为 true 能查询到，null的值
当为 false不能被查到

实例:

``` stylus
db.user.find({
	"login":{"$in":[null],"$sexists":true}
})
```
==补充find是支持正则的==
实例：

``` stylus
db.user.find({"name":/张*/i})
```
（6）、查询数组 $all $in $size $slice
数据空有hobby字段，是一个数组结构那么，我们怎么条件查询它。

实例：
``` stylus
db.user.find({“hobby”:[" 足球","篮球","保龄球"]})

//查询喜欢足球的
db.user.find({“hobby”:[" 足球"]})//这种写法是错误的

//正确的写法
db.user.find({“hobby”:"足球"})//这是查询单项

//查询的多条件的写法

//第一种查询 $all 满足花花，抒发的条件的数据 
db.user.find({“hobby”:{"$all":["花花","抒发"]}})

//第二种查询，满足其中一项$in,满足其中一个即返回
db.user.find({"hobby":{"$in":["花花","抒发"]}})

//第三种查询，查找数组length满足条件的
db.user.find({
	"hobby":{
		"$size":{"$gt":3} //爱好大于三个
	}
})

//第四种，$slice 限制
//错误写法
db.user.find(
{},{"name":1,"age":1,"hobby":1,"_id":0},
{"hobby":{"$slice":2 //只想显示前两个爱好}
)//发现，尼玛并没有过滤，全部显示

//正确写法
db.user.find({},
{	
	"name":1,
	"age":1,
	"hobby":{"$slice":2},
	"_id":0
}
)

//分页的写法
db.user.find({},
{	
	"name":1,
	"age":1,
	"hobby":[3,10],//第三页，每页10条数据
	"_id":0
}
)

```
（7）、find参数
find（query,fields,limit,skip,sort,batchSize,options）方法参数中有如下几个：

 ==注意find的{}是不可以省略的！区别与update==

 1. query ：条件
 2. fields ：返回的内容
 3. limit：返回的数量
 4. skip：跳过多少个现实
 5. sort：排序方式  ({"age":1} 由大到小，为-1由小到大)
 6. batchSize：游标
 7. options：(后面的介绍)
 
 举例子：分页
 

``` stylus
db.user.find().limit(2).skip(3).sort({"age":1})
//查询user表每次只返回两条数据，skip跳过3条数据，按age从大到小排序
```
举例：如何复杂查询 $where

``` stylus
//$where 复杂查询 他的值是支持js的，this代表的是当前user的数据库。
db.user.find({"$where":"this.age>30"})//查找数据数据集合中当前age大于30的数据
```
==注意：$where,要谨慎使用，如果是前段传过来的脚本验证测试时，容易把我们整个数据库攻破，where权限很大，直接暴露我们的数据库，推荐只在内部使用，不涉及外部数据的操作使用。==

问题：如何在js的脚本中快速运行find的语句

batchSize： 这里我们用到这个属性，叫做游标：数据库文档的指针。
原先找寻数据库，是全文检索，你需要哪里，我在返回哪里，效率极低。
现在：我们引入了batchSize游标，直接指向数据库中数据，下面一个章节将会介绍索引的使用。

打印所有单条数据
``` stylus
//cursor
var db = connect('study')//连接库

var rs  = db.user.find()//查询所有

// 循环   hasNext()判断是否有下一个哈希数据，有的话返回，无返回false
while(rs.hasNext()){
	printjson(rs.next())//逐条打印下一个的游标
}

//forEach循环
rs.forEach(function(data){
	printjson(data)//打印当前记录内容。
})
```
<h2 id="index" style="color:blue">MongoDB 索引</h2>

### 1、建立索引

==MongoDB最快的查询，是按_id查询，但是正常查询都是值得查询，这需要我们建立索引来帮助查询。==

这里那用户名举例子，查询百万级别的库，正常查询与索引查询的速度。

本次我们要用到nodejs，由于人电脑是mac不错介绍安装。

第一步，编写js生成随机生成用户名

``` javascript?linenums
//获取随机数
function GetRandomNum(min,max){
	var range = max -min
	var rand = Math.random()
	return (min + Math.round(rand * range))
}

function GetRandomUserName(min,max){
	var char ="0987654321_qwertyuiopasdfghjklzxcvbnm".split("")
	var outputText = ""
	for(var i = 1 ;i<GetRandomNum(min,max);i++){
		outputText = outputText + char[GetRandomNum(0,char.length)]
	}
	return outputText;
}

var starttime = (new Date()).getTime()
var db = connect('study')
db.asdf.drop()

var tempArry = []

for (var i =0; i<2000000;i++){
	tempArry.push({
	"username":GetRandomUserName(7,15),
   	"regediTime":new Date(),
	"randomNum0":GetRandomNum(100000,1000000),
   	"randomNum1":GetRandomNum(100000,1000000),
   	"randomNum2":GetRandomNum(100000,1000000),
   	"randomNum3":GetRandomNum(100000,1000000),
   	"randomNum4":GetRandomNum(100000,1000000),
   	"randomNum5":GetRandomNum(100000,1000000),
   	"randomNum6":GetRandomNum(100000,1000000),
  	 "randomNum7":GetRandomNum(100000,1000000),
   	"randomNum8":GetRandomNum(100000,1000000),
   	"randomNum9":GetRandomNum(100000,1000000)
	})
}

db.asdf.insert(tempArry)

var runtime = (new Date()).getTime() - startTime
print ('this run time is :' + runTime + 'ms')
//console.log(GetRandomUserName(7,15))
```
==以上命令制造百万级别数据库==

查看当前数据集合 asdf 的数据状态

``` javascript
db.asdf.stats()//查看数据状态
//返回
{
        "ns" : "study.asdf",
        "count" : 2000000,
        "size" : 533272199,
        "avgObjSize" : 266,
        "storageSize" : 236920832,
        "capped" : false,
        "wiredTiger" : {
		.......................
}
```
（1）、正常查找数据

``` javascript
//查找
db.asdf.find().skip(40000)//查找数据，但是跳过四万条

//数据过多时，输入 it 下一页
it
```
==查找单条数据 username:drehtyt8==
indexex

``` javascript?linenums
var starttime = (new Date()).getTime();
var db = connect('study');

//查找数据
var rs = db.asdf.find({"username":"drehtyt8"});

while (rs.hasNext()){//判断是否有重复的数据,第一条为原数据
    printjson(rs.next())//逐条打印
}
var runtime = (new Date()).getTime() - starttime;
print ('this run time is :' + runtime + 'ms');

```
==返回==

``` javascript?linenums	
{
        "_id" : ObjectId("5a37801641ce6fec9fc50145"),
        "username" : "drehtyt8",
        "regediTime" : ISODate("2017-12-18T08:41:11.384Z"),
        "randomNum0" : 792594,
        "randomNum1" : 415801,
        "randomNum2" : 203506,
        "randomNum3" : 391589,
        "randomNum4" : 831088,
        "randomNum5" : 588160,
        "randomNum6" : 706367,
        "randomNum7" : 353796,
        "randomNum8" : 565257,
        "randomNum9" : 276867
}
//this run time is :1219ms 一条数据的查询时间
```
（2）、建议索引查找数据

怎么建立索引查找数据?
==使用ensureIndex==
``` javascript?linenums

db.asdf.ensureIndex({"username":1}) //意思确保索引值建立索引(),建立username的索引

```
那么没建立索引的时候mongodb会自动给"_id"加上索引

``` javascript?linenums
db.asdf.getIndexes()//回去当前集合的索引

//返回
[
        {
                "v" : 1,
                "key" : {
                        "_id" : 1
                },
                "name" : "_id_",
                "ns" : "study.asdf"
        }
]
```
那么运行db.asdf.ensureIndex({"username":1})，一段时间后会自动为mongodb中的每一条username添加索引
成功后会返回

``` javascript?linenums
{
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 2,
        "ok" : 1
}
```
这时候重复db.asdf.getIndexes()，会发现username也有索引了

``` javascript?linenums
[
        {
                "v" : 1,
                "key" : {
                        "_id" : 1
                },
                "name" : "_id_",
                "ns" : "study.asdf"
        },
        {
                "v" : 1,
                "key" : {
                        "username" : 1
                },
                "name" : "username_1",
                "ns" : "study.asdf"
        }
]
```
这时候重新load之前查询文件,我们发现

``` stylus
> load ("C:/Program Files/MongoDB/Server/indexes.js")
connecting to: study
{
        "_id" : ObjectId("5a37801641ce6fec9fc50145"),
        "username" : "drehtyt8",
        "regediTime" : ISODate("2017-12-18T08:41:11.384Z"),
        "randomNum0" : 792594,
        "randomNum1" : 415801,
        "randomNum2" : 203506,
        "randomNum3" : 391589,
        "randomNum4" : 831088,
        "randomNum5" : 588160,
        "randomNum6" : 706367,
        "randomNum7" : 353796,
        "randomNum8" : 565257,
        "randomNum9" : 276867
}
this run time is :250ms
true
```
我们查询的时间只有7ms了 缩短近150倍

<span style="color:#e4393c">注意：mongoDB每一个集合可以建立64个索引值,索引越多，硬盘与内存的使用空间也会增加，量力而行之</span>

### 2、索引的利弊
==补充：基数==
什么是基数：
集合中模个字段，拥有不同值得数量称作基数。
举例：sex 字段值有1,0 拥有两个基数 基数少
			username 的值坑不同 基数比较多
那么衍生一个概念，并不是所有的字段建立索引查询都会变快。
当搜索的结构大于30%时，就会变慢。

==索引的原理==
原理：索引根据键值，建议一个有逻辑顺序的表。
如：0-z , 假设w 对应的是_id，_id对应就是username-_id 值
如果 搜索的记过大于30%,那么反而，相当于便利两个表，德堡尝失。

### 3、复合索引查询

复合索引查询：索引可以复合多组合的查询

``` javascript?linenums
//使用之前的200W数据
//按照建立索引教程，建立一个RandomNum4 的索引
 //建立成功后 我们复合查询
 
 db.asdf.find({"RandomNum4":224154,"username":"dydydyd"})
//14 ms - 16ms 之间的，复合索引

```
==注意：在mongodb2.0期间，当你写入两个索引，进行复合查询的时候，它只会按第一个索引作为基准进行查询，所以当你的索引顺序写入不同时候，他返回查询时间差距是比较大的 #e4393c==

==注意：在mongodb3.0期间，当你写入两个索引，进行复合查询的时候，它的基准是按照复合条件中在getIndexes里表里的第一位基准，再有大量索引是将会优化速度 #e4393c==

当前使用版本是3.2，所以复合索引是最新的基准模式，那么出现一个==问题 #E4393c==。
例如：RandomNum4 基数是数字，而username则字母加上数字，这样明显按RandomNum4的基准查询更快一些，但我们使用过的3.2的版本，复合索引是最新的模式，那么我们会按userName的索引进行查询。明显相率低了一级。

  解决方法： hint()
  
``` javascript?linenums
//用RandomNum,作为基准查询
 db.asdf.find({"RandomNum4":224154,"username":"dydydyd"}).hint({"RandomNum4":1})

```
==补充：查询:bool无需做索引，==快，其次<、>、<=、>=、最慢是!= ==

建议： 如果我么你的一各集合会反复查询，那么推荐不要索引字符串，推荐索引内嵌文档，更精确的查找。

#### 删除索引：
	

``` javascript?linenums
db.asdf.dropIndex("索引的唯一Id")
//唯一Id是什么
db.asdf.getIndexes()
[
        {
                "v" : 1,
                "key" : {
                        "_id" : 1
                },
                "name" : "_id_",
                "ns" : "study.asdf"
        },
        {
                "v" : 1,
                "key" : {
                        "username" : 1
                },
                "name" : "username_1",//这里就是username的唯一的id ==> "username_1",也就是name的值
                "ns" : "study.asdf"
        }
]
//返回
```


  [1]: https://www.mongodb.com/download-center
  [2]: ./images/2017-12-21_200857.png "2017-12-21_200857"
  [3]: ./images/mongodw.jpg "mongodw"
  
 ### 4、固定集合索引
 ==固定集合 #800000==
集合可以限制存储大小，及集合中的数量，限制的集合我们成为固定集合。

| 固定集合限制| |
| ---- | -------------- |
| max  |   限制集合文件的数量  |
| size | 限制文件的大小 |
 
 #### ==创建日志的固定集合 #007480==
 

``` javascript?linenums
//Capped集合是固定大小的集合,
db.createCollection("log",{"capped":true,"size":10000,"max":10})
```
==固定的集合是不可以修改的，只可以删除，建立，或把普通集合转换为固定的集合 #800000==
==size与max字段，当同时出现时，则会遵循优先原则，先先卸载前，执行谁。 #800000==

#### ==普通集合转换为固定集合 #007480==

``` javascript
//假如有普通的集合 logold 转换为固定集合
db.runCommand({"converToCapped":"logold","max":10})
//!!! 固定集合是不可以转换为普通集合的
```

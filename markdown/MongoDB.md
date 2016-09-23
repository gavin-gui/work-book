## MongoDB
@(guichao的笔记本)[java]

----------

### 查询
#### **条件操作符**
- `$lt`: 小于
- `$gt`: 大于
- `$lte`: 小于等于
- `$gte`: 大于等于
- `$ne`: 不等于

**实例**
``` javascript
>db.col.find(
    {
        'likes':{
            $gt:100,
            $lt:500
        }
    }
)
```
查询likes>100 && likes<500的数据

----------


#### AND条件
MongoDB的find方法可以传入多个键(key)，每个键以逗号隔开，相当于sql的AND条件。
``` javascript
db.col.find({key1:val1,key2:val2});
```

----------


#### OR条件
MongoDB OR条件语句使用关键字`$or`，语法格式如下：
``` javascript
>db.col.find(
    {
        $or:[
            {key1:val1},{key2:val2}
        ]
    }
).pretty();
```
**AND 和 OR联合使用**
```javascript
>db.col.find(
    {
        {'likes':100},
        $or:[
            {key1:val1},
            {key2:val2}
        ]
    }
).pretty();
```

#### IN 和 NIN
MongoDB IN/NIN条件语句使用关键字`$or`/`$nin`.

**实例**
``` javascript
>db.col.find(
    {
        'likes':{
            $in:[100,500]
        }
    }
)
```

```javascript
>db.col.find(
    {
        'tags':{
            $nin:['database']
        }
    }
)
```

#### 正则查询
MongoDB中使用Perl规则的正则语法。

**实例**
```javascript
>db.col.find(
    {
        'title':/mysql?/i
    }
).pretty();
```

#### 数组数据查询



----------


### limit和skip方法
1. limit方法
   读取指定数量的数据记录，limit方法接收一个整形参数
   limit()方法基本语法如下：
```javascript
>db.COLLECTION_NAME.find().limit(NUMBER)
```

2. skip方法
   跳过指定数量的数据

skip()方法的基本语法：
```javascript
>db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)
```
<i class="icon-tag"></i>注意：skip()方法默认参数是0。

----------


### 排序
MongoDB使用sort()方法对数据进行排序，sort方法可以通过参数指定排序字段，并使用1或-1来指定排序的方式，其中1为升序排列，而-1为降序排列。

sort()方法的基本语法如下：
```javascript
>db.COLLECTION_NAME.find().sort({key:1});
```
<i class="icon-tag"></i>注意：如果没有指定sort()方法的排序方式，默认使用文档的升序排列。

----------


### MongoDB索引
索引能够极大的提高查询效率，如果没有索引，MongoDB在读取数据时必须要扫描集合中的每个文件并选取那些符合条件的记录。
这种扫描全集合的查询效率是非常低的，特别是在处理大量数据的时候， 查询可能要花费几十秒甚至几分钟，这对网站的性能是致命的。
索引是特殊的数据结构，索引存储在一个易于遍历读取的数据集合中，索引是对数据表中一列或多列的值进行排序的一种结构。

MongoDB使用ensureIndex()方法来创建索引。

**语法**
ensureIndex()方法的基本语法格式如下：
```javascript
>db.COLLECTION_NAME.ensureIndex({key:1})
```
语法中Key值要创建索引的字段，1为至指定升序创建索引，-1则为使用降序来创建索引。

**实例**
```javascript
>db.col.ensureIndex({'title':1});
```
ensureIndex()方法也可以设置多个字段创建索引（关系数据库中称为复合索引）
```javascript
>db.col.ensureIndex({'title':1,'description':-1})
```
ensureIndex()接收可选参数，可选参数列表如下：
| Parameter          | Type          | Description                              |
| :----------------- | :------------ | :--------------------------------------- |
| background         | Boolean       | 建索引过程会阻塞其它数据库操作，background可指定以后台方式创建索引，即增加 "background" 可选参数。 "background" 默认值为false。 |
| unique             | Boolean       | 建立的索引是否唯一。指定为true创建唯一索引。默认值为false.       |
| name               | string        | 索引的名称。如果未指定，MongoDB的通过连接索引的字段名和排序顺序生成一个索引名称。 |
| dropDups           | Boolean       | 在建立唯一索引时是否删除重复记录,指定 true 创建唯一索引。默认值为 false. |
| sparse             | Boolean       | 对文档中不存在的字段数据不启用索引；这个参数需要特别注意，如果设置为true的话，在索引字段中不会查询出不包含对应字段的文档.。默认值为 false. |
| expireAfterSeconds | integer       | 指定一个以秒为单位的数值，完成 TTL设定，设定集合的生存时间。         |
| v                  | index version | 索引的版本号。默认的索引版本取决于mongod创建索引时运行的版本。       |
| weights            | document      | 索引权重值，数值在 1 到 99,999 之间，表示该索引相对于其他索引字段的得分权重。 |
| default_language   | string        | 对于文本索引，该参数决定了停用词及词干和词器的规则的列表。 默认为英语      |
| language_override  | string        | 对于文本索引，该参数指定了包含在文档中的字段名，语言覆盖默认的language，默认值为 |

**实例**
在后台创建索引：
```javascript
db.values.ensureIndex({open: 1, close: 1}, {background: true})
```
通过在创建索引时加`background:true`的选项，让创建工作在后台执行。

----------
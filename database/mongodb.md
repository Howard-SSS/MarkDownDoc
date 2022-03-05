# mongodb是什么

可拓展、面相文档的非关系数据库，C++语言编写，支持bson格式存储数据，支持丰富的查询语言

# 数据模型

**非规范化数据模型（嵌入式数据模型）**

**规范化数据模型**

# 基本

```java
use COLLECTION_NAME // 切换数据库，不存在则创建
db // 查看当前数据库
show dbs // 查看所有数据库
show collections // 查看当前数据库所有集合
db.system.users.find().pretty() // 查看用户
db.createUser({user:"NAME", pwd:"PASSWORD", roles:[{role:"OPTION", db: "COLLECTION_NAME"}]})
```

# ObjectId

12字节的BSON类型

- 前 4 个字节表示时间戳；
- 接下来的 3 个字节表示机器标识符；
- 紧接着的 2 个字节由进程 id（PID）组成；
- 最后 3 个字节是一个随机计数器的值。

# 插入

```java
db.createCollection( // 创建集合
    COLLECTION_NAME,
    {
        capped: <boolean>, // 可选,是否固定大小，超出则覆盖最早文档，该值为true须指定size
        size: <number>, // 可选,指定最大字节数
        max: <number> // 可选，指定文档最大数量
    }
)
```

```java
db.COLLECTION_NAME.insert(document) // 集合中插入文档, 若主键存在会抛出错误
```

```java
db.COLLECTION_NAME.replaceOne(document) // 集合中插入文档,若主键存在会替换内容
```

# 查询

```java
db.COLLECTION_NAME.find( // 非结构化查看集合
	<QUERY>, // 指定查询条件
    <PROJECTION> // 指定返回的键
) 
db.COLLECTION_NAME.find().pretty() // 结构化查看集合
```

**AND 条件**

```java
db.COLLECTION_NAME.find(
    {
        KEY1: VALUE1,
        KEY2: VALUE2
    }
)
db.COLLECTION_NAME.find(
    {
        $and: [{
            KEY1: VALUE1
        }, {
            KEY2: VALUE2
        }]
    }
)
```

**OR 条件**

```java
db.COLLECTION_NAME.find(
    {
        $or: [{
            KEY1: VALUE2
        }, {
            KEY2: VALUE2
        }]
    }
)
```

**limit 方法**

```java
db.COLLECTION_NAME.find().limit(number) // 查询number个文档
```

**skip 方法**

```java
db.COLLECTION_NAME.find().skip(number) // 跳过number个文档
```

**sort 方法**

```java
db.COLLECTION_NAME.find().sort({KEY1: 1/-1}) // 1-升序，-1-降序，指定字段排序，默认文档升序
```

|      |        |
| ---- | ------ |
| $gt  | >      |
| $gte | >=     |
| $lt  | <      |
| $lte | <=     |
| $ne  | !=     |
| $in  | 包含   |
| $nin | 不包含 |

# 更新

```java
db.COLLECTION_NAME.update(
	QUERY, // 查询条件
    UPDATE, // 更新操作
    {
        upsert: <boolean>, // 不存在是否插入，默认false
        multi: <boolean>, // 是否更新多条，默认false
        writeConcern: <document> // 抛出异常级别
    }
)
```

```java
db.COLLECTION_NAME.save(
	DOCUMENT,
    {
        writeConcern: <document>
    }
)
```

# 删除

```java
db.dropDatabase() // 删除当前数据库
db.COLLECTION_NAME.drop() // 删除集合
```

```java
db.COLLECTION_NAME.remove(
	QUERY,
	{
		justOne: <boolean>, // 是否只删除一个文档，默认false
		writeConcern: <document>
	}
)
```

# $type

| **类型**                | **数字** | **备注**         |
| :---------------------- | :------- | :--------------- |
| Double                  | 1        |                  |
| String                  | 2        |                  |
| Object                  | 3        |                  |
| Array                   | 4        |                  |
| Binary data             | 5        |                  |
| Undefined               | 6        | 已废弃。         |
| Object id               | 7        |                  |
| Boolean                 | 8        |                  |
| Date                    | 9        |                  |
| Null                    | 10       |                  |
| Regular Expression      | 11       |                  |
| JavaScript              | 13       |                  |
| Symbol                  | 14       |                  |
| JavaScript (with scope) | 15       |                  |
| 32-bit integer          | 16       |                  |
| Timestamp               | 17       |                  |
| 64-bit integer          | 18       |                  |
| Min key                 | 255      | Query with `-1`. |
| Max key                 | 127      |                  |

# 索引

```java
db.COLLECTION_NAME.createIndex(
    {KEY: 1}	
)
db.COLLECTION_NAME.ensureIndex(
    {KEY: 1/-1} // 根据字段创建索引，1-升序创建，-1-降序创建
)
db.COLLECTION_NAME.dropIndex(
    {KEY: 1} // 删除索引
)
```

索引存储在内存中

**查询限制**

在以下的查询中，不能使用索引：

- 正则表达式或否定运算符，例如 \$nin、​\$not 等；
- 算术运算符，例如 \$mod 等；
- \$where 子句。

**最大范围**

在定义索引时有以下几点需要注意：

- 集合的索引不能超过 64 个；
- 索引名称的长度不能超过 128 个字符；
- 复合索引最多可以拥有 31 个字段。

# 聚合

```java
db.COLLECTION_NAME.aggregate(
    [{
        $group: { // 计算总和，每个加NUMBER
            _id: "$KEY",
            FIELD_NAME: {
                $sum: NUMBER
            }
        }
     }, {
        $group: { // 计算平均值
            _id: "$KEY_NAME",
            FIELD_NAME: {
                $avg: "$KEY_NAME"
            }
        }
    }, {
        $group: { // 获取集合中所有文档对应值的最小值
            _id: "$KEY",
            FIELD_NAME: {
                $min: "$KEY_NAME"
            }
        }
    }, {
        $group: { // 获取集合中所有文档对应值的最大值
            _id: "$KEY_NAME",
            FIELD_NAME: {
                $max: "$KEY_NAME"
            }
        }
    }, {
        $group: { // 在结果文档中插入值到一个数组中
            _id: "$KEY_NAME",
            FIELD_NAME: {
                $push: "$KEY_NAME"
            }
        }
    }, {
        $group: { // 在结果文档中插入值到一个数组中，但不创建副本
            _id: "$KEY_NAME",
            FIELD_NAME: {
                $addToSet: "$KEY_NAME"
            }
        }
    }, {
        $group: { // 根据资源文档的排序获取第一个文档数据
            _id: "$KEY_NAME",
            FIELD_NAME: {
                $first: "$KEY_NAME"
            }
        }
    }, {
        $group: { // 根据资源文档的排序获取最后一个文档数据
            _id : "$KEY_NAME",
            FIELD_NAME: {
                $last: "$KEY_NAME"
            }
        }
    }]
)
```

# 管道

```java
db.COLLECTION_NAME.aggregate(
    {
        $project: {
            KEY1: 1,
            KEY2: 1
        }
    }
)
$project // 选择输出的字段
{$match: {KEY1: VALUE1}} // 过滤数据
$group // 分组
{$sort: {KEY1: 1/-1} // 排序,1-升序，-1-降序
{$skip: NUMBER} // 跳过NUMBER个文档
{$limit: NUMBER} // 只返回NUMBER个文档
$unwind // 将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值
```

# 备份和恢复

```java
mongodump
    -h dbhost // 备份地址
    -d dbname // 备份数据库实例
    -o dbdirectory // 备份数据存放位置
```

```java
mongorestore
```

# Java使用

```java
@Component
public class StudentDao {

    private static final String COLLECTION_NAME = "student";

    @Autowired
    private MongoTemplate mongoTemplate;

    public void insertStudent(Student student) {
        // db.COLLECTION_NAME.insert(DOCUMENT)
        mongoTemplate.insert(student, COLLECTION_NAME);
    }

    public void saveStudent(Student student) {
        // db.COLLECTION_NAME.save(DOCUMENT)
        mongoTemplate.save(student, COLLECTION_NAME);
    }

    public Student findById(Long id) {
        // db.COLLECTION_NAME.find({'id': ID})
        return mongoTemplate.findById(id, Student.class, COLLECTION_NAME);
    }

    public List<Student> findAll() {
        // db.student.find()
        return mongoTemplate.findAll(Student.class, COLLECTION_NAME);
    }

    public List<Student> findByAgeTo(int min, int max) {
        // db.COLLECTION_NAME.find({'age': {$gte: MIN}, 'age': {$lte: MAX}})
        Query query = new Query(Criteria.where("age").gte(min).lte(max));
        return mongoTemplate.find(query, Student.class, COLLECTION_NAME);
    }

    public List<Student> findByNameIn(String... names) {
        // db.COLLECTION_NAME.find({'name':{$in: [NAMES]}})
        Query query = new Query(Criteria.where("name").in(names));
        return mongoTemplate.find(query, Student.class, COLLECTION_NAME);
    }

    public Student findFirstByNameSort() {
        // db.COLLECTION_NAME.find().sort({'name': 1})
        Query query = new Query().with(Sort.by(Sort.Order.asc("name")));
        return mongoTemplate.findOne(query, Student.class, COLLECTION_NAME);
    }
    
    public List<Map> groupAge() {
        // db.COLLECTION_NAME.aggregate({$group: {_id: '$age', NUM: {$sum: 1}}})
        Aggregation aggregation = Aggregation.newAggregation(Aggregation.group("age").count().as("NUM"));
        return mongoTemplate.aggregate(aggregation, COLLECTION_NAME, Map.class).getMappedResults();
    }
    
    public List<Map> groupByAgeAndMaxByPhoto() {
        // db.COLLECTION_NAME.aggregate({$group: {_id: '$age', MAX: {$max: '$photo'}}})
        Aggregation aggregation = Aggregation.newAggregation(Aggregation.group("age").max("photo").as("MAX"));
        return mongoTemplate.aggregate(aggregation, COLLECTION_NAME, Map.class).getMappedResults();
    }
}
```

# 查询分析

explain方法

hint方法

# 原子操作

```java
{$set: {KEY: VALUE}} // 更新键值
{$unset: {KEY: 1}} // 删除一个键
{$inc: {KEY: NUMBER}} // 数值类型增减操作
{$push: {KEY: VALUE}} // KEY数组追加值
{$pushAll: {KEY: VALUE}} // KEY数组追加多个值
{$pull: {KEY: VALUE}} // 数组KEY删除等于VALUE的值
{$pop: {KEY: 1/-1}} // 1-删除数组最后一个值，-1-删除数组第一个值
${rename: {OLD_KEY_NAME: NEW_KEY_NAME}} // 更改字段名
${bit: {KEY:{and: NUMBER}}} // 位操作
```

# 正则表达式

```java
db.COLLECTION_NAME.find(KEY: {$regex: REGEX})
```

# GridFS

chunks存储文件的二进制数据，files存储文件的元数据；这两个集合放在名为fs的buket中。

存储时会将文件分割多个`chunk`，每个`chunk`大小为256KB，将多个`chunk`存储在`fs.chunks`中，文件信息存储在`fs.files`唯一一份文档中。

读取时在`fs.files`中找到对应文档获取`_id`，再根据`_id`在`js.chunks`中查找相关文档，根据`n`字段顺序读取文档的`data`字段数据

```java
@Configuration
public class MongoConfig {

    @Value("${spring.data.mongodb.host}")
    private String host;

    @Value("${spring.data.mongodb.port}")
    private Integer port;

    @Value("${spring.data.mongodb.database}")
    private String database;

    @Bean
    public GridFSBucket getgridFSBucket(MongoClient mongoClient) {
        MongoDatabase mongoDatabase = mongoClient.getDatabase(database);
        GridFSBucket gridFSBucket = GridFSBuckets.create(mongoDatabase);
        return gridFSBucket;
    }
}
```

```java
@Service
@Slf4j
public class MongoServiceImpl implements MongoService {

    @Autowired
    MongoTemplate mongoTemplate;

    @Autowired
    GridFsTemplate gridFsTemplate;

    @Autowired
    GridFSBucket gridFSBucket;

    public int increaseId() {
        Query query = new Query(Criteria.where("name").is("user"));
        Update update = new Update();
        update.inc("num", 1);
        IdItem idItem = mongoTemplate.findAndModify(query, update, IdItem.class, "increase");
        return idItem.getNum();
    }

    @Override
    public String saveFile(InputStream inputStream, String filename, String contentType) {
        ObjectId id = gridFsTemplate.store(inputStream, filename, contentType);
        return id.toString();
    }

    public BufferedImage getImage(String id) {
        Query query = new Query(Criteria.where("_id").is(id));
        GridFSFile gridFSFile = gridFsTemplate.findOne(query);
        GridFSDownloadStream in = gridFSBucket.openDownloadStream(gridFSFile.getId());
        GridFsResource resource = new GridFsResource(gridFSFile, in);
        try {
            InputStream inputStream = resource.getInputStream();
            return ImageIO.read(inputStream);
        } catch (IOException e) {
            log.error(e.getMessage());
            return null;
        } finally {
            in.close();
        }
    }
}
```

**mapReduce**

**全文检索**


<h1 align="center">🥭 Mango Core | 芒果核心</h1>

## MongoDB

[MongoDB](https://www.mongodb.com/) 是一个基于分布式文件存储的数据库，MongoDB 面向集合存储，更易存储对象类型的数据。



## Mango Core

[Mango Core](https://github.com/Yallage/mango-core) 是基于 MongoDB 数据库面向 Bukkit / Bungee 的封装

提供配置文件统一管理数据库，支持增删查改等基本操作，支持大数据查询异步查询。



## 添加 Mango Core 依赖

使用 Maven / Gradle 只需添加私有仓库与依赖项即可。

**使用 Gradle 配置 gradle.build**

```groovy
repositories {
	maven { url "https://repository.yallage.com/snapshots" }
}
```

```groovy
dependencies {
	implementation "com.yallage:mango-core:1.0.0-dev"
}
```



## 插件配置文件填写

```json
{
  "databases": { 				// 固定 json 节点
    "database1": { 				// 数据库名称
      "host": "localhost",		// 数据库 IP
      "port": "27017",			// 数据库端口
      "username": "root",		// 用于鉴权的用户名
      "password": "root",		// 用于鉴权的密码
      "database": "database1" 	// 用户名密码所拥有权限的数据库
    },
    "database2": {
      "host": "localhost",
      "port": "27017",
      "username": "root",
      "password": "root",
      "database": "database2"
    }
  }
}
```

填写后保存于 plugins/YaMangoCore/config.json

如果不手动创建此配置文件，第一次启动插件时将会自动创建一份。



## 使用 Mango Core API

**创建 MangoClient 实例**

通常是 MangoSyncClient 阻塞型客户端。

```java
Gson gson = new Gson();
MangoClient client = new MangoSyncClient(gson);
```

Gson 的作用是解析对象到 json 字符串。

再由 MongoDB 驱动提供的 Bson 构造器转换为 Mongo 文档。



**什么是数据实体？**

假设需要描述一个数据库，通常需要数据库 IP、端口、数据库名、用户名和密码。

用一个类进行描述：

```java
public class Database {
    String database;	// 数据库名
    String host;		// 数据库 IP
    int port;			// 端口
    String username;	// 用户名
    String password;	// 密码
}
```

是的，这与 json 是**一一对应**的，正是这一个类描述了配置文件中的一个数据库，像这样仅用于描述数据的类，就被称作数据实体。

通常数据实体还会有 getter、setter 和全字段构造器

`String database` 的 getter 与 setter 如下：

```java
public String getDatabase() {
	return database;
}

public void setDatabase(String database) {
	this.database = database;
}
```

全字段构造器如下：

```java
public Database(String database, String host, int port, String username, String password) {
    this.database = database;
    this.host = host;
    this.port = port;
    this.username = username;
    this.password = password;
}
```

如果使用 [Jetbrains IntelliJ IDEA](https://www.jetbrains.com/idea/) 可以使用快捷键 Alt + Insert 唤出自动生成面板进行 getter、setter 和全字段构造器的自动生成。

当然，如果使用 [lombok](https://projectlombok.org/) 只需要使用 @Data 和 @AllArgsConstructor 注解即可。



**使用 MangoClient 创建、删除、查询和更改**

假设有一个数据实体类 PlayerData ，它用来描述玩家某一个游戏的分数，它有 name 和 score 两个字段

如下：

```java
public class PlayerData {
    String name;
    int score;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getScore() {
        return score;
    }

    public void setScore(int score) {
        this.score = score;
    }

    public PlayerData(String name, int score) {
        this.name = name;
        this.score = score;
    }
}
```



现在使用上一步创建的 MangoClient 实例：

**创建一条 PlayerData 数据库记录**

```java
PlayerData data = new PlayerData("mango", 114514)
client.create("数据库名", "数据集合名 相当于 MySQL 的表名",data)
```



**根据玩家名字读取这一条数据**

```java
// 将所有字段 name 为 mango 的结果读取出来
Map<String, Object> index = new HashMap<>(){ put("name", "mango"); };
List<PlayerData> datas = client.read("数据库名", "数据集合名 相当于 MySQL 的表名", index, PlayerData.class);

// 仅读取在数据库中获取到的第一条数据
Map<String, Object> index = new HashMap<>(){ put("name", "mango"); };
PlayerData data = client.readOne("数据库名", "数据集合名 相当于 MySQL 的表名", index, PlayerData.class);
```

事实上 Map 里面的数据是对应到数据实体中的字段的，只不过是使用字符串进行表达



**删除数据**

```java
// 将所有字段 name 为 mango 的数据删除
Map<String, Object> index = new HashMap<>(){ put("name", "mango"); };
client.delete("数据库名", "数据集合名 相当于 MySQL 的表名", index);

// 仅删除在数据库中获取到的第一条数据
Map<String, Object> index = new HashMap<>(){ put("name", "mango"); };
client.deleteOne("数据库名", "数据集合名 相当于 MySQL 的表名", index);
```



**更新数据**

```java
Map<String, Object> index = new HashMap<>(){ put("name", "mango"); };
client.update("数据库名", "数据集合名 相当于 MySQL 的表名", index, new PlayerData("mango", 1919810));
```



以上就是使用 Mango Core 进行增加、删除、查询和更改操作的步骤，另有扩展方法可以参考 [MangoClient](https://github.com/Yallage/mango-core/blob/main/src/main/java/com/yallage/mango/core/interfaces/MangoClient.java)



## 异步读取数据库

当大量读取数据时，为避免卡服，应该创建异步任务。

**首先需要一个创建异步客户端**

根据 Bukkit 或是 Bungee 选择对应客户端

```java
Gson gson = new Gson();
MangoClient client = new MangoBukkitAsyncClient(gson);
```

或是

```java
Gson gson = new Gson();
MangoClient client = new MangoBungeeAsyncClient(gson);
```



**执行结果监听器**

需要监听 `MangoBukkitAsyncEvent` 或是 `MangoBungeeAsyncEvent` 事件。



**异步读取**

```java
Map<String, Object> index = new HashMap<>(){ put("name", "mango"); };
String id = client.read("数据库名", "数据集合名 相当于 MySQL 的表名", index);
```

与上一步的阻塞型读取不一样的是，这次的读取只返回了一个取件码，通过取件码即可从监听器中取出本次异步读取的结果了，需要通过取件码自行判断是否为对应的异步结果。

对应的方法是 `String getAsyncId()` 与 `<T> List<T> getData(Class<T> type)` ，getData() 要求一个类型数据，然后才能返回这一个类型数据的数据列表。

如下：

```java
List<PlayerData> datas = event.getData(PlayerData.class);
```


<h1 align="center">ð¥­ Mango Core | èææ ¸å¿</h1>

## MongoDB

[MongoDB](https://www.mongodb.com/) æ¯ä¸ä¸ªåºäºåå¸å¼æä»¶å­å¨çæ°æ®åºï¼MongoDB é¢åéåå­å¨ï¼æ´æå­å¨å¯¹è±¡ç±»åçæ°æ®ã



## Mango Core

[Mango Core](https://github.com/Yallage/mango-core) æ¯åºäº MongoDB æ°æ®åºé¢å Bukkit / Bungee çå°è£

æä¾éç½®æä»¶ç»ä¸ç®¡çæ°æ®åºï¼æ¯æå¢å æ¥æ¹ç­åºæ¬æä½ï¼æ¯æå¤§æ°æ®æ¥è¯¢å¼æ­¥æ¥è¯¢ã



## æ·»å  Mango Core ä¾èµ

ä½¿ç¨ Maven / Gradle åªéæ·»å ç§æä»åºä¸ä¾èµé¡¹å³å¯ã

**ä½¿ç¨ Gradle éç½® gradle.build**

```groovy
repositories {
	maven { url "https://repository.yallage.com/snapshots" }
}
```

```groovy
dependencies {
	implementation "com.yallage.mango:mango-core:1.0.0-dev"
}
```



## æä»¶éç½®æä»¶å¡«å

```json
{
  "databases": { 				// åºå® json èç¹
    "database1": { 				// æ°æ®åºåç§°
      "host": "localhost",		// æ°æ®åº IP
      "port": "27017",			// æ°æ®åºç«¯å£
      "username": "root",		// ç¨äºé´æçç¨æ·å
      "password": "root",		// ç¨äºé´æçå¯ç 
      "database": "database1" 	// ç¨æ·åå¯ç ææ¥ææéçæ°æ®åº
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

å¡«ååä¿å­äº plugins/YaMangoCore/config.json

å¦æä¸æå¨åå»ºæ­¤éç½®æä»¶ï¼ç¬¬ä¸æ¬¡å¯å¨æä»¶æ¶å°ä¼èªå¨åå»ºä¸ä»½ã



## ä½¿ç¨ Mango Core API

**åå»º MangoClient å®ä¾**

éå¸¸æ¯ MangoSyncClient é»å¡åå®¢æ·ç«¯ã

```java
Gson gson = new Gson();
MangoClient client = new MangoSyncClient(gson);
```

Gson çä½ç¨æ¯è§£æå¯¹è±¡å° json å­ç¬¦ä¸²ã

åç± MongoDB é©±å¨æä¾ç Bson æé å¨è½¬æ¢ä¸º Mongo ææ¡£ã



**ä»ä¹æ¯æ°æ®å®ä½ï¼**

åè®¾éè¦æè¿°ä¸ä¸ªæ°æ®åºï¼éå¸¸éè¦æ°æ®åº IPãç«¯å£ãæ°æ®åºåãç¨æ·ååå¯ç ã

ç¨ä¸ä¸ªç±»è¿è¡æè¿°ï¼

```java
public class Database {
    String database;	// æ°æ®åºå
    String host;		// æ°æ®åº IP
    int port;			// ç«¯å£
    String username;	// ç¨æ·å
    String password;	// å¯ç 
}
```

æ¯çï¼è¿ä¸ json æ¯**ä¸ä¸å¯¹åº**çï¼æ­£æ¯è¿ä¸ä¸ªç±»æè¿°äºéç½®æä»¶ä¸­çä¸ä¸ªæ°æ®åºï¼åè¿æ ·ä»ç¨äºæè¿°æ°æ®çç±»ï¼å°±è¢«ç§°ä½æ°æ®å®ä½ã

éå¸¸æ°æ®å®ä½è¿ä¼æ getterãsetter åå¨å­æ®µæé å¨

`String database` ç getter ä¸ setter å¦ä¸ï¼

```java
public String getDatabase() {
	return database;
}

public void setDatabase(String database) {
	this.database = database;
}
```

å¨å­æ®µæé å¨å¦ä¸ï¼

```java
public Database(String database, String host, int port, String username, String password) {
    this.database = database;
    this.host = host;
    this.port = port;
    this.username = username;
    this.password = password;
}
```

å¦æä½¿ç¨ [Jetbrains IntelliJ IDEA](https://www.jetbrains.com/idea/) å¯ä»¥ä½¿ç¨å¿«æ·é® Alt + Insert å¤åºèªå¨çæé¢æ¿è¿è¡ getterãsetter åå¨å­æ®µæé å¨çèªå¨çæã

å½ç¶ï¼å¦æä½¿ç¨ [lombok](https://projectlombok.org/) åªéè¦ä½¿ç¨ @Data å @AllArgsConstructor æ³¨è§£å³å¯ã



**ä½¿ç¨ MangoClient åå»ºãå é¤ãæ¥è¯¢åæ´æ¹**

åè®¾æä¸ä¸ªæ°æ®å®ä½ç±» PlayerData ï¼å®ç¨æ¥æè¿°ç©å®¶æä¸ä¸ªæ¸¸æçåæ°ï¼å®æ name å score ä¸¤ä¸ªå­æ®µ

å¦ä¸ï¼

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



ç°å¨ä½¿ç¨ä¸ä¸æ­¥åå»ºç MangoClient å®ä¾ï¼

**åå»ºä¸æ¡ PlayerData æ°æ®åºè®°å½**

```java
PlayerData data = new PlayerData("mango", 114514)
client.create("æ°æ®åºå", "æ°æ®éåå ç¸å½äº MySQL çè¡¨å",data)
```



**æ ¹æ®ç©å®¶åå­è¯»åè¿ä¸æ¡æ°æ®**

```java
// å°ææå­æ®µ name ä¸º mango çç»æè¯»ååºæ¥
Map<String, Object> index = new HashMap<>(){ put("name", "mango"); };
List<PlayerData> datas = client.read("æ°æ®åºå", "æ°æ®éåå ç¸å½äº MySQL çè¡¨å", index, PlayerData.class);

// ä»è¯»åå¨æ°æ®åºä¸­è·åå°çç¬¬ä¸æ¡æ°æ®
Map<String, Object> index = new HashMap<>(){ put("name", "mango"); };
PlayerData data = client.readOne("æ°æ®åºå", "æ°æ®éåå ç¸å½äº MySQL çè¡¨å", index, PlayerData.class);
```

äºå®ä¸ Map éé¢çæ°æ®æ¯å¯¹åºå°æ°æ®å®ä½ä¸­çå­æ®µçï¼åªä¸è¿æ¯ä½¿ç¨å­ç¬¦ä¸²è¿è¡è¡¨è¾¾



**å é¤æ°æ®**

```java
// å°ææå­æ®µ name ä¸º mango çæ°æ®å é¤
Map<String, Object> index = new HashMap<>(){ put("name", "mango"); };
client.delete("æ°æ®åºå", "æ°æ®éåå ç¸å½äº MySQL çè¡¨å", index);

// ä»å é¤å¨æ°æ®åºä¸­è·åå°çç¬¬ä¸æ¡æ°æ®
Map<String, Object> index = new HashMap<>(){ put("name", "mango"); };
client.deleteOne("æ°æ®åºå", "æ°æ®éåå ç¸å½äº MySQL çè¡¨å", index);
```



**æ´æ°æ°æ®**

```java
Map<String, Object> index = new HashMap<>(){ put("name", "mango"); };
client.update("æ°æ®åºå", "æ°æ®éåå ç¸å½äº MySQL çè¡¨å", index, new PlayerData("mango", 1919810));
```



ä»¥ä¸å°±æ¯ä½¿ç¨ Mango Core è¿è¡å¢å ãå é¤ãæ¥è¯¢åæ´æ¹æä½çæ­¥éª¤ï¼å¦ææ©å±æ¹æ³å¯ä»¥åè [MangoClient](https://github.com/Yallage/mango-core/blob/main/src/main/java/com/yallage/mango/core/interfaces/MangoClient.java)



## å¼æ­¥è¯»åæ°æ®åº

å½å¤§éè¯»åæ°æ®æ¶ï¼ä¸ºé¿åå¡æï¼åºè¯¥åå»ºå¼æ­¥ä»»å¡ã

**é¦åéè¦ä¸ä¸ªåå»ºå¼æ­¥å®¢æ·ç«¯**

æ ¹æ® Bukkit ææ¯ Bungee éæ©å¯¹åºå®¢æ·ç«¯

```java
Gson gson = new Gson();
MangoClient client = new MangoBukkitAsyncClient(gson);
```

ææ¯

```java
Gson gson = new Gson();
MangoClient client = new MangoBungeeAsyncClient(gson);
```



**æ§è¡ç»æçå¬å¨**

éè¦çå¬ `MangoBukkitAsyncEvent` ææ¯ `MangoBungeeAsyncEvent` äºä»¶ã



**å¼æ­¥è¯»å**

```java
Map<String, Object> index = new HashMap<>(){ put("name", "mango"); };
String id = client.read("æ°æ®åºå", "æ°æ®éåå ç¸å½äº MySQL çè¡¨å", index);
```

ä¸ä¸ä¸æ­¥çé»å¡åè¯»åä¸ä¸æ ·çæ¯ï¼è¿æ¬¡çè¯»ååªè¿åäºä¸ä¸ªåä»¶ç ï¼éè¿åä»¶ç å³å¯ä»çå¬å¨ä¸­ååºæ¬æ¬¡å¼æ­¥è¯»åçç»æäºï¼éè¦éè¿åä»¶ç èªè¡å¤æ­æ¯å¦ä¸ºå¯¹åºçå¼æ­¥ç»æã

å¯¹åºçæ¹æ³æ¯ `String getAsyncId()` ä¸ `<T> List<T> getData(Class<T> type)` ï¼getData() è¦æ±ä¸ä¸ªç±»åæ°æ®ï¼ç¶åæè½è¿åè¿ä¸ä¸ªç±»åæ°æ®çæ°æ®åè¡¨ã

å¦ä¸ï¼

```java
List<PlayerData> datas = event.getData(PlayerData.class);
```


<h1 align="center">🍋 Lemon Core | 柠檬核心</h1>

## Lemon Core

通过 Lemon Core 可以使用 WebSocket 信道进行双向交流，不受 Bungee 等代理需要通过媒介插件 / 实体进行通信，更安全、稳定和自由。



## WebSocket

WebSocket 是一种在单个TCP连接上进行全双工通信的协议，它的最大特点就是，服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话，正是如此，允许开发者使用 Lemon Core 从客户端主动推送消息。



## 添加 Lemon Core 依赖

使用 Maven / Gradle 只需添加私有仓库与依赖项即可。

**使用 Gradle 配置 gradle.build**

```groovy
repositories {
	maven { url "https://repository.yallage.com/snapshots" }
}
```

```groovy
dependencies {
	implementation "com.yallage:lemon-core:1.0.0-dev"
}
```



## 插件配置文件填写

```json
{
  "server": true,			// Lemon Core 由服务端和客户端两部分组成 这里决定是否开启服务端
  "server_name": "lemon",	// 服务端名称
  "server_port": 10086,		// 服务端运行的端口
  "server_secret": "",		// 用于安全校验的密钥 随机一串字符即可
  "server_ssl": false,		// 是否开启 ssl 加密
  "server_ssl_cert": "",	// ssl jks 格式证书
  "server_ssl_password": "",// ssl jks 证书密码
  "whitelist": false,		// 是否开启客户端 ip 白名单 开启后仅白名单的客户端可连接此服务端
  "allow": [				// ip 白名单列表
    "127.0.0.1"
  ],
  "center": [				// 客户端配置列表 一个客户端可以同时连接多个服务端
    {
      "host": "127.0.0.1",	// 需要连接服务端 ip
      "port": 10086,		// 服务端端口
      "secret": "",			// 服务端设置的安全校验密钥
      "ssl": false,			// 是否开启 ssl 加密 由服务端决定
      "ssl_cert": "",		// ssl jks 格式证书
      "ssl_password": ""	// ssl jks 证书密码
    }
  ]
}
```

填写后保存于 plugins/YaLemonCore/config.json

如果不手动创建此配置文件，第一次启动插件时将会自动创建一份。



## 获取 Lemon Client 客户端

```java
LemonClient client = new LemonClient("随机数字符串 作为本插件的唯一识别码");
```

这里可以使用任意字符串，如果不愿意其他插件识别本插件，可以使用 UUID 可以保证与其他的插件不冲突且每一次启动分配一个不一致的 UUID。当然，这个字符串本插件需要保存，用于识别来自其他其他服务端本插件的消息。再一次说明，唯一识别码是完全自定义的，可以是插件名或是 UUID 或是其他字符串， UUID 可以在 [这里](https://uutool.cn/uuid/) 在线生成。



## 发送消息

使用上一步获取的客户端即可发送消息

```java
client.send("消息内容");
```



## 接收消息

首先需要创建一个 Bukkit / Bungee 的事件监听器

然后监听 `LemonBukkitMessageEvent` 或是 `LemonBungeeMessageEvent`

在事件中可以获取到 `clientId` `pluginId` `message`  

需要注意的是：本插件发出的消息也会被本插件的事件监听器获取到，除非想要让本服务端处理该消息，否则则需要通过 `clientId` 辨别。

Lemon Client 实例中获取 `clientId` 方法如下：

```java
client.getClientId();
```


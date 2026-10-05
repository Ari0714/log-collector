# Log Collector

这是一个 Java 项目，用于模拟移动应用的行为日志数据，生成常见的 App 事件日志并输出为 JSON。项目主要面向大数据与日志分析场景，比如对日志采集、ETL、数仓建模、实时分析等进行演示和测试。

## 项目作用

该项目会随机生成以下类型的日志：

- App 启动日志
- 商品曝光/点击日志
- 商品详情页访问日志
- 商品列表加载日志
- 广告展示/点击日志
- 通知日志
- 前台/后台活跃日志
- 异常错误日志
- 评论、收藏、点赞日志

这些日志中包含公共字段（设备信息、用户信息、网络环境、版本信息等）以及事件字段，适合用于：

- 数据采集测试
- Spark / Flink / Hive 等大数据任务调试
- 日志结构分析
- 数据平台演示

## 项目结构

```text
logcollector
├── pom.xml
├── src
│   └── main
│       ├── java
│       │   ├── appclient
│       │   │   └── AppMain.java
│       │   └── bean
│       │       ├── AppBase.java
│       │       ├── AppStart.java
│       │       ├── AppDisplay.java
│       │       ├── AppNewsDetail.java
│       │       ├── AppLoading.java
│       │       ├── AppAd.java
│       │       ├── AppNotification.java
│       │       ├── AppActive_foreground.java
│       │       ├── AppActive_background.java
│       │       ├── AppErrorLog.java
│       │       ├── AppComment.java
│       │       ├── AppFavorites.java
│       │       └── AppPraise.java
│       └── resources
│           └── logback.xml
└── target
```

## 技术栈

- Java 8
- Maven
- Fastjson
- Logback

## 构建项目

在项目根目录执行：

```bash
mvn clean package
```

构建成功后，会生成带依赖的可执行 jar，通常位于：

```text
target/logcollector-1.0-SNAPSHOT-jar-with-dependencies.jar
```

## 运行方式

### 1：默认参数

```bash
java -jar target/logcollector-1.0-SNAPSHOT-jar-with-dependencies.jar
```

默认会生成 1000 条日志，发送间隔为 0 毫秒。

### 2：自定义参数

`AppMain` 接受两个参数：

- 参数一：每条日志发送前的延迟时间（毫秒）
- 参数二：循环生成的总次数

例如：

```bash
java -jar target/logcollector-1.0-SNAPSHOT-jar-with-dependencies.jar 100 5000
```

表示：每条日志之间等待 100 毫秒，总共生成 5000 条日志。

## 日志输出

项目使用 Logback 输出日志，日志会同时输出：

- 控制台
- `/tmp/logs/app-YYYY-MM-DD.log`

日志格式大致如下：

```json
{"mid":"0","uid":"0","vc":"5","vn":"1.2.7","l":"en","sr":"X","os":"8.2.9",...}
```

或者：

```text
1700000000000|{"ap":"app","cm":{"mid":"0",...},"et":[{"en":"display",...}]}
```

## 说明

这个项目本质上是一个日志数据模拟器，而不是完整的业务系统。它的目标是快速生成大量样例日志，方便你在大数据、数据处理、日志采集和分析相关组件中进行验证和演示。

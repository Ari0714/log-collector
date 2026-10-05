# Log Collector

This is a Java project that simulates mobile application behavior logs, generates common App event logs, and outputs them as JSON. It is mainly designed for big data and log analysis scenarios such as log collection, ETL, data warehouse modeling, and real-time analytics demos and testing.

## Project purpose

The project randomly generates the following log types:

- App launch logs
- Product exposure/click logs
- Product detail page access logs
- Product list loading logs
- Ad display/click logs
- Notification logs
- Foreground/background activity logs
- Error logs
- Comment, favorite, and like logs

These logs contain common fields (device information, user information, network environment, version details, etc.) and event-specific fields, making them suitable for:

- Data collection testing
- Big data job debugging with Spark / Flink / Hive
- Log structure analysis
- Data platform demos

## Project structure

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

## Tech stack

- Java 8
- Maven
- Fastjson
- Logback

## Build the project

Run the following command in the project root:

```bash
mvn clean package
```

After a successful build, the executable JAR with dependencies is generated and typically located at:

```text
target/logcollector-1.0-SNAPSHOT-jar-with-dependencies.jar
```

## Run the project

### 1: Default parameters

```bash
java -jar target/logcollector-1.0-SNAPSHOT-jar-with-dependencies.jar
```

By default, it generates 1000 logs with a send interval of 0 milliseconds.

### 2: Custom parameters

`AppMain` accepts two parameters:

- Parameter 1: Delay before sending each log entry (milliseconds)
- Parameter 2: Total number of loops to generate

For example:

```bash
java -jar target/logcollector-1.0-SNAPSHOT-jar-with-dependencies.jar 100 5000
```

This means: wait 100 milliseconds between logs and generate 5000 logs in total.

## Data flow

```mermaid
flowchart LR
    A[App event generator] --> B[Base/common fields enrichment]
    B --> C[Specific event modeling]
    C --> D[JSON log generation]
    D --> E[Logback output]
    E --> F[Console]
    E --> G[/tmp/logs/app-YYYY-MM-DD.log]

    C --> H[Examples: AppStart / AppDisplay / AppAd / AppComment]
    H --> D
```

The flow is: random app behavior simulation -> enrich with common metadata -> build concrete event payloads -> serialize to JSON -> emit to log sinks.

## Log output

The project uses Logback for logging, and logs are written to both:

- Console
- `/tmp/logs/app-YYYY-MM-DD.log`

A sample log format is roughly as follows:

```json
{"mid":"0","uid":"0","vc":"5","vn":"1.2.7","l":"en","sr":"X","os":"8.2.9",...}
```

Or:

```text
1700000000000|{"ap":"app","cm":{"mid":"0",...},"et":[{"en":"display",...}]}
```

## Notes

This project is essentially a log data simulator rather than a complete business system. Its goal is to quickly generate large volumes of sample logs so that you can validate and demonstrate components related to big data, data processing, log collection, and analysis.

# [DATA] 訊息佇列 03 - RabbitMQ 架設方式與操作管理介面

<!--more-->

## 前言
這陣子團隊在開發**相似商品搜尋**的 ML Product，為了要把系統架構解耦，改成**異步分散式處理**，因而接觸到訊息佇列(Message Queue)，作為兩個子系統（商品資料處理 & 特徵向量轉換）的通信中間層。我將透過三篇文章分享我在架設 RabbitMQ 與使用 Python 實作的學習。

<br>

**[ 系列文章目錄 ]**
1. [[DATA] 訊息佇列 01 - Message Queue 介紹與實際應用](/message-queue/)
2. [[DATA] 訊息佇列 02 - RabbitMQ 簡介與 5 種設計模式](/rabbitmq-intro/)
3. [[DATA] 訊息佇列 03 - RabbitMQ 架設方式與操作管理介面](/rabbitmq-management-interface)（本篇）
4. *[DATA] 訊息佇列 04 - RabbitMQ x Python 程式實作範例（待完成）*

## 下載＆安裝 RabbitMQ
介紹完五種模式，你是不是已經很想動手實作看看？ 接下來就要告訴大家如何自己搭建 RabbitMQ，官方有提供 RabbitMQ Server 在不同作業系統(Linux, MacOS & Windows)上的[安裝指南](https://www.rabbitmq.com/download.html)，不過我還是推薦使用 Docker 建立環境最簡單。


### 使用 Docker 指令
    
```bash
# for RabbitMQ 3.9, the latest series
docker run --rm --name rabbitmq -p 5672:5672 -p 15672:15672 \
-e RABBITMQ_DEFAULT_USER=root -e RABBITMQ_DEFAULT_PASS=1234 rabbitmq:management 
```
<br>

| 指令 / 參數 | 定義說明 |
| ----------- | ----------- |
| `docker run` | 執行容器 |
| `--rm` | 當容器終止時會自動刪除 |
| `--name rabbitmq` |  將容器命名為 rabbitmq |
| `-p 5672:5672` | 將本機端的 5672 port 關聯到容器裡的 5672 port（RabbitMQ） |
| `-p 15672:15672` | 將本機端的 15672 port 關聯到容器裡的 15672 port（Web UI） |
| <div style="width: 200pt">`-e RABBITMQ_DEFAULT_USER=root`</div> | 宣告環境變數，連線到 RabbitMQ 的 username |
| `-e RABBITMQ_DEFAULT_PASS=1234` |  宣告環境變數，連線到 RabbitMQ 的 password |
| `rabbitmq:management` | 指定容器抓 docker hub 上的 rabbitmq [official image](https://registry.hub,docker.com/_/rabbitmq/) |
 
{{< admonition type=info title="docker image version" open=true >}}
預設為最新版本(3.9)，若想要指定版本可以改換成 `rabbitmq:3.8-management` (3.8)
{{</admonition>}}


### 使用 Docker Compose 指令
    
```yml
# Create and start container
docker-compose -f docker-compose up

# docker-compose.yml
services:
    rabbitmq:
    image: rabbitmq:management
    container_name: rabbitmq
    ports:
        - 5672:5672
        - 15672:15672
    environment:
        - RABBITMQ_DEFAULT_USER=root
        - RABBITMQ_DEFAULT_PASS=1234
```

- docker-compose.yml 檔內的參數定義與 Docker 指令相同

將 RabbitMQ 啟動後，在瀏覽器 [http://localhost:15672](http://localhost:15672) 進入 RabbitMQ 的網頁版管理介面，如果能成功進入這個畫面，代表服務 RabbitMQ 已經可以使用了。
- 截圖

## Web 管理介面
輸入剛剛設定的 `RABBITMQ_DEFAULT_USER` 和 `RABBITMQ_DEFAULT_PASS` 登入，就會進入


## 參考
[https://godleon.github.io/blog/ChatOps/message-queue-concepts/](https://godleon.github.io/blog/ChatOps/message-queue-concepts/)
https://kucw.github.io/blog/2020/11/rabbitmq/
https://homuchen.com/posts/message-queue-advantages-use-cases/
https://www.cloudamqp.com/blog/part1-rabbitmq-for-beginners-what-is-rabbitmq.html


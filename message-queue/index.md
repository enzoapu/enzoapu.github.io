# [DATA] 訊息佇列 01 - Message Queue(MQ) 介紹

<!--more-->

<!-- # 訊息佇列 Message Queue(MQ) 介紹 -->

## 前言

這陣子團隊在開發**相似商品搜尋**的 ML Product，為了要把系統架構解耦，改成**異步分散式處理**，因而接觸到訊息佇列(Message Queue)，作為兩個子系統（商品資料爬蟲 & 圖片/文字向量轉換）的通信中間層。我將透過三篇文章分享我在架設 RabbitMQ 與使用 Python 實作的學習。

1. [[DATA] 訊息佇列 01 - Message Queue(MQ) 介紹](/message-queue/)*（本篇）*
2. [[DATA] 訊息佇列 02 - RabbitMQ 設計模式與管理介面](/rabbitmq-intro/)
3. [[DATA] 訊息佇列 03 - RabbitMQ x Python 實作範例](/rabbitmq-python/)


## 什麼是 Message Queue(MQ)？


![Message Queue - single](01_mq_single.png "Message Queue - single")

Message Queue(MQ)是一種訊息傳遞仲介，架構中擁有三個角色，分別為 Producer、Broker 及 Consumer，提供不同程序(process)或不同系統(system)的非同步(asynchronous)溝通。

{{< admonition type=tip title="什麼是非同步呢？" open=true >}}
一般常見的 HTTP API 就屬於同步式方法，發送方(sender)發出請求(request)等待接收方(receiver)處理完回應(response)，等待過程連結不能斷開去做其他事。
{{< /admonition >}}

由於先進先出(FIFO)的特性，發送方(producer)只要把訊息/任務往 MQ(broker) 裡面丟，接收方(consumer)就能夠依序從 MQ(broker)中取出訊息/任務，使雙方能夠獨立運作。

## 使用 Message Queue 有什麼好處？
### 任務緩衝

短時間內收到大量請求可能會導致系統過載，特別是 CPU / GPU 運算吃重(heavy computing)的情況，這時候 MQ 就發揮了緩衝的功能，Producer 不需等待地發送任務；Consumer 依自己的資源或算力取出適量的任務，處理完再繼續拿。

### 暫存容錯
若 Consumer 意外關閉，未處理完的訊息/任務還會存在 MQ 內，並不會丟失，只要再把 Consumer 重啟，又可以接續處理。
### 系統解耦
架構設計上拆分為不同元件(components)獨立開發，Producer、Broker 及 Consumer 不需部署在同一台機主機，不需知道彼此的 IP address，不用使用相同的程式語言。
### 水平擴展

![Message Queue - multi](02_mq_multi.jpg "Message Queue - multi")

Producer 可分散在不同來源、裝置收集（e.g. IoT applications）；Consumer 可以按照需求和資源，起在多台機器上，加速訊息/任務的處理。

## 常見工具
[icon images]
Message Queue 常見的開源工具有 RabbitMQ、Redis、Kafka（不同特性可參考這篇：）。
雲端服務則是像是 GCP 的 Cloud Pub/Sub 和 AWS 的 Amazon SQS。

## 應用案例


## 參考資料


## 觀看更多
1. [[DATA] 訊息佇列 01 - Message Queue(MQ) 介紹](/message-queue/)
2. [[DATA] 訊息佇列 02 - RabbitMQ 設計模式與管理介面](/rabbitmq-intro/)*（下一篇）*
3. [[DATA] 訊息佇列 03 - RabbitMQ x Python 實作範例](/rabbitmq-python/)

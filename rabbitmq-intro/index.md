# [DATA] 訊息佇列 02 - RabbitMQ 設計模式與管理介面

<!--more-->

<!-- # RabbitMQ 介紹 -->

## 前言
RabbitMQ 是使用廣泛的輕量級開源工具，支持多種訊息傳遞協定(e.g. AMQP 0-9-1)
有以下優勢：
- RabbitMQ 容易在本地端和雲端部署，滿足大規模(分散式)、高可用性的需求。
- RabbitMQ 為大多數流行的程式語言提供了多樣的[開發套件包](https://www.rabbitmq.com/devtools.html)。(Python, Java, Ruby, PHP, C#, JS, Go, etc.)
- RabbitMQ 提供了一個 Web 使用者介面來管理權限並監控各種狀態、指標。

## RabbitMQ 架構
圖片
    
看完上一篇我們已經知道 Message Queue 架構中的三個角色，在 RabbitMQ 中也延用相同概念：
- Producer 是發送訊息的應用程式
- Queue 是儲存訊息的緩衝區
- Consumer 是接收訊息的應用程式

不過在 RabbitMQ 中，根據不同模式會在 Producer 與 Queue 之間加上一層 Exchange。

- Exchange 的工作非很簡單，透過綁定(binding)與 Queue 連結，負責接收來自 Producer 的訊息，然後將訊息推送給 Queue。
- 透過定義 Exchange 的類型(type)來判斷要如何處理收到的訊息，是要推送給哪個特定 Queue？還是要推送給多個 Queue？或是應該被丟棄？
- Exchange 的類型(type)有分為 direct, topic, fanout & headers，文章下個段落介紹不同模式時會探討差別。

<!-- <br></br> -->
## 常見的設計模式
根據 [RabbitMQ Tutorials](https://www.rabbitmq.com/getstarted.html) 的範例，展示了六種常見的模式，以下簡單做介紹（Python 範例程式碼附在標題後方連結）

### 1. Simple 模式
The simplest thing that does *something* [(sample code)](https://www.rabbitmq.com/tutorials/tutorial-one-python.html)

<!--![Simple](01_python-one-overall.png "Simple")-->
{{< image alt="Simple" src="01_python-one-overall.png" caption="Simple" height="" width="360px">}}
    
 最基本的一種模式，只有一個 Queue(定義 Queue 的名稱)，Producer 直接將訊息傳進這個 Queue(hello)，也只有一個 Consumer 從這個 Queue(hello) 取出訊息。

### 2. Worker 模式
    
Distributing tasks among workers [(sample code)](https://www.rabbitmq.com/tutorials/tutorial-two-python.html)
    
<!--![Worker](02_python-two.png "Worker")-->
{{< image alt="Worker" src="02_python-two.png" caption="Worker" height="" width="360px">}}

- 相比 Simple，Worker 模式會有兩個以上的 Consumer(Worker)，從同一個 Queue 取出訊息，加速訊息處理(消化)速度。因此只要連接同一個 Queue，就可以在多台機器上 Consumer 平行處理。
- Consumer 彼此間不會取得相同的訊息，可以透過 `prefetch_count` 參數，控制每個 Consumer(Worker) 一次取出的訊息量，例如：P 將 1 ~ 10 依序傳進 Queue，`prefetch_count = 1` ，C1 依序取出 1、3、5、7、9（共五次），C2 依序取出 2、4、6、8、10（共五次）；`prefetch_count = 2` ，C1 依序取出 1, 2、5, 6、9, 10（共三次），C2 依序取出 3, 4、7, 8（共兩次）。

### 3. Publish/Subscribe 模式 
    
Sending messages to many consumers at once [(sample code)](https://www.rabbitmq.com/tutorials/tutorial-three-python.html)
    
<!--![Publish/Subscribe](03_python-three.png "Publish/Subscribe")-->
{{< image alt="Publish/Subscribe" src="03_python-three.png" caption="Publish/Subscribe" height="" width="400px">}}
    
- Publish/Subscribe 模式顧名思義屬，就像你訂閱某個 YT 頻道，當頻道創作者發佈了新影片，連同你的所有訂閱者都會收到通知。
- Producer 不會將訊息直接傳進 Queue，而是交給 *Exchange(type=fanout)*，由於 *fanout 的特性，Exchange* 會把訊息廣播給所有綁定的 Queue，每個 Consumer 就會接收到相同的訊息。因此當有另外的系統需要同步接收訊息，只需增加一組 Queue + Producer，綁定這個 *Exchange* 即可。

### 4. Routing 模式 
    
Receiving messages selectively [(sample code)](https://www.rabbitmq.com/tutorials/tutorial-four-python.html)
    
<!--![Routing](04_direct-exchange.png "Routing")-->
{{< image alt="Routing" src="04_direct-exchange.png" caption="Routing" height="" width="450px">}}
    
- Routing 模式同樣有一層 *Exchange(type=direct)*，但不同的是 *direct* 的特性，Exchange 與 Queue 的綁定(binding)還會帶上 routing key，Producer 傳送訊息到 Exchange 時也會帶上 routing key 這個參數。因此可以達到選擇性訊息分流，不同 Consumer 只需要接受到特定 routing 的訊息。
        
<!--![Logging system](05_python-four.png "Logging system")-->
{{< image alt="Logging system" src="05_python-four.png" caption="Logging system" height="" width="450px">}}
        
- 上圖為一個日誌系統(logging system)的範例，使用兩組 Queue，Q1 只有綁定一個 routing key(error)，因此負責寫檔(log file)的 C1 只會接收 error log messages，可節省硬碟(disk)空間。Q2 則是綁定多個 routing key(info, warning & error)，負責打印到控制台(console)的 C2 仍然可輸出所有層級(level)的 log messages。

### 5. Topics 模式 
    
Receiving messages based on a pattern [(sample code)](https://www.rabbitmq.com/tutorials/tutorial-five-python.html)
    
<!--![Topics](06_python-five.png "Topics")-->
{{< image alt="Topics" src="06_python-five.png" caption="Topics" height="" width="450px">}}
    
- Topics 與 Routing 模式很像，同樣有一層 Exchange(type=topic)，也透過 routing key 來分流訊息，差別在 topic **的特性能夠模糊綁定非固定的 routing key。
    - 設定模糊 routing key 的 patten 必須是以 `.`(dot) 分隔的字串，`*`(star) 只能代替一個單詞、`#`(bash) 可以代替零個或多個單詞。
- 我們用上圖的的範例讓你搞懂這到底是啥鬼XD
    - 假設我們要發送「描述動物的訊息」，訊息將使用由三個單詞（包含兩個 `.`）組成的 routing key 發送，第一個單詞是“速度”，第二個單詞是顏色”，第三個單詞是“物種”：<celerity>.<colour>.<species>。
    - 我們將 Exchange 與 Q1 以 “*.orange.*” 綁定，與 Q2 以 “*.*.rabbit” 和 “lazy.#” 綁定。這可以理解為：
        - Q1 會收到所有關於“橘色動物”的訊息（第二個單詞為 *orange*）
        - Q2 會收到所有關於“兔子”的訊息，以及所有關於“懶惰動物”的訊息。（第三個單詞為 rabbit，或第一個單詞 lazy）
    - 以下為 Producer 傳入的訊息及結果：
        1.  “quick.orange.rabbit” 會進入兩個 Queue：Q1 及 Q2
        2. “lazy.orange.elephant” 會進入兩個 Queue：Q1 及 Q2
        3. “quick.orange.fox” 會進入一個 Queue：Q1
        4. “lazy.brown.fox” 會進入一個 Queue：Q2
        5. "lazy.pink.rabbit" 會進入一個 Queue：Q2（僅一次，即使匹配兩個綁定）
        6. “quick.brown.fox” 不會進入任何 Queue（與任何綁定都不匹配）
    - 再讓我們看一些特殊的案例：
        1. “orange” 不會進入任何 Queue（違反 “*.orange.*”，前後各缺少一個單詞）
        2. “quick.orange.male.rabbit” 不會進入任何 Queue（違反  “*.*.rabbit”，前後多出一個單詞） 
        3. “lazy.orange.male.rabbit” 會進入一個 Queue：Q2（即使它有四個單詞，也與 “lazy.#” 匹配。
## 下載＆安裝 RabbitMQ
介紹完五種模式，你是不是已經很想動手實作看看？ 接下來就要告訴大家如何自己搭建 RabbitMQ，官方有提供 RabbitMQ Server 在不同作業系統(Linux, MacOS & Windows)上的[安裝指南](https://www.rabbitmq.com/download.html)，不過我還是推薦使用 Docker 建立環境最簡單。

### Docker 指令
    
```bash
# for RabbitMQ 3.9, the latest series
docker run --rm --name rabbitmq -p 5672:5672 -p 15672:15672 -e RABBITMQ_DEFAULT_USER=root -e RABBITMQ_DEFAULT_PASS=1234 rabbitmq:management 
```

- `docker run` 執行容器
- `--rm` 當容器終止時會自動刪除
- `--name rabbitmq`  將容器命名為 rabbitmq
- `-p 5672:5672` 將本機端的 5672 port 關聯到容器裡的 5672 port（RabbitMQ）
- `-p 15672:15672` 將本機端的 15672 port 關聯到容器裡的 15672 port（Web UI）
- `-e RABBITMQ_DEFAULT_USER=root` 宣告環境變數，連線到 RabbitMQ 的 username
- `-e RABBITMQ_DEFAULT_PASS=1234`  宣告環境變數，連線到 RabbitMQ 的 password
- `rabbitmq:management` 指定容器抓 docker hub 上的 rabbitmq [official image](https://registry.hub.docker.com/_/rabbitmq/)
    - 預設為最新版本(3.9)，若想要指定版本可以用 `rabbitmq:3.8-management`
### Docker Compose 指令
    
```docker
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

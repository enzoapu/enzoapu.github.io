# [COURSE] RISE - 設計 E2E 數據架構 學程心得

<!--more-->
## 前言
    
2022年底，我報名了 [ALPHA Camp](https://www.facebook.com/alphacamp.tw) 推出的「設計 E2E 現代數據架構」學程（業界首見👀），六週的內容超出預期的豐富，認識了許多 Data 領域的超強導師和優秀同儕，用力地學習、死命地交流，獲得的知識、經驗、人脈和資源，實在很超值！
    
~~因為繳交心得作業的期限快到了，~~所以這篇文章將透過回顧和反思來記錄這些時間的想法與收穫。
    

## 當初為什麽想申請 RISE、參加這門學程
    
當初是從 ALPHA Camp co-founder Youchi 的[臉書貼文](https://www.facebook.com/alphacamp.tw/posts/pfbid023VgjTruqFB8Q4DCgxvNtFK2uoXbMqCrntGk6m8tFt9kQG8wdYKan4kQFnbnAy1FLl)得知此計畫，看完 [RISE](https://www.notion.so/535cb38c368a438aa268a5528dac54ae) 跟 [Early Riser](https://www.notion.so/c15b8a99e68a402cba1d623c7cdae290) 介紹頁，當下就決定要報名了！
    
學程主打的「教練陪跑式實戰營隊設計」、「用宏觀視角培養領導者思維」、「結交數據領域的優秀人脈」都非常吸引我，特別是第三點，從報名表填寫到第二階段面試就能知道，願意報名並錄取這個計畫的其他同學，都是在數據領域、積極且有實戰經驗的優秀同儕。這樣這不僅能學習到不同公司、不同產業所使用技術、架構、工具，課後的交流與人脈的連結更讓這裡形成一個高質量的社群，我相信對職涯發展來說幫助很大。
    
## 關於 RISE 的學習體驗或學程內容
    
    
這邊拿 [Early Riser](https://www.notion.so/c15b8a99e68a402cba1d623c7cdae290) 介紹頁的資訊跟大家分享，六週的主題軟硬皆具、非常精實！

（推薦大家去內頁看看，AC寫得很精準，也很用心💪）

{{< admonition type=tip title="觀念建立：建立從商業到技術全局觀" open=true >}}
    
- `Week 1`釐清商業組織需求：利用 impact mapping，介紹自己所屬的產業、組織部門結構。
- `Week 2`釐清 tech stack：透過產業案例認識數據產業三大類型，並拆解自身團隊 data flow & tech stack。
- `Week 3`發想小組 Use Case：透過「需求分析框架與範例」與小組共同定義下階段須拆解的 use case。

{{</admonition>}}

{{< admonition type=tip title="落地解決：針對需求提出解決方案" open=true >}}
    
- `Week 4`前期規劃：小組定義 data journey，進行可行性驗證，並規劃專案的時程與 milestones。
- `Week 5`中期評估：小組拆解 user stories 並嘗試估算開發及維運成本，列出潛在風險。
- `Week 6`規格提案：小組提出從系統設計，到物件、資料表設計的完整規格書。
    
{{</admonition>}}
    
課程進行方式並非老師單向授課的形式，而是**讓大家自己預習課程內容，再來到課堂上透過提問、討論和實作來學習**，大致可以區分為：
    
**課前**
1. 閱讀導師為大家精選的文章閱讀
2. 完成當週主題相關的作業個人/小組作業

**課中**
1. 導師抽點同學回答與討論當週主題（靈魂拷問時間）
2. 導師同時會作當週主題的內容和經驗分享（金句名言時間）
3. 在導師設計好的 Miro 模板上個人/分組實作與分享（腦力激盪時間）

**課後**
1. 課程心得與反饋
2. Discord 群組交流和延伸議題討論
3. 作業 refine 和 final presentation 準備
    
## 其中最讓我印象深刻的地方
    
國父在第二週課程提到的一個關鍵點：「**工具的決策，缺點比優點還重要**」，過去在決定要選用或導入的技術/工具時，常看到的都是正面的好處和成果（從影片或文章），建議要多看一些缺點和限制，例如：`有很高的學習成本`、`服務穩定度不高`、`網路資源缺乏`等，這些才是後續容不容易維護的重點。

有時我會以個人的角度，私心想要接觸時下流行的技術、導入新的工具或挑戰沒實作過的系統架構，但身為主管會`以團隊/公司的角度`檢視這些項目的`學習成本`、`構建成本`、`維護成本`、`人才市場`、`可帶來多大效益`、`是否有其他替代方案`等，分析完成本效益後可能會發現這些並不是團隊當前迫切需要的，就算是其他部門明確提出的需求，也該評估其合理性和必要性，給予專業的優先度建議，因為「**沒有完美的解決方案或做不出來的系統，只是從 day 1 到 day n 都在做取捨而已**」。

以這次我們小組的 final presentation 為例，題目簡介：「營運部門 PM 想要查看即時的營收表現(Revenue)、用戶流量(Traffic) 的 Dashboard，所以 Data Team 要將本來的 Daily Batch Pipeline 導入並整合 Streaming Pipeline，最終規劃要 3個人力 & 4個人月來完成」。

然而導師最後的評語強烈提醒我，自己的思考還不夠全面，真的是`格局和高度的差距`阿！

下面列出國父和 Bryant 真實的回饋：
- 「BigQuery data warehouse 比 production DB replica 還不及時，似乎不是引入 BigQuery 能解決的痛點。」
- 「讓 DE/DA centralize 負責大部分的 transformation 已經解決多數問題，dataform 的引入似乎不算是在新組織架構與分工下關鍵要解決的問題。」
- 「不確定 dataflow 的必要性，是否有可能 pub/sub 直入 BigQuery 後，再在 BigQuery 做 transformation？這樣可以讓邏輯更集中在 BigQuery 一處，也比較不會跟 dataform 想解決的 metric layer 相衝突，避免又多一處有business logic 以及做 transformation。」
- 「Event Streaming 的部分是要先走 dataflow 再進 BQ 還是先進 BQ 在統一用 BQ 做計算在實務上會有很多討論，除了國父提出之外，我也會考慮人員的技能樹以及是否要多管一個 component。」
- 「如果外部單位提出來一個 Data 根本覺得不需要做，或是做了不合的需求該怎麼辦？例如這裡一直在討論的即時儀表板。課程和使用者資訊真的需要“即時”的看嗎？還是每一個小時看一次就差不多了呢？」
- 「如果要做 event streaming 的話，是不是可以一起把需要即時觀察的資訊透過 event 送出來，就不用再多導入一個 CDC 的系統？」

## 給有興趣申請 RISE 的朋友一些建議

此計畫很適合**學習動機強並樂意在社群中共學**的朋友參加，這門課也期待你是有設計和打造數據相關產品經驗，且有興趣學習管理更大規模的資料專案的技術人才，雖然過程中要投入蠻多時間和精力在準備課程和小組討論上，但收穫絕對會超乎你的想像！

這邊分享申請概要供大家參考，如果你沒信心自己是否符合資格，下面這些項目都可以預先準備：
- Linkedin 個人履歷頁面
- 技術能力 - 程式語言（SQL, Python, etc）
- 技術能力 - 數據工程相關工具（Spark, Airflow, etc)
- 目前職位的核心任務
- 你對於 data pipeline 的理解
- 你曾參與的 data project（主要要解決的問題 vs 你負責的任務）
- 「設計 end-to-end 數據架構」這個學程能如何在工作上幫助你？
- 你是否能每週投入至少 5 小時，且願意積極參與課堂與小組討論？
- 你對 RISE 的期望與可能的參與
- 你的推薦人

如果你對計畫內容或申請攻略有任何問題歡迎詢問我哦～

## 總結
很感謝自己當初報名這次的課程，這些內容都非常值得拿出來重複觀看，隨著工作經驗累積，我想一定會有不一樣的收穫～

「設計 E2E 現代數據架構」學程只是 RISE 計畫的起點，2023 我要好好利用這個資源豐沛的場域，累積人脈、持續學習、職涯升級🙏

<br>
{{< image alt="課程完結" src="final-presentation.png" caption="課程完結" height="" width="100%">}}

## 參考
[RISE - 給未來技術領袖的新學習場域](https://achq.notion.site/RISE-535cb38c368a438aa268a5528dac54ae)

[Early Riser - 設計 E2E 現代數據架構](https://www.notion.so/c15b8a99e68a402cba1d623c7cdae290)

[Early Riser 成果發表會回顧 (Linkedin Post)](https://www.linkedin.com/feed/update/urn:li:activity:7021370176159842304/)

[ALPHA Camp 臉書專頁](https://www.facebook.com/alphacamp.tw)


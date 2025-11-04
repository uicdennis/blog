### 探索 GPT 的陷阱：利用各種生成式預訓練 Transformer 工具中學到的經驗教訓

###### Dennis Liao, 2024-05-08

##### 前言

大型語言模型風行的年代，不使用GPT (Generative Pre-trained Transformer)工具解決問題，似乎有點對不起自己。對於眾多的(免費)GPT工具，如Bing Copilot，ChatGPT，Gemini ai及Claude ai，對於未知需要學習或處理的事務，都免不了要卜上一卦。這邊沒有特別強調免費，是因為我不免俗地跟風買了付費的GPT (Monica ai)，也因為花錢買了，不知不覺就非常依賴GPT工具了。起初對於這些GPT工具，真的會忍不住的讚嘆，當然也伴隨著被取代的恐懼，因為它們真的就像是萬事通，節省我不少時間，真的讓我驚豔的效率與確實，讓人忍不住依賴上它們。

隨著使用時日的增長，對它的期待也越來越高。漸漸地，對於它提供的答案，要求也越來越高。可能是因為講求效率的想法，越來越期待只問一次就能解決我的困難。可能是因為這樣的想法，造成使用GPT工具不太正確，也可能是GPT工具發生了所謂"迷失"，回覆品質已不能滿足我的期待。因此，想藉由今日的經驗，重置一下使用GPT工具的用法。

##### 臨時追加的需求 - GPT迷航記

今日忽然被追加與兩天後要討論的議題不太相關的任務 - 提供Email通知的功能。對於我這個對網路不太熟悉的人，忽然要研究這樣的議題，當然要求助GPT工具。首先登場的當然是自行購買的Monica GPT，穩紮穩打的問一下開發流程，也沒讓我失望地提供了程式碼。

<img title="" src="file:///Y:/Dennis/Blog/20240508_GPT陷阱/Monica-1.png" alt="" width="639">

![](Y:\Dennis\Blog\20240508_GPT陷阱\Monica-2.png)

但不知為何，我又問了Bing Copilot類似問題，這次也得到程式碼。

<img title="" src="file:///Y:/Dennis/Blog/20240508_GPT陷阱/Copilot-1.png" alt="" width="486">

於是乎，我就拷貝了Copilot的程式碼使用，執行後，事與願違地得到了錯誤。

不打緊，從哪邊跌倒，就從那邊再站起來。於是再次詢問Copilot"你為什麼給我錯誤的答案"(誤)，不是。是直接剪貼得到的錯誤訊息，以尋求解答。在此同時，因為錯誤訊息有著"Application-specific password"，我又雞婆的瞎猜是否與兩階段認證有關，還真的與兩階段認證有關。

<img title="" src="file:///Y:/Dennis/Blog/20240508_GPT陷阱/Copilot-2.png" alt="" width="587">

順著指示，打開Google帳戶設定的安全性，「應用程式密碼」在哪，根本找不到。於是乎再次求救，再問一次Copilot。

<img title="" src="file:///Y:/Dennis/Blog/20240508_GPT陷阱/Copilot-3.png" alt="" width="582">

挖哩勒，給我搞這套，居然拒絕回答，還祝我好運，真有你的。好吧，只好換人了。既然是Google，當然就請Gemini出場啦。

<img title="" src="file:///Y:/Dennis/Blog/20240508_GPT陷阱/Gemini-1.png" alt="" width="463">

Gemini也提供了範例程式碼。

<img title="" src="file:///Y:/Dennis/Blog/20240508_GPT陷阱/Gemini-2.png" alt="" width="440">

因為找不到「應用程式密碼」，我開始迷失了~~:scream:

<img title="" src="file:///Y:/Dennis/Blog/20240508_GPT陷阱/Gemini-3.png" alt="" width="524">

其實我開始慌了，原本以為三兩下就解決的事情，繞了一整個下午。終於我做了一個更蠢的決定，Gmail不行，那就Outlook.com吧!我又搬出Bing Copilot卜了一卦:

<img title="" src="file:///Y:/Dennis/Blog/20240508_GPT陷阱/Copilot-4.png" alt="" width="561">

嗯，感覺更難解決，毅然決然地放棄:sweat_smile:。

好吧，沒招了，只好使出號稱目前最強的Claude 3 Opus。既然是最強的，當然要出難題給他啦，又問了底下幾個問題:

- 如何使用OAuth 2.0存取Google帳號，請提供python範例程式碼。

- 若不使用OAuth 2.0，有其他辦法使用python登入Google帳號嗎?
  這個Google帳號已啟動兩階段認證，並且使用手機金鑰認證。

- Google帳號安全性頁面的登入Google方式已經沒有應用程式密碼選項，若改採兩步驟驗證電話號碼，應該怎麼做?

一個感覺就是自己騷包，問了一堆自己也無法了解的問題。不過，中間有著類似的答案，又繞回「應用程式密碼」。

<img title="" src="file:///Y:/Dennis/Blog/20240508_GPT陷阱/Claude-1.png" alt="" width="367">

最後，我決定直接請教Google大神，在一番胡亂操作下，終於在Google帳戶知道如何產生「應用程式密碼」了。

<img title="" src="file:///Y:/Dennis/Blog/20240508_GPT陷阱/Google_Account-1.png" alt="" width="325"> <img title="" src="file:///Y:/Dennis/Blog/20240508_GPT陷阱/Google_Account-2.png" alt="" width="221">

設定好後，真的可以在兩步驟驗證下找到應用程式密碼。

對啦，人工智慧比較了不起啦，不知道我們這種小白的辛苦，就不會告訴我"找不到的話，直接鍵入搜尋":disappointed:。

迷航了這麼久，終於解決了第一道難題。

哪泥(日語)，還有其他問題。折騰了一會兒，了解APP_PASSWORD是16碼，設定正確後，執行程式，還是得到錯誤。

![](Y:\Dennis\Blog\20240508_GPT陷阱\Error-1.png)

這應該是我把login()的用法給誤會了，加上有兩個Google Account，在不斷嘗試組合下的迷航。就在某一次的修改，把`port="587"`改成`port=587`，然後就好了(實際上，我不確定這不是真正關鍵)。

```python
with smtplib.SMTP(host="smtp.gmail.com", port=587) as smtp:
   ...
   smtp.login("example@gmail.com", pwd)  # 登入寄件者 Gmail
```

然後我就以為Copilot提供我錯誤的答案，但是我改對了。總之，我是開心的接受了這個結果。

##### 後話

隔日，我再次驗證`port="587"`，發現沒有影響正確性，查看`smtplib.SMTP()`的參數port，並沒有轉型別int，只有在未指定參數port時，才有一段程式碼轉換型別。不知道是我在使用`login()`時，誤用了錯誤的帳密組合，還是真的修改成`port=587`造成，總之，改成int總是比較好。

其實我後來還是將這段疑惑再次詢問Copilot與Monica，其解答都是建議使用`port=587`，為什麼`port="587"`又變成正確，這個疑惑就留給其他大神解答了。

經過這次經驗，了解到只想伸手跟GPT要結果，在不順遂的情況下，最好加強一下基礎知識後，再與GPT對話，否則迷失的會是自己，而不是GPT。

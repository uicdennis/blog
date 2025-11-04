### BookFast智能問答系統開發報告

###### 概觀 (Overview)

關於BookFast智能問答系統的開發，整套系統主要是建置在Micorsoft Azure語言服務，以及使用Microsoft Language Studio的自訂問題回應專案 (custom question answering)。另外使用Microsoft Bot Frameword SDK建置Bot Server，以及Bot Famework Emulator作為使用BookFast智能問答系統示範。

<img title="" src="file:///Y:/Dennis/Blog/20240418_BookFast QnA系統/第一次報告_2024-04-19/Azure_語言服務.png" alt="" width="156">

<img title="" src="file:///Y:/Dennis/Blog/20240418_BookFast QnA系統/第一次報告_2024-04-19/Custom_Question_Answering.png" alt="" width="543">

[Bot Frameword Emulator Release 4.14.1-371583 · microsoft/botframework-emulator-nightlies (github.com)](https://github.com/microsoft/botframework-emulator-nightlies/releases/tag/v4.14.1-371583)

###### 建置知識系統

因為要使用Microsoft Language Surdio，必須預先建立Microsoft Azure 語言服務。而要建立Microsoft Azure語言服務，則必須先有Microsoft Azure帳戶(Subscription)，幸運的是，Microsoft Azure提供許多免費服務([立即建立 Azure 免費帳戶 | Microsoft Azure](https://azure.microsoft.com/zh-tw/free/))，而BookFast智能問答系統的開發，幾乎都是使用到免費服務，因此在基本的服務底下，可以建置整個系統概念與雛形。

有了Azure帳戶後，就可以透過[語言服務問題解答功能的基本概念 - Training | Microsoft Learn](https://learn.microsoft.com/zh-tw/training/modules/build-faq-chatbot-qna-maker-azure-bot-service/)學習Azure語言服務及Language Studio問題解答功能。假如我們已建置好Azure語言服務及Language Studio Custom Question Answering專案後，我們就可以透過專案建置知識系統。

Language Studio Custom Question Answering專案提供檔案或URL的方式提供專屬知識系統，因為BookFast系統教學都建置於部落格的內容，但是需要密碼登入，因此此次示範採用檔案方式建置。而Custom Question Answering的知識檔案資源又分成結構式與非結構式，簡單來說，使用說明文件就是典型的非結構式檔案資源。另外，像使用Excel檔案進行問題與解答的形式蒐集，就是典型的結構性檔案資源，此次示範即使用Excel建置知識資源。

| Question   | Answer                                                                                                                                                                           | Category |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| 如何設定人流監控?  | 進入後台後，設定步驟如下: <br/>1. 設定 → 會館設定<br/>2. 點擊各會館右方的「容留按鈕」<br/>3. 編輯「人流監控設定」                                                                                                          | 0        |
| 什麼是人員帳號?   | 人員主要透過email方式註冊，用來登入POS系統的帳號。                                                                                                                                                    | 1        |
| 方案設定有什麼選擇? | 1. 儲值票券設定: 請參考 BookFast 後台系統教學 – 自訂票券篇<br/>2. 定期卡方案設定：請參考 BookFast後台系統教學 – 通行卡進出場篇<br/>3. 分鐘計費卡方案設定：請參考 BookFast後台系統教學 – 分鐘計費卡進出場篇                                               | 2        |
| 如何查看會員清單?  | 1. 登入後台系統<br/>2. 點選 會員管理 -> 會員清單 項目                                                                                                                                              | 3        |
| 建立老師的步驟為何? | 1. 課程管理 → 老師清單<br/>2. 新增老師資料                                                                                                                                                     | 1        |
| 如何建立老師資料?  | 進入BookFast後台介面後，步驟如下:<br/>1. 選取課程模組選單下的老師清單項目。<br/>2. 點擊新增老師按鈕。<br/>3. 輸入老師資訊 (名稱、圖片、介紹)。<br/>4. 儲存。<br/>「開課」可決定會員端 APP 師資介紹頁面，是否顯示有教團體課或私人課，但不影響課程的建立或預約。（此為團課＋私課模組皆有的商家才會顯示噢！） | 5        |
| ...        | ...                                                                                                                                                                              | ...      |

###### 呼叫Custom Question Answering

![](Y:\Dennis\Blog\20240418_BookFast%20QnA系統\第一次報告_2024-04-19\Azure_語言_Endpoint.png)<img title="" src="file:///Y:/Dennis/Blog/20240418_BookFast QnA系統/第一次報告_2024-04-19/QnA_project.png" alt="" width="291">

透過Azure Python SDK，可以了解如何叫用Custom Question Answering服務。基本上，Azure是透過Azure語言服務提供端點串接Custom Question Answering，因此我們需要

1. AZURE_LANGUAGE_ENDPOINT

2. AZURE_LANGUAGE_KEY

3. AZURE_QUESTIONANSWERING_PROJECT

```Python
import os
from azure.core.credentials import AzureKeyCredential
from azure.ai.language.questionanswering import QuestionAnsweringClient
from azure.ai.language.questionanswering import models as qna

endpoint = os.environ["AZURE_LANGUAGE_ENDPOINT"]
key = os.environ["AZURE_LANGUAGE_KEY"]
knowledge_base_project = os.environ["AZURE_QUESTIONANSWERING_PROJECT"]

client = QuestionAnsweringClient(endpoint, AzureKeyCredential(key))
with client:
    question="如何新增老師資料?"
    # question="如何新增教室?"
    output = client.get_answers(
        question=question,
        top=3,
        confidence_threshold=0.2,
        include_unstructured_sources=True,
        short_answer_options=qna.ShortAnswerOptions(
            confidence_threshold=0.2,
            top=1
        ),
        project_name=knowledge_base_project,
        deployment_name="test"
    )
    print(len(output.answers))
    ans_count = len(output.answers)
    if ans_count == 1 and output.answers[0].confidence < 0.7:
        best_candidate = output.answers[0]
    else:
        best_candidate = [a for a in output.answers if a.confidence > 0.7][0]
    print("Q: {}".format(question))
    print("A: {}".format(best_candidate.answer))
```

執行結果如下:

![](C:\Users\decar\AppData\Roaming\marktext\images\2025-11-04-10-53-45-image.png)

透過Azure Python SDK可以建置出QuestionAnsweringClient物件，可以透過此物件設定問答的信心度(confidence_threshold)與最好的幾個答案(top)，然後再從中選出最好或最合適的答案進行回傳。

###### 使用Bot Framework Emulator及Bot Server

在前面的部分有提到如何取得Bot Emulator，所以讓我們先看一下執行結果

![](Y:\Dennis\Blog\20240418_BookFast%20QnA系統\第一次報告_2024-04-19\chatbot-1.png)

使用Bot Emulator，需要一個 'api/messages' 的 (Bot Server) Web API，預設使用port 3978，也可更改為其他port number。透過[建立基本 Bot - Bot Service | Microsoft Learn](https://learn.microsoft.com/zh-tw/azure/bot-service/bot-service-quickstart-create-bot?view=azure-bot-service-4.0&tabs=python%2Cvs)的學習，可以很快建立好近端的Bot Server，結合Custom Question Answering服務，就可完成BookFast智能問答系統。

```shell
(.venv_bot) PS C:\Dennis\VSCode_Test\Azure ML\5_Bot\bookfast_qna_bot> python .\app.py
======== Running on http://localhost:8000 ========
(Press CTRL+C to quit)
```

![](Y:\Dennis\Blog\20240418_BookFast%20QnA系統\第一次報告_2024-04-19\result-2.png)

上面是執行近端BookFast Bot Server搭配Bot Emulator的結果。我們可以看到，雖然問題是"我想要建立老師資料，請問可以怎麼辦?"，但是透過Custom Question Answring比對，"如何建立老師資料?"的吻合度達0.93，因此提供了此題的答案。

Azure Bot Server其實是一個Web API，可以架設到Azure Web App服務，並且搭配Azure Bot服務，透過通道方式，進一步串接LINE或Microsoft Teams等。不過，這些進一步的服務必須使用付費方案，因此暫時沒有實現。

###### 後記

直接使用Azure付費方案可能是一種解法，但是還是花了一點時間尋找其他方式。透過虛擬機器架設Bot Server，雖然可以運行，但是有許多防火牆相關的知識與問題都不太明瞭，因此均以失敗坐收。自行部署Azure Web App建置Bot Server，也因為方案限制，不知道阻礙在哪，部署成功，但卻無法正常連接。因此，單就智能問答API的服務，基本上算是完成，但整套系統運行，似乎還有努力空間。

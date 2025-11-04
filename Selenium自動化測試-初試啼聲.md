# selenium

### 前言

Selenium 是一系列工具和函式庫的總括項目，可實現和支援 Web 瀏覽器的自動化。

[Selenium官方API文件](https://www.selenium.dev/selenium/docs/api/py/api.html)

它提供了用於模擬使用者與瀏覽器互動的擴充功能、用於擴展瀏覽器分配的分發伺服器以及用於實現W3C WebDriver 規範的基礎設施（使您可以為所有主要Web 瀏覽器編寫可互換的程式碼） 。Selenium 的核心是 WebDriver，這是一個用於編寫可在許多瀏覽器中互換運行的指令集的介面。支援的程式語言有Java, Python, C#, Ruby, Javascript及Koltin。

### 初嘗Selenium

學習Selenium步驟，依據[STEAM教育學習網教學](https://steam.oxxostudio.tw/category/python/spider/selenium.html)，大致如下:

+ 1. [安裝 selenium 函式庫](https://steam.oxxostudio.tw/category/python/spider/selenium.html#a1)
+ 2. [什麼是 Selenium WebDriver](https://steam.oxxostudio.tw/category/python/spider/selenium.html#a3)
+ 3. [下載 WebDriver](https://steam.oxxostudio.tw/category/python/spider/selenium.html#a4)
+ 4. [import selenium](https://steam.oxxostudio.tw/category/python/spider/selenium.html#a2)
+ 5. [使用 WebDriver 開啟第一個網頁](https://steam.oxxostudio.tw/category/python/spider/selenium.html#a5)
+ 6. [取得網頁元素](https://steam.oxxostudio.tw/category/python/spider/selenium.html#a6)
+ 7. [操作網頁元素](https://steam.oxxostudio.tw/category/python/spider/selenium.html#a7)
+ 8. [取得網頁元素的內容](https://steam.oxxostudio.tw/category/python/spider/selenium.html#a8)
+ 9. [搭配 JavaScript，發揮最大效益](https://steam.oxxostudio.tw/category/python/spider/selenium.html#a9)
+ [參考資料](https://steam.oxxostudio.tw/category/python/spider/selenium.html#end)

目前以Chrome版本 119.0.6045.105 與 Edge版本 119.0.2151.93 為練習標的。

### 牛刀小試

假如我們想要透過Chrome查看~~股票(誤)~~新聞，但是~~懶惰到~~需要使用自動化開啟，那就可以使用Selenium來達成目的。基於上述的學習，使用WebDriver，開啟Google新聞首頁。

```python
from selenium import webdriver
from selenium.webdriver.common.by import By

import time

driver = webdriver.Chrome()
driver.get('https://news.google.com/')

time.sleep(5)
```

是不是很簡單!

假如我們希望從Google搜尋切換到新聞頁面，那就比較複雜。首先需要知道功能表元素如何選取。我們知道需要呼叫find_element()來取得網頁元素(WenElement)，但是問題是要如何知道"功能表"這個圖示要如何選取?

打開Google首頁，在網頁空白處點擊滑鼠右鍵叫出選單，如下圖:

![](Y:\Dennis\Blog\20240325_Selenium自動化測試\2024-03-25\image\Page_Check.png)

點擊"檢查"，叫出"Chrome 開發人員工具"(Chrome DevTools)，依序取得元素訊息。

![](Y:\Dennis\Blog\20240325_Selenium自動化測試\2024-03-25\image\Element_Select.png)

因此，我們就可以利用By.CLASS_NAME方式取得功能表元素。

同理，點選功能表圖示叫出功能表，然後"新聞"的圖示上點擊滑鼠右鍵，再次點選"檢查"，如此就可取得"新聞"元素訊息。

![](Y:\Dennis\Blog\20240325_Selenium自動化測試\2024-03-25\image\News_Icon.png)

那麼我們就有

```python
from selenium import webdriver
from selenium.webdriver.common.by import By

import time

driver = webdriver.Chrome()
# Google搜尋
driver.get('https://www.google.com.tw/')
# driver.get('https://news.google.com/')
func_panel = driver.find_element(By.CLASS_NAME, 'gb_d')
func_panel.click()

# 延遲已等候應用程式面板顯現
time.sleep(1)

# 找不到'MrEfLc'
news = driver.find_element(By.CLASS_NAME, 'MrEfLc')
news.click()

time.sleep(5)
```

很不幸地這不能成功，這是因為Google網頁使用iframe的方式內嵌應用程式面板，而這個iframe有著'app'的名稱，因此修訂部分程式碼

```python
# 延遲已等候應用程式面板顯現
time.sleep(1)

# iframe = driver.find_element(By.TAG_NAME, 'iframe')
driver.switch_to.frame('app')
spans = driver.find_elements(By.TAG_NAME, 'a')
for el in spans:
    # print(el.text)
    if el.text == '新聞':
        news = el
        break
news.click()

time.sleep(5)
```

參考資料

- [Selenium官方API文件](https://www.selenium.dev/selenium/docs/api/py/api.html)

- [STEAM教育學習網教學](https://steam.oxxostudio.tw/category/python/spider/selenium.html)

- Chrome開人員工具面板操作說明([開始查看及變更 DOM &nbsp;|&nbsp; DevTools &nbsp;|&nbsp; Chrome for Developers](https://developer.chrome.com/docs/devtools/dom?hl=zh-tw)

- [Working with IFrames and frames | Selenium](https://www.selenium.dev/documentation/webdriver/interactions/frames/)

#Selenium, #Python, #RD

---
title: Python Html剖析器簡單應用
date: 2024-07-13 01:54:11
categories: 程式筆記
tags: 
-  Python
-  Html
---

今天在寫Discord機器人時需要取得rss feed內容
本來是一帆風順的，結果feed的描述竟然是html標籤，沒有接觸過html的我就研究了一番python內建的剖析器

記下來以免我以後忘了 .w.

## 建立類別
剖析器需要繼承類別後再修改內部內容
定義好函數後，使用時會自動呼叫函式來處理

```python
from html.parser import HTMLParser

class MyParser(HTMLParser):
    def __init__(self):
        super().__init__()

    def handle_starttag(self,tag,attr) -> None:
        print(tag)

    def handle_endtag(self,tag) -> None:
        print(tag)

    def handle_data(self,data) -> None:
        print(data)
```
* handle_starttag
當遇到開始的標籤時，會呼叫這個函式來處理
`<a href='https://google.com'>`會以`handle_starttag('a',[('href','https://google.com')])`被呼叫
簡單來說就是tag儲存標籤名稱，attr以tuple組成清單方式儲存屬性

* handle_endtag
跟starttag的功能差不多，只差endtag不會有屬性

* handle_data
處理標籤內容，不限格式
`<p>Hello</p>`會以`handle_data('Hello')`被呼叫
也就是說，可以利用此函式來取出標籤內容
非常符合我剛好的需求，因為內容都是一堆\<p>

**這三個函式視需求而定義，若不需要其中一個功能則可省略**

## 應用

接續上一份程式碼
```python
#範例內容
content = '<p>Sharkspeare筆記</p><p>記下以免我忘記</p>'

handler = MyParser()

#將內容提交給剖析器
handler.feed(content)
```
此時剖析器會根據內容而重複呼叫剛剛定義的函式處理
而上一段範例程式碼是呼叫後打印出來，所以會出現
```
p
Sharkspeare筆記
p
p
記下以免我忘記
p
```
## 儲存資料
由於處理資料的函式不會進行任何回傳，所以可以將資料存入物件的其中一個屬性，改寫成:
```python
class MyParser(HTMLParser):
    def __init__(self):
        super().__init__()
        self.result = "" #初始化用來儲存的變數
    
    #函式處理完之後將結果加到變數
    def handle_starttag(self,tag,attr) -> None:
        self.result += tag

    def handle_endtag(self,tag) -> None:
        self.result += tag

    def handle_data(self,data) -> None:
        self.result += data
```
最後結果就會留在物件的result屬性裡，可供你進行其他應用

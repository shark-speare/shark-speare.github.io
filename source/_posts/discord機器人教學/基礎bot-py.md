---
title: (一) 基礎bot.py
date: 2024-07-13 22:33:05
categories: [程式筆記,Discord機器人教學]
tags:
- Discord
- Python
series: Discord機器人教學
---

阿囉哈各位，我現在要進入高產模式
今天要來教各位怎麼從頭開始寫一個機器人

值得注意的是，此系列文章非單一課程，建議一次跟著學完而非單看一篇文章
因為會有許多地方是為後續作準備
當然我也會特別標記有這類情形的地方:
* 後續會更動

此系列從程式出發，請自行準備好機器人與Token


# 準備
創建一個資料夾並安裝套件:
```shell
pip install discord.py
```

# 基礎bot.py
在資料夾內新增`bot.py`檔案
接著就來看如何建立機器人

```python
# 此區段一律放在整個檔案最開始，但import部分可依據需求修改
import discord
from discord.ext import commands
import asyncio


# 設定機器人可以做的事
# Discord特殊規定，可以不用特別了解
intents = discord.Intents.all()

# 定義機器人物件
bot = commands.Bot(command_prefix=">",intents = intents)
```
此處的`command_prefix`為指令前綴，也就是一般的文字指令，不會出現在斜線指令列表裡，自己選一個喜歡的吧

---
* 後續會更動

```python
# 此區塊一律放在整個檔案最後

# 定義主程式
# token可以用json或是環境變數隱藏起來
async def main()
    await bot.start("(token)")

asyncio.run(main())
```
Discord機器人的運作方式為非同步處理，所以利用了asyncio的`async` `await`來處理
大部分函式定義都須加上`async`，而內部與discord模組相關的部分才須使用`await`

---
恭喜你，完成了最基礎的一步了
執行此檔案就可在Discord看見你的機器人上線了




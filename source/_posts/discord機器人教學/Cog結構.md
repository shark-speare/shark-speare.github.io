---
title: (三)Cog結構
date: 2024-07-31 11:09:49
categories: [程式筆記,Discord機器人教學]
tags:
- Python
- Discord
series: Discord機器人教學
---

哈囉各位
今天要來教一個新的概念: Cog
Cog有點像是模組，可以將一系列的功能分類到一個獨立檔案內

# 基本結構
```python
import discord
from discord.ext import commands

class MyCog(commands.Cog):
	def __init__(self,bot:commands.Bot):
		self.bot = bot

async def setup(bot):
	await bot.add_cog(MyCog(bot))
```
Cog使用類別方式來儲存觸發條件與指令，所以首先須定義一個類別，名稱自訂
Discord引入Cog時須使用`commands.Cog`類別的屬性，所以繼承該類別，並且需初始化bot屬性
最後檔案末端定義如上所示的`setup`函數即可完成基本Cog結構
**上列的程式碼為必須加入，每個區塊缺一不可**

# 指令
接著來示範如何加入指令
需先閱讀[此篇](https://shark-speare.github.io{% post_path 'discord機器人教學/指令與觸發事件' %}#指令)了解如何定義指令
擷取上一部分的程式碼作範例
```python
class MyCog(commands.Cog):
	def __init__(self,bot:commands.Bot):
		self.bot = bot

	@commands.command()
	async def command(self,ctx,arg):
		await ctx.send(f"You said {arg}.")
```
此處的寫法與主檔案內的寫法稍有不同
* `bot.command()`需替換成`commands.command()`
* 由於此為方法而非函數，所以須加上self參數
其餘皆相同

# 觸發條件
需閱讀[此篇](https://shark-speare.github.io{% post_path 'discord機器人教學/指令與觸發事件' %}#觸發條件)了解如何定義觸發條件
```python
class MyCog(commands.Cog):
	def __init__(self,bot:commands.Bot):
		self.bot = bot

	@commands.Cog.listener()
	async def on_ready(self):
		print("Bot is ready")
```
與指令相同，此處也須更改一些地方:
* `bot.event`需更改成`commands.Cog.listener()`
* 因為是方法所以需要`self`

# 小技巧
到此處基本的Cog就完成了，可根據自己需要做更多應用
Discord機器人預設開啟help指令
至頻道輸入help指令即可看到主檔案和cog檔案內的指令結構


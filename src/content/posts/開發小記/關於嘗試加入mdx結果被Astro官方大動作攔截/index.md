---
title: 關於嘗試加入mdx結果被Astro官方更新攔截
description: 我錯了我以後會關注官方的
slug: failed-to-add-mdx-support
published: 2026-06-27 13:52:00
category: 開發小記
tags: [web, Astro, mdx]
cover: cover.png
---

我現在用的網頁框架是 Astro ，它支援mdx格式，並且可以匯入自己的元件。結果我今天在幫佈景主題新增 mdx 的支持時一直出現錯誤。我如果把 mdx 插件拿掉就沒問題了，所以問題就是在 mdx 插件。但也不是檔案解析問題，因為不管我的文章是不是用 mdx 寫的都一樣會報錯，所以問題就是出現在插件本身而不是插件運行過程。

後來問 Claude 發現 Astro 官方大概在 6/22 的時候釋出了 Astro 7.0，而我用的佈景主題是 Astro 5.0。好死不死原本解析 mdx 檔案是靠一大串 JS 套件堆疊起來的，新版本改成用一個叫 Sätteri 的 Rust 處理器實作，內部整個大改。所以我使用 `pnpm add @astrojs/mdx` 抓到的是新版用 Rust 實作的版本而不是 Astro 5.0 支持的 JS 版本。

以後會記得了，安裝套件依賴記得要看版本而不是一律抓最新版 (◐‿◑)﻿

我本來是打算自己裝舊版本的用，但提了 [Issue](https://github.com/Spr-Aachen/Twilight/issues/151#issuecomment-4815437214) 之後作者說大概會更新到 7.0 ，所以我就再忍一下吧，因為我真的很想寫自己的小組件。如果沒有小組件的話，目前要這樣：

<div class="border-[1px] p-3 rounded-2xl text-center w-1/2">
<span class="font-bold text-center underline decoration-wavy">《臨江仙·滾滾長江東逝水》</span><br />
滾滾長江東逝水，浪花淘盡英雄。<br />
是非成敗轉頭空。<br />
青山依舊在，幾度夕陽紅。<br />
白髮漁樵江渚上，慣看秋月春風。<br />
<span class="text-red-400">
一壺濁酒喜相逢。<br />
古今多少事，都付笑談中。
</span>
</div>

```md
<div class="border-[1px] p-3 rounded-2xl text-center w-1/2">
<span class="font-bold text-center underline decoration-wavy">《臨江仙·滾滾長江東逝水》</span><br />
滾滾長江東逝水，浪花淘盡英雄。<br />
是非成敗轉頭空。<br />
青山依舊在，幾度夕陽紅。<br />
白髮漁樵江渚上，慣看秋月春風。<br />
<span class="text-red-400">
一壺濁酒喜相逢。<br />
古今多少事，都付笑談中。
</span>
</div>
```

有了小組件我可以

```mdx
import frame from 'components/frame'

<Frame>
<span class="font-bold text-center underline decoration-wavy">《臨江仙·滾滾長江東逝水》</span>
滾滾長江東逝水，浪花淘盡英雄。
是非成敗轉頭空。
青山依舊在，幾度夕陽紅。
白髮漁樵江渚上，慣看秋月春風。
<span class="text-red-400">
一壺濁酒喜相逢。
古今多少事，都付笑談中。
</Frame>
```

而且這個 `<Frame>` 是可以到處重複用的。希望作者快更新吧

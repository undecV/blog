---
title: "把電腦變成床頭時鐘：在 Windows 上打造一個低干擾的睡前待機模式"
description: "我不喜歡睡覺時關掉電腦，但夜晚點亮的螢幕就像一顆 disco 球，無時無刻地對外輻射著誘惑與慾望。這篇文章記錄我嘗試在 Windows 上，用腳本一鍵把螢幕、風扇和瀏覽器調整為低干擾狀態，讓待機的電腦變成一件有溫度的傢俱。"
date: 2025-12-24
updated: 2025-12-24
slug: "windows-bedside-display"
taxonomies:
  tags: [zh_TW, Windows, Scripting, Automation, AHK]
---

我有一個怪癖就是睡覺的時候不喜歡關掉電腦。

畢竟如果不顯示一些什麼東西的話，兩片烏漆墨黑還那麼大的黑幕，感覺非常的有壓迫感。

可是睡覺的時候總不能拿螢幕當小夜燈吧？

所以我平常的 routine 就是睡前「螢幕亮度調整到最低」，「風扇調整到低速模式」，「回到暗色的桌面」。
可是 Windows 的調整這些設定尤為麻煩，
一套 SOP 做下來人都清醒了，我本來是要去睡覺的不是嗎...

又想到 Apple 給 iPhone 的 iOS 實裝的「待機模式（Standby mode）」，
將 iPhone 連接至充電器並側放，即可讓螢幕變成桌上時鐘。

為了安撫我的睡眠，我想要在 Windows 電腦上也實現這個效果：
讓電腦螢幕「一鍵」變成低干擾的桌面時鐘。

## 夢起始的地方

首先，我們想要讓罹患 ADHD 的螢幕可以安靜下來。

年紀見長的小朋友們可能知道，在前 LCD 螢幕時代，我們有一個東西叫做「螢幕保護程式」。
螢幕保護程式在不動電腦多久之後會自動進入一個「螢幕保護」畫面。
可是這個功能雖然 Windows 11 仍然健在，但實際上也還是那個時代原封不動的東西，
畫風極為復古。
而且螢幕保護是一個相較來說變動極快的畫面，不適合當睡覺房間裡的桌邊裝飾，
畢竟應該沒人想要睡覺的時候，自己的電腦卻在床頭開趴。

可是我們需要只是像床頭時鐘一樣，漂亮地顯示時間日期即可。

有一類應用專門用於美化瀏覽器的起始頁／新分頁，
他們讓起始頁變成一種低干擾、可視化且具儀式感的頁面，還提供豐富的客製化。
以替代原本真的啥都沒有的空白頁，或是資訊過載的瀏覽器預設起始頁。

這類應用通常以瀏覽器擴充套件的形式存在，一些亦有部署網頁版本，例如：

- [Tabliss](https://tabliss.io/)
- [Bonjourr](https://bonjourr.fr/)
- [Blooft](https://www.blooft.com/)
- [Anori](https://anori.app/)

![Bonjourr 預覽]()

除了最基礎床頭時鐘本職工作的日期和時間，
既然天生連著網路，我們甚至可以顯示天氣等低密度、低干擾的資訊，
進一步，我們還能加入適量的人生雞湯，作為一種溫和且自我說服式的精神滋養。

如果使用功能豐富的 Anori，真的像是手機擺設小部件一般，甚至有行事曆和新聞訂閱。
真的可以像 iOS 的待機模式顯示行事曆，讓明天的 agenda 來焦慮今天的自己。

最後我選了 Tabliss ~~，不為什麼，單純是用習慣了~~。

## 一閃一閃小星星

夜晚中點亮的電腦螢幕，
根本就是房間裡的 disco 球，
無時無刻地對外輻射著誘惑與慾望。

所以除了讓電腦螢幕安靜下來，我也希望能夠降低它的亮度。

> 註：我用一臺筆電外接一個螢幕，孤陋寡聞的我不瞭解其他情況。

Windows 調節螢幕亮度的功能，只能調整我筆電本體的螢幕。
而我的外接螢幕可能是過於古老，作業系統中甚至沒有找到直接調整亮度的地方。
我總不可能每天都按螢幕上的實體按鈕吧，那可多麻煩。

其實一些螢幕提供 DDC/CI 協定，可以用軟體控制亮度。
可我怎麼用了那麼多年電腦怎麼才知道有這種東西...

[Twinkle Tray](https://twinkletray.com/) 正是調節螢幕亮度的軟體，可以同時控制多個螢幕的亮度。

而且 Twinkle Tray 可以通過指令控制，例如把調整「所有螢幕」的亮度到「100%」閃光彈：

```powershell
& "$env:LOCALAPPDATA\Programs\twinkle-tray\Twinkle Tray.exe" --All --Set=100
```

有了指令調整螢幕亮度，我們已經朝著自動化睡眠邁入了第一步。

## 筆電的自動著陸

最好睡的環境莫過於坐飛機，
優秀睡眠的品質莫過於下飛機。
而筆電風扇的聲音，完美地模擬了飛機引擎的噪聲（不是）。

現代的筆電都會提供風扇運轉的模式，以適應各種工作場景。
而我的筆電全速運轉的時候，那個風扇聲音宛如飛機起飛。
但我可不想在睡覺的時候還要收直椅背、拉起桌板，
所以在睡覺之前，我會把風扇調整成靜音（Silent）模式。

可如果你和我一樣是封閉硬體廠商的受害者，
例如〇碩的 Armoury Create 是不提供外界操作介入的，
最便利的可能就只是鍵盤上那個無法被軟體模擬的硬組合鍵。

好在 [G-Helper](https://github.com/seerge/g-helper) 提供了第三方的控制面板，
雖然無法直接通過指令列控制，
但好在提供了可以被軟體模擬的組合鍵，使得我們可以便利的操作它。

> 本章節內容為供應商相依（Vendor-dependent），實作方式請依實際需求自行調整。

通過 PowerShell 模擬按鍵：

```powershell
(New-Object -ComObject WScript.Shell).SendKeys("^+%{F16}")
```

由於 API 限制，上面的指令有 `F17` 之類找不到的鍵，可以通過 AHK 模擬按鍵：

```powershell
$ahkExe = "C:\Program Files\AutoHotkey\v2\AutoHotkey.exe"
$ahk = Join-Path $env:TEMP "oneshot.ahk"
"Send '^+!{F17}'" | Set-Content -Path $ahk -Encoding UTF8
Start-Process -FilePath $ahkExe -ArgumentList @($ahk) -Wait -NoNewWindow
Remove-Item $ahk -Force
```

## 雙子座的瀏覽器

最後，我們想要讓兩個網頁分別佔據左右兩個螢幕，
就像雙子座一樣，外表相似、步調一致，但各自獨立運行。

但這看似簡單的問題卻沒有那麼簡單。

<!-- 眾所周知我是 Firefox 用戶，
但 Firefox 提供的 Kiosk 模式是為了商業看板設計的，故意設計的關閉困難 ~~（其實只要 `Alt+F4`）~~ 。
但我只需要做一個床頭時鐘，沒必要自己搞自己。 -->

顯示網頁當然就是 Chrome，它有幾個參數：

- `--app` 可以讓網頁像應用一樣最小 UI 模式啟動；
- `--window-position` 決定啟動時視窗的位置，座標原點是主螢幕的左上角為 `0,0`；
- `--start-fullscreen` 直接進入全螢幕模式；

然而在我的測試下，可能是由於作業系統的限制，
若是同一個 Instance（例如，兩個一樣的 Chrome），即使 Profile 不同，也無法準確的套用 `--window-position` 設定，
第二個視窗會被誘拐到第一個視窗附近。

我們需要兩個彼此獨立、卻行為一致的瀏覽器視窗，
所以我們用最簡單暴力的 workaround：直接用兩個不同的、不常用的 Chrome。

最後我們就可以一鍵啟動兩個雙子 Chrome：

- 右邊主螢幕使用 Chrome Dev，座標是 `0,0`，
- 左邊副螢幕使用 Chrome Beta，座標是 `-1920,0`，

```powershell
& "C:\Program Files\Google\Chrome Beta\Application\chrome.exe" --window-position=-1920,0 --start-fullscreen --app="https://web.tabliss.io/"
& "C:\Program Files\Google\Chrome Dev\Application\chrome.exe" --window-position=0,0 --start-fullscreen --app="https://web.tabliss.io/"
```

> 請按照自己的佈置按需設定。

最後，把它們捏到一起，包裝成一個 PowerShell 腳本，就有了一鍵啟動的「睡前模式」：

- 螢幕亮度調整到最低
- 風扇調整到低速模式
- 兩個螢幕打開兩個全螢幕網頁

![Cross Panel Screenshot]()

終於，待機的螢幕不再是冷冰冰的黑幕，
而變成了一種有生活感、有溫度的傢俱。

## Appendix

安裝上面所有提到的工具都可以從 `winget` 安裝。

通過 `winget` 安裝 Twinkle Tray：

```powershell
winget install --id "xanderfrangos.twinkletray"
```

通過 `winget` 安裝 G-Helper：

```powershell
winget install --id "seerge.g-helper"
```

通過 `winget` 安裝 AutoHotkey v2：

```powershell
winget install --id "AutoHotkey.AutoHotkey"
```

通過 `winget` 安裝兩個不常用的 Chrome：

```powershell
winget install --id "Google.Chrome.Beta"
winget install --id "Google.Chrome.Dev"
```

由於 Chrome 的滾動更新，`winget` 的 Metadata 更新可能不是那麼的及時，這讓雜湊（hash）驗證失效，導致安裝失敗，如果遇到的話可以等，或是繞過 Hash 驗證。

```powershell
winget settings --enable InstallerHashOverride
winget install --id "Google.Chrome.Beta" --ignore-security-hash
winget settings --disable InstallerHashOverride
```

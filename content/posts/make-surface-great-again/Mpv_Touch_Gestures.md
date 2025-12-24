---
title: "全能型影片播放器 mpv 被迫支援觸控螢幕辛酸畫面流出"
description: "觸控螢幕上最好的影片播放器，竟然是不支援觸控的播放器？本篇介紹如何用開源如開掛的播放器 mpv，透過社群腳本與設定打造支援手勢操作、預覽縮圖與現代化介面的極致體驗。並提供一鍵安裝懶人包，讓 mpv 擁有媲美手機播放器的直覺觸控操作體驗！"
date: 2025-07-11
updated: 2025-07-11
slug: "mpv-touch-gestures"
taxonomies:
  tags: [zh_TW, Awesome list, Windows, Software, Application, Tablet, Surface, Touch Screen, Touch Gesture, Media Player, Video Player, mpv]
---

[YouTube][] 作為年輕人的電視，可以說是承包了我所有休息時間。而它作為全世界最大的影片平台，從電腦到手機，從網頁到應用，它的播放器也許是最多人使用的存在。

[YouTube]: https://www.youtube.com/

雖然作為播放器，它沒有過多的元素，功能更不複雜，但也確實還是實用的。除掉那些該有的基本操作之外，錦上添花的也有像是進度條預覽的功能。並且作為支援行動裝置的播放器，自然有一些觸手友善的簡單手勢功能。包括經典的縮放，雙擊快進快退，以及大家都歧視[^1]但我就是喜歡的長按兩倍速。

[^1]: 商周 - 網路溫度計 DailyView：[Netflix小編稱1.5倍速追劇，為什麼被炎上？](https://www.businessweekly.com.tw/focus/blog/3018096)

自從 iPhone 定義了滑手機就是搓玻璃的時代，觸控就是行動裝置互動的基本，所以手機上的影片播放器正是為了觸控螢幕而設計。而這些為了觸控螢幕而生的手勢，也讓我感覺到了操作影片播放器的直覺和便利。

但在 Windows 上，作為一個桌面端的作業系統，其平台的影片播放器多是為了桌面而設計，少有支援觸控。 ~~畢竟除了 Surface 也沒幾台 Windows 設備支援觸控螢幕，~~ 更不要說桌面系統支援觸控是千古難題（想看我吐槽可以看看[上篇文章][make_surface_great_again]）。

[make_surface_great_again]: @/posts/make-surface-great-again/Make_Surface_Great_Again.md

電腦是這樣的，手機的影片播放器只要全身心的支援觸控螢幕，拖拉推拽，煎炒烹炸就可以，可電腦要考慮的事情就很多了。

其實網頁版的 YouTube 是支援觸控螢幕的，這讓我在 Surface 有了行動裝置本該有的體驗。但，本地檔案的播放器呢？

Windows 內建的播放器在觸控支援上還是有的，但其功能也就只能說夠用，一些進階的功能是沒有的，例如說直覺的手勢，但對我來說最致命的還是缺少支援一些奇奇怪怪的格式。

而第三方播放器，大家常用的例如 PotPlayer 和 KMPlayer 免費專有軟體，不得不說他們功能確實強大，但大多都帶有廣告，甚至一些還在安裝的時候夾帶私貨。雖說這是賺錢的事兒不寒磣吧，但我既然還有得選，就⋯⋯

自由軟體，常見的 [Codec Guide][] 的 MPC-HC 其實是幾百個世紀前（？）的播放器改版維護至今，雖然介面老派，但功能也絕對不輸，老當益壯說的就是他。

[Codec Guide]: https://codecguide.com/

[VLC][] 也有現代而不當代的介面，不僅支援眾多歪門邪道的編碼，甚至在串流上有所造詣，而且介面也可以安裝外觀造型。但可惜的是，也許是因為他的團隊重心想在編碼和串流這些背後的重要技術，前端播放介面的更新比較慢熱，我非常期待的第四代至今還在難產 :( ，但還是我最常用的播放器。

[VLC]: https://www.videolan.org/vlc/

但是話說回來，他們都不支援觸控耶？直到我再次遇到了 [mpv][]，其實我一直知道這貨的存在，可是講道理，雖然它是強到根本就像轉生異世界開掛龍傲天一般的存在，但那精簡到沒有的介面、指令的操作、手動格式綁定、甚至官方都沒有提供開箱即用的安裝安裝程式（第三方支援），無一不讓我望而卻步。

[mpv]: https://mpv.io/

> 欸欸欸欸欸別走啊！我後面準備好了一份懶人包，一鍵安裝 mpv 以及這次介紹的所有模組。

但要知道，這種使用門檻高到鯉魚躍龍門一般難如登天的軟體，萬一不小心上了手，那可就像成癮一般再也回不去了，頭都不回的那種：像是音樂播放器 [Foobar2000][]，又像是多協定下載器 [Aria2][]，再像是文字編輯器的 [Vim][] 和 [Emacs][] ⋯⋯這都是超硬核的骨灰級玩家才嚐得到的醍醐味。

[Foobar2000]: https://www.foobar2000.org/
[Aria2]: https://aria2.github.io/
[Vim]: https://www.vim.org/
[Emacs]: https://www.gnu.org/s/emacs/

話是那麼說，但 mpv 其實不難理解，他的播放器介面渾然一體，按鈕、進度條漂浮在影片之上，平時隱藏不見，滑鼠移過去就會顯示，稱之 OSC（Over Screen Control），更多高級功能藏在快速鍵中。

可是我為什麼只是想看個影片還要背快速鍵啊，我不知道還要考試，我又沒有唸書！也沒關係，我們可以通過 scripts 對 mpv 做客製化，這正是它的迷人之處。

而 mpv 的 scripts，就像瀏覽器擴充套件、或是遊戲模組，可以理解為超級靈活的設定檔，幾乎讓 mpv 有無限的可能。我們入門玩家也不需要自己寫，社群裡也已經有前輩們準備了。

> 以下都是一鍵安裝腳本包含的內容，如果想要使用腳本，這些都 **不需要** 操作。

我推薦使用 Scoop 軟體套件管理器安裝 mpv，簡單無痛。

- 首先去 [Scoop][] 官方網站，按照指定的步驟安裝 Scoop；
- 到軟體倉庫中找到 [extras/mpv][Scoop/extras/mpv] 並且安裝。

> 注意！有 `mpv` 和 `mpv-git` 前者是穩定版較穩定，後者是開發版較新但可能不穩定。

[Scoop]: https://scoop.sh/
[Scoop/extras/mpv]: https://scoop.sh/#/apps?q=mpv&id=b05b47128464d8969416289383fbfc69a47353e3

如果用 Scoop 安裝，mpv 的設定檔通常放在 `%USERPROFILE%\scoop\apps\mpv\current\portable_config`，設定檔路徑可能隨著按照的方式各有不同。這個設定檔資料夾因為太長下面統稱 `$configDir`，除了 mpv 的設定檔 `mpv.conf` 直接放在這個資料夾下，裡面還有兩個資料夾：

- Scripts 通常是「`.lua` 檔案」放這裡面：`$configDir/scripts`
- Scripts 對應的「`.conf` 設定檔」放這裡面：`$configDir/script-opts`

以上就是初學者所需知道的一切，只需要丟東西到指定地方即可。

為了強迫 mpv 支援更友善的介面和觸控手勢，我們建立一個設定檔 `mpv.conf` 添加兩三個選項。而設定檔的內容只要參照 [官方文檔][] 就有。

[官方文檔]: https://mpv.io/manual/master/

```toml,name=mpv.conf
# 關閉原生的 OSD Bar，uosc 會替代他的功能。
osd-bar=no

# 觸控手勢需要使用邊框和禁止視窗拖動。
border=yes
window-dragging=no
```

> TL;DR: 如果沒禁止「視窗拖動」，拖動視窗的任意部分都會拖動整個視窗，會使得觸控手勢失效，「邊框」是為了在禁止視窗拖動之後還能移動視窗（拖動邊框即可）。

然後給它加裝一些 scripts。都只是按照他們提供的說明，簡單下載指定的檔案放到指定的地方即可。

- [uosc][]（LGPL-2.1）是更友善、現代、介面豐富的 osc 介面；
- [thumbfast][]（MPL-2.0）支援進度條預覽功能；
- [mpv-pointer-event][]（GPL-2.0）和 [mpv-touch-gestures][]（GPL-2.0）支援觸控手勢。

[uosc]: https://github.com/tomasklaen/uosc
[thumbfast]: https://github.com/po5/thumbfast
[mpv-pointer-event]: https://github.com/christoph-heinrich/mpv-pointer-event
[mpv-touch-gestures]: https://github.com/christoph-heinrich/mpv-touch-gestures

最後就可以在 `script-opts` 資料夾，客製化 scripts 的一些設定，他們預設的設定檔裡通常都有詳盡的說明，所以我就不再贅述。

其實比想象的還要簡單得多對吧！但我還是整理了上述所有除了安裝 Scoop 之外的所有步驟，製作成了這個一鍵安裝懶人包。可以把下面的安裝指令貼到 Powershell 中一鍵安裝。

雖說我已經檢查過這個指令的有效性，但我還是希望您在使用的時候能明白每列指令在做啥 :) ，這個指令用 Scoop 安裝 mpv 之後，偵測設定檔資料夾路徑，直接從社群腳本的倉庫下載所需檔案並放到指定位置。

> 時效性：截稿時 2025 年 7 月，安裝指令的內容可能隨時改變和失效。

```powershell,name=touch_mpv.ps1
# This script is licensed under the MIT License, See <https://opensource.org/licenses/MIT>.

# Install mpv and dependencies
Write-Host "Installing mpv and dependents (yt-dlp and ffmpeg) ..."
scoop bucket add extras
scoop install extras/mpv
scoop bucket add main
scoop install main/yt-dlp
scoop install main/ffmpeg
Write-Host "Done"

# Locate mpv installation and config folders
$mpvDir = (scoop prefix mpv).Trim()
$configDir = Join-Path $mpvDir "portable_config"
$scriptsDir = Join-Path $configDir "scripts"
$scriptOptsDir = Join-Path $configDir "script-opts"

Write-Host "Config path: `"$configDir`""

New-Item -ItemType Directory -Force -Path $configDir, $scriptsDir, $scriptOptsDir | Out-Null

# Install uosc
Write-Host "Installing uosc ..."
$uoscZip = Join-Path $env:TEMP "uosc.zip"
Invoke-WebRequest -Uri "https://github.com/tomasklaen/uosc/releases/latest/download/uosc.zip" -OutFile $uoscZip
Expand-Archive -Path $uoscZip -DestinationPath $configDir -Force
Remove-Item -Force $uoscZip
Invoke-WebRequest -Uri "https://github.com/tomasklaen/uosc/releases/latest/download/uosc.conf" -OutFile (Join-Path $scriptOptsDir "uosc.conf")
Write-Host "Done"

# Create mpv.conf
Write-Host "Creating mpv.conf ..."
$mpvConf = Join-Path $configDir "mpv.conf"
if (-not (Test-Path $mpvConf)) {
@"
# uosc provides seeking & volume indicators (via flash-timeline and flash-volume commands)
# if you decide to use them, you don't need osd-bar
osd-bar=no

# uosc will draw its own window controls and border if you disable window border
border=yes
window-dragging=no

# If the UI feels sluggish/slow while playing video, you can remedy this a bit
# video-sync=display-resample

"@ | Set-Content -Encoding UTF8 $mpvConf
}
Write-Host "Done"

# Install thumbfast
Write-Host "Installing thumbfast ..."
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/po5/thumbfast/refs/heads/master/thumbfast.lua" -OutFile (Join-Path $scriptsDir "thumbfast.lua")
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/po5/thumbfast/refs/heads/master/thumbfast.conf" -OutFile (Join-Path $scriptOptsDir "thumbfast.conf")
Write-Host "Done"

# Install pointer-event
Write-Host "Installing pointer-event ..."
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/christoph-heinrich/mpv-pointer-event/refs/heads/master/pointer-event.lua" -OutFile (Join-Path $scriptsDir "pointer-event.lua")
Write-Host "Done"

# Install touch-gestures
Write-Host "Installing touch-gestures ..."
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/christoph-heinrich/mpv-touch-gestures/refs/heads/master/touch-gestures.lua" -OutFile (Join-Path $scriptsDir "touch-gestures.lua")
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/christoph-heinrich/mpv-touch-gestures/refs/heads/master/touch-gestures.conf" -OutFile (Join-Path $scriptOptsDir "touch-gestures.conf")
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/christoph-heinrich/mpv-touch-gestures/refs/heads/master/pointer-event.conf" -OutFile (Join-Path $scriptOptsDir "pointer-event.conf")
Write-Host "Done"

Write-Host "mpv + uosc + gesture environment is ready!" -ForegroundColor Green
```

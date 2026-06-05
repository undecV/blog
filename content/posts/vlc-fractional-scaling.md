---
title: "Windows 下手動修正 VLC 界面縮放"
description: "Fix VLC UI fractional scaling in Windows.<br /><del>は？Windows 竟然支援高解析度螢幕？</del>"
date: 2021-05-10
updated: 2021-05-10
slug: "vlc-fractional-scaling"
taxonomies:
  tags: [zh_TW, VLC, Media Player, Windows, High DPI, Touchscreen, Fractional Scaling]
---

Fix VLC UI fractional scaling in Windows.

> This work © 2017 by undecV is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

高 DPI 螢幕是指是尺寸小並且有者高解析度的螢幕，但是在這高 DPI 的螢幕下，各種按鈕選單就小的就跟他的丁丁一樣，除非上滑鼠，不要說手指，用筆都很難按到。

為了適配最新的螢幕科技，Windows 引入了 Fractional Scaling 縮放功能，讓視窗在高 DPI 螢幕上不會看得太小。

具體設定專案是「設定」->「系統」->「顯示器」->「縮放與版面配置」的「變更文字、應用程式與其他項目的大小」。

但是這就像把一張低畫素圖片放大一樣，一些沒有對此優化過的舊軟體放大後會出現鋸齒、錯位是在所難免的。
原因就是早期的軟體界面設計都是用絕對的畫素作度量。

VLC 的界面勉強可以觸控，VLC 在觸控螢幕下，左右滑可以前後跳轉進度，上下滑可以調節音量，其他動作，甚至播放暫停就只能點按鈕了。
但是 VLC 的界面似乎不會隨著 Windows 的設定縮放，按鈕都太小很難按到。

其他開源的 Windows 播放器，例如 MPC-HC，甚至連 Windows 內建的 UWP 應用程式 竟然都連種陽春的觸屏手勢支援都沒有。
Windows Phone 版的 VLC 看似是一個解決辦法，但是官方說了這不是「完整版的 VLC」，而且上一個 release tag 在一年前。
而，最新還在測試版的 VLC 4.0 將會改頭換面，就一起期待吧。

所以需要就強行讓 VLC 縮放界面（。

Step 1. 創立手稿程式。在你喜歡的地方創立一個 TXT 文字檔案，開啟輸入以下手稿語言指令碼內容：

```batch
set QT_AUTO_SCREEN_SCALE_FACTOR=0
set QT_SCREEN_SCALE_FACTORS=%縮放比例%
START "" "%VLC的安裝路徑%\vlc.exe" %*
```

例如 VLC 預設的安裝路徑，縮放比例是 1.5，內容就是這樣：

```batch
set QT_AUTO_SCREEN_SCALE_FACTOR=0
set QT_SCREEN_SCALE_FACTORS=1.5
START "" "C:\Program Files\VideoLAN\VLC\vlc.exe" %*
```

儲存並且改後綴名為 `.bat`，例如檔名叫做 `VLC.bat`。

Step 2. 測試。這時候你可以，雙擊「開啟」這個手稿程式，或是將影片拖動到這個檔案的圖示上，就可以用縮放後的界面播放影片了。

Step 3. 放置。確認手稿程式沒有 BUG 之後，放到你喜歡的地方，例如 VLC 預設的安裝路徑。

Step 4. 將手稿程式關聯到檔案，以後就可以直接用縮放後的 VLC 開啟影片檔案了。

找到你要看的影片檔案，例如 mp4 檔案，右鍵 Open with -> Choose another app -> 滑到底 More Apps ↓ -> 滑到底 `Look for another app on this PC`，選擇方才放好的手稿程式。

下次遇到其他格式的影片檔案只要在 Open with -> Choose another app 彈出的列表中尋找，若沒有就再重複上述動作。

是不是巨醜（，更新之前就只能將就著用吧。

## Reference

- VideoLAN: [VLC media player continuous nightly builds](https://nightlies.videolan.org/)
- The VideoLAN Forums: [How to Enable hiDPI Scaling?](https://forum.videolan.org/viewtopic.php?t=144476)
- bgstack15.wordpress.com: [Fix vlc ui scaling problems](https://bgstack15.wordpress.com/2019/04/11/fix-vlc-ui-scaling-problems/)
- Unix & Linux Stack Exchange: [How to increase DPI for VLC media player in Cinnamon on UHD display?](https://unix.stackexchange.com/questions/557181/how-to-increase-dpi-for-vlc-media-player-in-cinnamon-on-uhd-display)

---
title: "YouTubeあるある - 瀏覽器多視窗"
description: "<del>DD必知必會101</del>"
date: 2022-05-11
updated: 2022-05-11
slug: "youtube_popup_video"
taxonomies:
  tags: [zh_TW, Firefox, Chrome, Add-on, Extension, YouTube]
---

> <del>DD必知必會101</del>

瀏覽器多視窗是現代瀏覽器的基本技能，只要把標籤頁拖動到隨意位置就可以很容易的創立新視窗，平鋪這些視窗就可以在一個螢幕上同時看多個頁面，這功能讓工作和摸魚都變得高效。瀏覽器完整的視窗有功能豐富的工具列，但是對於一個只播放影片的視窗來說這些工具列就顯得多餘，希望有辦法可以有專門視窗可以關閉工具列，只顯示內容。

Firefox 內建的畫中畫（Picture-in-picture）功能就可以讓影片逃出頁面，成為一個獨立的小視窗，甚至沒有標題欄。在設定中打開選項後，在播放中的影片右邊會出現一個小泡泡，點下去就可以打開畫中畫視窗。這已經可以滿足很多使用情況的需求，但是沒有辦法在這個小視窗中調整音量、畫面、播放速度等選項，還是需要回到原本的頁面。

免費、開源的擴充套件 ***YouTube Popout Player***（[Firefox Add-on][YouTube Popout Player Firefox], [Chrome Extension][YouTube Popout Player Chrome]）可以利用跳出式視窗（pop-up window）實現全視窗的影片播放，相較畫中畫模式，跳出式視窗一樣沒有多餘的工具列，但有一個醜醜的標題欄；利用 YouTube 的嵌入式播放器，調整音量、畫面、播放速度等選項一應俱全。在影片連結上右鍵可以在不進入影片頁面就打開跳出式視窗。啟用擴充套件之後，在影片右下角的全螢幕按鈕的左邊會多出一個按鈕，完全融入了 Youtube 播放器的介面設計。

極少數時候，一些創作者會設定影片禁止使用嵌入式播放器，這個方法就失效了。

免費、開源的擴充套件 ***Popup window***（[Firefox Add-on][Popup window Firefox], [Chrome Extension][Popup window Chrome]）可以讓任何的網頁放入跳出式視窗，與繁體簡體中文轉換工具 新同文堂 師出同門。Twitch 自帶的跳出式視窗會有一個地址欄，但如果使用 Popup window 搭配劇院模式就又可以實現乾淨的視窗。

同個家族的免費、開源的擴充套件 ***Popup Video***（[Firefox Add-on][Popup Video Firefox]）與上面介紹的 YouTube Popout Player 功能相近，但啟用的按鈕沒有融入介面設計，在導覽頁和播放器右上角會有出現一個白色的按鈕。

免費、專有的擴充套件 ***Enhancer for YouTube™***（[Firefox Add-on][Enhancer for YouTube Firefox], [Chrome Extension][Enhancer for YouTube Chrome]） 功能豐富，也帶有跳出式視窗的功能，可以看看專文介紹：[YouTubeあるある - 戲院級的觀影體驗！擴充套件 Enhancer for YouTube™]({filename}/posts/20220510-YouTube_Enhancer_for_YouTube.md)

免費、專有的擴充套件 ***Video Popup Tool***（[Firefox Add-on][Video Popup Tool Firefox]）可以讓其他網站的 HTML5 播放器也能利用跳出式視窗，例如學習教學平臺上的線上課程。

跳出式視窗沒有辦法用滑鼠拖到螢幕邊緣進行 Snap 視窗分割，可以搭配 ***PowerToys***（[官方網站][PowerToys]）的 FancyZones 功能，甚至可以預設好視窗的大小，做到沒有黑邊的 DD。

![大型DD現場](/images/YouTube_Popup_Video/YouTube_Popup_Video.png)

[YouTube Popout Player Firefox]: https://addons.mozilla.org/firefox/addon/youtube-popout-player/
[YouTube Popout Player Chrome]: https://chrome.google.com/webstore/detail/youtube-popout-player/kmfikkopdhmbdbkndkamabamlkkgkpod
[Popup window Firefox]: https://addons.mozilla.org/firefox/addon/popup-window/
[Popup window Chrome]: https://chrome.google.com/webstore/detail/popup-window/nnlippelgfbglbhiccffmnmlnhmbjjpe
[Popup Video Firefox]: https://addons.mozilla.org/firefox/addon/popup-video-webextension/
[Enhancer for YouTube Firefox]: https://addons.mozilla.org/firefox/addon/enhancer-for-youtube/
[Enhancer for YouTube Chrome]: https://chrome.google.com/webstore/detail/enhancer-for-youtube/ponfpcnoihfmfllpaingbgckeeldkhle
[Video Popup Tool Firefox]: https://addons.mozilla.org/firefox/addon/popup-tool/
[PowerToys]: https://docs.microsoft.com/zh-tw/windows/powertoys/install

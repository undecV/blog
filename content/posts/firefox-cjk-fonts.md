---
title: "如何讓 Firefox 不要再用新細明體"
description: "令和 8 年了，Firefox 的中文字型還是有點懷舊；本文記錄如何在 Windows 上用 Noto CJK 和 user.js 修改 Firefox 的中文預設字型。"
date: 2026-06-05
updated: 2026-06-05
slug: "firefox-cjk-fonts"
taxonomies:
  tags: [zh_TW, Firefox, Font, Web]
---

<del>沒錯，在下正是高傲的 Firefox 自適應用戶。</del>

> ~~*此處應有新細明體笑話。*~~

2026 年了，Firefox 還是把中文字型 fallback 到新細明體……

Windows 的中文字型支援真的是夠用而已，Windows 11 的現況是，無襯線（Sans-serif）的黑體字是適合現代螢幕的「微軟正黑」，但是襯線（Serif）的明體字還是「新細明體」。我們暫且不說設計語言一致性這種高貴的事情，這兩個字型就是排版在一起都能讓人感受到時代的碰撞。

但這也怪不得誰，畢竟 CJK 字型要處理的漢字量動輒成千上萬，還有不同書寫系統的字形差異，所以現代、完整、好看的中文字型選擇少之又少，這是電腦科學界的千古難題……直到 Google 和 Adobe 推出了 Noto CJK / Source Han 系列。

Windows 10 / 11 在 2025 年也內建了 Noto Sans / Serif TC 等 CJK 區域字型 [^1]，這可真是德政！Chrome 甚至搶先一步調整了預設 fallback：如果系統上有 Noto CJK / Noto TC 這類字型，就優先使用它們 [^2]，真不愧是親爹。但 Firefox 卻……是相當的保守了。所以我們需要修改 Firefox 的設定，讓 Firefox 用上 Noto 字型。

但在修改之前，我們不得不提及一下 Noto CJK 系列字型的基本知識。Noto CJK 分為兩個家族的三套字型：

- `Noto Sans CJK`: 無襯線的黑體字；
- `Noto Sans Mono CJK`: 無襯線的黑體字，但是等寬；
- `Noto Serif CJK`: 有襯線的明體字。

按照發行方式，以正體中文的 `TC` 為例，分為：

- 通常版：`Noto Sans CJK TC`；
- 區域限定子集（region-specific subset）：`Noto Sans TC`。

區域限定子集少了 `CJK` 標記。「區域限定子集」檔案只包含特定區域常用的字形與字元集合，因此檔案較小，系統載入和瀏覽器處理上也比較輕量。Mono 等寬字型 **沒有** 區域限定子集。

Windows 秉持著「做事只做半套」的核心理念，內建的主要是「區域限定子集」，所以若我們希望使用 Noto Sans Mono CJK 等寬字型，還得自己再安裝一下「通常版」。

> 如果沒有打算修改 Mono 字型，可以跳過這個步驟。

安裝的方式我推薦使用 [Scoop](https://scoop.sh/) 套件管理器，雖然是命令列，但是最簡單。

首先按照官方說明安裝好 Scoop，然後打開命令列輸入：

```powershell
# 添加 `nerd-fonts` 倉庫，但裡面不止有 Nerd fonts
scoop bucket add nerd-fonts
# 安裝 Noto Sans CJK，包括 Mono
scoop install nerd-fonts/Noto-CJK-Mega-OTC
# 安裝 Noto Serif CJK
scoop install nerd-fonts/Noto-Serif-CJK-Super-OTC
```

這樣字型就安裝好了。通常安裝字型不需要重開電腦，只需要重開要用到新字型的軟體即可。

接下來是修改 Firefox，正常的修改方式是 `Settings` → `Fonts` → `Advanced...` 裡面修改。
但是非常麻煩，不僅步驟繁雜，如果你又常常在大東亞共榮圈的各個網站裡面反覆橫跳，要改的就不是一個語系，而是五個書寫系統。

所以我們用特殊的方法修改，一次設定五個書寫系統。

> 警告：修改 `about:config` 有風險，請在「安全、理智、自願」的基礎上進行。

我們在 Firefox 的 Profile 資料夾下建立 `user.js` 檔案，這個檔案會在 Firefox 每次啟動的時候「注入」設定。

> 這裡的「注入」比較像「每次啟動時把設定寫進 Firefox 的偏好設定資料庫」。所以 `user.js` 刪掉之後，已經寫進去的設定不會自動還原；只是 Firefox 之後不會再被 `user.js` 強制覆寫。不刪的話，每次 Firefox 啟動都會「注入」一次，讓設定與 `user.js` 檔案內相同。

<details open>
<summary>如果安裝了 Mono 字型的設定</summary>

```javascript,name=user.js
user_pref("font.name.sans-serif.zh-TW", "Noto Sans TC");
user_pref("font.name.serif.zh-TW", "Noto Serif TC");
user_pref("font.name.monospace.zh-TW", "Noto Sans Mono CJK TC");

user_pref("font.name.sans-serif.zh-HK", "Noto Sans HK");
user_pref("font.name.serif.zh-HK", "Noto Serif HK");
user_pref("font.name.monospace.zh-HK", "Noto Sans Mono CJK HK");

user_pref("font.name.sans-serif.zh-CN", "Noto Sans SC");
user_pref("font.name.serif.zh-CN", "Noto Serif SC");
user_pref("font.name.monospace.zh-CN", "Noto Sans Mono CJK SC");

user_pref("font.name.sans-serif.ja", "Noto Sans JP");
user_pref("font.name.serif.ja", "Noto Serif JP");
user_pref("font.name.monospace.ja", "Noto Sans Mono CJK JP");

user_pref("font.name.sans-serif.ko", "Noto Sans KR");
user_pref("font.name.serif.ko", "Noto Serif KR");
user_pref("font.name.monospace.ko", "Noto Sans Mono CJK KR");
```

</details>

<details>
<summary>如果沒有打算修改 Mono 字型，僅使用預裝的字型的設定</summary>

```javascript,name=user.js
user_pref("font.name.sans-serif.zh-TW", "Noto Sans TC");
user_pref("font.name.serif.zh-TW", "Noto Serif TC");

user_pref("font.name.sans-serif.zh-HK", "Noto Sans HK");
user_pref("font.name.serif.zh-HK", "Noto Serif HK");

user_pref("font.name.sans-serif.zh-CN", "Noto Sans SC");
user_pref("font.name.serif.zh-CN", "Noto Serif SC");

user_pref("font.name.sans-serif.ja", "Noto Sans JP");
user_pref("font.name.serif.ja", "Noto Serif JP");

user_pref("font.name.sans-serif.ko", "Noto Sans KR");
user_pref("font.name.serif.ko", "Noto Serif KR");
```

</details>

修改之後重開 Firefox，如果系統中確實有安裝字型，那麼應該就可以看到了。

雖然但是，為什麼 Firefox 看上去（雙關）還是如此守舊？由於我是高傲的 Firefox 自適應用戶，我願意先幫它找理由：瀏覽器的字型選擇不只是美觀問題，還牽涉到歷史包袱、平台 fallback、語言標籤、字型可見性，以及隱私。

由於 HTML / CSS 的設計，網站可以透過「你有哪些字型」、「某段文字渲染後尺寸是多少」來推測和識別使用者，這就是隱私裡說的「指紋」[^3]。在較嚴格的隱私模式下，Firefox 會限制網站可見或可用的字型範圍，以降低字型指紋追蹤的風險。不過 Firefox 也知道完全擋掉語言相關字型會直接傷害可讀性，所以對中文、日文、韓文等語言字型仍有例外處理 [^4]。

根據我的觀測結果（2026-06），Chrome 除了預設就會優先使用 Noto 之外，對於等寬字即使沒有 Noto CJK Mono，也會做 CJK fallback；Firefox 則需要我們自己幫它一把。

[^1]: [Wikipedia: Microsoft Windows字型列表](https://zh.wikipedia.org/zh-tw/Microsoft_Windows%E5%AD%97%E5%9E%8B%E5%88%97%E8%A1%A8)
[^2]: [PSA: Windows default CJK fonts changes](https://groups.google.com/a/chromium.org/g/blink-dev/c/t1Mc7oJdNQY)
[^3]: [Firefox Browser Fingerprinting Protection](https://www.firefox.com/en-US/features/block-fingerprinting/)
[^4]: [Firefox's protection against fingerprinting](https://support.mozilla.org/en-US/kb/firefox-protection-against-fingerprinting)

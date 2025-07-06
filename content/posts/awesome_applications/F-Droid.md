---
title: "Android 生態的平行宇宙：F-Droid"
description: |
  有些 App 超好用，但在 Google Play 根本找不到。
  無論是強大還是小巧、實用甚至免費、無廣告的應用都在這裡。
  來看看我介紹開源應用和他們的產地。
date: 2025-07-06
updated: 2025-07-06
slug: "fdroid"
taxonomies:
  tags: [zh_TW, Awesome list, Android, Application, FOSS, F-Droid]
---

無論是在 Android 還是 iOS 上，「應用市場」絕對是手機上最重要的應用沒有之一，應用市場可以說本身就是一個生態系統的代表。

除去薛錢的部分，以應用安裝的角度來說，應用市場的本質其實就像電腦上的「軟體套件管理器（Package Manager）」：諸如 Red Hat 上的 RPM、或是 Debian 上的 APT、又或是 Windows 的 Winget（[專文介紹][Post_Windows_Package_Managers]），是極為便利的存在，讓人安裝軟體不再需要在網頁搜尋裡眾裡尋她千百度，可以直接在這個管理器中統一的安裝和升級。

[Post_Windows_Package_Managers]: @/posts/command_line_useages/Windows_Package_Managers.md

對於搓玻璃多過滾鍵盤的新生代，安裝應用程式本就應該只是在應用市場點一下安裝那麼簡單。想當年，在電腦上安裝軟體，首先需要用搜尋引擎找到軟體官方網站，再在裡面找到對應電腦版本安裝包，再下載下來安裝，然後一直點下一步的時候，還要防範藏在裡面的捆綁軟體陷阱⋯⋯那種野性而純樸的年代，大概很難想像和理解了吧。

不像在電腦上為所欲為的開放環境，手機的生態卻是封閉、受控，更是惡劣得多。在手機上，內建的官方應用市場往往是安裝手機應用唯一的途徑。這一是為了安全，但實則是為了壟斷生態。

而「側載（Sideloading）」，通常是指在應用市場之外下載和安裝應用程式。好處當然就是帶來了自由奔放的環境。而壞處就是給了壞壞程式可乘之隙。

不像 [Altstore][] 之於 [Apple App Store][]，限制之多，不僅是歐盟地區限定，還有應用簽名之類的麻煩事，搞得像是黑市交易一樣遮遮掩掩，可見 Apple 是根本就沒有誠心開放側載的，即使在歐盟多管閒事的法律壓力下，仍是處處設限。

[Altstore]: https://altstore.io/
[Apple App Store]: https://www.apple.com/app-store/

而在那個很強大的國家，某些魔改的「安卓」，因為一些大人的原因，雖然不是徹底封死了側載的門路，但也是用各種幌子百般阻撓。假借安全之名，拼命的想要排異側載的應用。

但真正的 Android 上就沒有這麼多無理的限制，[Google Play][] 不是唯一的安裝渠道，側載應用非常容易，系統也提供了相應的權限、來源控制等防呆措施，一定程度上在安全和自由上做到了平衡。

[Google Play]: https://play.google.com/

而無論是 iOS 的 App Store 還是 Google Play，眾所周知是需要繳納入場費和抽成税的。這一是支付平台託管和審查費用，另一個理由當然就是這些資本機器需要賺錢。這時一定會有一些愛好自由的人，不願把自己的心血送進資本機器被壓榨。

所以，我們需要一個開放自由的應用市場，這正是「[F-Droid][]」，一個 Android 生態的平行宇宙！

[F-Droid]: https://f-droid.org/

F-Droid 裡面收容了很多很多優秀的自由和開源（FOSS）應用程式。不僅大多應用都是免費的，有些甚至不輸 Google Play 中的付費應用。

而且 F-Droid 不只是一個「應用市場」，更是一個開放的「應用套件管理系統」，雖然因此看起來較難理解，其實比想像的簡單得多，也就兩個部分：

一是「前端（Client）」，即是我們所看到的「應用市場」介面：從眾多「倉庫」中，瀏覽、安裝和管理其中的應用。

二是「倉庫（Repository）」，用於收容應用程式的資訊和安裝檔。

對於一般用戶來說，也就只要安裝一個前端，例如官方的前端，然後直接使用內建的倉庫，或再導入其他的倉庫，就可以像其他所熟知的應用市場一樣用了。

真正決定內容的是「倉庫」，倉庫是由社群用愛發電維護的，會收容怎樣的應用也是各有各的風格和標準。最有名的倉庫理所當然正是「F-Droid 官方倉庫」，但它遵循嚴格的開源原教旨主義（就像 Linux 世界的 Debian），比較要求開源的根正苗紅（完全純粹，不得含有任何非開源程式碼）。所以我們還有更加開放一些的知名第三方倉庫「[IzzyOnDroid][]」。甚至，一些應用程式因為各種理由，自己維護一個倉庫例如「Breezy Weather」。

[IzzyOnDroid]: https://apt.izzysoft.de/fdroid/

> 當然，倉庫是否可靠，安不安全，還得靠自己的慧眼判斷！

在前端中添加倉庫也非常的簡單，只要把倉庫的連結貼到前端相應的設定中。或者一些前端有提供掃描二維碼的功能，掃描倉庫提供的二維碼也是更便利的方法。

那講到前端，由於「F-Droid」官方前端可能是為了相容性或是什麼原因，介面有些返祖，但確是我用過好用的。但也可以選擇其他的前端，最有名的可能是「[Droid-ify][]」和「[Neo Store][]」，它們提供了更現代的介面和更多功能。

[Droid-ify]: https://droidify.eu.org/
[Neo Store]: https://github.com/NeoApplications/Neo-Store

Droid-ify 以簡單易用的設計和輕量化特性著稱，而 Neo Store 則以現代化的用戶介面和高自定義性吸引使用者。相較於官方 F-Droid 客戶端，他們的介面應該比較容易被接受，並且它們內建了更多知名的非官方倉庫這點尤為便利。它們之中我更推薦 Droid-ify，因為支援正體中文。

而為什麼我說 F-Droid 是 Android 生態的平行宇宙？因為一些極為優秀的應用只通過 F-Droid 發布，並且還沒有在 Google Play 裡面上架。
包括 [專文介紹][Post_Breezy_Weather] 過的「[Breezy Weather][]」，強大的終端機模擬器「[Termux][]」、在分享照片時過濾掉隱私資訊的「[Imagepipe][]」、還有臨時讓螢幕長亮一段時間的「[Coffee][]」⋯⋯ 這些無論是強大還是小巧、實用甚至免費、無廣告的應用讓我根本欲罷不能，所以我說在手機裡面裝一個 F-Droid 絕對是穩賺不賠，值得一試。

[Post_Breezy_Weather]: @/posts/awesome_applications/Breezy_Weather.md
[Breezy Weather]: https://github.com/breezy-weather/breezy-weather
[Termux]: https://termux.com/
[Imagepipe]: https://codeberg.org/Starfish/Imagepipe
[Coffee]: https://github.com/mueller-ma/Coffee

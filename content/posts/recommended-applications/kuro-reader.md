---
title: "Android 上的本地漫畫閱讀器推薦：Kuro Reader"
description: "電腦裡面開了一家漫畫圖書館，怎麼辦？急，在線等。這裏有一款介面現代且流暢、支援壓縮檔、可以直接讀取本地網路的無廣告漫畫閱讀器。"
date: 2025-06-30T15:00:00
updated: 2025-06-30T15:00:00
slug: "kuro-reader"
taxonomies:
  tags: [zh_TW, Awesome list, Android, Application, Comic Reader, SMB, CIFS]
---

專有・免費增值・[Homepage](https://kurotoshiro.dev/)・[Google Play](https://play.google.com/store/apps/details?id=br.com.kurotoshiro.kuro_reader)

電腦裡面開了一家漫畫~~和同人誌~~圖書館，怎麼辦？急，在線等。

相比直接在電腦上面看，不受桌椅束縛的在手機、平板上面看漫畫可還是比在電腦上優雅多了。

「漫畫」相較「電子書」來說其實更為複雜，
電子書已經有了 EPUB / PDF 等檔案解決方案，甚至分發都有 OPDS 這種開放標準系統。
但漫畫閱讀器考慮的事情就很多了，漫畫的包裝方式例如 CBZ 其實就是一拖拉庫的圖檔塞進 ZIP 壓縮檔。
而且漫畫閱讀器對圖檔的渲染也需要優化，所以漫畫閱讀器還是和電子書閱讀器有所差別的。

這裡撇開那些大平臺自有的閱讀器不談，
Android 上的漫畫閱讀器，我粗略地分成「線上」和「本地」漫畫閱讀器：
「線上」漫畫閱讀器，即是即時加載網路上的資源，例如 [Mihon](https://mihon.app/)（前 Tachiyomi）及其變種；
「本地」漫畫閱讀器，則是閱讀在本機或內部網路的資源。

但與其把漫畫拷貝到手機上，手機上可憐的內部存儲容量不說，不如在本地網路直接訪問電腦來的方便。
那現在有兩個選擇：一個是「架設網路服務」，通過 [Komga](https://komga.org/) 等服務架設漫畫伺服器，再用 Mihon 閱讀。
雖然體驗是極好的，但這還是太麻煩了，不僅要維護伺服器服務，Komga 對資料夾的結構還有要求，而且掃描、建立索引也要一段時間。

另一個方式就是通過本地網路，例如 SMB / CIFS 或 NFS、FTP、SFTP 等協定，直接讀取電腦上的資料夾和檔案。
雖然沒有那麼優雅，但卻是快捷便利的選擇。只要在電腦上，通過網路芳鄰開啟分享存放漫畫的資料夾即可。

所以我對於「本地」漫畫閱讀器的要求是：

- 好看、易用、流暢使用；
- 活躍或穩定更新；
- 能讀 CBZ / ZIP 之類的壓縮檔案；
- 本地網路 SMB / CIFS 等協定支援。

既然是漫畫閱讀器，能讀壓縮檔案是基本了，但這也排除了很多電子書閱讀器。

[Seeneva](https://seeneva.app/) 是一個好看好用的本地閱讀器，但不支援本地網路的協定。
我曾經試過用 [CIFS Documents Provider](https://github.com/wa2c/cifs-documents-provider) 將本地網路的資料夾掛載，
再用 Seeneva 讀取，但效果不太理想，還是只用在手機裡面的檔案最好。

其實以前一直在使用的 [Perfect Viewer](https://play.google.com/store/apps/details?id=com.rookiestudio.perfectviewer)，
雖說其介面有點返祖，卻還是非常好用的。
但它的問題是（也有可能是我的問題），它的 SMB 連線極不穩定，經常在加載的時候整個程式當機，或者中間有一兩頁顯示不出來。
我 *合理懷疑* 它可能用的是遠古時代的 SMBv1 客戶端。

還有 [CDisplayEx](https://play.google.com/store/apps/details?id=com.progdigy.cdisplay.free)
雖然 Android App 的體驗不錯，但從社群討論可見它有著流氓軟體的傳言，讓我望而卻步，而且（好像）也有同樣的連線問題。

直到我搜尋到了 [Kuro Reader](https://kurotoshiro.dev/)，
它有著優雅的、現代的、可以靈活客製化的用戶介面設計，
先進的圖書館管理系統（書架），分類、標註、收藏等功能（但對我而言可能較少用到）。

它看上去對 SMB 有做特別的優化，我 *合理懷疑* 它支援 SMBv2+ 的現代協定。
不像 Perfect Viewer 是邊看邊下載，它是先整個檔案抓下來再開啟。
雖然這在打開的時候需要等待，但是因為多線程的加持，其實下載速度感覺還是蠻快的。（雖然我還是希望邊看邊下載功能未來會實現。）
但我使用的時間還不長，看上去是穩定的，尚未發現連線問題。

（截稿時）就連免費版本都 **沒有廣告**！也就只是鎖定了 暗黑模式 和 Google Drive 同步這些非核心功能。
付費版本也只要 NT$ 100，可以說是良心到根本就是 shut up and take my money 了。
對於希望輕量、無後端、能直接走本地網路讀取漫畫的人來說，Kuro Reader 是目前我找到最稱職的選擇。

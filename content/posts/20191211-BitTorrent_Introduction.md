---
title: "BitTorrent 從入門到出門"
description: "BitTorrent Introduction"
date: 2019-12-11
updated: 2019-12-11
slug: "bittorrent_intro"
taxonomies:
  tags: [zh_TW, BitTorrent, Download, File Sharing, Tutorial, Computer Concepts]
---

> Copyright © 2017 undecV.<br />
> [![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)<br />
> This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

**BitTorrent 協定** 俗稱 `BT下載`、`種子下載`，常見的形態還有 `Magnet（磁力鏈接）`，是現代主流的網路檔案分享方式之一。

## 概論

Bit 譯作「比特」是電腦數據的基本單位，Torrent 意爲「激流」。BitTorrent 通常可以指本文所述的 **開放的 BitTorrent 協定** 或是 BitTorrent 官方客戶端，以及 BitTorrent 公司。

相比於 BitTorrent，傳統的中心化檔案下載模式，通常是一個伺服器（發布者）分發給所有客戶端（下載者），客戶端越多，下載速率被平分，也就越慢；一旦伺服器端故障，檔案便不再可下載。

而 BitTorrent 是一種基於端對端（Peer-to-Peer, P2P）的傳輸協定。不同於傳統的下載方式，BitTorrent 同時下載的客戶端越多，速度越快；並且在特定情況下，檔案發布者退出也不會導致檔案無法下載。

這麼神奇的功效得益於「我為人人，人人為我」的精神：每個 **下載者** 同時也是 **分享者**，不僅 **發布者** 會傳給下載者，每個下載者將自己已經取得的部分「分享」給還未取得這部分的其他下載者，其他下載者也他們已經載好但是你還沒有的部分「分享」給你。這樣的傳輸方式可以加快傳輸的速度、減輕發布者伺服器負擔，甚至當下載者們有了檔案的每個部分，發布者退出網路也不會影響檔案的分享。

當你想要使用 BitTorrent 下載檔案時，你會先取得一個 **種子文件**，裏頭記載着 **追蹤伺服器** 以及檔案的資訊，追蹤伺服器根據這些檔案的資訊帶你找到發佈者、分享者、下載者這些 Friends。其中，**DHT 網路** 可以當成是由參與的用戶組成的分散式追蹤伺服器。

也可以使用 **磁力鏈接**，看上去就像一個特殊的網址，它包含了你要載的檔案的資訊。如果沒有或是你不想用種子文件，客戶端可以直接根據磁力鏈接找到你要的檔案和 Friends。

### 角色

- **檔案（File）** 被分割成一個個小 **塊（Pieces）**，檔案資料以 塊 為單元被分享。
- **下載者** 或稱之 **節點（Peers）** 是想要檔案的人。
- **分享者** 或稱之 **種子（Seeds）** 是任何分享檔案的人。不只是 **發布者**，當下載者開始分享它已經取得的部分，他們也會變成種子。
- **客戶端（Client）** 也就是你執行 BitTorrent 下載的軟體。
- **種子文件（Torrent file, `*.torrent`）** 記載了追蹤伺服器以及你要載的一個或一堆檔案的資訊。
- **磁力鏈接（Magnet）** 像特殊的網址，它包含了你要載的檔案的資訊。
- **追蹤伺服器（Tracker）** 根據你提供的種子文件裡面的檔案資訊，帶你找到 Friends。
- **DHT** 譯作 **分散式哈希表** 可以想象成是特殊的分散式的追蹤伺服器。它由參與的用戶組成網路，將資訊以分布的方式存儲在每個客戶端之中。

### 種子與可用性

BitTorrent 檔案的可見性來自種子們的分享，所以當檔案的種子們 **沒有完整的檔案** 又甚至根本沒有種子了，下載就會 **無法完成**，直到有更多片段或完整檔案的種子們再次上線。

打開 **使用 DHT 網路** 選項 或 **手動添加追蹤伺服器**（後文會提到）可能可以搶救一下。

客戶端中的詳細任務訊息會給出當前分享網路是否有完整的檔案的 **可用性（Availability）**。

所以，BitTorrent 的檔案下載具有時效性，走過路過不要錯過，也請記得分享：）

### 吸血

BitTorrent 的活力來源於「分享」的精神，只下載不分享的「吸血」行爲招人唾棄，會嚴重破壞 BitTorrent 的社會環境。
也請不要使用諸如 *迅雷* 等吸血下載工具，現在的主流下載客戶端會對吸血行爲有保護機制。
一些追蹤伺服器也許會追蹤分享率和分享時間。

## 操作

實際操作比上面的理論（？簡單多了，就像傳統下載，將下載地址導入到下載工具（客戶端），只是導入的東西變成了 **種子文件** 或 **磁力鏈接**。

### 客戶端

一些 BitTorrent 專用客戶端，例如：

- [qBittorrent](https://www.qbittorrent.org/): `自由軟體` 第三方客戶端，可訂製的種子搜尋功能。

> <p style="color:crimson;font-weight:900;text-align:center;text-transform:uppercase;">請注意！ - Warning</p>
> 據 ghacks.net 悉<sup>[uT:1]</sup>（Dec 9, 2019），uTorrent 官方客戶端被多家包括 ESET-NOD32 以及 Microsoft 在內的防毒軟體標記爲惡意軟體（Malware）<sup>[uT:2]</sup>。

以及一些多協定的下載工具，例如：

- [aria2](https://aria2.github.io/): `自由軟體` 多協議命令行下載工具，功能強大需要調教。

### 設定客戶端

BitTorrent 客戶端就像一般下載工具，除了那些都有的例如代理、下載速度限制、最大活躍下載任務、下載位置等設定，例舉一些 BitTorrent 特有的設定：

- 上傳速度限制：分享的速度限制；
- 分享率：當分享的資料量達到檔案的 N 倍後結束任務；
- 分享時間：當分享的時間超過 T 時間後結束任務；
- 啓用 DHT 網路；
- ...

### 手動添加追蹤伺服器（可選步驟）

除了種子文件中提供的追蹤伺服器，你也可以將追蹤伺服器預置在客戶端中。

- Github · [ngosang/trackerslist](https://github.com/ngosang/trackerslist): Updated list of public BitTorrent trackers（**⚠ 未測試**）

### 來源

一些自由軟體爲了減輕其分發的成本，會使用 BitTorrent 做爲取得的方式。
也有一些種子文件的彙集網站。

你會取得一個 `*.torrent` 的 **種子文件**，或是在 `🧲` 磁鐵圖標 附近找到像這樣的 **磁力鏈接**：

```URI
magnet:?xt=urn:btih:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

將他們導入到你選擇的客戶端中就可以開始下載。

## 溫馨提示

### 流量警告

通常的，運營商統計的數據流量包括了下載和上傳（分享）的流量。在使用計量型方案請謹慎使用。

### 身份暴露警告

BitTorrent 的設計沒有考量匿名性，當你下載任何檔案的時候，你此時的 IP 地址 會被所有參與下載的 Friends 看到。

### 海賊版警告

去中心化的設計 BitTorrent 可以減輕發布者的負擔，但是也爲海賊版提供了平臺。

**支持和購買正版才能夠讓美好的創作永續！**

### 封鎖警告

由於以上全部或部分的問題，一些網絡會封鎖＼禁止 BitTorrent 協定，例如學術網路或公司企業網路。

## REFERENCE

- Wikipedia: [BitTorrent](https://en.wikipedia.org/wiki/BitTorrent_(protocol))
- Wikipedia: [Magnet URI scheme](https://en.wikipedia.org/wiki/Magnet_URI_scheme)
- Quora: [Stanislav Shalunov's answer to How do BitTorrent magnet links work?](https://www.quora.com/How-do-BitTorrent-magnet-links-work/answer/Stanislav-Shalunov)
- Stack overflow: [What exactly is the info_Hash in a torrent file](https://stackoverflow.com/a/28601408)

---
title: "Raspberry Pi 4B 搭建 NAS 第一話：外接硬碟和 USB 篇"
description: "Raspberry Pi 4B NAS Episode 1 (HDD & USB)<br /><del>爬文是不可能爬文的，这辈子不可能爬文的。Spec 又不會查，Run 不起來纔是正常的樣子。</del>"
date: 2020-04-03 00:00:00
updated: 2020-04-03 00:00:00
slug: "rpi4b_nas_ep1"
taxonomies:
  tags: [zh_TW, Raspberry Pi, NAS, HDD, USB, USB HUB]
---

<del>爬文是不可能爬文的，这辈子不可能爬文的。Spec 又不會查，Run 不起來纔是正常的樣子。</del>

原本想拿 rPi4B 做 NAS 的，想當然耳，結果翻車了。

通常使用 USB 的設備吃電都是不多的，例如滑鼠、鍵盤、隨身碟等，但是使用機械行動硬碟的時候可能就需要考慮輸出功率的因素了。
機械硬碟吃功率主要看：

- 冷啓動時的峯值功率 (Peak)
- 讀寫運作中的功率（r/w）
- 沒有讀寫的閒置功率（Idle）
- 待機功率（Standby and Sleep）

現代的機械硬碟長時間沒有讀寫時會（停轉磁碟）待機以節省功率，這時候讀資料需要把磁碟重新轉起來，這也是爲什麼有時候從抓資料的時候硬碟會「當掉幾秒」的原因。

根據[^1]（WD 紅碟的 Spec）[^2]（Toshiba 行動硬碟的 Spec）[^3]：

- 壹個典型的 3.5吋 內接式硬碟吃的是 12V 供電，峯值在 1.75A (21W)、讀寫大約 4.8W、閒置 3.1W，待機 0.6W；
- 壹個典型的 2.5吋 內接式硬碟吃的是 5V 供電，峯值在 1A (5W)、讀寫大約 1.4W、閒置 0.6W，待機 0.2W；
- 壹個典型的 2.5吋 行動硬碟，吃的是 USB 供電，最多 0.9A。

具體請查 Spec，可以看出機械硬碟並不是省電的碟。
不是給手機充電（使用充電協定）的情況下，通常 USB 標準給外接裝置供電都是 5V，USB 2.0 最大 0.5A、USB 3.0 最大 0.9A [^4]，而 USB3.1 的 Type-C 有支援最大 3A。

對於真正的 NAS，功率必然是需要考慮的問題，在硬碟、BIOS 和 驅動程式支援的情況下，Staggered Spin-up (SSU) 可以讓硬碟陣列輪流轉以控制使用中的功率 [^7]；Power-Up In Standby (PUIS) 讓硬碟冷啟動時直接進入待機狀態而避免過大的峰值功率 [^8]。

而像 rPi4B 這種本身功率就不高，只有 USB-C 3A@5V 輸入，但卻有 2x2.0 2x3.0，若按照標準算全滿則是 2.8A，那板板本身就只能用愛發電了。所以就有了最大輸出的概念，<del>就像跟玩文字遊戲的手機充電器一樣，~</del>rPi4B 的 USB 電流最大總和只有 1.2A [^5]。也就是說，插好插滿最多只能輸出 6W，超出限制的時候 rPi4B 會保護性關機。

所以 rPi4B 本身是只能插一個行動硬碟的。我直接插上了兩個硬碟之後 rPi4B 自動重開了，然後聽到硬碟在唱跳 Rap，思考了一下人生之後趕緊給關了機。

那麼怎麼實現 rPi4B 做 NAS 呢，可以使用帶外接電源的 USB HUB [^6]。不帶電源的 HUB 當然用電都是從那唯一壹個 USB 接頭輸出，大約在 0.5A 甚至 0.1A，可以帶的裝置也就有限。[^4] 而帶電源的也需要看電源的功率，現在常見一種多一個 USB Micro-B 供電介面的小型 HUB，通常也只能從手機充電器供給 2A@5V (10W) 的功率（請查 Spec），仍然不足每個介面都插上行動硬碟。而一些附電源的 HUB 則需要根據提供的電源功率來試算。

*[rPi]: Raspberry Pi
*[rPi4B]: Raspberry Pi 4B

[^1]: [WD Red NAS Hard Drives (PDF)](http://products.wdc.com/library/SpecSheet/ENG/2879-800002.pdf)
[^2]: [Toshiba Canvio Basics](https://www.toshiba-storage.com/products/toshiba-portable-hard-drives-canvio-basics-2/)
[^3]: Super User: [How much power does a hard drive use?](https://superuser.com/questions/565653/how-much-power-does-a-hard-drive-use)
[^4]: Wikipedia: [USB](https://en.wikipedia.org/wiki/USB)
[^5]: Raspberry Pi Documentation: [FAQs](https://www.raspberrypi.org/documentation/faqs/#pi-power)
[^6]: Gary Explains @ YouTube: [Build a Raspberry Pi NAS with 4 Hard Drives and RAID](https://youtu.be/O-FfOWdZAQ4)
[^7]: Wikipedia: [Spin-up](https://en.wikipedia.org/wiki/Spin-up)
[^8]: Wikipedia: [Power-up in standby](https://en.wikipedia.org/wiki/Power-up_in_standby)

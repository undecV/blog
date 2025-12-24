---
title: "Raspberry Pi 4B 搭建 NAS 第二話：網路篇"
description: "Raspberry Pi 4B NAS Episode 2 (Network)<br /><del>究竟讀寫速度會卡在 USB 還是 SMB 呢？rPi4B NAS 翻車最終話！。</del>"
date: 2020-04-03 01:00:00
updated: 2020-04-03 01:00:00
slug: "rpi4b-nas-ep2"
taxonomies:
  tags: [zh_TW, Raspberry Pi, NAS, HDD, Ethernet, Wi-Fi]
---

<del>究竟讀寫速度會卡在 USB 還是 SMB 呢？rPi4B NAS 翻車最終話！\滑稽。</del>

供電的問題解決了，SMB 網路芳鄰也成功搭建了起來，那接下來就是喜聞樂見的測速時間了。

注意，以下使用的單位，小寫 `b` 是 `bits`，大寫 `B` 是 `Bytes`，`8 bit = 1 Byte`，全部使用十進位的 `1 MB = 1,000,000 Bytes`。

依據 [^1]（WD 紅碟的 Spec），壹個典型的 2.5吋、5400RPM 的硬碟，144MB/s，根據可愛的 Shizuku [^2] 測試我的行動硬碟，結果差不多。

USB 3.0 的理論速度是 5Gb/s，大約是 625MB/s [^4]；根據 rPi 官方的說法 [^5] 和基準測試 [^6] 有約 343MB/s，所以 USB 在機械硬碟的速度下不會成為瓶頸。

同樣的，根據 Spec 和官方的測速 [^6]，Ethernet 可以有幾乎真正的 GbE 的 1Gb/s 的速度，當然要達到真正的 GbE 需要交換機、路由器、網路線、以及其他終端裝置支援。指令 ethtool eth0 可以檢視當前的連線頻寬（理想速度），然而，即使是按照理想的 1Gb/s 的速度計算，GbE 也僅有 125Mb/s。
在全程使用 GbE 的情景下，這速度會是瓶頸。

而 Wi-Fi 就更慘了，還是根據官方測速 [^6]，在 5GHz 之下僅有 114Mb/s（約 14MB/s）的實際速度。指令 iwconfig 可以檢視當前的連線頻寬（理想速度）。沒錯，明明是 Wi-Fi 5 卻連 4 都不如\滑稽。
在 rPi4B 使用 Wi-Fi 的情況下，這速度會是瓶頸。

但是吼，話雖這麼說，不要忘記了，rPi4B 再怎麼說也只要一兩張小學生的 Low Cost 裝置，你還想怎樣（錘。

可以使用 iperf3 [^3] 進行網路的實際速度的測試。
可以使用可愛的 Shizuku [^2] 對本地和遠端的硬碟做讀寫速度測試。

而，一般家用無線路由器，甚至終端裝置（電腦、手機）的 Wi-Fi 速度亦有可能成為瓶頸，例如一個標稱 AC1200 的家用無線路由器 [^7]，在 Wi-Fi 5（即 802.11ac）的 5GHz 之下的理論速度是 867Mb/s（約 108MB/s），而 Wi-Fi 的實際速度是非常看臉的，會收到距離、角度、障礙物、雜訊、姿勢、走位等諸多因素的影響。
在 rPi4B 使用 GbE，而終端裝置使用 Wi-Fi 卻吃不到 GbE 以上速度的時候，這速度會是瓶頸。

而官方討論區中所述的問題，很幸運的我沒有遇到。大概是說，NTFS 在 Linux 上效能不佳，可以使用原生的 ext4 檔案系統 [^8]，以及行動硬碟的相容問題 [^9]。

總結，在假設路由器、網路線等其他裝置都支援 GbE，無線網路 AC1200 的情況下：

- 直接的硬碟讀寫速度：144MB/s；
- rPi4B、電腦都使用有線網路：瓶頸會是有線網路 GbE，125MB/s；
- rPi4B 使用有線網路，電腦使用無線網路：瓶頸會是無線網路，108MB/s；
- rPi4B 使用無線網路：瓶頸會是 rPi4B 的無線網路，14MB/s；

在家拉光纖指日可待。

*[rPi]: Raspberry Pi
*[rPi4B]: Raspberry Pi 4B
*[GbE]: Gigabit Ethernet

[^1]: [WD Red NAS Hard Drives (PDF)](http://products.wdc.com/library/SpecSheet/ENG/2879-800002.pdf)
[^2]: Crystal Dew World： [CrystalDiskMark](https://crystalmark.info/en/software/crystaldiskmark/)
[^3]: Wikipedia: [Iperf](https://en.wikipedia.org/wiki/Iperf)
[^4]: Wikipedia: [USB](https://en.wikipedia.org/wiki/USB)
[^5]: Raspberry Pi Documentation: [USB](https://www.raspberrypi.org/documentation/hardware/raspberrypi/usb/README.md)
[^6]: The MagPi magazine: [Raspberry Pi 4 specs and benchmarks](https://magpi.raspberrypi.org/articles/raspberry-pi-4-specs-benchmarks)
[^7]: MikroTik Routers and Wireless: [hAP ac²](https://mikrotik.com/product/hap_ac2)
[^8]: Raspberry Pi Forums: [Slow samba transfer speed on RPi4 to a usb 3.0 drive (10,4mb/s)](https://www.raspberrypi.org/forums/viewtopic.php?f=91&t=243962)
[^9]: Raspberry Pi Forums: [STICKY: If you have a Raspberry Pi 4 and are getting bad speeds transferring data to/from USB3.0 SSDs, read this](https://www.raspberrypi.org/forums/viewtopic.php?p=1501426)

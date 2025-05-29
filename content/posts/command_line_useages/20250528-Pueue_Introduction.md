---
title: "把任務放進佇列！用 Pueue 排隊指令"
description: "Pueue Introduction<br /><del>阿跟不上我的速度吧 指令（？</del>"
date: 2025-05-28 04:00:00
updated: 2025-05-28 04:00:00
slug: "pueue_introduction"
weight: 5
taxonomies:
  tags: [zh_TW, Command Line, CLI, Tutorial, Task Queuing, Software]
---

> 本文適用 Pueue v4.0.0 (截稿時 2025-05)

有時我們會需要執行一拖拉庫需要漫長時間、需要大量系統資源的指令，
例如巨量資料的計算、機器學習，或是更常用的檔案壓縮、影片轉檔。
一旦執行下去就得等到海枯石爛，是要看著他們執行根本就是一種煎熬。
同時執行會佔用過多的系統資源，不僅互相拖慢速度，也影響到其他程式的流暢執行，大幅降低了摸魚體驗。
這時候就會希望它們放進佇列（Queue），能夠自動排隊執行，還能動態調整佇列順序、暫停、取消⋯⋯有這種工具嗎？

> 佇列（ㄓㄨˋ ㄌㄧㄝˋ，zhù liè）。讀錯請重修國小國文。

我們的需求：

- 希望指令在一個先進先出佇列中排隊，同時有壹個或多個指令執行；
- 佇列可以按需調整，新增、刪除、調整順序，甚至中止當前正在執行的任務。

Linux 上我們有 GNU Parallel 和 task-spooler，
本站之前也有文章介紹過如何在 Windows / WSL 的環境下玩 task-spooler。
但是現在，[Nukesor/pueue][] 讓我們不僅是 Linux，終於也可以在**原生** Windows 上也能把指令放進佇列！

[Nukesor/pueue]: https://github.com/Nukesor/pueue

Pueue 是 Rust 寫的工具，可以用 Rust 的 Cargo 工具安裝，
但 Cargo 實際上是載源代碼下來編譯，還得安裝編譯器，好像不是那麼適合終端用戶。
所以這裡建議通過 Package Manager 安裝，[官方文檔][pueue/readme#installation] 列出了支援的軟體套件管理器，
支援多種平台，沒有的話……沒有的話再從 Cargo 安裝吧。

Windows 的話可以用 [Scoop](https://scoop.sh/) 安裝，其用法可以參考本站[其他文章](@/posts/command_line_useages/20250528-Windows_Package_Managers.md)：

```shell
scoop install pueue
```

Pueue 是「伺服器—客戶端」架構，安裝之後使用前需要先把伺服器開起來。
我們可以以「服務（services）」的方式啟用伺服器一勞永逸：

在 Linux 上可以參考[官方文檔][pueue/wiki/Get-started#systemd]，在 Windows 上：

```shell
pueued service install
pueued service start
```

[pueue/readme#installation]: https://github.com/Nukesor/pueue?tab=readme-ov-file#installation
[pueue/wiki/Get-started#systemd]: https://github.com/Nukesor/pueue/wiki/Get-started#systemd

接下來就可以使用它了，這裡列出一些基本的用法：

```powershell
# 看看程式怎麼用
pueue help

# 增加一個「睡五秒」任務
pueue add "sleep 5"
#> New task added (id 0).  # 增加成功返回任務 ID

# 暫停、開始 `default` 預設佇列
pueue pause
pueue start

# 停止運行中的 id=0 任務
pueue kill 0
# 移除已停止的 id=0 任務
pueue remove 0

# 甚至，可以有不止一個佇列：
# 增加叫做 "compress" 的組
pueue group add "compress"
# 列出當前所有的組
pueue group
# 暫停 "compress" 組
pueue pause --group "compress"
# 刪除 "compress" 組
pueue group delete "compress"

# 整一個 PowerShell 限定的花活
pueue add "Get-ChildItem"
# 顯示上一個指令做了什麼
pueue log
# 列出目前所有佇列中的任務，兩個指令一樣，但我勸你把話說清楚
pueue status
pueue
```

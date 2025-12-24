---
title: "［封存］把檔案壓縮影片轉檔任務放進佇列！用 WSL Task Spooler 排隊 Windows 的指令"
description: "[Archived] WSL Task Spooler Work With Batch Script"
date: 2021-04-12 01:00:00
updated: 2021-04-12 01:00:00
slug: "wsl-tsp"
weight: 9
taxonomies:
  tags: [zh_TW, Command Line, CLI, WSL, Windows, Linux, Task Queuing, File Archive]
---

> 🛑（2025-05）現在我更推薦使用新的工具軟體 Pueue 達成同樣的任務，請見本站的介紹文：[把指令放進佇列！用 Pueue 排隊指令](@/posts/command-line-guide/Pueue_Introduction.md)。

有時候需要執行一些時間長、需要大量系統資源的指令，例如巨量資料的計算、機器學習，或是更常用的檔案壓縮、影片轉檔。希望這些高負載的指令可以被順序執行而不是同時執行，同時執行會佔用過多的系統資源，不僅互相拖慢速度，也影響到其他程式的流暢執行，大幅降低了摸魚體驗。

Linux 上有一個滿足這個需求的工具 [Task Spooler][tsp] (tsp)，Windows 上我沒有找到理想的替代品，但是，Windows 有 WSL。

Task Spooler 可以：

- 希望指令在一個先進先出佇列中排隊，同時有壹個或多個指令執行；
- 佇列可以按需調整，新增、刪除、調整順序，甚至中止當前正在執行的任務。

這篇文章將 Linux 中的 Task Spooler 透過 WSL 中與 Windows 的批次檔聯動，並演示了排隊 7-Zip 的多個任務。後面的章節會簡要提及 WSL 1 啓動的步驟。

作業環境：

- Windows 10
- [7-Zip][7z]
- WSL 1

[tsp]: https://vicerveza.homeunix.net/~viric/soft/ts/
[7z]: https://www.7-zip.org/

## 新增環境變數

在指令中，若是需要使用自己安裝的程式需要指定路徑，若想要直接呼叫程式就需要把程式的路徑新增到環境變數。

假設 7-Zip 安裝在 `C:\Program Files\7-Zip`，在 PowerShell 指令會需要呼叫 「`& "C:\Program Files\7-Zip\7z.exe"`」，若將安裝的路徑新增到環境變數之後，只需要呼叫「`7z`」即可。

1. 搜尋 "Edit the system enviroment varibles"，點選 "Enviroment Varibles..."；
2. 在 "System varibles" 中，找到並點選環境變數 `Path`，點選 "Edit..."；
3. 點選 "New" 並填入軟體的安裝路徑，例如 `C:\Program Files\7-Zip\`，點選 "OK"。
4. 生效會需要重新開機。（但是後面也會有需要重新開機的步驟所以可以先等等）

## 7-Zip 指令與批次檔

Windows 版本的 7-Zip 原生支援指令行模式，[User's Guide][7z_CL] 說明了詳細的用法。例如下面的指令將 `D:\src` 以 7z 的格式壓縮到 `D:\dst.7z`：

```cmd
7z a -t7z "D:\dst.7z" "D:\src"
```

[7z_CL]: https://sevenzip.osdn.jp/chm/cmdline/index.htm

一個小小的訣竅可以節省壓縮檔案的步驟，創立一個 Batch 批次檔手稿程式，新增一個 txt 文字檔，並將副檔名改成 `.bat` 即可，例如 `zipit.bat`，編輯內容並存檔：

```batch
7z a -tzip "%~1.zip" "%~1" -mx=9 -mcu=on
@PAUSE
```

在檔案總管中，拖動 ***一個*** 檔案或者資料夾到這個 BAT 檔案的圖示上，
便會在同個地方把這個檔案或者資料夾以 ZIP 格式壓縮，並且指定最高壓縮比（最小檔案大小、最長壓縮時間），並且最重要的指定以 Unicode 編碼檔名。這個手稿程式在後面的範例會再次用到。

## WSL 安裝

M$ 秉承著打不贏就加入他們的精神，
聯合一眾 Friends 做出了曠古未聞空前絕後驚世駭俗的 Windows Subsystem for Linux (WSL)，
WSL 可以讓 Windows 執行 Linux 的程式，並且可以串接 Windows 的指令。

1. 在開始功能表中搜尋 "Turn Windows features on or off"，開啟 "Windows Subsystem for Linux"，在安裝完成後會需要重新開機。
2. 在 "Microsoft Store" 中，搜尋 "WSL" 即可找到諸如 Debian、Ubuntu、SUSE、Kali 各發行版，如果你是 Linux 新手，可以試試好上手的 [Ubuntu][WSL_Ubuntu_Store]，後面也會拿這個舉例。
3. 開始選單會出現安裝後的 Ubuntu，初次使用會需要設定 UNIX 使用者名稱和密碼。這些都不會影響到原本的作業系統，忘記密碼也可以透過特殊手段重設。當你看到 `使用者名稱@電腦名:~$` 的時候，到此你的 WSL 的設定就完成了。

[WSL_Ubuntu_Store]: https://www.microsoft.com/store/productId/9NBLGGH4MSV6

進行 WSL 的更新，執行需要輸入剛剛設定的 UNIX 密碼：

```bash
sudo apt-get update && sudo apt-get upgrade
```

## Task Spooler

> TL;DR：Task Spooler 的絕妙的設計，是 伺服器＼客戶端，使用者使用 tsp 指令創立任務時，會創立一個客戶端，到後臺等待伺服器的通知後執行，完成後回報給伺服器，伺服器再通知下一個客戶端的指令執行，伺服器負責發號施令而不會執行指令。

在 WSL 中下載安裝 Task Spooler：

```bash
sudo apt-get install task-spooler
```

Task Spooler 的指令是 `tsp`，列出一些基本用法：

```bash
tsp -V          # 檢查版本
tsp -h          # 印出說明
tsp sleep 5     # 新增指令 `sleep 5` 到任務佇列，返回的數字是任務的 ID
tsp             # 印出任務列表
tsp -c $id      # 小寫 c，檢查指定任務的輸出，
                #     替換 $id 為什麼任務 ID，不指定的話會印出最後一個
tsp -S $num     # 大寫 S，指定同時執行任務的數量，
                #     替換 $num 為數量，不指定的話會印出當前的設定
tsp -C          # 大寫 C，清除已經完成的任務
```

我們也可以專門開一個終端機，使用 `watch` 指令來實時監視 Task Spooler 的任務列表：

```bash
watch -n 0.1 tsp
```

## WSL 串接指令

Windows 的 Batch \ PowerShell 與 WSL 之間可以 [互相串接指令][WSL_InterOP] ，在 Batch \ PowerShell 中串接 WSL 的指令：

```PowerShell
wsl ls -la  # 執行 WSL 端的 ls 指令列出當前路徑下所有檔案和資料夾等內容
wsl tsp     # 執行 WSL 端的 Task Spooler，印出任務列表
```

在 WSL 中也可以呼叫 Windows 端的工具軟體，參數會原樣傳遞：

```bash
7z.exe a -t7z "D:\dst.7z" "D:\src"  # 執行 Windows 端的 7-Zip
```

注意，若 WSL 安裝過移植版的「p7zip」，指令 `7z` 會是指「p7zip」而不是 Windows 版的「7z.exe」。

[WSL_InterOP]: https://docs.microsoft.com/zh-tw/windows/wsl/interop

## 在批次檔中使用 Task Spooler

創立一個 Batch 批次檔手稿程式，實現在檔案總管中點開就可以監視 Task Spooler 的任務列表。

```batch
wsl watch -n 0.1 tsp
```

編輯或者複製之前範例的批次檔手稿程式 `zipit.bat`，與上面的例子作用是相同的，只是將指令託管到 Task Spooler 的佇列中：

```batch
wsl tsp -f "7z.exe" a -tzip "%~1.zip" "%~1" -mx=9 -mcu=on
```

在檔案總管中，拖動 ***一個*** 檔案或者資料夾到這個 BAT 檔案的圖示上，
便會在同個地方把這個檔案或者資料夾以 ZIP 格式壓縮，並且指定最高壓縮比和指定以 Unicode 編碼檔名。

這個指令，會彈出一個終端機視窗，會返回一個數字這個任務的 ID，任務完成後視窗會自動關閉。

在監視 Task Spooler 的任務列表的終端機中，就會出現當前任務的狀態。

> TL;DR：參數 `-f` 會阻止客戶端把指令 Fork 到背景等待執行。在我的測試中，若不加這個參數會導致加入佇列後直接執行的任務卡住，導致任務無法完成，也就沒辦法繼續執行後面的任務。

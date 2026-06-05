---
title: "打通 Windows 和 Linux 的傳送魔法陣！在 Shell 之間反覆橫跳"
description: "通常要做一件事情是可以在一個 Shell 裡面完成的。但無論是使用舊工具或是跨平臺，都使得我們不得不在不同的 Shell 之間反复橫跳。這篇文章介紹的巢狀指令呼叫根本就是指令界的人〇〇蚣！可以在不同 Shell 之間互通有無。"
date: 2025-05-28 03:00:00
updated: 2025-05-28 03:00:00
slug: "nested-shell-scripting"
weight: 4
taxonomies:
  tags: [zh_TW, Command Line, CLI, WSL, Windows, Linux, Tutorial, Computer Concept]
---

通常，要做一件事情是可以在一個 Shell 裡面完成的。
但，CMD 和 PowerShell 目前還是各有千秋，
以及 Windows Subsystem Linux (WSL) 使得可以使用 Linux 中的程式，
這使得我們不得不在這些 Shell 裡面反复橫跳。

## Nested Shell Scripting

上文說過，Shell 不過是一個程式，本質上只是能執行程式的直譯器，而 Shell 可以呼叫程式，
因此只要能呼叫，Shell 就能嵌套執行另一個 Shell 的程式碼。所以 Shell 之間可以互通有無。

<del>這根本就是指令界的人〇〇蚣！</del>

由於 Windows 11 預設的 Windows Terminal 預設的是 PowerShell，在懶得切換 Shell 的前提下，我最經常做的就是檢查檔案的散列值：

```powershell
wsl sha256sum -c SHA256SUMS
```

回顧一下指令的構成，指令按空格分割，第一個是執行的程式，而後所有的部分作為「參數」丟給程式。
這個指令的意思是，執行 `wsl` 並且把 `sha256sum -c SHA256SUMS` 作為參數丟給它。然後 `wsl` 會打開預設的 WSL 並且把這些參數作為指令執行，就是打開 `sha256sum` 計算散列值程式，並且把 `-c SHA256SUMS` 作為參數丟給它，讓程式知道要做的是「比對檔案 `SHA256SUMS` 中的散列值的一致性」。

當然，執行這個指令前提是你已經調教好了一個能用的 WSL。
並且遇到了 Linux 和 WIndows 跨平臺的時候，還得記得解決 BOM 和 換行符 的問題。

以及「跳脫字元」和「引號定義」，這在一個 Shell 中不成大問題，但跨 Shell 的時候傳遞參數，由於不同的 Shell 有不同的跳脫字元和引號定義，在傳遞的時候整條鏈路都必須要做處理，甚至需要多次「跳脫跳脫字元」，這個根本就是連老手都會翻車的地獄試煉，必須要精通整條鏈路上所有 Shell 的行為，尤其是跳脫規則與參數傳遞邏輯。

如此如此這般這般，就打通了 Windows 和 Linux 兩個世界線的傳送魔法陣。

## More Command-line Tools

我們常用的那些指令列工具，像是 `ls`、`cat`、`grep`、`top`，可能都是幾個世紀前的產物了。不過現在 Rust-lang、Go-lang 等新語言的流行，使得很多指令列工具被挖出來砍掉重寫，也讓這些老工具煥發第二春。
例如說 [sharkdp/bat](https://github.com/sharkdp/bat) 升級了 `cat` 印出文字檔；
又例如說 [ogham/exa](https://github.com/ogham/exa) 升級了 `ls` 印出資料夾中的內容；
再例如說 [BurntSushi/ripgrep](https://github.com/BurntSushi/ripgrep/tree/master) 升級了 `grep` 搜尋工具；
這也順便帶來的另一個好處，他們在重寫的時候，有可能順便考慮一下跨平臺：這使得以前一些 Linux 獨佔的工具有機會在 Windows 中原生的使用。
甚至許多這些重寫的工具也都已經登上 Scoop 或 Winget，直接安裝即可使用，無痛邁入新世代。

所以在遇事不決 WSL 之前，不妨先搜尋一下需要的工具或功能是不是已經有偉人寫好了。

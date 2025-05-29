---
title: "重灌軟體自動化！Windows 的軟體管理系統"
description: "Windows Package Managers<br /><del>我從來不覺得重灌軟體開心過。</del>"
date: 2025-05-28 02:00:00
updated: 2025-05-28 02:00:00
slug: "windows_package_managers"
weight: 2
taxonomies:
  tags: [zh_TW, Command Line, CLI, Tutorial, Computer Concepts, Software]
---

現如今重灌電腦已經不是什麼困難的事情。
而重灌電腦最麻煩的就是灌回那些常用的軟體。
傳統上 Windows 要重灌軟體，通常是這樣的步驟：打開瀏覽器、搜尋軟體官方網站、找到載點下載安裝包、安裝一直按下一步，然後重複一百萬遍……
真的好想讓電腦變成自己的形狀。

在 Linux 上有各種軟體套件管理系統（Package Manager）來幫你管理軟體：
例如 Debian 的 `apt`、或者 Fedora 的 `dnf`，一個指令就可以幫你從軟體倉庫載軟體下來自動安裝。
甚至有 [chezmoi](https://www.chezmoi.io/) 之類的軟體幫你管理設定檔案，
可以從零開始直接安裝軟體到拷貝設定檔案「一鍵裝機」一條龍，直接變成你最熟悉的形狀。

好在 Windows 上也有了這類套件管理系統，最有名的莫過於 Winget、Scoop、Chocolatey。

## Live Example

> <del>用 Edge 載其他瀏覽器是不是一種 NTR？</del>

舉例一個非常實在的例子，重灌作業系統之後第一件事就是重灌瀏覽器，
通常我們需要打開 Edge -> 用 Bing 搜尋你要的瀏覽器 -> 找到對應的載點 -> 等下載 -> 手動安裝。
現在只需要打開終端機然後貼上這一列指令：

```powershell
# 從 MS Store 安裝 Mozilla Firefox 並且簽訂魔法少女契約
winget install "Mozilla Firefox" -s "msstore" --accept-source-agreements --accept-package-agreements
```

```powershell
# 從 winget 預設倉庫安裝 Google Chrome
winget install --id "Google.Chrome"
```

這樣，重灌作業系統的時候就再也不需要用 Edge 來載其他瀏覽器了。
<del>Edge 再也不會因為被戴綠帽子而傷心落淚。</del>

## Winget

首先是 [Winget][]，由 Microsoft 官方不榮譽[^1]出品，**作業系統內建**，相容 Microsoft Store，開源，開放軟體倉庫。

[Winget]: https://learn.microsoft.com/zh-tw/windows/package-manager/winget/
[^1]: https://en.wikipedia.org/wiki/Windows_Package_Manager#History

```powershell
# 搜尋軟體
winget search $PACKAGE
# 安裝軟體
winget install $PACKAGE
# [推薦] 從 MS Store 安裝軟體並且簽訂魔法少女契約
winget install $PACKAGE --source "msstore" --accept-source-agreements --accept-package-agreements
# [推薦] 指定 ID 不容易裝錯
winget install --id $PACKAGE_ID
# 更新、刪除軟體
winget upgrade --all
winget uninstall $PACKAGE
```

## Scoop

[Scoop](https://scoop.sh/) 精通指令列工具，也能裝一些常見的軟體。
它可以有自己的獨立安裝路徑（`~/scoop/`），不依賴系統（登錄檔、環境變數），
好處是可以不需要作業系統管理員權限（UAC-free），也就是人們常說的「綠色軟體」，不會動到系統層級設定「污染」環境。

```powershell
scoop install neovim
scoop update *
scoop uninstall python
```

## Chocolatey

[Chocolatey](https://chocolatey.org/) 雖然看上去有些笨重，但若有一些複雜的安裝流程就可以靠它。甚至是字型也可以！

```powershell
choco install $PACKAGE
choco upgrade $PACKAGE
choco uninstall $PACKAGE
```

而，Scoop 和 Chocolatey 不是作業系統內建，但他們都有指令化的安裝方式。
由於安裝方式因地制宜，這裡就不贅述，可以參考他們的官方網站和文檔。

## Versus

> TL;DR: 他們的差異

安裝程式的本質是把程式的執行檔、依賴庫、設定檔、捷徑、登錄檔等放到他們該去的地方。安裝檔正是在做這些事情。

Winget 本質是「安裝包管理器（Installer Manager）」，就是自動幫你載安裝包下來安裝。
但，有時候就不喜歡安裝包的行為，一個是安裝檔行為不可控，可能偷渡一些設定、GUI 介面也難以自動化。

「軟體套件管理器（Package Manager）」則傾向管理軟體檔案與設定過程的全流程。
所以 Chocolatey 把安裝軟體的過程用 Script 改寫，透明且靈活。

更是有 Scoop，將軟體放在自己肚子裡，安裝軟體甚至可以不需要管理員權限。
因為不會修改到作業系統相關的內容例如說登錄檔，也就是說不會「污染」作業系統的環境。
（當然安裝 GUI 程式或是普通的安裝到系統 Scoop 也是可以做。）

現在就可以把常用的軟體安裝寫成 Shell Script，
在重灌之後直接執行，就可以一鍵完成原本要花幾百年才能完成的重灌軟體的工作！

---

Reference:

- [ScoopInstaller/Scoop#4777#2295777](https://github.com/ScoopInstaller/Scoop/discussions/4777#discussioncomment-2295777)
- [ScoopInstaller/Scoop#4777#2296112](https://github.com/ScoopInstaller/Scoop/discussions/4777#discussioncomment-2296112)

---

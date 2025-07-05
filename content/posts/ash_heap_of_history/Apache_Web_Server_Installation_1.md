---
title: "［封存］Apache 伺服器簡易教程 - 安裝篇"
description: "[Archived] Apache HTTP server - Part 1"
date: 2017-02-07 01:00:00
updated: 2017-02-07 01:00:00
slug: "apache_web_server_installation_1"
taxonomies:
  tags: [zh_TW, Linux, Apache, Web Server]
---

~~你可能已經成爲自由軟體的受害者w~~

> This work © 2017 by undecV is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

廢話不說，小考太多...

**Apache** 是一個開放原始碼的網頁伺服器軟體，是最流行的 Web 伺服器軟體之一。
如果你想要架設一個網站，「LAMP」系列 **自由軟體** 會是你不錯的選擇：

- Linux，作業系統（Operating System）
- Apache，網頁伺服器（Web server）
- MySQL，資料庫（Database）
- PHP，手稿語言（Scripting language）

因爲你只是要完成作業而且我也沒空打那麼多字，那麼接下來就是，
簡易的 Apache HTTP Server 搭建方法：

以下指令適用 Ubuntu 系列作業系統。

1. 如果你還沒或者太久沒執行過這兩行的話：

    ```bash
    sudo apt-get update
    sudo apt-get upgrade
    ```

    > Ps. 如果你不知道這兩行在做什麼：<br />
    > Ask Ubuntu: command line - [What does "sudo apt-get update" do?](http://askubuntu.com/questions/222348/what-does-sudo-apt-get-update-do)

2. 安裝 Apache HTTP Server。

    ```bash
    sudo apt-get install apache2
    ```

如果成功的話...那麼恭喜你，你的 Apache 可能已經部署完成。

那麼問題來了，我怎麼看到我的網頁呢？

3. 打開你的瀏覽器（Web browser），輸入`localhost` 或者`127.0.0.1` 。
    如果看到了 **測試頁面** ，那麼恭喜你，你的 Apache 已經部署完成。（如圖）
    ![APACHE_TEST_PAGE](/images/posts/ash_heap_of_history/Apache_Web_Server_Installation_1/APACHE_TEST_PAGE.png)

那麼問題又來了，我怎麼在原本的作業系統，或是同一網路下的主機（例如連到同一個路由器的另一台電腦）看到我的網頁呢？

4. 設定你的虛擬機的網路：（如圖）
    ![NET_SETTING](/images/posts/ash_heap_of_history/Apache_Web_Server_Installation_1/NET_SETTING.PNG)

5. 打開你的瀏覽器，輸入你虛擬機的 IP 位址。
    如果不知道虛擬機的 IP 位址，請使用指令 `ifconfig` 。
    如果看到了測試頁面，那麼恭喜你，你虛擬機的網路設置完成。

---

- Environment:
  - Windows 10
  - with VirtualBox 5.1.10
  - Lubuntu 16.10
- See also:
  - Wikipedia: [Apache HTTP Server](https://en.wikipedia.org/wiki/Apache_HTTP_Server)
  - Wikipedia: [LAMP (software bundle)](https://en.wikipedia.org/wiki/LAMP_(software_bundle))
- Release Notes:
  - 2.1 Update (20170207):
    - 修改標題，啊現在你知道你點開的是什麼了吧。
    - 修改檔案目錄。
  - 2.0 Update (20170128):
    - 變更授權：`Creative Commons License` 。
    - 細節修正。
    - 內容整合。

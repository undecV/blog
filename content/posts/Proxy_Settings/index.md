---
title: "如何在不同環境下設定 Proxy 代理伺服器"
description: "Proxy Settings"
date: 2018-05-20
updated: 2018-07-09
slug: "proxy_settings"
taxonomies:
  tags: [zh_TW, Proxy, Tutorial, Computer Concept]
---

> This work © 2017 by undecV is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

## 代理

代理（Proxy）是一種網絡服務，允許兩個通訊方之間進行非直接的連線。

例如：A 與 B 之間需要進行通訊，但是無法（或者不想）建立直接的連線；A 與 P、B 與 P 之間能進行正常的通訊，那麼，藉由 P，可在 A 與 B 之間建立通訊的通道。在這當中 P 就是 代理伺服器（Proxy Server）。

```plain
+----------+      |防|                       +----------+
|          | <--- |火| ----- 無法鏈接 -----> |          |
| 你的電腦 |      |牆|      +----------+     | 目標網站 |
|          | <------------> |          | <-> |          |
+----------+   沒有被牆的   | 遠程電腦 |     +----------+
  /______\                  |          |       /______\
                            +----------+
                              /______\
```

代理伺服器可以是閘道器、路由器，也可是一臺電腦或者虛擬機，也可是你電腦中的軟體；代理伺服器亦可不止一個。

## 本地代理

**本地代理**，是代理的一種應用方式，將網絡連線轉送到本地的地址，通常用於將網絡連線轉到一些網路工具軟體中。

本地代理通常使用本機地址（`localhost`, `127.0.0.1`, `[::1]`）。

## 通訊協定

代理所使用的通訊協定通常是 HTTP 和 SOCKS，SOCKS 常用的有版本 4 和 5 所以經常被簡寫成 SOCKS4 和 SOCKS5。

不同的協定之間不能互通，甚至 SOCKS4 和 SOCKS5 之間也不能互通，設定時請注意差別。

通常情況下，建議優先選擇 SOCKS 5 協定。

參考：
[SOCKS - 維基百科，自由的百科全書](https://zh.wikipedia.org/zh-tw/SOCKS)

## 設定

> **請注意！在下面的截圖中，一些設定的内容請根據實際情況填寫。**

在設定過程中，必要填寫代理伺服器的 **Address（地址）** 和 **Port（端口）**，選擇填寫的有轉發規則。

- 如果你的代理伺服器設定在遠端，那麼需要填寫的通常是遠端電腦伺服器的地址和端口。
- 如果你的代理伺服器設定在電腦上（代理軟體），那麼需要填寫的通常是本機地址和軟體指定的端口。

### Windows 10

Windows 10 的設定（`Settings`）中提供了代理的選項。在系統中設定代理的好處是不需要爲每一個軟體都配置代理，也使得不支持設定代理的軟體也能夠使用代理。

Windows 10 的代理 **默認** 使用的是 **HTTP 協定**。

Step 1: 打開代理的設定頁面，你有兩種方法打開它：

- 在開始菜單 `Start menu` 中搜尋代理 `Proxy` 進入系統設置 `Setings`。
- 或是：`Start menu` → `Setings` → `Network & Internet` → `Proxy` → `Manual proxy setup`

接下來：

1. 打開 `Use a proxy server`；
2. 填入 Address（地址） 和 Port（端口）；
3. 通常地，打開 `Don't use the proxy server for local (internet) address`（不要將代理用於本地地址）選項。

如果需要設定 **除了 HTTP 之外的協定**（例如 SOCKS）或 **多種代理協定**，可以在 Address（地址）處以下列格式填寫， Port（端口）無需填寫：

```plain
http=<ADDRESS>:<PORT>;https=<ADDRESS>:<PORT>;socks=<ADDRESS>:<PORT>
```

例如：

```plain
http=127.0.0.1:9487;https=127.0.0.1:9487;socks=127.0.0.1:9487
```

```plain
socks=127.0.0.1:9487
```

![Win10_Proxy](./images/Win10_Proxy_E.png)

### 網頁瀏覽器 - Web Browser

瀏覽器的代理設定，使用這個方法將代理只應用於瀏覽器。通常的，在瀏覽器的設定頁面中就可以設定代理。使用瀏覽器内置的代理設定簡單，不必安裝額外的插件或軟體；但是更推薦使用瀏覽器的插件（Add-ons, or Extensions），他們更加便利和功能强大，插件方法的設定見下一節。

瀏覽器內置的設定，例如：

Firefox:
`Menu button` → `Options` → `Geneal` → `Network proxy` → `Settings...` → `Manual proxy configuration`

Chrome:
`Menu button` → `Settings` → `Advabced` → `System` → `Open proxy settings` → (System setting)

Vivaldi:
`Menu button` → `Tools` → `Settings` → `Network` → `Proxies` → `Proxy Settings` → (System setting)

以 [Mozilla Firefox](https://www.mozilla.org/zh-TW/firefox/all/) 57+ 為例：

![Firefox_Proxy_1](./images/Firefox_Proxy_1_E.png)
![Firefox_Proxy_2](./images/Firefox_Proxy_2.png)

其他的瀏覽器設定大致相同。

### 網頁瀏覽器插件 - Web Browser Add-ons / Extensions

插件的下載地址：

- [FoxyProxy Standard](https://getfoxyproxy.org/)
  - [Firefox Add-ons](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/)
  - [Chrome Extensions](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp)
- [Proxy SwitchyOmega](https://github.com/FelisCatus/SwitchyOmega)
  - [Firefox Add-ons](https://addons.mozilla.org/en-US/firefox/addon/switchyomega/)
  - [Chrome Extensions](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif)

將插件下載並安裝到瀏覽器后，點擊插件的圖標並進入插件的設定頁面並進行設定，在開關、切換、修改代理設定也只需要通過點擊插件的圖標即可。

以 FoxyProxy Standard 為例：

![FoxyProxy_1](./images/FoxyProxy_1_E.png)
![FoxyProxy_2](./images/FoxyProxy_2.png)
![FoxyProxy_3](./images/FoxyProxy_3_E.png)

### 其他軟體中的設定

通常的，一些需要網絡連線的應用軟體會提供代理伺服器的設定，若無，可以使用系統代理的方法解決。

以下載器 [Free Download Manager](https://www.freedownloadmanager.org) 爲例：

![FDM_1](./images/FDM_1_E.png)

通常的，代理的設定在軟體設定界面的網路設定分頁／分類中。

---

Release Notes:

- 3.0 Release (20180709).
- 2.0 Release (20180520).
- 1.0 Release (20180210).

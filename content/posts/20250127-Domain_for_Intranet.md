---
title: "如何為內網服務設定自訂網域與 HTTPS：保留 TLD、ACME 證書與本地 DNS 的實踐"
description: |
    想為內網中的 Web Server 設定像 `web.internal` 這樣的域名，還希望能支援 HTTPS？
    本文探討保留 TLD 的限制、ACME 證書如何簽給內部服務，以及如何透過本地 DNS 配合真實域名實現內網 HTTPS 的實踐方式。
slug: domain_for_intranet
date: 2025-01-27
updated: 2025-06-15
taxonomies:
  tags: [zh_TW, Network, Domain, TLD, DNS, ACME]
---

假設一個情況，內部 NAT 網路有以下設施僅為內網服務：

- 路由器兼任 DNS Server `192.168.1.1/24`
- 本地 Web Server `192.168.1.100/24`

如何為這本地 Web Server 設定域名，最好還能有 HTTPS？

## Local TLDs

首先想到的是使用幾個保留域名，
可以在 DNS Srever 中設定這樣的記錄：

```dns
A   web.local.      192.168.1.100
A   web.lan.        192.168.1.100
A   web.internal.   192.168.1.100
```

`*.local.` 雖然是本地專用的 TLD，但被指定為 mDNS 專用，所以亂用會出事。

`*.intranet.`、`*.private.`、`*.corp.`、`*.home.`、`*.lan.` ... 也是 RFC 6762 規範的保留 TLDs，
但 Firefox 無法解析，會跳到搜尋，需要到 `about:config` 打開相關的選項，但這哪是個人事阿。

`*.internal.` 既是保留 TLD，又可以被解析，看上去是目前私有 TLD 的最佳解。
但講道理，別人域名都兩三個字母，這真的是又臭又長，你要我打這個單字我還不如打 IP ...

保留 TLDs 誰都可以添加記錄，所以除了自己簽發，自然是沒有 TLS 證書的。還要考慮發行證書，這又哪是個人事阿。

## Real Domain

Domain Name System 比我想象的還要 naïve ...
我原本還在想，要是在 CloudFlare 裡面添加 NAT 的 IP，那萬一被人 nslookup 到了是有多丟人。
我還擔心 ACME 不給本地 IP 簽發證書，沒想到人家根本就不管你網路。

假設購買的域名是 `domain.example`，
在設定 DHCP 的時候，DNS 伺服器首選 本地 的 DNS Server，
在 本地 的 DNS Srever 中設定這樣的記錄：

```dns
A   lan.domain.example.     192.168.1.100
```

由於記錄位於 NAT 之內的 本地 的 DNS Srever，所以這個 `lan.domain.example` 是無法被外部網路解析的。

ACME 是自動簽發 TLS 證書給伺服器，讓伺服器有 HTTPS 可以用。
在伺服器發出 ACME 請求後，ACME 會給一個「權杖」，把權杖輸進 DNS 記錄，ACME 看到了就證明了你對域名的控制權。
所以 ACME 並不關心伺服器在哪個網路下。

只要在 本地 Web Server 中就像架設對外的網站一樣，指定託管商（例如 CloudFlare）的 API 就行，讓伺服器自己完成 ACME 挑戰。

Reference: Wikipedia - [Non-IANA domains](https://en.wikipedia.org/wiki/List_of_Internet_top-level_domains#Non-IANA_domains)

---
title: "YouTubeあるある - 我不想再看短影片"
description: "<del>抖〇一響，父母白養</del>"
date: 2022-05-10
updated: 2022-05-10
slug: "youtube-never-shorts"
taxonomies:
  tags: [zh_TW, Firefox, Chrome, Add-on, Extension, YouTube]
---

<!-- # No More YouTube Shorts -->

> <del>抖〇一響，父母白養</del>

短影片像是癌症擴散一般風靡了各大影音平臺，
但是短影片極度簡化的播放器功能缺失甚至沒有調整進度的功能，
好在通過轉換網址，YouTube 一般播放器也可以播放短影片的。

短影片播放器的網址：`https://www.youtube.com/shorts/<VIDEO_ID>`</br>
一般的播放器的網址：`https://www.youtube.com/watch?v=<VIDEO_ID>`

通過 REDIRECTOR 免費、開源擴充套件（[Firefox Add-on][REDIRECTOR Firefox Add-on], [Chrome Extension][REDIRECTOR Chrome Extension]），
可以自動把短影片的地址轉換到一般的播放器。

[REDIRECTOR Firefox Add-on]: https://addons.mozilla.org/firefox/addon/redirector/
[REDIRECTOR Chrome Extension]: https://chrome.google.com/webstore/detail/redirector/ocgpenflpmgnfapjedencafcfakcekcd

把下面這段資料存成文字檔，在 REDIRECTOR 擴充套件中導入（Import），啟用之後便會自動把短影片轉址到一般的播放器。

最後，好孩子不要去找範例網址XD

```json
{
    "createdBy": "Redirector v3.5.3",
    "createdAt": "2022-05-09T15:03:30.599Z",
    "redirects": [
        {
            "description": "No More YouTube Shorts",
            "exampleUrl": "https://www.youtube.com/shorts/z4Wl0asydDA",
            "exampleResult": "https://www.youtube.com/watch?v=z4Wl0asydDA",
            "error": null,
            "includePattern": "https://www.youtube.com/shorts/*",
            "excludePattern": "",
            "patternDesc": "",
            "redirectUrl": "https://www.youtube.com/watch?v=$1",
            "patternType": "W",
            "processMatches": "noProcessing",
            "disabled": false,
            "grouped": false,
            "appliesTo": [
                "main_frame",
                "sub_frame",
                "stylesheet",
                "script",
                "image",
                "imageset",
                "object",
                "xmlhttprequest",
                "history",
                "other"
            ]
        }
    ]
}
```

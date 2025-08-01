---
title: "程式如何接收參數"
description: "從 Hello World 到能聽懂人話的程式：介紹命令列參數的使用方法，讓程式能根據輸入做不同事情。"
date: 2025-08-01 09:00:00
updated: 2025-08-01 09:00:00
slug: "argument_introduction"
weight: 6
taxonomies:
  tags: [zh_TW, Command Line, CLI, Tutorial, Computer Concept, Windows]
---

通常我們設計程式時，不會只讓它做單一件事。就像廚房里八百萬年不會拿出來用一次的攪拌機，它可以打果汁，也能打冰沙，甚至可以用來攪和麵糊。程式也是一樣，我們希望它有彈性，能根據不同需求做不同事情，而不需要每次都去修改程式碼、重新編譯或重寫一個新程式。這就是為什麼有了「命令列參數」，我們只要改變「參數（Argument）」，程式就能換出不同的用途，就像同一台攪拌機能處理不同食材一樣。

大多數的 Shell 在啟動一個程式時，會把使用者在命令列打的「以空白分隔的字串」作為參數傳遞給程式。例如說 `ls -a -l ./asd` 即是：呼叫程式 `ls` 並傳遞三個參數 `["-a", "-l", "./asd"]` 給他。

程式啟動時，這些參數會以「參數陣列」的形式傳入程式，通常包含：

- argv[0]：程式本身或它的相對或絕對路徑；
- argv[1] ... argv[n]：後續輸入的每個參數。

> 眾所周知，電腦科學家們不會數數，都是從 0 開始數數。我們也按照這個慣例，用「第 0 個」表示真正的第一個參數。

第 0 個參數表示程式本身或它的相對或絕對路徑，這是約定俗成的設計，讓程式知道它的人生三大問題之二：「我是誰？」、「我從哪裡來？」；而後所有的輸入，讓程式知道它的人生最後一大問題：「我來做什麼？」。

我們用 C 實際操作一次。

```c,name=arg.c
#include <stdio.h>

int main(int argc, char *argv[]) {  // 主程式入口，argc 是參數數量，argv 是參數陣列。
    for (int i = 0; i < argc; i++) {  // 迴圈 argc 次，用於遍歷 argv[] 參數陣列。
        printf("argv[%d] = %s\n", i, argv[i]);  // 印出第 i 個參數。
    }
    return 0;  // 結束程式。
}
```

編譯並執行：

```bash
gcc arg.c -o arg  # 編譯程式成執行檔 `arg`
./arg hello world 123
```

得到輸出：

```plain
argv[0] = ./arg
argv[1] = hello
argv[2] = world
argv[3] = 123
```

Python 的概念相同，但語法更簡單。程式碼如下：

```python,name=arg.py
import sys

for i, arg in enumerate(sys.argv):  # 遍歷 argv[] 參數陣列。
    print(f"argv[{i}] = {arg}")  # 印出第 i 個參數。
```

所以我們在程式中就可以接收到「傳遞」進來的「參數」，之後我們就可以對這些參數進行解析和應用。

我們來寫個可愛的小程式以展示它的用法：我們的程式是一個禮貌的猴子，他只喜歡香蕉，你丟給他一堆水果，它只會把香蕉吃掉，其他的還給你。

```python,name=monkey.py
import sys

for arg in sys.argv:  # 遍歷 argv[] 參數陣列。
    if arg != "🍌":
        print(arg, end=" ")
print()
```

我們執行：

```bash
python monkey.py 🍇 🍊 🍋 🍌 🍎
```

會得到輸出：

```plain
🍇 🍊 🍋 🍎
```

但通常，我們會設計一套「參數」的規則，就像微波爐會設計多個按鈕（加熱、解凍、烤箱），搭配「說明書」告訴你怎麼使用。
例如我們常用的 `ls -a -l ./asd` 就表示：

- 呼叫 `ls` 程式，
- 並且 `-a` 是「印出所有內容」，
- 以及 `-l` 是「以表格的樣子印出來」，
- 最後 `./asd` 印出當前子資料夾 `asd` 裡面的內容。

也會有一些約定俗成的用法，例如 `--help` 或 `/?` 表示「印出說明書」。

Linux 上，通常也有「整本」的說明書叫做 `man`，它不是在說你的軟體是真男人，而是 Manual page 的簡寫，例如查詢 `ls` 的說明書可以輸入 `man ls`。

但是我們寫程式的時候，自己解析這些參數實在是太麻煩了，所以我們可以使用一些套件例如 Python 上官方提供的 argparse 以及第三方但是強大的 [Click](https://click.palletsprojects.com) 來設計程式接收的參數。

有了這些命令列參數，我們就能讓同一支程式靈活應對不同需求，而不用每次都從頭寫一次。

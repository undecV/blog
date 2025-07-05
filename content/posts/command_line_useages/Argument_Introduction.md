---
title: "程式如何接收參數"
description: "Argument Introduction<br />Hello World 之後的下一個範例程式"
date: 2025-05-29 09:00:00
updated: 2025-05-29 09:00:00
slug: "argument_introduction"
draft: true
weight: 6
taxonomies:
  tags: [zh_TW, Command Line, CLI, Tutorial, Computer Concept, Windows]
---

```c,name=arg.c
#include <stdio.h>

int main(int argc, char *argv[]) {
    for (int i = 0; i < argc; i++) {
        printf("argv[%d] = %s\n", i, argv[i]);
    }
    return 0;
}
```

```python,name=arg.py
import sys

for i, arg in enumerate(sys.argv):
    print(f"argv[{i}] = {arg}")
```

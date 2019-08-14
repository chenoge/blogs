---
title: linux-wc命令
date: 2019-08-14 14:26:55
tags: [linux,wc]
---

#### wc命令

利用`wc指令`我们可以计算文件的`Byte数、字数、或是列数`。若不指定文件名称、或是所给予的文件名为`-`，则`wc指令`会**从标准输入设备读取数据**。

```
wc [-clw][--help][--version][文件...]
```

- `-c`或`--bytes`或`--chars `只显示`Bytes数`
- `-l`或-`-lines` 只显示行数
- `-w`或`--words` 只显示字数
- `--help` 在线帮助
- `--version` 显示版本信息

```shell
# testfile文件的统计信息
wc testfile
# 3 92 598 testfile
# testfile文件的行数为3、单词数92、字节数598 
```


---
title: macos15+lualatex+ctex报错
tags:
  - macos
  - ctex
  - lualatex
  - latex
---

在mac15中使用lualatex编译ctex的文件会报字体找不到的错误，同样的文件用xelatex编译却是正常的。
最小工作实例如下：
```latex
%arara: lualatex
\documentclass{ctexart}
\begin{document}
你好，世界！
\end{document}
```

在 github 中找到了相关[issue](https://github.com/CTeX-org/ctex-kit/issues/722)，阅读后找到了
```bash
sudo tlmgr conf texmf OSFONTDIR /System/Library/AssetsV2/com_apple_MobileAsset_Font7
```
这个解决方案。不过似乎只是暂时的，需要等待以后出现更好的方案。

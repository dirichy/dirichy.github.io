---
title: "LaTeX smart equation"
---
# 动机
在$ \LaTeX $ 写作中，我们有时候无法预知公式的大小，从而选择行内公式或者行间公式。
对过大公式使用行内公式还会导致公式超出页面的问题。因此我希望找到一种方法能自动调节公式的类型。
# 实现
将以下代码放在导言区即可将`\(`,`\)`定义的行内公式改为智能公式。
当公式长度超过行宽的一半或者公式高度超过1.5em时会自动采用行间公式，其他时候则会采用行内公式。
```latex
\newlength\inlineHeight
\newlength\inlineWidth
\long\def\(#1\){
  \settoheight{\inlineHeight}{$#1$}
  \settowidth{\inlineWidth}{$#1$}
  \ifdim \inlineWidth > 0.5\textwidth
  $$#1$$
  \else
  \ifdim \inlineHeight > 1.5em
  $$#1$$
  \else
  $#1$
  \fi
  \fi
}
```
# 其他技巧
可以结合
```latex
\everymath{\displaystyle}
```
使用，自动将所有公式转为displaystyle。

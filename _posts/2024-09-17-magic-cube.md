---
title: 魔方中的群论知识
tags:
    - Math
    - 群论
---

# 题目

证明下列魔方不可还原：\\
<div>  <img src="/img/post/jh.jpg" alt="Image 1" style="margin-right: 10px;"/>  <img src="/img/post/zl.jpg" alt="Image 2" /> <img src="/img/post/zj.jpg" alt="Image 3" /></div>
<!-- \begin{minipage}{0.3\linewidth} -->
<!-- \centering 1. -->
<!-- \includegraphics[width=\linewidth]{jh.jpg} -->
<!-- \end{minipage} -->
<!-- \begin{minipage}{0.3\linewidth} -->
<!-- \centering 2. -->
<!-- \includegraphics[width=\linewidth]{zl.jpg} -->
<!-- \end{minipage} -->
<!-- \begin{minipage}{0.3\linewidth} -->
<!-- \centering 3. -->
<!-- \includegraphics[width=\linewidth]{zj.jpg} -->
<!-- \end{minipage} -->
图中看不到的色块都是正确的. 

# 证明

（建议配合一个魔方食用）\\
首先我们固定魔方的六个中心（即忽略魔方整体的转动），则魔方的每个操作都由某个面顺时针旋转90度复合构成，我们将对魔方的所有操作组成的集合记作$G$，则$G$关于操作的复合构成一个群。记将黄、白、红、橙、蓝、绿色中心块顺时针转90度为$y,w,r,o,b,g$，称它们为基本操作。则$G=\langle y,w,r,o,b,g\rangle$。记$G$的单位元为$e$，则一个状态$a$可复原当且仅当能通过$G$中的某个元素$a'$对复原状态进行操作能得到状态$a$，易知$a$与$a'$是一一对应的，故以下我们不再区分状态和操作，认为$a$和$a'$是同一对象。
1. 将魔方的8个角和12个棱编号$1\sim20$，作一个群同态$f:G\to S_{20},f(x)$为$x$对应的状态棱块和角块的置换，易知$f$为一个群同态。且对于每个基本操作$x$，都有$f(x)$为偶置换（因为对应面的棱和角分别构成一个4-循环，故为偶置换），从而$\forall x\in G,f(x)$为偶置换。但图片中的魔方只交换了两个棱块，对应的置换为奇置换，故无法还原。
1. 将魔方的12个棱块的24个色块编号$1\sim 24$，作一个群同态$g:G\to S_{24},g(x)$为$x$对应的状态这些色块的置换（注意即使色块颜色相同也被赋予了不同的编号因此要看成不同的）。注意到对于每个基本操作$x$，都有$g(x)$为偶置换（因为对应面的棱的色块分别构成两个4-循环，故为偶置换），从而$\forall x\in G,g(x)$为偶置换。但图片中的魔方只交换了两个色块，对应的置换为奇置换，故无法还原。
1. 将魔方的8个角编号$1\sim 8$，作一个群同态$\phi:G\to S_{8},\phi(x)$为$x$对应的状态角块的置换。将魔方的白色和黄色中心对应的面称为好面，白色和黄色称为好色。作映射$h_i:G\to \mathbb{Z}_3,h_i(x)$为$x$对应的状态编号为$i$的角块顺时针扭转到好色在好面上需要的次数。由于角块扭转3次和不扭转等价，故映射良定义。且有$h_i(ab)=h_i(b)+h_{\phi(b)(i)}(a)$。作映射$h:G\to\mathbb{Z}_3,h(x)=\sum_{i=1}^8h_i(x),h(ab)=\sum_{i=1}^8h_i(ab)=\sum_{i=1}^8h_i(b)+h_{\phi(b)(i)}(a)=h(a)+h(b)$，故$h$为群同态。对于基本操作$s$，若$s=y,w$，$h_i(s)=0$，故$h(s)=0$。若$s=r,o,b,g$，则有四个$i$使得$h_i(s)=0$,有两个$i$使$h_i(s)=1$,有两个$i$使$h_i(s)=-1$故$h(s)=0$。从而$\forall x\in G,h(x)=0$。但显然题目所给状态对应的$h$为$2$，故无法还原。
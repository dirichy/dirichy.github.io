---
tags:
  - Math
  - LCDT
---

# 用控制收敛定理求解下面两道题目：
1. 设 $\lim_{n \rightarrow+\infty} \frac{a_n}{n^2}=a, \lim _{n \rightarrow+\infty} \frac{b_n}{n^2}=b$. 证明极限 $\lim _{n \rightarrow+\infty} \frac{1}{n^5} \sum_{k=0}^n a_k b_{n-k}$ 存在并求其值.

1. 设 $\lim _{n \rightarrow+\infty} \beta_n=0$, 函数 $f$ 在 $[-1,2]$ 上有界, 在 $[0,1]$ 上 Riemann 可积, 证明： $\lim _{n \rightarrow+\infty} \frac{1}{n} \sum_{k=1}^n f\left(\frac{k}{n}+\beta_n\right)=\int_0^1 f(x) d x$.

# 证明：
1. 构造$[0,1]$上的函数序列$f_n$如下：
$$f_n(x)=\frac{a_{[nx]}b_{n-[nx]}}{n^4}$$
对于$0<x<1$我们有:
$$\lim_{n\to\infty}\frac{a_{[nx]}}{n^2}=\lim_{n\to\infty}\frac{a_{[nx]}}{[nx]^2}\frac{[nx]^2}{n^2}=ax^2$$.
同理$\lim_{n\to\infty}\frac{b_{n-[nx]}}{n^2}=b(1-x)^2$. 
故$f_n(x)\to abx^2(1-x)^2, x\in(0,1)$. 
注意到$|f_n(x)|\leq \sup\{\frac{a_m}{m^2}\}\sup\{\frac{b_m}{m^2}\}$可积，故满足控制收敛定理的条件。且有$\int_0^1 f_{n+1}(x) dx=\frac{1}{(n+1)^5}\sum_{k=0}^{n}a_kb_{n-k}$。故$\lim_{n\to\infty}\frac{1}{n^5}\sum_{k=0}^{n}a_kb_{n-k}=\lim_{n\to\infty}\int_0^1f_n(x) dx=\int_0^1 f(x) dx=\frac{ab}{30}$.

1. 构造$[0,1]$上的函数序列$f_n$如下：
$$f_n(x)=f\left(\frac{[nx]+1}{n}+\beta_n\right)$$
由$f(x)$黎曼可积知其几乎处处连续，且在$[0,1]$中$f$的连续点处显然有$\lim_{n\to\infty}f_n(x)=f(x)$. 
又$f$有界，且$\exists N,\forall n<N,\beta_n\in(-1,1)$，故$|f_n(x)|<\sup_{x\in[-1,2]}|f(x)|$从某项开始一致有界，故满足控制收敛定理的条件。
从而$\lim_{n\to\infty}\frac{1}{n}\sum_{k=1}^nf\left(\frac{k}{n}+\beta_n\right)=\lim_{n\to\infty}\int_0^1f_n(x) dx=\int_0^1f(x) dx$. 
# 点评
这两道题目其实都可以用数学分析的方法来解答（留作习题读者自证），但是过程会非常繁琐。

通过构造函数列的方法将欲求的数列的每一项看成某个函数的积分，从而使用控制收敛定理，是一个非常巧妙的方法。
控制收敛定理仿佛“吸收”了证明的难点，让我们的证明变得非常简洁。
这也从侧面说明了勒贝格积分理论的优越性。

读者可以通过控制收敛定理来证明stolz定理来练手。

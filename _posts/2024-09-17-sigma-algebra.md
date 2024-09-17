---
title: 处理Sigma代数的强大方法
tags:
  - Math
  - Sigma 代数
  - 集合论
---

# 问题

设$\Omega$是一不可数集，$\mathcal{F}$是包含$\Omega$中一切单点集的最小$\sigma-$代数，则$\Delta:=\{(\omega,\omega):\omega \in \Omega\}\notin \mathcal{F}\times\mathcal{F}$.

# 证明

令$$\mathcal{A}:=\{A\in\mathcal{F}:|A|\leq\aleph_0\vee|A^c|\leq\aleph_0\}$$
下证$\mathcal{F}=\mathcal{A}$
1. $\mathcal{F}\supset\mathcal{A}\supset\{{\omega}:\omega\in\Omega\}$显然成立. 
1. 要证$\mathcal{A}\supset\mathcal{F}$, 只需证$\mathcal{A}$为$\sigma$-代数.

    1. $|\Omega^c|=|\emptyset|\leq\aleph_0$, 故$\Omega\in\mathcal{A}$
    1. $\forall A\in\mathcal{A}$,若$|A|\leq\aleph_0$, 则$|(A^c)^c|=|A|\leq\aleph_0$,故$A^c\in\mathcal{A}$；若$|A^c|\leq\aleph_0$，则显然也有$A^c\in\mathcal{A}$. 
    1. $\forall A_n\in\mathcal{F}, n\in\N$, $I_1=\{n:|A_n|\leq\aleph_0\},I_2=\{n:|(A_n)^c|\leq\aleph_0\}$, 若$I_2=\emptyset$, 则$|\bigcup_{n\in\N}A_n|\leq\aleph_0$; 若$I_2\neq\emptyset$, 则$|\bigcup_{n\in\N}A_n|=|\bigcap_{n\in\N}(A_n)^c|\leq|\bigcap_{n\in I_2}(A_n)^c|\leq\aleph_0$. 
定义映射$f:\mathcal{F}\to\mathcal{F}$
$$
f(A):=\left\{
	\begin{array}{lcl}
	A \quad &,&|A|\leq\aleph_0\\
	A^c \quad &,&|A^c|\leq\aleph_0\\
	\end{array}
	\right.
$$
显然有$f(A)=f(A^c),f(A\cup B)\subset f(A)\cup f(B), f(A\cap B)\subset f(A)\cup f(B)$. 对$A\subset \Omega\times\Omega,\omega_1\in \Omega$，定义$A(\omega_1):=\{\omega_2\in\Omega:(\omega_1,\omega_2)\in A\}$. 令$$\Lambda:=\{A\in\mathcal{F}\times\mathcal{F}:|\bigcup_{\omega_1\in\Omega}f(A(\omega_1))|\leq\aleph_0\}$$
下证$\Lambda=\mathcal{F}\times\mathcal{F}$. 显然$\Lambda$为$\pi-$系。由单调类定理，我们只需要证明其包含可测矩形，且其为$\lambda-$系. 
1. 令$\mathcal{C} :=\{A_1\times A_2 :A_i\in\mathcal{F},i=1,2\}$
$\forall A\in\mathcal{C},\exists\A_i\in\mathcal{F},i=1,2,A=A_1\times A_2,\forall \omega_1\in\Omega$, 
$$
f(A(\omega_1))=\left\{
	\begin{array}{ccl}
	f(A_2) &,&\omega_1\in A_1\\
	\emptyset&,&\omega_1\notin A_1
\end{array}
	\right.
$$
则$\bigcup_{\omega_1\in\Omega}f(A(\omega_1))=f(A_2)$，
从而我们有$|\bigcup_{\omega_1\in\Omega}f(A(\omega_1))|\leq\aleph_0$. 故$\mathcal{C}\subset\Lambda$
1. 下证$\Lambda$为$\lambda -$系.
    1. $\Omega\times\Omega\in\mathcal{C}\in\Lambda$
    1. $B,C\in\Lambda,C\subset B$，
$$
\begin{aligned}
&\Big|\bigcup_{\omega_1\in\Omega}f((B\minus C)(\omega_1))\Big|\\
\leq&\Big|\bigcup_{\omega_1\in\Omega}\left(f(B(\omega_1))\cup f(C(\omega_1))\right)\Big|\\
=&\Big|\Big(\bigcup_{\omega_1\in\Omega}f(B(\omega_1))\Big)\cup\Big(\bigcup_{\omega_1\in\Omega}f(C(\omega_1))\Big)\Big|\leq\aleph_0\\
\end{aligned}
$$
    1. $B_n,n\in\N,B_n\subset B_{n+1}$
$\forall \omega_1\in\Omega$, 若$\exists n:B_n(\omega_1)$不可数，不妨设$n=1$,则
$$
\begin{aligned}
&|f((\bigcup_{n\in\N}B_n)(\omega_1))|=|\bigcap_{n\in\N}(B_n(\omega_1))^c|\\
\leq&|B_1(\omega_1)^c|=|f(B_1(\omega_1))|\leq|\bigcup_{n\in\N}f(B_n(\omega_1))|
\end{aligned}
$$
若$\forall n:B_n(\omega_1)$可数,
$$
\begin{aligned}
&|f((\bigcup_{n\in\N}B_n)(\omega_1))|=|\bigcup_{n\in\N}(B_n(\omega_1))|
\end{aligned}
$$ 
$\begin{aligned}
\text{故}&|\bigcup_{\omega_1\in\Omega}f((\bigcup_{n\in\N}B_n)(\omega_1))|\\
\leq&|\bigcup_{\omega_1\in\Omega}\bigcup_{n\in\N}f(B_n(\omega_1))|\\
=&|\bigcup_{n\in\N}\bigcup_{\omega_1\in\Omega}f(B_n(\omega_1))|=\aleph_0
\end{aligned}$\\
即
$\bigcup_{n\in\N}B_n\in\Lambda$.

故$\Lambda=\sigma(\mathcal{C})=\mathcal{F}\times\mathcal{F}$
而$\bigcup_{\omega_1\in\Omega}f(\Delta(\omega_1))=\bigcup_{\omega_1\in\Omega}\{\omega_1\}=\Omega$, 不可数集，故$\Delta\times\Delta\notin\mathcal{F}\times\mathcal{F}$.

# 点评

一个小的集合$A$关于某种运算$\Gamma$取闭包得到一个大的集合$\Gamma(A)$是我们常见的一种定义集合的手段，例如$\{0\}$加上对后继运算的封闭性得到自然数集，$\R$的子集加上极限运算得到其闭包等。但这种定义通常都是非构造性的，因此要判断一个元素是否在我们生成的集合中就是一件比较困难的事情。通常我们的做法是用一个性质$p$来把一个元素和这个闭包“分离”。这个性质$p$必须满足对$\Gamma$运算的封闭性，也即满足性质$p$的元素的运算结果也满足性质$p$，这样的话根据单调类我们只需要验证$A$中元素满足性质$p$就可以得到$\Gamma(A)$中的元素也满足性质$p$。从而最终得到不满足性质$p$的元素不在$\Gamma(A)$中。例如我们要证明$\R/\Q$的代表元集$T$不是Borel集，而Borel集是开集对可数并以及余集的闭包，故我们需要寻找一个性质$p$能关于可数并和余集封闭，而且开集满足这个性质，而且$T$不满足这个性质。在这个例子中我们可以取性质$p$为可测。通常情况下要寻找一个合适的性质来分离闭包和某个特定的元素并不容易，但是这几乎是我们唯一能做的证明一个元素不在运算生成的闭包中的方法。

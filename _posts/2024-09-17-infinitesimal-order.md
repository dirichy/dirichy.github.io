---
title: 无穷小量的偏序关系的结构
tags:
  - Math
  - 数学分析
---

# 问题

对于两个非$$0$$无穷小量$$\{a_n\},\{b_n\}$$，定\\义$$\{a_n\}\prec \{b_n\}\iff\{\frac{a_n}{b_n}\}$$为无穷小量。
	1. $$\{a_n\}$$为非零无穷小量，求证：$$\exists\{b_n\},\{c_n\}\text{为非零无穷小量},\{b_n\}\prec \{a_n\}\prec\{c_n\}$$.
	1. $$\forall i\in\{0,1,\cdots N\},\{a_{i,n}\}$$为非零无穷小量，求证：$$\exists\{b_n\},\{c_n\}$$为非零无穷小量，$$\forall i\in\{0,1,\cdots N\},\{b_n\}\prec \{a_{i,n}\}\prec\{c_n\}$$.
	1. $$\forall i\in\mathbb{N},\{a_{i,n}\}$$为非零无穷小量，求证：$$\exists\{b_n\},\{c_n\}\text{为非零无穷小量},\forall i\in\mathbb{N},\{b_n\}\prec \{a_{i,n}\}\prec\{c_n\}$$.

# 证明

1. 取$$b_n=a_n^2,c_n=\sqrt{\vert a_n\vert }$$即可. 
1. 取$$b_n=\prod_{i=0}^{N}a_{i,n}^2,c_n=\sum_{i=0}^{N}\sqrt{\vert a_{i,n}\vert }$$即可.
1. 令$$S_n=\inf\{k:\vert a_k\vert \geq 1\}\cup\{n\},T_n=\sup\{k:-1\leq k\leq n,\sum_{i=0}^{k}\sqrt{\vert a_{i,n}\vert }<\frac1k\}$$. 令$$b_n=\prod_{i=0}^{S_n}a_{i,n}^2,c_n=\sum_{i=0}^{T_n}\sqrt{\vert a_{i,n}\vert }$$. 下证它们满足条件. 显然$$S_n,T_n$$是良定义的，我们首先证明$$\lim S_n=\infty,\lim T_n=\infty$$. $$\forall k\in\mathbb{N}$$，我们有$$\lim_{n\to\infty}\displaystyle\max_{0\leq i\leq k}\vert a_{i,n}\vert =0$$，故$$\exists N\in\mathbb{N},\forall m>N,\displaystyle\max_{0\leq i\leq k}\vert a_{i,m}\vert <1$$. 不妨设$$N>k$$，则$$\forall m>N,\forall i\leq k,\vert a_{i,m}\vert <1$$. 故$$S_m\geq k$$. 从而有$$\lim S_n=\infty$$. $$\forall k\in\mathbb{N}$$，我们有$$\lim_{n\to\infty}\sum_{i=0}^{k}\sqrt{\vert a_{i,n}\vert }=0$$，故$$\exists N\in\mathbb{N},\forall m>N,\sum_{i=0}^{k}\sqrt{\vert a_{i,n}\vert }<\frac1k$$. 不妨设$$N>k$$，则$$\forall m>N,T_m\geq k$$. 从而有$$\lim T_n=\infty$$. 接下来我们来证明$$\forall i\in\mathbb{N},\{b_n\}\prec \{a_{i,n}\}\prec\{c_n\}$$. $$\exists N\in\mathbb{N},\forall n>N,S_n,T_n>i$$. 故$$\lim\frac{\vert a_{i,n}\vert }{c_n}=\lim\frac{\vert a_{i,n}\vert }{\sum_{t=0}^{T_n}\sqrt{\vert a_{t,n}\vert }}\leq\lim\frac{\vert a_{i,n}\vert }{\sqrt{\vert a_{i,n}\vert }}=0$$. $$\lim\frac{b_n}{\vert a_{i,n}\vert }=\lim\frac{\prod_{t=0}^{S_n}a_{t,n}^2}{\vert a_{i,n}\vert }\leq\lim\frac{a_{i,n}^2}{\vert a_{i,n}\vert }=0$$. 故$$\{b_n\},\{c_n\}$$符合题意. 

# 点评

这道题目研究了非零无穷小序列关于收敛速度成的偏序关系，三个小问的结论逐步加强。第一小问这个偏序中没有极大元，也没有极小元；第二小问说明有限集总有界；第三小问说明即使是可数集也总是有界的。这与$$\mathbb{R}$$中的序关系很不一样，可数集$$\mathbb{Z}$$在$$\mathbb{R}$$中是无界的。这说明这个偏序关系有相当复杂的结构。由于极限的收敛可以用差为无穷小来描述，而级数的收敛也是极限的收敛，故这个问题实际上说明，在正项级数敛散性判别中，即使选取可数个收敛的正项级数，也一定有一个级数收敛的比它们更慢。故用可数个正项级数做比较不可能判定所有正项级数的敛散性，这说明级数的敛散性是很复杂的一件事情。

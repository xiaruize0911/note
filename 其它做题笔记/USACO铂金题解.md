# USACO 铂金题解

## USACO 2018 Platium

### B. Sort It Out

很巧妙的转换

注意到操作并不会影响没有被选中的牛的相对顺序

所以没有被选中的一定单调递增

要使得选中的尽可能少，就要选尽可能长的没有被选中的序列，即原序列的 $LIS$

所以原题等价于求原序列第 $k$ 大 $LIS$

用树状数组维护前缀最大值和最大值的方案数

### C. The Cow Gathering

先考虑怎么找到一个合法的点，显然可以拓扑排序

建图 每次当一个点 $deg=1$ 就加入队列 最后剩下的一个点一定合法

回到原图，每个 $(u,v)$ 的限制条件相当于一条 $u \rightarrow v$ 的边

发现，当以 $u$ 下方的一个点为root时必然会存在环，即不合法

所以可以从一个合法的出发dfs, 所有 $u$ 即其子树都不合法，其他合法

### D. Lifeguards

明显可以先把被别的包含的区间去掉，这样一定更优

此时按区间左端点排序同时可以保证右端点递增

考虑dp， $dp_{i,j}$ 表示到第 $i$ 个区间，删除了 $j$ 个区间，并且保证 $i$ 没有被删除的最大覆盖长度

暴力可以 $\mathcal{O(n\times k^2)}$ 转移

考虑转移分为两种

1. 前一个区间的右端点小于当前的左端点，可以直接维护一个max
2. 否则发现转移的时候，可以考虑把式子拆开，此时要维护的是前缀 $dp_{i,j}-r_i$ 的max，这个明显可以单调队列维护

### F. Sprinklers

令 $a_x$ 表示第 $x$ 行的喷头位置

首先，发现每一行必定连续一段合法，且 $L_i = \min \limits_{x=1}^i a_x, R_i=\max\limits_{x=i}^n a_x$

可以 $\mathcal{O(n)}$ 求出每一列对应的上界记为 $top_y$

形式化转换原问题位以下式子

$$
\sum\limits_{i=1}^{n}\sum\limits_{l=L_i}^{R_i}\sum\limits_{r=l+1}^{R_i}\sum\limits_{j=top_l}^{i-1}1 

= \sum\limits_{i=1}^{n}(\frac{(R_i-L_i+1)\times(R_i-L_i)}{2}\times i - \sum\limits_{j=L_i}^{R_i-1}top_j\times R_i + \sum\limits_{j=L_i}^{R_i-1}j \times top_j)

$$

可以前缀和优化到 $\mathcal{O(n)}$

### G. Out of Sorts

将排序的过程看做是 $i$ 以后比 $a_i$ 小的值向前移动至 $i$ 之前的过程

实际上我们要考虑的就是这个会进行多少次

会发现每次排序，后面比 $a_i$ 小的值总是前进一个

所以就是要求在 $a_i$ 后面比 $a_i$ 小的最远的数的位置

考虑答案怎么求，发现每相邻两个数对答案的贡献为两个数的 $t_i$ 的 max

因为只有当两边都合法时，这个分隔才不会再被调用

### I. Disruption

树剖裸题

## USACO 2020 Platium

### A. Non-Decreasing Subsequences

cdq分治

$$
dp_{i,j}=
\begin{cases}
    前缀以j结尾的序列个数 , i > mid\\
    后缀以j结尾的序列个数 , i\leq mid\\
\end{cases}
$$

考虑怎么转移，

对于 $i\leq mid$

$$
dp_{i,j}= [a_j==i]+\sum\limits_{j<k, a_k>a_j} dp_i,k
$$

对于 $i > mid$

$$
dp_{i,j}= [a_j==i]+\sum\limits_{mid\leq k \leq j, a_k<a_j} dp_i,k
$$

可以用树状数组维护前缀和 直接转移即可，转移完做一个前缀和，因为上面的式子转移的是正好为 $j$

询问就乘法原理，从前后分别选一个拼接，也可以前缀和优化

### B. Falling Portals

考虑出发点的高度与终点的高度，贪心的来说，当终点在起点下方时，必然应该一直向下落速度更快的地方走，反之同理，下文只考虑第一种情况，另一种同理差不多

此时，以 $x$ 轴为时间， $y$ 轴为高度，把图画出来，发现所有的传送都发生在交点上，且对于每一个询问，交点恰好是一个上凸包

单调队列维护这个上凸包以后，每次取斜率尽可能小的，在凸包上二分即可(因为答案必然为凸包上一点)

时间复杂度 $\mathcal{O}(n\log{n})$

~~感觉还是不太懂，挖坑待填~~

### C. Delegation

显然要二分，考虑当前要check的长度是 $w$

对于每个点，用一个 ``multiset`` 维护所有的儿子向上的那条链的长度

每次去除最小的一条，选择最小的可以使这条链达到 $w$ 的链与这条链拼接

最后每个点只能向上一条链(可以长度为0)，直接判断即可

时间复杂度 $\mathcal{O}(n\log{n})$

### D. Help Yourself

考虑 $k=1$ 的情况，用线段树维护区间和，单点加，区间乘就行

考虑怎么计算 $k\neq 1$ 的情况，可以二项式拆开，每个节点都维护 $0 \leq i \leq k$ 下的答案

具体来说对于一个区间 $[l,r]$ ， 有以下 $3$ 种转移

1. 将 $dp_i +1, i\in[0,l-1]$ 转移到 $dp_r$
2. 将 $dp_i, i\in [l,r]$ 转移到 $dp_r$
3. 将 $dp_i, i\in[r+1,2N]$ 都乘 $2$
   
   
### E. Sprinklers 2: Return of the Alfalfa

手摸一下发现最后的分界线一定是一条从左上到右下的折线

考虑对这个折线``dp`` 

``dp[i][j][0/1]`` 表示当前考虑到 $(i,j)$ ``0`` 表示当前的折线是向右的 ``1`` 即表示当前折线是向下的

$$
dp_{i,j,0}= dp_{i,j-1,0} + [s_{i,j}\ is\  empty] \times dp_{i,j-1,1}
$$

对于 $1$ 的转移同理

### H. Sleeping Cows

把两个并在一起排序

``dp[i][j][0/1]`` 表示考虑到第 $i$ 项，剩下 $j$ 头牛确定要住进棚子但是没有确定进哪一个，当前有没有跳过牛的方案数

转移分为两种，对于当前点为牛

$$

\begin{aligned}

dp_{i,j,0} &= dp_{i-1,j-1,0}\\

dp_{i,j,1} &= dp_{i-1,j,0}+dp_{i-1,j,1}+dp_{i-1,j-1,1}

\end{aligned}


$$

对于当前点为牛棚 

$$

\begin{aligned}

dp_{i,j,0} &= dp_{i-1,j+1,0}\times (j+1)+dp_{i-1,j,0} \\

dp_{i,j,1} &= dp_{i-1,j+1,1}\times (j+1)

\end{aligned}

$$

注意开 ``long long`` 会``MLE``
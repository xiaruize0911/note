# DP

by xiaruize

跟着 [Troverld](https://www.luogu.com.cn/blog/Troverld/dp-shua-ti-bi-ji) 刷的一些 $dp$ 题

## [P4046 [JSOI2010] 快递服务](https://www.luogu.com.cn/problem/P4046)

朴素的 $dp$ 状态是 ``dp[i][j][k][p]`` 表示考虑到第 $i$ 个需求，三个货车分别在 $j,k,p$ 的最小代价

发现其实必然有一个货车在 $a_i$ 上，所以 ``dp`` 可以去掉一维

即 ``dp[i][j][k]`` 表示考虑到第 $i$ 个需求，除了在 $a_i$ 上的货车，剩下两个在 $j,k$ 的最小代价

状态数为 $\mathcal{O(nm^2)}$ 转移是线性的 空间可以把第一维滚动

## [P2518 [HAOI2010] 计数](https://www.luogu.com.cn/problem/P2518)

枚举前面几位是一样的，再枚举第一位不一样的填几，后面的直接组合算

感觉不是很有 ``dp``

## [P4158 [SCOI2009] 粉刷匠](https://www.luogu.com.cn/problem/P4158)

``dp[i][j][k][0/1]`` 表示到 $(i,j)$ 用了 $k$ 次操作，当前颜色是否正确 下的最优解

转移只需要关心前一个格子的颜色（需要特判每一行的开头）

时间复杂度 $\mathcal{O(NMT)}$

## [P4302 [SCOI2003] 字符串折叠](https://www.luogu.com.cn/problem/P4302)

明显的区间dp题，转移只需要对因子判断循环即可，时间复杂度是 $\mathcal{O(n^3\log{n})}$

## [P4290 [HAOI2008] 玩具取名](https://www.luogu.com.cn/problem/P4290)

状压 考虑``dp[i][j]=msk` 表示 $[i,j]$ 可以转换到的字母 $\mathcal{O(n^3)}$ 转移即可

## [P2167 [SDOI2009] Bill的挑战](https://www.luogu.com.cn/problem/P2167)

``dp[i][msk]`` 表示到第 $i$ 位，当前匹配的状态为 $msk$ 的方案数

暴力比较转移会 ``TLE`` 可以考虑预处理一个 ``pre[i][j]`` 表示第 $i$ 位放 $j$ 这个字母能匹配的串的状态

这样时间复杂度就是 $\mathcal{O}(26M2^n)$ ， 其中 $M$ 为最长的串的长度

## [CF149D Coloring Brackets](https://www.luogu.com.cn/problem/CF149D)

``dp[l][r][0/1/2][0/1/2]`` 表示当前区间为 $[l,r]$ 是一个完整的括号序列，左侧染色的状态 右侧染色的状态 下的方案数

转移考虑两种

1. 当 $S_l,S_r$ 是一对在原序列中匹配的括号时，计算 $[l+1,r-1]$
2. 否则递归计算 $[l,match_i],[match_i+1,r]$

合并可以直接枚举

## [P4448 [AHOI2018初中组] 球球的排列](https://www.luogu.com.cn/problem/P4448)

妙妙题啊 两个部分都挺智慧的！

Part 1

首先，发现当 $(x,y)$ 满足 $xy$ 为平方数，且 $(x,z)$ 也满足时，$(y,z)$ 一定也满足

即可以将原序列分为几组数，每一组都两两相乘为平方数，且不同组两两相乘一定不为平方数

于是问题转化为：

给定一个由几种颜色组成的序列 $a_i$ ，同色有区别，求同样颜色不相邻的排列数

Part 2

考虑怎么求解上面的问题，可以先将原数组按颜色排序

考虑如下 ``dp``状态

``dp[i][j][k]`` 表示当前考虑到第 $i$ 个数，前面有 $j$ 对相邻的同色且与 $a_i$ 不同， $k$ 对同色且与 $a_i$ 相同

转移分类讨论

1. $a_i\neq a_{i-1}$

   此时第三维一定为 $0$

   1. 将当前球放在两个不同色之间

      $$
      dp_{i,j,0} \leftarrow \sum\limits_{k=0}^j dp_{i-1,j-k,k} \times (i-j)
      $$
   2. 将当前球放在两个同色之间

      $$
      dp_{i,j,0} \leftarrow \sum\limits_{k=0}^{j+1} dp_{i-1,j-k+1,k} \times (j+1)
      $$
2. $a_i=a_{i-1}$

   令 $cnt$ 表示到 $i$ 与 $a_i$ 相同的个数

   1. 放在同色球旁边

      $$
      dp_{i,j,k} \leftarrow \sum\limits_{k=1}^{cnt}dp_{i-1,j,k-1} \times (2\times cnt - (k-1))
      $$
   2. 放在两个颜色相同的球中间

      $$
      dp_{i,j,0} \leftarrow \sum\limits_{k=0}^{cnt} dp_{i-1,j-k+1,k} \times (j+1)
      $$
   3. 放在两个颜色不同且与当前不同的球之间

      $$
      dp_{i,j,k} \leftarrow \sum\limits_{k=0}^{cnt}dp_{i-1,j,k} \times (i-(cnt\times 2 -k)-j)
      $$

答案为 ``dp[n][0][0]``

时间复杂度为 $\mathcal{O}(n^3)$

## [P2476 [SCOI2008] 着色方案](https://www.luogu.com.cn/problem/P2476)

上题双倍经验，乘一下阶乘逆元

## [P2160 [SHOI2007] 书柜的尺寸](https://www.luogu.com.cn/problem/P2160)

第很多次做到这个题了 老套路了

首先对于书按照 $h$ 排递减序，这样每层第一个选择就可以确定这一层的高度

暴力 ``dp`` 是 ``dp[i][j][k][l]`` 表示考虑到第 $i$ 本书，三层宽度分别为 $j,k,l$ 时最小的高度

发现 $j,k,l$ 知二求一，故把 $l$ 这一维去掉，变成 ``dp[i][j][k]``

时间复杂度 $\mathcal{O}(n(nt)^2)$

## [P2365 任务安排](https://www.luogu.com.cn/problem/P2365)

斜率优化板子题

## [P5785 [SDOI2012] 任务安排](https://www.luogu.com.cn/problem/P5785)

斜率优化板子题

## [P3299 [SDOI2013] 保护出题人](https://www.luogu.com.cn/problem/P3299)

其实是求 $\max\limits_{j=0}^{i-1} {\frac{S_i-S_j}{x_i+(i-j-1)\times m}}$

可以把这个东西拆成斜率的形式

即 $(S_i,x_i+im)$ 和 $(S_j,m(j+1))$ 的斜率

单调栈维护这个东西 对于每次查询 在单调栈上二分即可

## [P4056 [JSOI2009] 火星藏宝图](https://www.luogu.com.cn/problem/P4056)

将点先排序，$x$ 优先，否则 $y$

此时，注意到按顺序，每个点都可以由他前面的所有 $y$ 小于当前点的点转移过来

观察性质，发现在所有 $y$ 一样的点中，从 $x$ 尽可能大的点转移一定是不劣的，所以可以维护一个数组存每个 $y$ 最大的 $x$

时间复杂度 $\mathcal{O}(nm)$

有点卡常，但是可以吸氧通过

## [P2593 [ZJOI2006] 超级麻将](https://www.luogu.com.cn/problem/P2593)

和上面的一个很类似

``dp[i][j][k][0/1]`` 表示考虑到 $i$ ， 选了 $j$ 个 $i-1$ ， $k$ 个 $i$ ，当前是否有对 是否可能

转移分讨即可

## [P2581 [ZJOI2005] Genotype](https://www.luogu.com.cn/problem/P2581)

和上面一题差不多 状压维护每一段可以合成的字母

多了一个 $dp$ 计算 $[l,r]$ 这个区间最小的原序列长度就行

## [P2317 [HNOI2005] 星际贸易](https://www.luogu.com.cn/problem/P2317)

题目分为两个部分

第一部分是背包板子

第二部分

令 ``dp[i][j]`` 表示到 $i$ 这个位置，有 $j$ 份燃料的最小代价

$$
dp_{i,j} = min(dp_{k,j+2}+f_i,dp_{i,j-1}+p_i)
$$

注意到后一半可以直接转移，而前一半可以单调队列维护

## [P2523 [HAOI2011] Problem c](https://www.luogu.com.cn/problem/P2523)

**多测要清零**

显然编号并不影响答案，其实是要满足这样一个式子

$$
(\sum\limits_{j=1}^n[a_j\geq i] )\leq n-i+1
$$

于是可以处理处 $a_i= n-i+1- (\sum\limits_{j=1}^m[q_j\geq i] )$

令 ``dp[i][j]`` 表示**从后往前**， 考虑到第 $i$ 位， 还有 $j$ 个没有确定的方案数

$$
dp_{i,j-k}=\sum dp_{i+1,j} \times \binom{j}{k}
$$

注意取模后最后 $dp_{1,0}$ 也有可能为 $0$， 要单独判断是否转移过

## [P3830 [SHOI2012] 随机树](https://www.luogu.com.cn/problem/P3830)

对于第一个询问，每个新增的点对答案的贡献为 $\frac{2}{n}$

第二个询问 设 ``dp[i][j]`` 表示 $i$ 个点，深度大于等于 $j$ 的概率

$$
dp_{i,j}=\sum\limits_{k=1}^{i-1}\frac{dp_{k,j-1}+dp_{i-k,j-1}-dp_{k,j-1}\times dp_{i-k,j-1}}{i-1}
$$

可以发现上面是个容斥，表示至少有一边深度大于等于 $j-1$ 的概率，分母上的 $i-1$ 是因为无论原来怎么放，新加入一个点的方案数是一样的，概率也是一样的

## [P2466 [SDOI2008] Sue 的小球](https://www.luogu.com.cn/problem/P2466)

经过必然会收集，这样一定是最优的

所以一定是一段连续的区间，考虑区间 $dp$

``dp[i][j][0/1]`` 表示当前考虑区间 $[i,j]$ ，当前在左/右端点，最小的损耗是多少

转移是 $\mathcal{O}(1)$ 的，只有可能从 $[i+1,j],[i,j-1]$ 这两个区间转移

## [P2462 [SDOI2007] 游戏](https://www.luogu.com.cn/problem/P2462)

考虑建图，对于每个串暴力枚举加入的字母，排序用 ``map`` 查一下即可

拓扑序上直接 ``dp``

## [P4460 [CQOI2018] 解锁屏幕](https://www.luogu.com.cn/problem/P4460)

考虑状压 $dp$ 枚举子集是 $\mathcal{O}(3^n)$ 不能通过

发现转移的时候判断是否可行可以预处理，即枚举 $i,j$ ， 预处理出要使得 $i\rightarrow j$ 需要哪些已经被选中

这样就可以 $\mathcal{O}(1)$ 的转移，时间复杂度 $\mathcal{O}(2^n)$

p.s. 不会计算几何qwq

## [P4728 [HNOI2009] 双递增序列](https://www.luogu.com.cn/problem/P4728)

``dp[i][j]`` 表示考虑到第 $i$ 个数，当前两个序列，一个以 $a_i$ 结尾，另一个以长度为 $j$ 最后一个数最小为 $dp_{i,j}$

转移考虑两种情况 $dp_{i-1,j-1}\rightarrow dp_{i,j}$ 和 $dp_{i-1,i-j} \rightarrow dp_{i,j}$

## [P4495 [HAOI2018] 奇怪的背包](https://www.luogu.com.cn/problem/P4495)

用了点数学，不会证明qwq

对于每个 $V_i$ 发现能表示的数和 $\gcd(V_i,P)$ 是一样的， 所以可以直接把 $V_i$ 换成 $\gcd(V_i,P)$，不会影响答案

考虑选了多个，其能覆盖的重量为 $\gcd(V_i)$ 的倍数

可以预处理 $P$ 的所有约数，在约数上 $dp$

设 ``dp[i][j]`` 表示考虑到第 $i$ 个约数，当前的 $gcd$ 等于第 $j$ 个约数的方案数

$$
dp_{i,j}=dp_{i-1,j}+([i=j]+\sum\limits_{\gcd(p_k,p_i)=p_j} dp_{i-1,k})\times (2^{S_i}-1)
$$

其中，$p_i$ 表示 $P$ 的第 $i$ 个约数， $S_i$ 表示第 $i$ 个约数在 $V_i$ 中出现的次数

最后答案为 $\sum\limits_{v_j|w_i} dp_{n,j}$，

发现 $w_i$ 在这个查询中等价于 $\gcd(w_i,P)$ 所以可以对于 $P$ 的约数预处理答案，这样查询就是 $\mathcal{O}(1)$ 的

## [P6239 [JXOI2012] 奇怪的道路](https://www.luogu.com.cn/problem/P6239)

看到 $k$ 的范围，不难想到对其状压，一个直观的 $dp$ 状态为

``dp[i][j][msk]`` 表示考虑前 $i$ 个点，连了 $j$ 条边，最后 $k$ 个点的连边奇偶数状态为 $msk$ 的方案数

$$
dp_{i,j,msk}=dp_{i-1,j,msk\times 2}+\sum\limits_{x=max(i-k,1)}^{i-1}\sum\limits_{tmp=0}^{2^{k+1}-1} dp_{i,j-1,tmp\oplus 1\oplus 2^{i-x}}
$$

## [P4574 [CQOI2013] 二进制A+B](https://www.luogu.com.cn/problem/P4574)

``dp[i][j][k][p][0/1]`` 表示考虑到第 $i$ 位， $a,b,c$ 分别用了 $j,k,p$ 个 $1$ ， 当前位是否进位下， $c$ 的最小值

暴力转移即可

最后判 $-1$ 注意特判


## [[IOI2005] Riv 河流](https://www.luogu.com.cn/problem/P3354)

第一眼的 $dp$ 状态为 ``dp[i][j]`` 表示以 $i$ 为根的子树建立 $j$ 个的最小代价，但是发现并不好转移，因为最近的厂在哪是不确定的

于是更改 $dp$ 状态为 ``dp[i][j][k]`` 表示以 $i$ 为根的子树，最近的有厂的祖先是 $j$(不包括 $i$ 自身)， 建了 $k$ 个厂的最小代价

转移需要先分类讨论当前点上是否有建厂 然后需要将贡献都统计到 $dp$ 中

具体的，就是在每一个点做背包 $dp$ ，然后将当前是否建厂的结果取 $min$ 并加上当前点到最近祖先的代价

## [CF1067A Array Without Local Maximums](https://www.luogu.com.cn/problem/CF1067A)

第一眼想法:

``dp[i][j][0/1]`` 表示考虑到 $i$ ，$a_i=j$ ，$[a_i\leq a_{i-1}]$  的方案数

但是发现这个状态不方便转移，因为没法处理 $=$ 的情况，所以考虑改变 $dp$ 状态，将最后一位改成 ``[0/1/2]`` 分别表示 $a_i<a_{i-1},a_i=a_{i-1},a_i>a_{i-1}$ 的方案数，转移可以前缀和优化做到 $\mathcal{O}(200)$

最终时间复杂度为 $\mathcal{O}(200n)$

## [Connecting Vertices](https://www.luogu.com.cn/problem/CF888F)

``dp[i][j][0/1]`` 表示 $[i,j]$ 这个区间， $i$ 到 $j$ 强制连边/强制不连边的方案数

## [CF1088E Ehab and a component choosing problem](https://www.luogu.com.cn/problem/CF1088E)

分成两部分做 先求最大值

最大值一定可以只选一块 所以可以 ``dp[x]`` 表示 $x$ 的子树中，强制选择 $x$ 下的最大值

然后可以再做一次同样的 $dp$， 在每次值为最大值时将 $dp_x$ 清零, $cnt+1$

## [P4362 [NOI2002] 贪吃的九头龙](https://www.luogu.com.cn/problem/P4362)

``dp[i][j][0/1]`` 表示当前在 $i$ 点，子树内有 $j$ 个是大头吃的， 当前点是不是大头吃的

转移要对 $m=2$ 和 $m>2$ 分讨

## [CF906C Party](https://www.luogu.com.cn/problem/CF906C)

``dp[msk]``  表示这些人互相认识的最小代价，转移直接暴力即可，记录前缀输出方案

## [CF11D A Simple Task](https://www.luogu.com.cn/problem/CF11D)

考虑环一定由一条路经和一个回到起点的边组成

``dp[msk][i]`` 表示路经包括 ``msk`` 里的点，并且终点在 ``i`` 的方案数，每次转移考虑将终点向后移，不可以回到当前的起点(lowbit(msk))之前

再枚举当前终点时判一下能不能回到起点即可

## [CF24D Broken robot](https://www.luogu.com.cn/problem/CF24D)

保留 $4$ 位小数！

所以可以暴力执行 $100$ 次普通 $dp$ ，这样误差较小，即可通过

## [Dead Ends](https://www.luogu.com.cn/problem/CF53E)

$n\leq 10$ 考虑状压

``dp[msk][s]`` 表示当前连接了 ``msk`` 里的点，其中 ``s`` 中的点是叶子结点的方案数

采用刷表法，每次拿一个点向后转移之前，要先将当前点的 $dp$ 值除以叶子个数，因为会有重复转移

## [P2750 [USACO5.5] 贰五语言Two Five](https://www.luogu.com.cn/problem/P2750)

正序考虑每个数 ``dp[a][b][c][d][e]``  表示5行每行分别填了 $a,b,c,d,e$ 个数的方案数

当 $a>b>c>d>e$ 时满足题意 可以记忆化搜索

## [CF261D Maxim and Increasing Subsequenc](https://www.luogu.com.cn/problem/CF261D)

注意到，答案的上线是 $a$ 中不同元素的个数，且当这个个数 $\leq t$ 必然可以取到

剩下考虑对 $a$ 离散化，然后暴力转移即可，时间复杂度是 $2\times 10^7$ 级别的

## [P4766 [CERC2014] Outer space invaders](https://www.luogu.com.cn/problem/P4766)

对于端点离散化， ``dp[l][r]`` 表示搞定所有被区间 $[L,R]$ 完全包含的最小代价

每次，对于一个区间，最后一次操作一定可以等于这个区间中的距离最大值，所以可以确定最后消灭的是哪个

在这个的限制下枚举端点，区间 ``dp`` 即可

## [CF730J Bottles](https://www.luogu.com.cn/problem/CF730J)

第一问显然贪心

第二问 ``dp[i][j]`` 表示选了 $i$ 个，当前的容量和为 $j$ 时，选定的瓶子里已有液体的最大值

暴力转移即可，时间复杂度 $O(N^4)$
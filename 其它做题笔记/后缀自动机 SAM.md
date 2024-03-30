#后缀自动机SAM #算法

SAM满足如下性质
1. 有向无环图 每个转移只有一个字符
2. 接受且只接受 $S$  的后缀
3. 节点数在满足上述条件下最小

考虑不满足性质 $3$，那么 $trie$ 就可以做到

将这个 $trie$ 建出来后，发现有很多完全相同的子树

定义 $endpos(s)$ 表示串 $s$ 在 $S$ 中所有匹配的结束位置的集合，对于所有的 $endpos(a)=endpos(b)$ ，可以考虑把这两个点合起来 **$endpos$ 的数量是 $\mathcal{O}(n)$ 的** ，可以发现每个集合都是由一个更大的集合划分得到的，形成一个树形结构，每个节点表示一个 $endpos$ ，一般将这个树称为 $parent\ tree$，这棵树在满二叉树时达到 $2n-1$ 个点

对于构建，维护 $nxt_{ch}$ 表示当前状态接受 $ch$ 后转移到的状态，$lnk$ 为当前节点在 $parent\ tree$ 上的父亲节点，$len$ 表示当前这个 $endpos$ 下最长的串的长度，$last$ 表示加入之前的状态

对于插入一个字符 $ch$ ，新建一个节点 $cur$，然后沿着 $lnk$ 向上找，如果一个点不能向 $ch$ 转移，那么这个点的 $nxt_{ch}=cur$ 

- 如果到根都没有找到，那么将根指向 $cur$ 即可
- 若找到的点为 $p$ ，$p$  的 $nxt_{ch}=q$ 
	- 若 $len_p+1=len_q$，那么 $lnk_{cur}=q$  即可
	- 否则就说明当前转移不连续，需要将它拆开，将所有 $p$ 即以上的可以通过 $ch$ 转移的节点连向一个新的节点 $r$ ，然后将 $r$ 指向 $q$ 和 $cur$ 

```cpp
namespace SAM {
const int MAXM = 1e6 + 5;
struct State {
    int fa, len, next[26];
} sam[MAXM];
int cnt = 1, last = 1;
void insert(int ch) { // 插入时要-'a'（或其他）
    int cur = ++cnt, p;
    sam[cur].len = sam[last].len + 1;
    for (p = last; p && !sam[p].next[ch]; p = sam[p].fa)
        sam[p].next[ch] = cur;
    int q = sam[p].next[ch];
    if (q == 0) {
        sam[cur].fa = 1;
    } else if (sam[p].len + 1 == sam[q].len) {
        sam[cur].fa = q;
    } else {
        int r = ++cnt;
        sam[r] = sam[q];
        sam[r].len = sam[p].len + 1;
        for (; p && sam[p].next[ch] == q; p = sam[p].fa)
            sam[p].next[ch] = r;
        sam[cur].fa = sam[q].fa = r;
    }
    last = cur;
}
} // namespace SAM
```
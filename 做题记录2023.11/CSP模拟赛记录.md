# CSP模拟赛记录

## 2023.10.15

### A. 超速

题面有点绕 差评

直接二分

<details>
<summary> 可爱的code捏 </summary>

```cpp
// Author: xiaruize
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define ull unsigned long long
#define ALL(a) (a).begin(), (a).end()
#define pb push_back
#define mk make_pair
#define pii pair<int, int>
#define pis pair<int, string>
#define sec second
#define fir first
#define sz(a) int((a).size())
#define Yes cout << "Yes" << endl
#define YES cout << "YES" << endl
#define No cout << "No" << endl
#define NO cout << "NO" << endl
#define debug(x) cerr << #x << ": " << x << endl
#define mms(arr, n) memset(arr, n, sizeof(arr))
#define rep(i, a, n) for (int i = (a); i <= (n); ++i)
#define per(i, n, a) for (int i = (n); i >= (a); --i)
int max(int a, int b) {
    if (a > b)
        return a;
    return b;
}
int min(int a, int b) {
    if (a < b)
        return a;
    return b;
}
void chkmx(int &x, int y) {
    if (y > x)
        x = y;
}
void chkmi(int &x, int y) {
    if (y < x)
        x = y;
}
const int INF = 0x3f3f3f3f3f3f3f3f;
const int MOD = 1000000007;
const int N = 2e5 + 10;

// bool st;
int n;
int v[N], l[N];
int m;
int a[N], f[N];
int q;
int lim;
// bool en;

bool check(int x) {
    double t = 0;
    rep(i, 1, n) { t = t + (double)l[i] / (v[i] + a[x]); }
    return t < lim;
}

void solve() {
    int s, t;
    cin >> s >> t;
    lim = t - s;
    int l = 0, r = m;
    while (l < r) {
        int mid = l + r >> 1;
        if (check(mid))
            r = mid;
        else
            l = mid + 1;
    }
    cout << f[r] << endl;
}

signed main() {
    freopen("speed.in", "r", stdin);
    freopen("speed.out", "w", stdout);
    // cerr<<(&en-&st)/1024.0/1024.0<<endl;
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int testcase = 1;
    cin >> n;
    rep(i, 1, n) cin >> v[i];
    rep(i, 1, n) cin >> l[i];
    cin >> m;
    a[m + 1] = 1e9;
    rep(i, 1, m - 1) cin >> a[i];
    rep(i, 1, m) cin >> f[i];
    cin >> testcase;
    while (testcase--) solve();
    return 0;
}
```

</details>

### B. 对常规的斗争

二次差分板子题 挺显然的

<details>
<summary> 可爱的code捏 </summary>

```cpp
// Author: xiaruize
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define ull unsigned long long
#define ALL(a) (a).begin(), (a).end()
#define pb push_back
#define mk make_pair
#define pii pair<int, int>
#define pis pair<int, string>
#define sec second
#define fir first
#define sz(a) int((a).size())
#define Yes cout << "Yes" << endl
#define YES cout << "YES" << endl
#define No cout << "No" << endl
#define NO cout << "NO" << endl
#define debug(x) cerr << #x << ": " << x << endl
#define mms(arr, n) memset(arr, n, sizeof(arr))
#define rep(i, a, n) for (int i = (a); i <= (n); ++i)
#define per(i, n, a) for (int i = (n); i >= (a); --i)
int max(int a, int b) {
    if (a > b)
        return a;
    return b;
}
int min(int a, int b) {
    if (a < b)
        return a;
    return b;
}
void chkmx(int &x, int y) {
    if (y > x)
        x = y;
}
void chkmi(int &x, int y) {
    if (y < x)
        x = y;
}
const int INF = 0x3f3f3f3f3f3f3f3f;
const int MOD = 1000000007;
const int N = 2e5 + 10;

// bool st;
int n;
int a[N], s[N];
map<int, int> mp;
// bool en;

void solve() {
    cin >> n;
    rep(i, 1, n) {
        cin >> a[i];
        mp[a[i]] = 0;
    }
    rep(i, 1, n) {
        int t = i - mp[a[i]];
        s[1]++;
        s[t + 1]--;
        // cerr << t << endl;
        t = n - i + 1;
        s[t + 1]--;
        s[n - mp[a[i]] + 2]++;
        // rep(i, 0, 10)
        // {
        //     cerr << s[i] << ' ';
        // }
        // cerr << endl;
        mp[a[i]] = i;
    }
    rep(i, 1, n) s[i] += s[i - 1];
    rep(i, 1, n) s[i] += s[i - 1];
    rep(i, 1, n) { cout << s[i] << ' '; }
}

signed main() {
    freopen("fight.in", "r", stdin);
    freopen("fight.out", "w", stdout);
    // cerr<<(&en-&st)/1024.0/1024.0<<endl;
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int testcase = 1;
    // cin >> testcase;
    while (testcase--) solve();
    return 0;
}
```

</details>

### C. 海报

Part 1 不考虑查询

我们不考虑环，假设是在一条链上，那么一个很显然的``dp``状态为 $dp_{i,j}$ 表示考虑到第 $i$ 个人， 从 $i$ 开始往前连续 $j$ 个人举海报

然后考虑环上的情况， 我们发现： 可以把最前面有多少个连续的人举海报这个东西扔到状态里面 即增加一维$k$，表示最前面有多少个连续的人举海报

时间复杂度 $\mathcal{O}(n)$ 空间复杂度 $\mathcal{O}(n)$

Part 2 考虑查询

暴力修改重构， 时间复杂度 $\mathcal{O(nq)}$ ，显然不能通过

考虑用线段树维护 ``dp`` 数组，具体来说，线段树上每个节点存一个 $dp_{i,j}$ 表示当前节点表示的区间中，前 $i$ 个人和后 $j$ 个人都举海报，且第 $i+1$ 个人和倒数第 $j+1$ 个人不举海报的最大价值，合并的时候暴力转移即可

特别注意要特判**区间全部选中的情况**

时间复杂度 $\mathcal{O}(nlogn)$ 空间复杂度 $\mathcal{O}(n)$

<details>
<summary> 可爱的code捏 </summary>

```cpp
// Author: xiaruize
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define ull unsigned long long
#define ALL(a) (a).begin(), (a).end()
#define pb push_back
#define mk make_pair
#define pii pair<int, int>
#define pis pair<int, string>
#define sec second
#define fir first
#define sz(a) int((a).size())
#define Yes cout << "Yes" << endl
#define YES cout << "YES" << endl
#define No cout << "No" << endl
#define NO cout << "NO" << endl
#define debug(x) cerr << #x << ": " << x << endl
#define mms(arr, n) memset(arr, n, sizeof(arr))
#define rep(i, a, n) for (int i = (a); i <= (n); ++i)
#define per(i, n, a) for (int i = (n); i >= (a); --i)
int max(int a, int b) {
    if (a > b)
        return a;
    return b;
}
int min(int a, int b) {
    if (a < b)
        return a;
    return b;
}
void chkmx(int &x, int y) {
    if (y > x)
        x = y;
}
void chkmi(int &x, int y) {
    if (y < x)
        x = y;
}
const int INF = 0x3f3f3f3f3f3f3f3f;
const int MOD = 1000000007;
const int N = 4e4 + 10;

// bool st;
int n;
int a[N];
int q;
// bool en;

struct segment_tree {
#define ls p << 1
#define rs p << 1 | 1

    struct node {
        int dp[4][4];
    } tr[N << 2];

    void pushup(int p, int l, int r) {
        memset(tr[p].dp, 0, sizeof(tr[p].dp));
        int mid = l + r >> 1;
        rep(i, 0, 3) {
            if (i > mid - l + 1)
                break;
            rep(j, 0, 3) {
                if (j > r - mid)
                    break;
                rep(k, 0, 3) {
                    if (k > mid - l + 1)
                        break;
                    rep(d, 0, 3) {
                        if (d > r - mid)
                            break;
                        if (k + d > 3)
                            continue;
                        if (i == mid - l + 1 && j == r - mid) {
                            if (i + j > 3)
                                continue;
                            tr[p].dp[i + j][i + j] =
                                max(tr[p].dp[i + j][i + j], tr[ls].dp[i][i] + tr[rs].dp[j][j]);
                        } else if (i == mid - l + 1) {
                            if (i + d > 3)
                                continue;
                            tr[p].dp[i + d][j] = max(tr[p].dp[i + d][j], tr[ls].dp[i][i] + tr[rs].dp[d][j]);
                        } else if (j == r - mid) {
                            if (j + k > 3)
                                continue;
                            tr[p].dp[i][j + k] = max(tr[p].dp[i][j + k], tr[ls].dp[i][k] + tr[rs].dp[j][j]);
                        } else
                            tr[p].dp[i][j] = max(tr[p].dp[i][j], tr[ls].dp[i][k] + tr[rs].dp[d][j]);
                    }
                }
            }
        }
    }

    void build(int p, int l, int r) {
        if (l == r) {
            tr[p].dp[1][1] = a[l];
            return;
        }
        int mid = l + r >> 1;
        build(ls, l, mid);
        build(rs, mid + 1, r);
        pushup(p, l, r);
        // cerr << p << ' ' << l << ' ' << r << endl;
        // rep(i, 0, 3)
        // {
        // 	rep(j, 0, 3)
        // 	{
        // 		cerr << tr[p].dp[i][j] << ' ';
        // 	}
        // 	cerr << endl;
        // }
    }

    void update(int p, int l, int r, int pos, int val) {
        if (l == r) {
            tr[p].dp[1][1] = val;
            return;
        }
        int mid = l + r >> 1;
        if (mid >= pos)
            update(ls, l, mid, pos, val);
        else
            update(rs, mid + 1, r, pos, val);
        pushup(p, l, r);
        // cerr << p << ' ' << l << ' ' << r << endl;
        // rep(i, 0, 3)
        // {
        // 	rep(j, 0, 3)
        // 	{
        // 		cerr << tr[p].dp[i][j] << ' ';
        // 	}
        // 	cerr << endl;
        // }
    }

    int getans() {
        int res = 0;
        rep(i, 0, 3) {
            rep(j, 0, 3) {
                if (i + j <= 3) {
                    res = max(res, tr[1].dp[i][j]);
                }
            }
        }
        return res;
    }
} seg;

void solve() {
    cin >> n;
    rep(i, 1, n) { cin >> a[i]; }
    seg.build(1, 1, n);
    cout << seg.getans() << endl;
    cin >> q;
    while (q--) {
        int x, v;
        cin >> x >> v;
        seg.update(1, 1, n, x, v);
        cout << seg.getans() << endl;
    }
}

signed main() {
    freopen("poster.in", "r", stdin);
    freopen("poster.out", "w", stdout);
    // cerr<<(&en-&st)/1024.0/1024.0<<endl;
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int testcase = 1;
    // cin >> testcase;
    while (testcase--) solve();
    return 0;
}
```

</details>

### D. 机器人锦标赛

题面还是比较烦 但是其实暴力加就可以 遇到 $1$ 显然就不用继续操作了

<details>
<summary> 可爱的code捏 </summary>

```cpp
// Author: xiaruize
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define ull unsigned long long
#define ALL(a) (a).begin(), (a).end()
#define pb push_back
#define mk make_pair
#define pii pair<int, int>
#define pis pair<int, string>
#define sec second
#define fir first
#define sz(a) int((a).size())
#define Yes cout << "Yes" << endl
#define YES cout << "YES" << endl
#define No cout << "No" << endl
#define NO cout << "NO" << endl
#define debug(x) cerr << #x << ": " << x << endl
#define mms(arr, n) memset(arr, n, sizeof(arr))
#define rep(i, a, n) for (int i = (a); i <= (n); ++i)
#define per(i, n, a) for (int i = (n); i >= (a); --i)
int max(int a, int b) {
    if (a > b)
        return a;
    return b;
}
int min(int a, int b) {
    if (a < b)
        return a;
    return b;
}
void chkmx(int &x, int y) {
    if (y > x)
        x = y;
}
void chkmi(int &x, int y) {
    if (y < x)
        x = y;
}
const int INF = 0x3f3f3f3f3f3f3f3f;
const int MOD = 1000000007;
const int N = 3e5 + 10;

// bool st;
struct node {
    int op, a, b;
};
vector<node> op[N];
vector<int> g[N], p[N], fa[N];
bool vis[N];
int n, m, s, res, f[N];
// bool en;

void upd(int x, int y) {
    if (vis[x])
        return;
    g[x][y] = 1;
    for (int v = y, t;; v = t) {
        if (v == m * 2 - 2)
            break;
        t = fa[x][v];
        if (op[x][t - m].op == 1)
            g[x][t] = g[x][op[x][t - m].a] & g[x][op[x][t - m].b];
        else
            g[x][t] = g[x][op[x][t - m].a] | g[x][op[x][t - m].b];
    }
    if (g[x][m * 2 - 2]) {
        vis[x] = true;
        res++;
    }
}

void solve() {
    cin >> m >> n >> s;
    rep(i, 1, n) {
        g[i].resize(m * 2);
        fa[i].resize(m * 2);
        op[i].resize(m);
        rep(j, 0, m - 2) {
            cin >> op[i][j].a >> op[i][j].b >> op[i][j].op;
            op[i][j].a--;
            op[i][j].b--;
            fa[i][op[i][j].a] = fa[i][op[i][j].b] = j + m;
        }
    }
    rep(i, 0, m - 1) p[i].resize(n);
    rep(i, 1, n) {
        rep(j, 0, m - 1) {
            int x;
            cin >> x;
            p[j][x] = i;
        }
    }
    rep(i, 0, m - 1) {
        while (res < s && f[i] < n) upd(p[i][f[i]++], i);
    }
    rep(i, 0, m - 1) cout << f[i] << ' ';
}

signed main() {
    freopen("robot.in", "r", stdin);
    freopen("robot.out", "w", stdout);
    // cerr<<(&en-&st)/1024.0/1024.0<<endl;
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int testcase = 1;
    // cin >> testcase;
    while (testcase--) solve();
    return 0;
}
```

</details>

### 2023.10.15比赛总结

``300`` 分没挂，最后一题不打暴力有点可惜

## 2023.10.16

### A. 魔力子串

直接``vector`` 扔 ``map``里面 没什么好说的

警示后人: **能用map就不要哈希**

<details>
<summary> 可爱的code捏 </summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define ull unsigned long long
#define ALL(a) (a).begin(), (a).end()
#define pb push_back
#define mk make_pair
#define pii pair<int, int>
#define pis pair<int, string>
#define sec second
#define fir first
#define sz(a) int((a).size())
#define Yes cout << "Yes" << endl
#define YES cout << "YES" << endl
#define No cout << "No" << endl
#define NO cout << "NO" << endl
#define debug(x) cerr << #x << ": " << x << endl
#define double long double
#define mms(arr, n) memset(arr, n, sizeof(arr))
#define rep(i, a, n) for (int i = (a); i <= (n); ++i)
#define per(i, n, a) for (int i = (n); i >= (a); --i)
int max(int a, int b) {
    if (a > b)
        return a;
    return b;
}
int min(int a, int b) {
    if (a < b)
        return a;
    return b;
}
const int INF = 0x3f3f3f3f3f3f3f3f;
const int MOD = 1000000007;
const int N = 2e5 + 10;

// bool st;
int n;
string s;
int cnt[52];
map<vector<int>, int> mp;
map<char, int> chg;
int tot = 0;
// bool en;

int ch(char c) {
    if (chg.count(c))
        return chg[c];
    return chg[c] = tot++;
}

void solve() {
    cin >> n >> s;
    rep(i, 0, n - 1) ch(s[i]);
    vector<int> tmp(tot);
    mp[tmp]++;
    rep(i, 0, n - 1) {
        cnt[ch(s[i])]++;
        vector<int> tmp(tot);
        rep(i, 0, tot - 2) {
            tmp[i] = cnt[i + 1] - cnt[i];
            // cerr << tmp[i] << ' ';
        }
        // cerr << endl;
        mp[tmp]++;
    }
    int res = 0;
    for (auto v : mp) {
        // cerr << v.second << endl;
        res += v.second * (v.second - 1) / 2;
        res %= MOD;
    }
    cout << res << endl;
}

signed main() {
    freopen("magic.in", "r", stdin);
    freopen("magic.out", "w", stdout);
    // cerr<<(&en-&st)/1024.0/1024.0<<endl;
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int testcase = 1;
    // cin >> testcase;
    while (testcase--) solve();
    return 0;
}
```

</details>

### B. 吃树

结论题

1. 当正好存在 $\frac{n}{k}$ 个节点的子树大小为 $k$ 的倍数时, $k$ 作为块的大小是合法的

2. 对于每种合法的块的大小，有且仅有 $1$ 种构造方案

<details>
<summary> 可爱的code捏 </summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define ull unsigned long long
#define ALL(a) (a).begin(), (a).end()
#define pb push_back
#define mk make_pair
#define pii pair<int, int>
#define pis pair<int, string>
#define sec second
#define fir first
#define sz(a) int((a).size())
#define Yes cout << "Yes" << endl
#define YES cout << "YES" << endl
#define No cout << "No" << endl
#define NO cout << "NO" << endl
#define debug(x) cerr << #x << ": " << x << endl
#define double long double
#define mms(arr, n) memset(arr, n, sizeof(arr))
#define rep(i, a, n) for (int i = (a); i <= (n); ++i)
#define per(i, n, a) for (int i = (n); i >= (a); --i)
int max(int a, int b)
{
	if (a > b)
		return a;
	return b;
}
int min(int a, int b)
{
	if (a < b)
		return a;
	return b;
}
const int INF = 0x3f3f3f3f3f3f3f3f;
const int MOD = 1000000007;
const int N = 1e6 + 10;

// bool st;
int n;
vector<int> g[N];
int siz[N], cnt[N];
// bool en;

void dfs(int x, int fa)
{
	siz[x] = 1;
	for (auto v : g[x])
	{
		if (v == fa)
			continue;
		dfs(v, x);
		siz[x] += siz[v];
	}
	cnt[siz[x]]++;
}

int res = 0;

void calc(int x)
{
	int c = 0;
	for (int i = x; i <= n; i += x)
	{
		c += cnt[i];
	}
	if (c == n / x)
	{
		// cerr << x << endl;
		res++;
	}
}

void solve()
{
	cin >> n;
	rep(i, 1, n - 1)
	{
		int u, v;
		cin >> u >> v;
		g[u].push_back(v);
		g[v].push_back(u);
	}
	dfs(1, 0);
	rep(i, 1, n)
	{
		if (i * i > n)
			break;
		if (n % i == 0)
		{
			calc(i);
			if (i * i != n)
				calc(n / i);
		}
	}
	cout << res << endl;
}

signed main()
{
	freopen("eat.in", "r", stdin);
	freopen("eat.out", "w", stdout);
	// cerr<<(&en-&st)/1024.0/1024.0<<endl;
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
	int testcase = 1;
	// cin >> testcase;
	while (testcase--)
		solve();
	return 0;
}
```

</details>

### C. 弹弹床

想不到状态的可爱 $dp$ 题捏~~~

对于 $[1,i]$ 这个区间, 必然被从右侧进入再出来一定次数， 所以令 $dp_{i,j}$ 表示区间 $[1,i]$ 在若干步以后, 剩下 $j$ 个向右的没有确定终点

$$
dp_{i,j}=\begin{cases}
dp_{i-1,j} \times j + dp_{i-1,j-1}, S_i=R\\
dp_{i-1,j}\times j +dp_{i-1,j+1} \times (j+1) \times j, S_i=L
\end{cases}
$$

可以获得``60``分的好成绩

考虑怎么处理中间的数值， 发现上述``dp``可以反过来再做一次, 算出区间 $[i,n]$ , 剩下 $j$ 个向左的方案数

然后枚举终点， 左右匹配即可

<details>
<summary> 可爱的code捏 </summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define ull unsigned long long
#define ALL(a) (a).begin(), (a).end()
#define pb push_back
#define mk make_pair
#define pii pair<int, int>
#define pis pair<int, string>
#define sec second
#define fir first
#define sz(a) int((a).size())
#define Yes cout << "Yes" << endl
#define YES cout << "YES" << endl
#define No cout << "No" << endl
#define NO cout << "NO" << endl
#define debug(x) cerr << #x << ": " << x << endl
#define double long double
#define mms(arr, n) memset(arr, n, sizeof(arr))
#define rep(i, a, n) for (int i = (a); i <= (n); ++i)
#define per(i, n, a) for (int i = (n); i >= (a); --i)
int max(int a, int b) {
    if (a > b)
        return a;
    return b;
}
int min(int a, int b) {
    if (a < b)
        return a;
    return b;
}
const int INF = 0x3f3f3f3f3f3f3f3f;
const int MOD = 1000000007;
const int N = 5e3 + 10;

// bool st;
int n;
char s[N];
int f[N][N], g[N][N];
int fac[N];
// bool en;

void solve() {
    cin >> n;
    cin >> (s + 1);
    if (n == 1) {
        cout << "1" << endl;
        return;
    }
    f[0][0] = 1;
    rep(i, 1, n) {
        rep(j, 1, i) {
            if (s[i] == 'L')
                f[i][j] = (f[i - 1][j] * j % MOD + f[i - 1][j + 1] * (j + 1) % MOD * j % MOD) % MOD;
            else
                f[i][j] = (f[i - 1][j - 1] + f[i - 1][j] * j % MOD) % MOD;
            f[i][j] %= MOD;
        }
    }
    g[n + 1][0] = 1;
    per(i, n, 1) {
        rep(j, 1, n - i + 1) {
            if (s[i] == 'R')
                g[i][j] = (g[i + 1][j] * j % MOD + g[i + 1][j + 1] * (j + 1) % MOD * j % MOD) % MOD;
            else
                g[i][j] = (g[i + 1][j - 1] + g[i + 1][j] * j % MOD) % MOD;
            g[i][j] %= MOD;
        }
    }
    fac[0] = 1;
    rep(i, 1, n) fac[i] = fac[i - 1] * i % MOD;
    rep(i, 1, n) {
        int res = 0;
        rep(j, 0, i - 1) {
            if (j)
                (res += fac[j] * fac[j - 1] % MOD * f[i - 1][j] % MOD * g[i + 1][j - 1] % MOD) %= MOD;
            (res += fac[j] * fac[j] % MOD * f[i - 1][j] % MOD * g[i + 1][j] % MOD * 2 % MOD) %= MOD;
            (res += fac[j + 1] * fac[j] % MOD * f[i - 1][j] % MOD * g[i + 1][j + 1] % MOD) %= MOD;
        }
        cout << res << ' ';
    }
}

signed main() {
    freopen("jump.in", "r", stdin);
    freopen("jump.out", "w", stdout);
    // cerr<<(&en-&st)/1024.0/1024.0<<endl;
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int testcase = 1;
    // cin >> testcase;
    while (testcase--) solve();
    return 0;
}
```

</details>

### D. 数星星

小丑了 上次补这题是``ACV``的

> 这道题去年做过 我记得 x–r--z- 去年还补过 可惜没有什么用 --zc

将查询按照右端点排序，可以考虑把所有贡献算在当前节点的最后一次出现上

那么，当处理到当前右端点时，直接查询左端点即可

然后用一个set去维护每个区间的覆盖情况，用树状数组维护每个时刻的值，树剖做链上修改

<details>
<summary> 巨大的code捏 </summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define ull unsigned long long
#define ALL(a) (a).begin(), (a).end()
#define pb push_back
#define mk make_pair
#define pii pair<int, int>
#define pis pair<int, string>
#define sec second
#define fir first
#define sz(a) int((a).size())
#define Yes cout << "Yes" << endl
#define YES cout << "YES" << endl
#define No cout << "No" << endl
#define NO cout << "NO" << endl
#define debug(x) cerr << #x << ": " << x << endl
#define double long double
#define mms(arr, n) memset(arr, n, sizeof(arr))
#define rep(i, a, n) for (int i = (a); i <= (n); ++i)
#define per(i, n, a) for (int i = (n); i >= (a); --i)
#define endl '\n'
int max(int a, int b) {
    if (a > b)
        return a;
    return b;
}
int min(int a, int b) {
    if (a < b)
        return a;
    return b;
}
const int INF = 0x3f3f3f3f3f3f3f3f;
const int MOD = 1000000007;
const int N = 1e5 + 10;

// bool st;
int n, m, q;
int a[N];
vector<int> g[N];
int dep[N];
int siz[N];
int fa[N];
int top[N];
int son[N], sum[N];
int dfn[N], rnk[N], tot;
// bool en;

void dfs(int x, int y) {
    dep[x] = dep[y] + 1;
    siz[x] = 1;
    fa[x] = y;
    for (auto v : g[x]) {
        if (v == y)
            continue;
        dfs(v, x);
        siz[x] += siz[v];
        if (siz[v] > siz[son[x]])
            son[x] = v;
    }
}

void dfs2(int x, int tp) {
    dfn[x] = ++tot;
    rnk[tot] = x;
    top[x] = tp;
    sum[tot] = sum[tot - 1] + a[x];
    if (son[x])
        dfs2(son[x], tp);
    for (auto v : g[x]) {
        if (v != son[x] && v != fa[x]) {
            dfs2(v, v);
        }
    }
}

struct BIT {
    int tr[N];

    void add(int x, int v) {
        while (x) {
            tr[x] += v;
            x -= x & -x;
        }
    }

    int sum(int x) {
        int res = 0;
        while (x <= m) {
            res += tr[x];
            x += x & -x;
        }
        return res;
    }
} tr;

pii w[N];

struct query {
    int l, r, id;
    bool operator<(const query &y) const { return l < y.l; }
} s[N];

set<query> st;

bool cmp(query a, query b) { return a.r < b.r; }

int ans[N];

void add(int l, int r, int id) {
    while (1) {
        auto it = st.upper_bound({ r, 0, 0 });
        if (it == st.begin())
            break;
        it--;
        if ((*it).r < l)
            break;
        auto [xl, xr, xid] = (*it);
        // cerr << xl << ' ' << xr << ' ' << xid << endl;
        st.erase(it);
        tr.add(xid, sum[max(l, xl) - 1] - sum[min(r, xr)]);
        if (xl < l)
            st.insert({ xl, l - 1, xid });
        if (xr > r)
            st.insert({ r + 1, xr, xid });
    }
    tr.add(id, sum[r] - sum[l - 1]);
    st.insert({ l, r, id });
}

void calc(int u, int v, int id) {
    // cerr << u << ' ' << v << endl;
    while (top[u] != top[v]) {
        if (dfn[top[u]] < dfn[top[v]])
            swap(u, v);
        add(dfn[top[u]], dfn[u], id);
        u = fa[top[u]];
    }
    if (dfn[u] > dfn[v])
        swap(u, v);
    add(dfn[u], dfn[v], id);
}

void solve() {
    cin >> n >> m >> q;
    rep(i, 1, n) { cin >> a[i]; }
    rep(i, 1, n - 1) {
        int u, v;
        cin >> u >> v;
        g[u].push_back(v);
        g[v].push_back(u);
    }
    dfs(1, 0);
    dfs2(1, 1);
    rep(i, 1, m) { cin >> w[i].first >> w[i].second; }
    rep(i, 1, q) {
        cin >> s[i].l >> s[i].r;
        s[i].id = i;
    }
    sort(s + 1, s + q + 1, cmp);
    rep(i, 1, q) {
        rep(j, s[i - 1].r + 1, s[i].r) calc(w[j].first, w[j].second, j);
        ans[s[i].id] = tr.sum(s[i].l);
    }
    rep(i, 1, q) cout << ans[i] << endl;
}

signed main() {
    freopen("star.in", "r", stdin);
    freopen("star.out", "w", stdout);
    // cerr<<(&en-&st)/1024.0/1024.0<<endl;
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int testcase = 1;
    // cin >> testcase;
    while (testcase--) solve();
    return 0;
}
```

</details>

### 20231016比赛总结

拿了大众分 没有严重的挂分

$\mathcal{O(n\sqrt{n}log^2n)=O(n^2)}$

写代码之前先好好算时间复杂度qwq

## 2023.10.17

### A. 子段排序

构造题 注意到排序操作 = $a_i<a_j$ 时的 ``swap``

<details>
<summary> 可爱的code捏 </summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define ull unsigned long long
#define ALL(a) (a).begin(), (a).end()
#define pb push_back
#define mk make_pair
#define pii pair<int, int>
#define pis pair<int, string>
#define sec second
#define fir first
#define sz(a) int((a).size())
#define Yes cout << "Yes" << endl
#define YES cout << "YES" << endl
#define No cout << "No" << endl
#define NO cout << "NO" << endl
#define debug(x) cerr << #x << ": " << x << endl
#define double long double
#define mms(arr, n) memset(arr, n, sizeof(arr))
#define rep(i, a, n) for (int i = (a); i <= (n); ++i)
#define per(i, n, a) for (int i = (n); i >= (a); --i)
int max(int a, int b)
{
	if (a > b)
		return a;
	return b;
}
int min(int a, int b)
{
	if (a < b)
		return a;
	return b;
}
const int INF = 0x3f3f3f3f3f3f3f3f;
const int MOD = 1000000007;
const int N = 1e3 + 10;

// bool st;
int n;
int a[N], b[N];
// bool en;

void solve()
{
	cin >> n;
	rep(i, 1, n)
	{
		cin >> a[i];
	}
	rep(i, 1, n) cin >> b[i];
	vector<pii> res;
	rep(i, 1, n)
	{
		if (a[i] != b[i])
		{
			int p = i, mi = INF;
			while (a[p] != b[i] && p <= n)
			{
				mi = min(mi, a[p]);
				p++;
			}
			// cerr << i << ' ' << p << endl;
			if (a[p] > mi || p > n)
			{
				cout << "-1" << endl;
				return;
			}
			per(j, p - 1, i)
			{
				res.push_back({j, j + 1});
				swap(a[j], a[j + 1]);
			}
		}
		// rep(j, 1, n) cerr << a[j] << ' ';
		// cerr << endl;
	}
	cout << "0" << endl;
	cout << res.size() << endl;
	for (auto v : res)
		cout << v.first << ' ' << v.second << endl;
}

signed main()
{
	freopen("subarray.in", "r", stdin);
	freopen("subarray.out", "w", stdout);
	// cerr<<(&en-&st)/1024.0/1024.0<<endl;
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
	int testcase = 1;
	cin >> testcase;
	while (testcase--)
		solve();
	return 0;
}
```

</details>

### B. 伪快速排序

$dp_{i,j}$ 表示 $[1,i]$ , 最后一个数为 $j$ 的方案数

$$
dp_{i,j}=\sum\limits_{k=0}^{i-1} dp_{k,j-1} \cdot dp_{i-k-1,j-1} \cdot \binom{i-1}{k}
$$

<details>
<summary> 可爱的code捏 </summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define ull unsigned long long
#define ALL(a) (a).begin(), (a).end()
#define pb push_back
#define mk make_pair
#define pii pair<int, int>
#define pis pair<int, string>
#define sec second
#define fir first
#define sz(a) int((a).size())
#define Yes cout << "Yes" << endl
#define YES cout << "YES" << endl
#define No cout << "No" << endl
#define NO cout << "NO" << endl
#define debug(x) cerr << #x << ": " << x << endl
#define double long double
#define mms(arr, n) memset(arr, n, sizeof(arr))
#define rep(i, a, n) for (int i = (a); i <= (n); ++i)
#define per(i, n, a) for (int i = (n); i >= (a); --i)
int max(int a, int b)
{
	if (a > b)
		return a;
	return b;
}
int min(int a, int b)
{
	if (a < b)
		return a;
	return b;
}
const int INF = 0x3f3f3f3f3f3f3f3f;
int MOD = 1000000007;
const int N = 3e2 + 10;

// bool st;
int dp[N][N];
int c[N][N];
// bool en;

void solve()
{
	int n, k;
	cin >> n >> k;
	k = min(k, n);
	cout << dp[n][k] << endl;
}

signed main()
{
	freopen("qsort.in","r",stdin);
	freopen("qsort.out","w",stdout);
	// cerr<<(&en-&st)/1024.0/1024.0<<endl;
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
	int testcase = 1;
	cin >> testcase >> MOD;
	c[0][0] = 1;
	rep(i, 1, 300)
	{
		c[i][0] = c[i][i] = 1;
		rep(j, 1, 300)
		{
			c[i][j] = (c[i - 1][j - 1] + c[i - 1][j]) % MOD;
		}
		dp[0][i] = 1;
	}
	rep(i, 1, 300)
	{
		dp[i][1] = 1;
		rep(j, 2, 300)
		{
			rep(p, 0, i - 1)
			{
				(dp[i][j] += dp[p][j - 1] * dp[i - p - 1][j - 1] % MOD * c[i - 1][p] % MOD) %= MOD;
			}
		}
	}
	while (testcase--)
		solve();
	return 0;
}
```

</details>

### C. 二进制式

表达式题, 先见表达式树, 考虑 ``dfs`` 算每个节点为 $1$ 的概率

然后从根向下 ``dfs`` 即可算每个点的答案

<details>
<summary> 可爱的code捏 </summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define ull unsigned long long
#define ALL(a) (a).begin(), (a).end()
#define pb push_back
#define mk make_pair
#define pii pair<int, int>
#define pis pair<int, string>
#define sec second
#define fir first
#define sz(a) int((a).size())
#define Yes cout << "Yes" << endl
#define YES cout << "YES" << endl
#define No cout << "No" << endl
#define NO cout << "NO" << endl
#define debug(x) cerr << #x << ": " << x << endl
#define double long double
#define mms(arr, n) memset(arr, n, sizeof(arr))
#define rep(i, a, n) for (int i = (a); i <= (n); ++i)
#define per(i, n, a) for (int i = (n); i >= (a); --i)
int max(int a, int b)
{
	if (a > b)
		return a;
	return b;
}
int min(int a, int b)
{
	if (a < b)
		return a;
	return b;
}
const int INF = 0x3f3f3f3f3f3f3f3f;
const int MOD = 998244353;
const int N = 2e5 + 10;

// bool st;
int n, len;
char s[N << 4];
struct node
{
	int l, r;
	int op, val; // 1 and 2 or 3 xor
} p[N << 2];
int tot = 0, cnt = 0;
stack<int> st, ope;
int res[N];
// bool en;

int qpow(int a, int b)
{
	int res = 1;
	while (b)
	{
		if (b & 1)
			res = res * a % MOD;
		a = a * a % MOD;
		b >>= 1;
	}
	return res;
}

void dfs(int x)
{
	if (p[x].l > n)
		dfs(p[x].l);
	if (p[x].r > n)
		dfs(p[x].r);
	if (p[x].op == 1)
		p[x].val = p[p[x].l].val * p[p[x].r].val % MOD;
	else if (p[x].op == 2)
		p[x].val = (1 - (1 - p[p[x].l].val + MOD) % MOD * (1 - p[p[x].r].val + MOD) % MOD + MOD) % MOD;
	else
		p[x].val = ((1 - p[p[x].l].val + MOD) % MOD * p[p[x].r].val % MOD + (1 - p[p[x].r].val + MOD) % MOD * p[p[x].l].val % MOD) % MOD;
}

void calc(int x, int v)
{
	if (x <= n)
	{
		res[x] = v;
		return;
	}
	if (p[x].op == 1)
	{
		calc(p[x].l, v * p[p[x].r].val % MOD);
		calc(p[x].r, v * p[p[x].l].val % MOD);
	}
	else if (p[x].op == 2)
	{
		calc(p[x].l, v * (1 - p[p[x].r].val + MOD) % MOD);
		calc(p[x].r, v * (1 - p[p[x].l].val + MOD) % MOD);
	}
	else
	{
		calc(p[x].l, v);
		calc(p[x].r, v);
	}
}

void solve()
{
	cin >> n >> (s + 1);
	len = strlen(s + 1);
	cnt = n;
	int inv2 = qpow(2, MOD - 2);
	rep(i, 1, n)
	{
		p[i].val = inv2;
	}
	rep(i, 1, len)
	{
		if (s[i] == 'x')
			st.push(++tot);
		else if (s[i] == '&')
			ope.push(1);
		else if (s[i] == '|')
			ope.push(2);
		else if (s[i] == '^')
			ope.push(3);
		else if (s[i] == ')')
		{
			int b = st.top();
			st.pop();
			int a = st.top();
			st.pop();
			int opr = ope.top();
			ope.pop();
			p[++cnt] = {a, b, opr};
			st.push(cnt);
		}
	}
	dfs(cnt);
	calc(cnt, 1);
	rep(i, 1, n)
	{
		cout << res[i] << endl;
	}
}

signed main()
{
	freopen("binary.in","r",stdin);
	freopen("binary.out","w",stdout);
	// cerr<<(&en-&st)/1024.0/1024.0<<endl;
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
	int testcase = 1;
	// cin >> testcase;
	while (testcase--)
		solve();
	return 0;
}
```

</details>

### D. 网格图

挖坑待填

### 20231017比赛总结

除了最后一题没有挂分

**认真看数据范围，下面不一定包括上面！！！**

## 2023.10.18

### A. 玛雅历

形似儒略日, 稍微简单一点, 注意到公元前很烦, 考虑直接将公元前向后平移 $1$ 年, 然后使公元 $0$ 年存在

<details>

<summary> 可爱的code捏 </summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define ull unsigned long long
#define ALL(a) (a).begin(), (a).end()
#define pb push_back
#define mk make_pair
#define pii pair<int, int>
#define pis pair<int, string>
#define sec second
#define fir first
#define sz(a) int((a).size())
#define Yes cout << "Yes" << endl
#define YES cout << "YES" << endl
#define No cout << "No" << endl
#define NO cout << "NO" << endl
#define debug(x) cerr << #x << ": " << x << endl
#define double long double
#define mms(arr, n) memset(arr, n, sizeof(arr))
#define rep(i, a, n) for (int i = (a); i <= (n); ++i)
#define per(i, n, a) for (int i = (n); i >= (a); --i)
int max(int a, int b)
{
	if (a > b)
		return a;
	return b;
}
int min(int a, int b)
{
	if (a < b)
		return a;
	return b;
}
const int INF = 0x3f3f3f3f3f3f3f3f;
const int MOD = 1000000007;
const int N = 2e5 + 10;

// bool st;
int cnt400 = 0;
int a[N];
int num[10] = {0, 23040000000, 1152000000, 57600000, 2880000, 144000, 7200, 360, 20, 1};
int month[13] = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
// bool en;

void solve()
{
	rep(i, 1, 8)
	{
		cin >> a[i];
		char c;
		cin >> c;
	}
	cin >> a[9];
	int daycnt = 0;
	rep(i, 1, 9) daycnt = daycnt + num[i] * a[i];
	// cerr << daycnt << endl;
	int yearcnt = daycnt / cnt400;
	daycnt %= cnt400;
	int yearnum = -3113 + yearcnt * 400, monthnum = 8;
	int curyearnum = 0;
	if ((yearnum + 1) % 4 == 0 && ((yearnum + 1) % 100 != 0 || (yearnum + 1) % 400 == 0))
		curyearnum = 366;
	else
		curyearnum = 365;
	while (daycnt >= curyearnum)
	{
		daycnt -= curyearnum;
		yearnum++;
		if ((yearnum + 1) % 4 == 0 && ((yearnum + 1) % 100 != 0 || (yearnum + 1) % 400 == 0))
			curyearnum = 366;
		else
			curyearnum = 365;
	}
	if (yearnum % 4 == 0 && (yearnum % 100 != 0 || yearnum % 400 == 0))
		month[2] = 29;
	while (daycnt >= month[monthnum])
	{
		daycnt -= month[monthnum];
		monthnum++;
		if (monthnum > 12)
		{
			monthnum = 1;
			yearnum++;
			if (yearnum % 4 == 0 && (yearnum % 100 != 0 || yearnum % 400 == 0))
				month[2] = 29;
			else
				month[2] = 28;
		}
	}
	int daynum = 11;
	while (daycnt)
	{
		daycnt--;
		daynum++;
		if (daynum > month[monthnum])
		{
			daynum = 1;
			monthnum++;
			if (monthnum > 12)
			{
				monthnum = 1;
				yearnum++;
				if (yearnum % 4 == 0 && (yearnum % 100 != 0 || yearnum % 400 == 0))
					month[2] = 29;
				else
					month[2] = 28;
			}
		}
	}
	if (yearnum <= 0)
		cout << daynum << ' ' << monthnum << ' ' << -(yearnum - 1) << " BC" << endl;
	else
		cout << daynum << ' ' << monthnum << ' ' << yearnum << endl;
}

signed main()
{
	freopen("maya.in", "r", stdin);
	freopen("maya.out", "w", stdout);
	// cerr<<(&en-&st)/1024.0/1024.0<<endl;
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
	int testcase = 1;
	cin >> testcase;
	rep(i, 0, 399)
	{
		if (i % 4 == 0 && (i % 100 != 0 || i % 400 == 0))
			cnt400 += 366;
		else
			cnt400 += 365;
	}
	// cerr << cnt400 << endl;
	while (testcase--)
		solve();
	return 0;
}
```

</details>

### B. 大融合

如题, 大融合, 可以拆成 $2$ 题

第一部分算最小的 $S$, 直接``map``即可, 考场差点写 ``trie``

第二部分显然二分, 考虑``check``, 可以令 $dp_{i,j}$ 表示到第 $i$ 行, 第 $j$ 列最多获得多少

显然这个 $dp$ 可以单调队列优化

发现 $3$ 个机器人分别走分别贡献其实是诈骗条件, 和只有一个走并贡献 $100\%$ 等价

<details>

<summary> 可爱的code捏 </summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define ull unsigned long long
#define ALL(a) (a).begin(), (a).end()
#define pb push_back
#define mk make_pair
#define pii pair<int, int>
#define pis pair<int, string>
#define sec second
#define fir first
#define sz(a) int((a).size())
#define Yes cout << "Yes" << endl
#define YES cout << "YES" << endl
#define No cout << "No" << endl
#define NO cout << "NO" << endl
#define debug(x) cerr << #x << ": " << x << endl
#define double long double
#define mms(arr, n) memset(arr, n, sizeof(arr))
#define rep(i, a, n) for (int i = (a); i <= (n); ++i)
#define per(i, n, a) for (int i = (n); i >= (a); --i)
int max(int a, int b)
{
	if (a > b)
		return a;
	return b;
}
int min(int a, int b)
{
	if (a < b)
		return a;
	return b;
}
const int INF = 0x3f3f3f3f3f3f3f3f;
const int MOD = 1000000007;
const int N = 1e3 + 10;

// bool st;
int n, m, q, d;
map<int, bool> mp;
int minS;
int dp[N][N];
int a[N][N], col[N][N];
// bool en;

int solveminS()
{
	cin >> n >> m >> q >> d;
	rep(i, 1, m)
	{
		int x;
		cin >> x;
		int tmp = 10;
		while (tmp <= x * 10)
		{
			// cerr << x % tmp << endl;
			mp[x % tmp] = true;
			tmp *= 10;
		}
	}
	int res = 0;
	rep(i, 1, q)
	{
		int l, b, v;
		cin >> l >> b >> v;
		if (!mp.count(b))
			res += v;
	}
	return res;
}

bool check(int x)
{
	int ld = max(1, d - x), rd = min(n, d + x);
	memset(dp, -0x3f, sizeof(dp));
	dp[1][1] = 0;
	deque<pii> que[N][2];
	int res = 0;
	rep(i, 1, n)
	{
		deque<pii> cur[2];
		rep(j, 1, n)
		{
			while (!cur[0].empty() && cur[0].front().second < j - rd)
				cur[0].pop_front();
			while (!cur[1].empty() && cur[1].front().second < j - rd)
				cur[1].pop_front();
			while (!que[j][0].empty() && que[j][0].front().second < j - rd)
				que[j][0].pop_front();
			while (!que[j][1].empty() && que[j][1].front().second < j - rd)
				que[j][1].pop_front();
			if (j > ld)
			{
				while (!cur[col[i][j - ld]].empty() && cur[col[i][j - ld]].back().first <= dp[i][j - ld])
					cur[col[i][j - ld]].pop_back();
				cur[col[i][j - ld]].push_back({dp[i][j - ld], j - ld});
			}
			if (i > ld)
			{
				while (!que[j][col[i - ld][j]].empty() && que[j][col[i - ld][j]].back().first <= dp[i - ld][j])
					que[j][col[i - ld][j]].pop_back();
				que[j][col[i - ld][j]].push_back({dp[i - ld][j], i - ld});
			}
			if (!cur[0].empty())
				dp[i][j] = max(dp[i][j], cur[0].front().first + a[i][j] + (col[i][j] == 0));
			if (!cur[1].empty())
				dp[i][j] = max(dp[i][j], cur[1].front().first + a[i][j] + (col[i][j] == 1));
			if (!que[j][0].empty())
				dp[i][j] = max(dp[i][j], que[j][0].front().first + a[i][j] + (col[i][j] == 0));
			if (!que[j][1].empty())
				dp[i][j] = max(dp[i][j], que[j][1].front().first + a[i][j] + (col[i][j] == 1));
			res = max(res, dp[i][j]);
			// cerr << dp[i][j] << ' ';
		}
		// cerr << endl;
	}
	// cerr << x << ' ' << res << endl;
	return res >= minS;
}

void solve()
{
	rep(i, 1, n)
	{
		rep(j, 1, n)
		{
			cin >> col[i][j];
			col[i][j]--;
		}
	}
	rep(i, 1, n)
	{
		rep(j, 1, n)
		{
			cin >> a[i][j];
		}
	}
	int l = 0, r = n;
	while (l < r)
	{
		int mid = l + r >> 1;
		if (check(mid))
			r = mid;
		else
			l = mid + 1;
	}
	if (check(r))
		cout << r << endl;
	else
		cout << "-1" << endl;
}

signed main()
{
	freopen("harbinbeer.in", "r", stdin);
	freopen("harbinbeer.out", "w", stdout);
	// cerr<<(&en-&st)/1024.0/1024.0<<endl;
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
	int testcase = 1;
	// cin >> testcase;
	minS = solveminS();
	// cerr << minS << endl;
	solve();
	return 0;
}
```

</details>

### C. 好 ♂ 朋 ♂ 友

**卡常狗能不能死一死啊**

很巧妙的性质: 同一个左端点的所有区间的 ``or`` 值最多只有 $30$ 种情况

对于每个左端点, 二分或者倍增+``st``表, 算出每一段的位置和值, 对于合法的段区间 $+1$

将查询离线, 按左端点排序, 对于每一个查询, 先将 $ql$ 左侧的节点作为左端点的贡献都去掉, 再在线段树上查询当前的答案

被卡常了qwq

<details>
<summary> 被卡常的巨大code捏 </summary>

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define ull unsigned long long
#define ALL(a) (a).begin(), (a).end()
#define pb push_back
#define mk make_pair
#define pii pair<int, int>
#define pis pair<int, string>
#define sec second
#define fir first
#define sz(a) int((a).size())
#define Yes cout << "Yes" << endl
#define YES cout << "YES" << endl
#define No cout << "No" << endl
#define NO cout << "NO" << endl
#define debug(x) cerr << #x << ": " << x << endl
#define double long double
#define mms(arr, n) memset(arr, n, sizeof(arr))
#define rep(i, a, n) for (int i = (a); i <= (n); ++i)
#define per(i, n, a) for (int i = (n); i >= (a); --i)
int max(int a, int b)
{
	if (a > b)
		return a;
	return b;
}
int min(int a, int b)
{
	if (a < b)
		return a;
	return b;
}
const int INF = 0x3f3f3f3f3f3f3f3f;
const int MOD = 1000000007;
const int N = 1e5 + 10;
const int M = 1e6 + 10;

// bool st;
int n, k, t;
int a[N];
set<int> s;
int res[M];
int st[N][20];
int lg[N];
pair<pii, int> q[M];
vector<pii> vec[N];
// bool en;

struct segment_tree
{
#define ls p << 1
#define rs p << 1 | 1
	struct node
	{
		int sum, laz;
	} tr[N << 2];

	void pushup(int p)
	{
		tr[p].sum = tr[ls].sum + tr[rs].sum;
	}

	void pushdown(int p, int l, int r)
	{
		int mid = l + r >> 1;
		tr[ls].sum += (mid - l + 1) * tr[p].laz;
		tr[ls].laz += tr[p].laz;
		tr[rs].sum += (r - mid) * tr[p].laz;
		tr[rs].laz += tr[p].laz;
		tr[p].laz = 0;
	}

	void update(int p, int l, int r, int ll, int rr, int val)
	{
		if (ll <= l && r <= rr)
		{
			tr[p].sum += (r - l + 1) * val;
			tr[p].laz += val;
			return;
		}
		pushdown(p, l, r);
		int mid = l + r >> 1;
		if (ll <= mid)
			update(ls, l, mid, ll, rr, val);
		if (rr > mid)
			update(rs, mid + 1, r, ll, rr, val);
		pushup(p);
	}

	int query(int p, int l, int r, int ll, int rr)
	{
		if (ll <= l && r <= rr)
			return tr[p].sum;
		pushdown(p, l, r);
		int mid = l + r >> 1;
		int res = 0;
		if (ll <= mid)
			res += query(ls, l, mid, ll, rr);
		if (rr > mid)
			res += query(rs, mid + 1, r, ll, rr);
		return res;
	}
} seg;

int get(int l, int r)
{
	int t = lg[r - l + 1];
	return (st[l][t] | st[r - (1 << t) + 1][t]);
}

void solve()
{
	cin >> n >> t >> k;
	rep(i, 1, k)
	{
		int x;
		cin >> x;
		s.insert(x);
	}
	lg[1] = 0;
	rep(i, 1, n)
	{
		if (i != 1)
			lg[i] = lg[i >> 1] + 1;
		cin >> a[i];
		st[i][0] = a[i];
	}
	rep(i, 1, 19)
	{
		for (int j = 1; j + (1 << i) - 1 <= n; j++)
		{
			st[j][i] = st[j][i - 1] | st[j + (1 << i - 1)][i - 1];
		}
	}
	rep(i, 1, n)
	{
		int la = i - 1;
		int j = i;
		while (j <= n)
		{
			per(p, 19, 0)
			{
				if (j + (1 << p) > n)
					continue;
				if (get(i, j + (1 << p)) == get(i, j))
					j = j + (1 << p);
			}
			if (s.find(get(i, j) % 10) != s.end())
			{
				vec[i].push_back({la + 1, j});
				seg.update(1, 1, n, la + 1, j, 1);
			}
			// cerr << i << ' ' << la + 1 << ' ' << j << endl;
			la = j;
			j++;
		}
	}
	rep(i, 1, t)
	{
		cin >> q[i].first.first >> q[i].first.second;
		q[i].second = i;
	}
	sort(q + 1, q + t + 1);
	int p = 1;
	rep(i, 1, t)
	{
		// cerr << q[i].first.first << ' ' << q[i].first.second << endl;
		while (p < q[i].first.first)
		{
			for (auto v : vec[p])
				seg.update(1, 1, n, v.first, v.second, -1);
			p++;
		}
		res[q[i].second] = seg.query(1, 1, n, q[i].first.first, q[i].first.second);
	}
	rep(i, 1, t) cout << res[i] << '\n';
}

signed main()
{
	freopen("friend.in", "r", stdin);
	freopen("friend.out", "w", stdout);
	// cerr<<(&en-&st)/1024.0/1024.0<<endl;
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
	int testcase = 1;
	// cin >> testcase;
	while (testcase--)
		solve();
	return 0;
}
```
</details>

### D. 图的直径

挖坑待补

### 20231018比赛总结

前 $2$ 题还好, T1第一遍写忽略了很多细节, T2假的代码过了乐

T3注意常数(**卡常狗死一死**)

能拿的分应该都拿到了吧

## 2023.10.19

### A. 潜艇
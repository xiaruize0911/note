## arc064

### [ARC064E] Cosmic Rays

建图 跑``dijkstra``即可

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
struct node
{
    int x, y, r;
} s[N];
int n;
vector<pair<int, double>> g[N];
double dis[N];
// bool en;

void solve()
{
    cin >> s[0].x >> s[0].y >> s[1].x >> s[1].y;
    s[0].r = s[1].r = 0;
    cin >> n;
    n++;
    rep(i, 2, n) cin >> s[i].x >> s[i].y >> s[i].r;
    rep(i, 0, n)
    {
        dis[i] = INF;
        rep(j, 0, n)
        {
            if (i == j)
                continue;
            g[i].push_back({j, max((double)0, sqrt((s[i].x - s[j].x) * (s[i].x - s[j].x) + (s[i].y - s[j].y) * (s[i].y - s[j].y)) - s[i].r - s[j].r)});
        }
    }
    dis[0] = 0;
    priority_queue<pair<double, int>, vector<pair<double, int>>, greater<pair<double, int>>> q;
    q.push({0, 0});
    while (!q.empty())
    {
        auto [ds, x] = q.top();
        q.pop();
        if (dis[x] != ds)
            continue;
        if (x == 1)
        {
            cout << fixed << setprecision(10) << ds << endl;
            return;
        }
        for (auto [v, w] : g[x])
        {
            if (dis[v] > dis[x] + w)
            {
                dis[v] = dis[x] + w;
                q.push({dis[v], v});
            }
        }
    }
}

signed main()
{
    // freopen(".in","r",stdin);
    // freopen(".out","w",stdout);
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

### [ARC064F] Rotated Palindromes

因为原串是回文串，所以它的循环节必然为回文串

考虑枚举最小的循环节的大小

设长度为 $x$ 的最小循环节的可能数 $f(x)$

那么

$$
f(x)=k^{\lceil\frac{x}{2}\rceil}-\sum\limits _{v|x} f(v)
$$

- 对于每个长度 $x$ 为奇数的循环节，在 $2$ 操作后会有 $x$ 种不同情况
- 对于每个长度 $x$ 为偶数的循环节，在 $2$ 操作后会有 $\frac{x}{2}$ 种不同情况

累加贡献即可

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

// bool st;
int n, m;
int res = 0;
vector<int> vec;
map<int, int> dp;
// bool en;

void solve()
{
    cin >> n >> m;
    rep(i, 1, n)
    {
        if (i * i > n)
            break;
        if (n % i == 0)
        {
            vec.push_back(i);
            if (i * i != n)
                vec.push_back(n / i);
        }
    }
    sort(ALL(vec));
    for (auto v : vec)
    {
        dp[v] = qpow(m, (v + 1) / 2);
        for (auto p : vec)
        {
            if (p >= v)
                break;
            if (v % p == 0)
                dp[v] = ((dp[v] - dp[p]) % MOD + MOD) % MOD;
        }
        // cerr << dp[v] << ' ';
    }
    for (auto v : dp)
    {
        if (v.first % 2 == 1)
            res = (res + v.second * v.first % MOD) % MOD;
        else
            res = (res + v.second * v.first / 2 % MOD) % MOD;
    }
    cout << res << endl;
}

signed main()
{
    // freopen(".in","r",stdin);
    // freopen(".out","w",stdout);
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

## arc065

### [ARC065F] シャッフル

$dp_{i,j}$ 表示考虑到 $i$ 用了 $j$ 个 $1$ 的方案数

考虑转移的时候，不能从 $[1,i-1]$ 转移，因为有一些是不可能取到的

预处理出前缀和 $sum_i$，当前点能交换到的最右边的位置 $r_i$

则 $i$ 这个位置能被转移到的区间为 $[\max{(0,sum_{r_i}-r_i+i)},\min{(i,sum_{r_i})}]$

直接转移即可

$$
dp_{i,j}\leftarrow \begin{cases}
    dp_{i-1,j}\\
    dp_{i-1,j-1} (j\neq 0)
\end{cases}
$$

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
const int N = 3e3 + 10;

// bool st;
int n, m;
char s[N];
int sum[N];
int r[N];
int dp[N][N];
// bool en;

void solve()
{
    cin >> n >> m;
    cin >> (s + 1);
    rep(i, 1, n)
    {
        sum[i] = sum[i - 1] + (s[i] == '1');
    }
    rep(i, 1, m)
    {
        int st, en;
        cin >> st >> en;
        r[st] = max(r[st], en);
    }
    dp[0][0] = 1;
    rep(i, 1, n) r[i] = max(r[i], max(r[i - 1], i));
    rep(i, 1, n)
    {
        rep(j, max(0, sum[r[i]] - (r[i] - i)), min(i, sum[r[i]]))
        {
            dp[i][j] += dp[i - 1][j];
            if (j)
                (dp[i][j] += dp[i - 1][j - 1]) %= MOD;
        }
    }
    cout << dp[n][sum[n]] << endl;
}

signed main()
{
    // freopen(".in","r",stdin);
    // freopen(".out","w",stdout);
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

### [ARC065E] へんなコンパス

两点之间的距离是恒定不变的

考虑将曼哈顿距离转换为切比雪夫距离，具体来说就是 $(x,y)\rightarrow (x+y,x-y)$ 此时两个点 $(x_1,y_1),(x_2,y_2)$ 之间的距离为 $\max(|x_1-x_2|,|y_1-y_2|)$

设原来的距离为 $dis$， 则距离为 $dis$ 的两点需要满足

$$

\begin{cases}
    |x_1-x_2|=dis\\
    |y_1-y_1|\leq dis
\end{cases}

或

\begin{cases}
    |x_1-x_2|\leq dis\\
    |y_1-y_1|= dis
\end{cases}

$$

考虑如何计算满足第一个条件的点对数量，可以按 $x$ 排序，然后对于每个点，合法区间的左右端点都具有单调性，two-pointers即可

然后将 $(x,y)\rightarrow (y,x)$ 再做上述操作

但是这样会在 $|x_1-x_2|=|y_1-y_2|$ 时计算重复，所以第二次时保证 $|y_1-y_2|<dis$ 即可

感觉luogu黑题有点虚标，可能你看到这个的时候已经不是了吧~~~

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
struct node
{
	int x, y, id;
} s[N];
int n, a, b;
struct union_set
{
	int fa[N], sz[N];
	void init()
	{
		rep(i, 1, n) fa[i] = i, sz[i] = 0;
	}

	int get(int x)
	{
		if (fa[x] == x)
			return x;
		return fa[x] = get(fa[x]);
	}

	void merge(int x, int y)
	{
		int fx = get(x), fy = get(y);
		if (fx == fy)
			return;
		fa[fy] = fx;
		sz[fx] += sz[fy];
		sz[fy] = 0;
	}
} uni;
// bool en;

bool cmp(node a, node b)
{
	if (a.x != b.x)
		return a.x < b.x;
	return a.y < b.y;
}

void calc(int dis, int cur)
{
	int l, r;
	l = r = 1;
	int la = 1;
	rep(i, 1, n)
	{
		while (l <= n && s[i].x - s[l].x > dis)
			l++;
		while (l <= n && s[i].x - s[l].x == dis && s[i].y - s[l].y > dis - cur)
			l++;
		while (r <= n && s[i].x - s[r].x > dis)
			r++;
		while (r <= n && s[i].x - s[r].x == dis && s[r].y - s[i].y <= dis - cur)
			r++;
		if (l >= r || s[i].x - s[l].x != dis)
			continue;
		// cerr << i << ' ' << l << ' ' << r - 1 << endl;
		uni.sz[uni.get(s[i].id)] += r - l;
		la = max(la, l);
		if (la >= r)
			continue;
		uni.merge(s[i].id, s[la].id);
		while (la < r - 1)
		{
			uni.merge(s[la].id, s[la + 1].id);
			la++;
		}
	}
}

void solve()
{
	cin >> n >> a >> b;
	uni.init();
	rep(i, 1, n)
	{
		int x, y;
		cin >> x >> y;
		s[i].x = x + y;
		s[i].y = x - y;
		s[i].id = i;
	}
	int dis = max(abs(s[a].x - s[b].x), abs(s[a].y - s[b].y));
	sort(s + 1, s + n + 1, cmp);
	calc(dis, 0);
	rep(i, 1, n) swap(s[i].x, s[i].y);
	sort(s + 1, s + n + 1, cmp);
	calc(dis, 1);
	cout << uni.sz[uni.get(a)] << endl;
}

signed main()
{
	// freopen(".in","r",stdin);
	// freopen(".out","w",stdout);
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

## arc092

### [ABC091D] Two Sequences

考虑对于每一位计算贡献

$a_i+b_j$ 的 $2^k$ 为 $1$ 当且仅当 $2^k\leq a_i+b_j<2^{k+1}$ 或 $3\times 2^k\leq a_i+b_j<4\times 2^{k+1}$

对于每一位，取模后对排序，二分即可

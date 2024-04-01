#dp #预处理 

$dp_{i,j}$ 表示第 $i$ 个选择，$i$ 前面的第一个为 $j$ 的方案数

预处理不合法的区间，暴力转移

```cpp
// Author: xiaruize
#ifndef ONLINE_JUDGE
bool start_of_memory_use;
#endif
#include <bits/stdc++.h>
using namespace std;
#ifndef ONLINE_JUDGE
clock_t start_clock = clock();
#endif
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
const int N = 300 + 10;

int n, m, tmp;
int l[N], r[N];
int dp[N][N];
bool st[N][N];

void input(int &n, int *fa)
{
    int T, x;
    string s, t;
    scanf("%lld\n", &T);
    for (; T--;)
    {
        getline(cin, t);
        s += t;
    }
    s += ' ', n = 0, x = 0;
    for (char c : s)
    {
        if (c == ' ')
            fa[++n] = x + 1, x = 0;
        else if (isdigit(c))
            x = x * 10 + c - '0';
    }
}

void solve()
{
    scanf("%lld", &m);
    input(n, l);
    input(n, r);
    rep(i, 1, n)
    {
        rep(a, l[i], r[i])
        {
            rep(b, a + 1, r[i])
            {
                st[a][b] = true;
            }
        }
    }
    int res = 0;
    dp[0][0] = 1;
    rep(i, 1, m)
    {
        dp[i][0] = 1;
        rep(j, 0, i - 1)
        {
            rep(k, 0, j - 1)
            {
                if (st[k][i])
                    continue;
                // cerr << "trans" << i << ' ' << j << ' ' << k << endl;
                (dp[i][j] += dp[j][k]) %= MOD;
            }
        }
        // rep(j, 0, i - 1) cerr << i << ' ' << j << ' ' << dp[i][j] << endl;
        rep(j, 1, m)(res += dp[i][j]) %= MOD;
    }
    printf("%lld", res + m + 1);
}

#ifndef ONLINE_JUDGE
bool end_of_memory_use;
#endif

signed main()
{
    // freopen(".in","r",stdin);
    // freopen(".out","w",stdout);
    // ios::sync_with_stdio(false);
    // cin.tie(0);
    // cout.tie(0);
    int testcase = 1;
    // cin >> testcase;
    while (testcase--)
        solve();
#ifndef ONLINE_JUDGE
    cerr << "Memory use:" << (&end_of_memory_use - &start_of_memory_use) / 1024.0 / 1024.0 << "MiB" << endl;
    cerr << "Time use:" << (double)clock() / CLOCKS_PER_SEC * 1000.0 << "ms" << endl;
#endif
    return 0;
}
```
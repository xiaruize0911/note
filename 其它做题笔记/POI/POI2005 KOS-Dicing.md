#网络流 #二分 

考虑二分答案，用 $Dinic$ 来 $check$ 

具体来说，就是对每一个人限制流量，然后检查能不能把所有场全部流满

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

struct MF {
    struct edge {
        int v, nxt, cap, flow;
    } e[N];

    int fir[N], cnt = 0;

    int n, S, T;
    int maxflow = 0;
    int dep[N], cur[N];

    void init() {
        // cerr << "flag" << endl;
        memset(fir, -1, sizeof(fir));
        cnt = 0;
    }

    void addedge(int u, int v, int w) {
        e[cnt] = { v, fir[u], w, 0 };
        fir[u] = cnt++;
        e[cnt] = { u, fir[v], 0, 0 };
        fir[v] = cnt++;
    }

    bool bfs() {
        queue<int> q;
        memset(dep, 0, sizeof(int) * (n + 1));

        dep[S] = 1;
        q.push(S);
        while (q.size()) {
            int u = q.front();
            q.pop();
            for (int i = fir[u]; ~i; i = e[i].nxt) {
                int v = e[i].v;
                if ((!dep[v]) && (e[i].cap > e[i].flow)) {
                    dep[v] = dep[u] + 1;
                    q.push(v);
                }
            }
        }
        return dep[T];
    }

    int dfs(int u, int flow) {
        if ((u == T) || (!flow))
            return flow;

        int ret = 0;
        for (int &i = cur[u]; ~i; i = e[i].nxt) {
            int v = e[i].v, d;
            if ((dep[v] == dep[u] + 1) && (d = dfs(v, min(flow - ret, e[i].cap - e[i].flow)))) {
                ret += d;
                e[i].flow += d;
                e[i ^ 1].flow -= d;
                if (ret == flow)
                    return ret;
            }
        }
        return ret;
    }

    void dinic() {
        while (bfs()) {
            memcpy(cur, fir, sizeof(int) * (n + 1));
            maxflow += dfs(S, INF);
            // cerr << maxflow << endl;
        }
    }
} mf;

// bool st;
int n, m;
pii a[N];
// bool en;

bool check(int x) {
    mf.init();
    mf.n = n + m + 1;
    mf.S = 0;
    mf.maxflow = 0;
    mf.T = n + m + 1;
    for (int i = 1; i <= n; i++) mf.addedge(0, i, x);
    for (int i = 1; i <= m; i++) {
        mf.addedge(a[i].first, i + n, 1);
        mf.addedge(a[i].second, i + n, 1);
        mf.addedge(i + n, n + m + 1, 1);
    }
    mf.dinic();
    return mf.maxflow >= m;
}

void solve() {
    cin >> n >> m;
    for (int i = 1; i <= m; i++) cin >> a[i].first >> a[i].second;
    int l = 0, r = m;
    while (l < r) {
        int mid = l + r >> 1;
        if (check(mid))
            r = mid;
        else
            l = mid + 1;
    }
    cout << l << endl;
}

signed main() {
    // freopen(".in","r",stdin);
    // freopen(".out","w",stdout);
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
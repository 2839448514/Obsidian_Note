## ST表求区间最大值

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <math.h> // 包含数学库，用于 log2 函数
using namespace std;

// 定义长整型别名
typedef long long ll;

// 定义全局变量
vector<ll> v; // 存储数组的值
int dp_max[505550][222]; // 稀疏表，存储区间最大值
int dp_min[550550][222]; // 稀疏表，存储区间最小值
ll n, q; // n 为数组大小，q 为查询次数

// 初始化稀疏表（Sparse Table）
void st_init()
{
    // 初始化稀疏表的 0 层，区间长度为 1 时直接赋值
    for (int i = 1; i <= n; i++)
    {
        dp_max[i][0] = v[i]; // 最大值表，长度为 1 的区间
        dp_min[i][0] = v[i]; // 最小值表，长度为 1 的区间
    }

    // 计算稀疏表其他层的值
    int p = log2(n); // p 为最大层数，即 log2(n)
    for (int k = 1; k <= p; k++) // 枚举层数 k，区间长度为 2^k
    {
        for (int i = 1; i + (1 << k) - 1 <= n; i++) // 从第 i 个位置开始，不能越界
        {
            // dp_max[i][k] 表示从 i 开始，长度为 2^k 的区间的最大值
            dp_max[i][k] = max(dp_max[i][k - 1], dp_max[i + (1 << (k - 1))][k - 1]);
            // dp_min[i][k] 表示从 i 开始，长度为 2^k 的区间的最小值
            dp_min[i][k] = min(dp_min[i][k - 1], dp_min[i + (1 << (k - 1))][k - 1]);
        }
    }
}

// 进行区间查询，返回区间 [l, r] 内最大值和最小值的差
int st_query(int l, int r)
{
    int k = log2(r - l + 1); // k 为区间 [l, r] 的层数

    // 从 dp_max 和 dp_min 中分别查询最大值和最小值
    int x = max(dp_max[l][k], dp_max[r - (1 << k) + 1][k]); // 区间最大值
    int y = min(dp_min[l][k], dp_min[r - (1 << k) + 1][k]); // 区间最小值

    return x - y; // 返回区间最大值与最小值的差
}

int main()
{
    cin >> n >> q; // 输入数组大小 n 和查询次数 q

    // 调整数组大小，+1000 是为了防止越界（保留冗余空间）
    v.resize(n + 1000);

    // 输入数组的值
    for (int i = 1; i <= n; i++)
    {
        cin >> v[i];
    }

    // 初始化稀疏表
    st_init();

    // 处理 q 次查询
    for (int i = 0; i < q; i++)
    {
        int l, r; // 查询区间 [l, r]
        cin >> l >> r;

        // 输出区间 [l, r] 的最大值和最小值的差
        cout << st_query(l, r) << endl;
    }

    return 0;
}

```

## ST表求区间最大公约数

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <map>
#include <set>
#include <queue>
#include <deque>
#include <stack>
#include <string>
#include <sstream>
#include <list>
#include <climits>
#include <unordered_map>
#include <unordered_set>
#include <cstdlib>
#include <ctime>
#include <math.h>
#include <cstring>
#include <iomanip>

using namespace std;

typedef long long ll;

ll n, m;
vector<ll> v;
int dp_max[505550][222];


void st_init()
{
    for (int i = 1; i <= n; i++)
    {
        dp_max[i][0] = v[i];
    }
    ll p = log2(n);
    for (int k = 1; k <= p; k++)
    {
        for (int i = 1; i + (1 << k) - 1 <= n; i++)
        {
            // dp_max[i][k] = max(dp_max[i][k - 1], dp_max[i + (1 << (k - 1))][k - 1]);
            dp_max[i][k] = __gcd(dp_max[i][k - 1], dp_max[i + (1 << (k - 1))][k - 1]);
        }
    }
}
int st_query(int l, int r)
{
    ll k = log2(r - l + 1);

    ll x = __gcd(dp_max[l][k], dp_max[r - (1 << k) + 1][k]);

    return x;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);

    cin >> n >> m;

    v.resize(n + 1000);

    for (int i = 1; i <= n; i++)
    {
        cin >> v[i];
    }

    st_init();
    for (int i = 0; i < m; i++)
    {
        int l, r;
        cin >> l >> r;
        cout << st_query(l, r) << endl;
    }
    return 0;
}
```

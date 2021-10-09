---
title: Luogu P2177 内存杀手 题解
date: 2021-10-09 21:53:52
tags:
 - C++
 - OI
 - 二分
 - 哈希
categories:
 - OI
---

本题前置算法知识：
 - 二分答案
 - 哈希

不同于其他题解，这里给出一种时间复杂度为 $O(nm \log_2 \min (n,m))$ 的做法，目前为最优解。  
{% asset_img record.png %}

<!-- more -->

### 算法分析
二维哈希其他题解都已经讲过了，这里就不再赘述，我们讲重点。如果有做过 [反对称](https://loj.ac/p/2452) 这道题，应该就知道这题也是类似的做法。

回到本题，题目要求我们找到一个边长最大的 $01$ 方阵，且满足旋转 $\pi$ 后与原来一样，即这个方阵中心对称。
考虑一个已经符合题意的方阵，它的边长为 $a$，则以它为中心，边长小于 $a$ 的方阵也一定是中心对称的。
这就说明，同一个中心的方阵系中，其合法性是具有单调性的，对于一个点 $P$，一定可以找到一个 $b$，满足
所有边长小于等于 $a$ 且已 $P$ 为中心的方阵都是中心对称的，而所有边长大于 $a$ 且已 $P$ 为中心的方阵都不是中心对称的。

有了这个性质，我们就可以进行二分答案。即确定一个边长，然后根据边长找出所有方阵，进行判断并缩小答案所在区间。
需要注意的是，我们不能对边长进行二分。为什么？由于边长的奇偶性会影响中心的位置，
中心可能在矩阵的格子上，也可能在格子之间，而上述性质都是建立在中心相同的条件之上。
我们对点的位置进行讨论。对于每一种情况进行一次二分答案，最后取两者的最大值即可。

时间复杂度 $O(nm \log_2 \min (n,m))$。

### 参考程序
```cpp
#include <bits/stdc++.h>
using namespace std;

/** constexpr 是 C++ 11 的新增关键字，用来定义一个编译期常量，推荐用这个替代 #define */
constexpr int maxn = 300 + 10;
constexpr int mod = 998244353;
constexpr int base1 = 258 + 1e9 + 7;
constexpr int base2 = 97 + 1e9 + 9;

/**
 * using 语句，也是 C++ 11 新增语法，等价于 typedef，但要更加强大。
 * 如 template <typename T> using MapType = map<int, T>;
 */
using ll = long long;

int n, m;
ll mat[2][maxn][maxn], h[2][maxn][maxn], power1[maxn], power2[maxn];
char tmp[maxn];

int calc(int which, int x1, int y1, int x2, int y2) {
    ll res = h[which][x2][y2] -
             h[which][x1 - 1][y2] * power1[x2 - x1 + 1] % mod -
             h[which][x2][y1 - 1] * power2[y2 - y1 + 1] % mod +
             h[which][x1 - 1][y1 - 1] * power1[x2 - x1 + 1] % mod * power2[y2 - y1 + 1] % mod;
    return (res + mod) % mod;
}

bool check(int x, int y, int a) {
    int r = (a + 1) / 2;
    if (a % 2) {
        int x1 = x - r + 1, y1 = y - r + 1;
        int x2 = x + r - 1, y2 = y + r - 1;
        int rx1 = n - x1 + 1, ry1 = m - y1 + 1;
        int rx2 = n - x2 + 1, ry2 = m - y2 + 1;
        return calc(0, x1, y1, x2, y2) == calc(1, rx2, ry2, rx1, ry1);
    } else {
        int x1 = x - r + 1, y1 = y - r + 1;
        int x2 = x + r, y2 = y + r;
        int rx1 = n - x1 + 1, ry1 = m - y1 + 1;
        int rx2 = n - x2 + 1, ry2 = m - y2 + 1;
        return calc(0, x1, y1, x2, y2) == calc(1, rx2, ry2, rx1, ry1);        
    }
}

bool check(int a) {
	/** 这里传入边长，根据边长判断 */
    int r = (a + 1) / 2;
    if (a % 2) {
        for (int i = r; i + r - 1 <= n; ++i)
            for (int j = r; j + r - 1 <= m; ++j)
                if (check(i, j, a))
                    return true;
        return false;
    } else {
        for (int i = r; i + r <= n; ++i)
            for (int j = r; j + r <= m; ++j)
                if (check(i, j, a))
                    return true;
        return false;
    }
}

void preprocess() {
	/** 求二维哈希 */
    power1[0] = power2[0] = 1;
    for (int i = 1; i <= max(n, m); ++i) {
        power1[i] = power1[i - 1] * base1 % mod;
        power2[i] = power2[i - 1] * base2 % mod;
    }
    
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            h[0][i][j] = (h[0][i][j - 1] * base2 + mat[0][i][j]) % mod;
            h[1][i][j] = (h[1][i][j - 1] * base2 + mat[1][i][j]) % mod;            
        }
    }
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            h[0][i][j] = (h[0][i - 1][j] * base1 + h[0][i][j]) % mod;
            h[1][i][j] = (h[1][i - 1][j] * base1 + h[1][i][j]) % mod;
        }
    }
}

void divide() {
	/** 对不同的位置情况分别二分 */
    int l = 1, r = min(n, m) / 2 + 1, ans1 = -1, ans2 = -1;
    while (l <= r) {
        int mid = (l + r) / 2;
        if (check(mid * 2)) {
            ans1 = mid * 2;
            l = mid + 1;
        } else {
            r = mid - 1;
        }
    }
    l = 1, r = min(n, m) / 2 + 1;
    while (l <= r) {
        int mid = (l + r) / 2;
        if (check(mid * 2 + 1)) {
            ans2 = mid * 2 + 1;
            l = mid + 1;
        } else {
            r = mid - 1;
        }
    }
    cout << max(ans1, ans2) << endl;
}

int main() {
    ios::sync_with_stdio(false);
    cin >> n >> m;
    for (int i = 1; i <= n; ++i) {
        cin >> (tmp + 1);
        for (int j = 1; j <= m; ++j) {
            mat[0][i][j] = tmp[j] - '0';
            /** 储存中心对称的矩阵 */
            mat[1][n - i + 1][m - j + 1] = tmp[j] - '0';
        }
    }
    preprocess();
    divide();
    return 0;
}
```
### 提示
如果你觉得写的没问题却 WA 了，检查一下是不是哈希冲突了，有条件就写双哈希，这道题写单哈希很容易被卡。
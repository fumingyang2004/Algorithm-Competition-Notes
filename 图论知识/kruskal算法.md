# 最小生成树的权-kruskal

获取一个无向图的最小生成树的权值

- 对图的边集合按权值从小到大排序
- 对每条边，检查两端的点是不是在一个并查集里
  - 是，则说明它们已经处于最小生成树里
  - 否，则获取其祖先并将其合并在同一个并查集里，权值递增。
```
#include<iostream>   // 包含输入输出流库
#include<cstring>    // 包含字符串处理库
#include<vector>     // 包含向量容器库
#include<algorithm>  // 包含算法库
#include<cmath>      // 包含数学库
using namespace std;

const int N = 1e5 + 4; // 定义常量N，表示最大节点数+4
int p[N];              // 数组p，用于存储每个节点的父节点
int cnt;               // 计数器，记录连接的边数
int n, m;              // n为节点数，m为边数
int ans;               // 最小生成树的总权值

// 并查集查找操作，找到节点a的根节点并进行路径压缩
int find(int a) {
    if (a != p[a]) p[a] = find(p[a]);
    return p[a];
}

// 定义边结构体
typedef struct edge {
    int a, b, w; // a和b为边的两个端点，w为边的权值
} edge;

edge e[2 * N]; // 边数组，存储所有边的信息

// 边的比较函数，用于按权值从小到大排序
static bool cmp(edge e1, edge e2) {
    return e1.w < e2.w;
}

// Kruskal算法，求最小生成树
void kruskal() {
    for (int i = 1; i <= m; i++) { // 遍历所有边
        int x = e[i].a; // 获取边的一个端点
        int y = e[i].b; // 获取边的另一个端点
        int z = e[i].w; // 获取边的权值

        int px = find(x); // 查找x的根节点
        int py = find(y); // 查找y的根节点

        if (px != py) { // 如果x和y不在同一个集合
            cnt++; // 计数器加1
            p[px] = py; // 将x的根节点指向y的根节点，合并两个集合
            ans += z; // 将边的权值加到结果中
        }
    }
}

int main() {
    ios :: sync_with_stdio(false); cin.tie(0); // 优化输入输出
    cin >> n >> m; // 读取节点数和边数

    for (int i = 1; i <= m; i++) {
        int x, y, z;
        cin >> x >> y >> z; // 读取每条边的信息
        e[i].a = x; // 边的一个端点
        e[i].b = y; // 边的另一个端点
        e[i].w = z; // 边的权值
    }
    sort(e + 1, e + m + 1, cmp); // 按权值对边进行排序

    for (int i = 1; i <= n; i++) p[i] = i; // 初始化并查集，每个节点的父节点指向自己

    kruskal(); // 调用Kruskal算法

    if (cnt < n - 1) cout << "impossible" << endl; // 如果连通边数小于n-1，输出impossible
    else cout << ans; // 否则输出最小生成树的总权值
}

```
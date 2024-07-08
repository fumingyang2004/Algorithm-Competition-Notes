# 区间DP模版

- 先在小区间进行DP得到最优解，然后再利用小区间的最优解合并求大区间的最优解
- 操作往往涉及到区间合并问题

模板如下：
```
 for (int len = 1; len <= n; len++) {
    for (int i = 1; len + i - 1 <= n; i++) {
        int j = len + i - 1;
        if (i == j) dp[i][j] = 0;
        else {
            for (int t = i; t <= j - 1; t++) 
            dp[i][j] = min(dp[i][j], dp[i][t] + dp[t + 1][j] + w[i][j]);
        }
    }
}
```



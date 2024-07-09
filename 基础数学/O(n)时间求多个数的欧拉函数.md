# 求一连串数字的欧拉函数

- 答案存放在数组eular里面
- 基于线性筛法

与线性筛相比，改进的地方在于：
1. 素数时，直接在对应的位置放上i - 1。这个好理解。
2. 欧拉筛法判断未来的合数时，如果i是pj的倍数时，说明i = kpj，也就是说pj * i = kpj * pj，那么pj * i的质因子分解方法和i实际上是一样的。而φ(i) = i * xxx，那么φ(pj * i) 就是 (pj * i) * xxx = pj * φ(i)；
3. 如果i不是pj的倍数时，这时pj就是i * pj的一个全新的质因数，φ(pj * i) = pj * i *xxxx* (1 - 1/pj)，而φ(i) = i * xxxx. 故φ(pj * i) = (pj - 1) φ(i).

```
int st[N];
int prime[N]; 
int euler[N];
int cnt;

void Eular(int n) {
    euler[1] = 1;
    for (int i = 2; i <= n; i++) {
        if (!st[i]) {
            prime[cnt++] = i;
            euler[i] = i - 1;
        }
        for (int j = 0; prime[j] * i <= n; j++) {
            st[prime[j] * i] = true;
            if (i % prime[j] == 0) {
                euler[i * prime[j]] = prime[j] * euler[i];
                break;
            } else {
                euler[i * prime[j]] = (prime[j] - 1) * euler[i];
            }
        }
    }
}
```
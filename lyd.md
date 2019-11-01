本文数域讨论范围为$\mathbb N$。

# 质数

### 分布

较为稀疏，对一个足够大的 $N$，$[1,N]$ 的质数约有 $O(\frac{N}{\ln N})$ 个。

### 单体判定质数

特判；试除$[2,\sqrt N]$。

```c++
bool is_prime(int n)
{
    if (n < 2) return false; //特判
    for (int i = 2; i <= sqrt(n); i++)
        if (n % i == 0) return false;
    return true;
}
```

### 筛质数

##### 埃氏筛法

对每一个质数 $p$，将 $p\cdot i$，其中$i\in [p,\lfloor \frac{n}{p}\rfloor]$标记为合数。时间复杂度 $O(N\log\log N)$。（证明待补充）

```c++
void primes(int n) {
    memset(v, 0, sizeof(v)); // 合数标记
    for (int i = 2; i <= n; i++) {
        if (v[i]) continue;
        // 现在 i 是质数
        for (int j = i; j <= n/i; j++) v[i*j] = 1;
    }
}
```

##### 线性筛法

用 ```v[]``` 记录每一个数的最小质因子。时间复杂度 $O(N)$，因为每一个合数只会被其最小值因子筛一次。

$\forall x, \exists i:v_x\cdot i=x$，因此对每一个 $i$，```enum each p<=v[i];``` 。

```c++
int v[N], prime[N];
void primes(int n)
{
	memset(v, 0, sizeof(v)); // min factor
	m = 0; // number of primes
	for (int i = 2; i <= n; i++)
    {
		if (!v[i]) // not vis, i is prime 
			v[i]=i, prime[++m]=i;
        
        //enum each p<v[i]
		for (int j=1; prime[j]<=v[i] && prime[j]<=n/i && j<=m; j++) 
			v[i*prime[j]] = prime[j];
	}
}
```

### 单体质因数分解

只需检查质因数范围 $[2,\sqrt N]$，之后检查 $>1$ 残余。时间复杂度  $O(N)$。

```c++
void divide(int n) {
    m = 0;
    for (int i = 2; i*i <= n; i++) {
        if (n % i == 0)
        {
            p[++m] = i;
            while (n % i == 0) n /= i, c[m]++;
        }
    }
    if (n > 1) // 残余质数
        p[++m] = n, c[m] = 1;
}
```

### 灵活运用

注意到上面的算法有两个重要思想：

1. 质因数上界$\sqrt{N}$。这意味着预处理 $\sqrt{Max}$ 内的质数，然后用搜索算法组成（非正解）。而
2. 答案主动去标记询问。

# 约数

$d\mid n (d\ne0)$，其中 $n=0$ 时必然成立。

### 约数和

$\prod _ {i=1}^{m}\sum _{j=0}^{c_i}p_i^j$

### 单体正约数集合

扫描 $d\in [1,\sqrt N]$，$d,n/d$ 都是质数。时间复杂度  $O(\sqrt N)$。

```c++
int factor[1600], m = 0;
for (int i = 1; i*i <= n; i++) {
    if (n % i == 0) {
        factor[++m] = i;
        if (i != n/i) factor[++m] = n/i;
    }
}
```

### $[1,N]$ 正约数集合

每个因数主动标记。

### 数论分块

$\forall i, [i, n/(n/i)]$ 中，$n/i$ 相等，且 $n/(n/i)$ 是最大边界。（待证明）

### 最大公约数

$\gcd(a,b)\cdot {\rm lcm}(a,b)=a\cdot b$

考虑公约数集合，对 $\forall d\mid a,b$ 进行等价可逆变形可得：

$$\gcd(a,b)=\gcd(b,a-b)$$ 更相损减数 

$$\gcd(a,b)=\gcd(b,a\bmod b)$$ 欧几里得算法 

高精度时建议使用更相损减数。(lyd)

### 互质

$\gcd(a,b)=1$，称 $a,b$ 互质。

在实现中：

- $\gcd(0,a)=a$
- $1$ 与任何数互质
- 其他的 $a$ 对自身不互质。

### 欧拉函数

 $\varphi(n)$ 表示 $[1,n]$ 中与 $n$ 互质的数的个数，包含 $1$，除了 $1$ 不包含自己的个数。

 设 $n = p_1^{k_1}p_2^{k_2} \cdots p_s^{k_s}$ ，其中 $p_i$ 是质数，定义 $\varphi(n) = n \cdot \prod_{i = 1}^s{\frac{p_i - 1}{p_i}}$ （容斥原理）。

显然 $\varphi(1) = 1$，$\varphi(2)=1$ 。

当 n 是质数的时候，显然有 $\varphi(n) = n - 1$ 。若 $n = p^k$ ，其中 $p$ 是质数，那么 $\varphi(n) = p^k - p^{k - 1}$ 。

##### 性质

- 欧拉函数是积性函数。若 $a,b$ 互质，则 $\varphi(ab) = \varphi(a) \cdot \varphi(b)$ 。
-  $n = \sum_{d | n}{\varphi(d)}$ （lyd）

剩下的省略在[oi-wiki]( https://oi-wiki.org/math/euler/ )和lyd。

## 乘法逆元

 https://oi-wiki.org/math/inverse/ 


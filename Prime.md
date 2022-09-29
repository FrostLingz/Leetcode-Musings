## 高效寻找素数
#### 正常判断素数的方法

```java
int countPrimes(int n) {
    int count = 0;
    for (int i = 2; i < n; i++)
        if (isPrim(i)) count++;
    return count;
}

// 判断整数 n 是否是素数
boolean isPrime(int n) {
    for (int i = 2; i < n; i++)
        if (n % i == 0)
            // 有其他整除因子
            return false;
    return true;
}
```
> 这样写的话时间复杂度 O(n^2)，问题很大。  
> 首先用 isPrime 函数来辅助的思路就不够高效；  
> 而且就算要用 isPrime 函数，这样实现也是存在计算冗余的。  

#### 时间复杂度更低的方法
> i 不需要遍历到 n，而只需要到sqrt(n)即可。  
> 后两个乘积就是前面两个反过来，反转的分界点就在sqrt(n)
> 12 = 2 × 6  
> 12 = 3 × 4   
> 12 = sqrt(12) × sqrt(12)  
> 12 = 4 × 3   
> 12 = 6 × 2   

```java
boolean isPrime(int n) {
    for (int i = 2; i * i <= n; i++)
        ...
}
```

#### 高效实现 countPrimes
```java
int countPrimes(int n) {
    boolean[] isPrim = new boolean[n];
    // 将数组都初始化为 true
    Arrays.fill(isPrim, true);

    for (int i = 2; i < n; i++) 
        if (isPrim[i]) 
            // i 的倍数不可能是素数了
            for (int j = 2 * i; j < n; j += i) 
                    isPrim[j] = false;

    int count = 0;
    for (int i = 2; i < n; i++)
        if (isPrim[i]) count++;

    return count;
}
```

> 首先，回想刚才判断一个数是否是素数的isPrime函数，由于因子的对称性，其中的 for 循环只需要遍历[2,sqrt(n)]就够了。  
> 这里也是类似的，我们外层的 for 循环也只需要遍历到sqrt(n)。

```java
for (int i = 2; i * i < n; i++) 
    if (isPrim[i]) 
        ...
```

> 除此之外，很难注意到内层的 for 循环也可以优化。  
> 把i的整数倍都标记为false，但是仍然存在计算冗余。  
> 比如i = 4时算法会标记 4 × 2 = 8，4 × 3 = 12 等等数字，  
> 但是 8 和 12 已经被i = 2和i = 3的 2 × 4 和 3 × 4 标记过了。  
> 我们可以稍微优化一下，让j从i的平方开始遍历，而不是从2 * i开始。

```java
for (int j = i * i; j < n; j += i) 
    isPrim[j] = false;
```
#### 完整代码

> 这个算法有一个名字，叫做 Sieve of Eratosthenes。
```java
int countPrimes(int n) {
    boolean[] isPrim = new boolean[n];
    Arrays.fill(isPrim, true);
    for (int i = 2; i * i < n; i++) 
        if (isPrim[i]) 
            for (int j = i * i; j < n; j += i) 
                isPrim[j] = false;

    int count = 0;
    for (int i = 2; i < n; i++)
        if (isPrim[i]) count++;

    return count;
}
```

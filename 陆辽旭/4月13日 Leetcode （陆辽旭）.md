## 4月13日 Leetcode （陆辽旭）

#### 264.丑数||（[264. 丑数 II - 力扣（LeetCode）](https://leetcode.cn/problems/ugly-number-ii/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240413173657.png)

##### code：

```c
int min(int a, int b, int c) {
    int min_val = (a < b) ? a : b;
    min_val = (min_val < c) ? min_val : c;
    return min_val;
}

int nthUglyNumber(int n) {
    int ugly[n]; // 存储丑数序列
    ugly[0] = 1; // 第一个丑数是1
    int p2 = 0, p3 = 0, p5 = 0; // 分别指向已经乘过2、3、5的丑数的位置
    for (int i = 1; i < n; i++) {
        // 计算下一个丑数，取三个指针指向的位置乘以对应的质因子2、3、5中的最小值
        int next_ugly = min(ugly[p2] * 2, ugly[p3] * 3, ugly[p5] * 5);
        ugly[i] = next_ugly;
        // 更新指针
        if (next_ugly == ugly[p2] * 2) p2++;
        if (next_ugly == ugly[p3] * 3) p3++;
        if (next_ugly == ugly[p5] * 5) p5++;
    }
    return ugly[n - 1]; // 返回第n个丑数
}
```


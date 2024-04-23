## 4月20日 Leetcode （陆辽旭）

#### 188.买卖股票的最佳时机IV（[188. 买卖股票的最佳时机 IV - 力扣（LeetCode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240421185940.png)

##### code：

```c
int maxProfit(int k, int* prices, int pricesSize) {
    int n = pricesSize;
    if (n == 0) {
        return 0;
    }
    // 限制交易次数为 k 次
    k = fmin(k, n / 2);
    // 用于记录买入和卖出的状态
    int buy[n][k + 1], sell[n][k + 1];
    memset(buy, 0, sizeof(buy)); // 初始化为 0
    memset(sell, 0, sizeof(sell)); // 初始化为 0
 
    buy[0][0] = -prices[0]; // 第一天买入
    sell[0][0] = 0; // 第一天不卖出
    // 初始化第一天交易次数不同的情况
    for (int i = 1; i <= k; ++i) {
        buy[0][i] = sell[0][i] = INT_MIN / 2; // 将第一天的买入状态初始化为最小值
    }
    for (int i = 1; i < n; ++i) {
        buy[i][0] = fmax(buy[i - 1][0], sell[i - 1][0] - prices[i]); // 不交易时保持上一次买入的状态，或者今天买入
        for (int j = 1; j <= k; ++j) {
            buy[i][j] = fmax(buy[i - 1][j], sell[i - 1][j] - prices[i]); // 不交易时保持上一次买入的状态，或者今天买入
            sell[i][j] = fmax(sell[i - 1][j], buy[i - 1][j - 1] + prices[i]); // 不交易时保持上一次卖出的状态，或者今天卖出
        }
    }
    // 最后求得最大利润
    int ret = 0;
    for (int i = 0; i <= k; i++) {
        ret = fmax(ret, sell[n - 1][i]); // 取最后一天卖出时的最大利润
    }
    return ret;
}
```





#### 404.左叶子之和（[404. 左叶子之和 - 力扣（LeetCode）](https://leetcode.cn/problems/sum-of-left-leaves/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240421190858.png)

##### code：

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */


int sumOfLeftLeaves(struct TreeNode* root){
    if (root == NULL) return 0;
    if (root->left == NULL && root->right== NULL) return 0;
    int leftValue = sumOfLeftLeaves(root->left);    // 左
    if (root->left && !root->left->left && !root->left->right) { // 左子树就是一个左叶子的情况
        leftValue = root->left->val;
    }
    int rightValue = sumOfLeftLeaves(root->right);  // 右
    int sum = leftValue + rightValue;               // 中
    return sum;
}
```


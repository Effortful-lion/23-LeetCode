## 4月19日 Leetcode （陆辽旭）

#### 222.完全二叉树的节点个数（[222. 完全二叉树的节点个数 - 力扣（LeetCode）](https://leetcode.cn/problems/count-complete-tree-nodes/description/)）

![](C:\Users\abeik\Desktop\3g\每日一题\4.19\QQ截图20240419174404.png)

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
int countNodes(struct TreeNode* root) {
    if (root == NULL) {
        return NULL;
    }
    struct TreeNode* queue[50000];
    int front = 0, rear = 0, count = 0;
    queue[rear++] = root;
    while(front != rear) {
        int len = rear - front;
        for (int i = 0; i < len; ++i) {
            struct TreeNode* temp = queue[front++];
            if (temp->left != NULL) {
                queue[rear++] = temp->left;//左孩子存在
            }
            if (temp->right != NULL) {
                queue[rear++] = temp->right;//右孩子存在
            }
            count++;//加入节点
        }
    }
    return count;
}
```





#### 110.平衡二叉树（[110. 平衡二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/balanced-binary-tree/description/))

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240419180114.png)

##### code:

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
int height(struct TreeNode* root) {
    if (root == NULL) {
        return 0;
    } else {
        return fmax(height(root->left), height(root->right)) + 1;//返回最大高度，最底层高度是1，向上逐层递增
    }
}

bool isBalanced(struct TreeNode* root) {
    if (root == NULL) {
        return true;
    } else {
        //根节点左右子树存在，并且最大高度之差的绝对值要小于等于1
        return fabs(height(root->left) - height(root->right)) <= 1 && isBalanced(root->left) && isBalanced(root->right);
    }
}
```





#### 123.买卖股票的最佳时机|||（[123. 买卖股票的最佳时机 III - 力扣（LeetCode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240419180613.png)

##### code：

```c
//求最大值的宏
#define max(a, b) ((a) < (b) ? (b) : (a))
 
int maxProfit(int* prices, int pricesSize) {
    // 初始化第一次买入和卖出的状态
    int buy1 = -prices[0], sell1 = 0;
    // 初始化第二次买入和卖出的状态
    int buy2 = -prices[0], sell2 = 0;
    // 遍历股票价格数组
    for (int i = 1; i < pricesSize; ++i) {
        // 更新第一次买入的状态
        buy1 = max(buy1, -prices[i]);
        // 更新第一次卖出的状态
        sell1 = max(sell1, buy1 + prices[i]);
        // 更新第二次买入的状态
        buy2 = max(buy2, sell1 - prices[i]);
        // 更新第二次卖出的状态
        sell2 = max(sell2, buy2 + prices[i]);
    }
    // 返回最大利润，即第二次卖出的利润
    return sell2;
}
```


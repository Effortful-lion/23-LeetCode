## 4月23日 Leetcode （陆辽旭）

#### 700.二叉搜索树中的搜索（[700. 二叉搜索树中的搜索 - 力扣（LeetCode）](https://leetcode.cn/problems/search-in-a-binary-search-tree/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240423184901.png)

##### code：

```c
struct TreeNode* searchBST(struct TreeNode* root, int val) {
    while(root != NULL) {
        if (root->val == val) {
            break;
        }
        else if (root->val < val) {//小于目标值，往右子树搜
            root = root->right;
        }
        else if (root->val > val) {//大于目标值，往左子树搜
            root = root->left;
        }
    }
    return root;
}
```





#### 98.验证二叉搜索树（[98. 验证二叉搜索树 - 力扣（LeetCode）](https://leetcode.cn/problems/validate-binary-search-tree/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240423191111.png)

##### code：

```c
bool isValidBSTHelper(struct TreeNode* root, long min_val, long max_val) {
    // 如果根节点为空，直接返回 true，因为空树也是 BST
    if (root == NULL) {
        return true;
    }

    // 检查当前节点值是否在范围内
    if (root->val <= min_val || root->val >= max_val) {
        return false;
    }

    // 递归检查左右子树，更新范围
    return isValidBSTHelper(root->left, min_val, root->val) && 
           isValidBSTHelper(root->right, root->val, max_val);
}

bool isValidBST(struct TreeNode* root) {
    // 使用 long 类型的最小值和最大值作为初始范围
    return isValidBSTHelper(root, LONG_MIN, LONG_MAX);
}
```





#### 530.二叉搜索树的最小绝对值（[530. 二叉搜索树的最小绝对差 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240423194654.png)

##### code：

```c
void traversal(struct TreeNode* cur, struct TreeNode** pre, int *result) {
    if (cur == NULL) return;
    //BST中序遍历是升序
    traversal(cur->left, pre, result);   // 左
    if (*pre != NULL){       // 中
        *result = fmin(*result, cur->val - (*pre)->val);
    }
    *pre = cur; // 记录前一个
    traversal(cur->right, pre, result);  // 右
}


int getMinimumDifference(struct TreeNode* root) {
    int result = 114514;
    struct TreeNode* pre = NULL; // 初始值为NULL
    traversal(root, &pre, &result); // 传递pre的指针的指针
    return result;
}
```





#### 797.所有可能得路径（[797. 所有可能的路径 - 力扣（LeetCode）](https://leetcode.cn/problems/all-paths-from-source-to-target/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240423202429.png)

##### code：

```c
int** ret;
int* temp;
int result;

void dfs(int** graph, int cur, int index, int count, int* returnSize, int** returnColumnSizes, int* graphColSize) {
    // 当前节点为目标节点时，将路径存入 ret 中
    if (cur == result) {
        int* p = malloc(sizeof(int) * (count + 1));
        for (int i = 0; i < count; i++) {
            p[i] = temp[i];
        }
        p[count] = cur;
        ret[*returnSize] = p;
        (*returnColumnSizes)[(*returnSize)++] = count + 1;
        return;
    }
    // 遍历当前节点的相邻节点
    for (int i = 0; i < graphColSize[index]; i++) {
        temp[count++] = index; // 将当前节点加入
        cur = graph[index][i];
        // 递归搜索下一个节点
        dfs(graph, cur, cur, count, returnSize, returnColumnSizes, graphColSize);
        count--; // 回溯
    }
}

int** allPathsSourceTarget(int** graph, int graphSize, int* graphColSize, int* returnSize, int** returnColumnSizes) {
    ret = malloc(sizeof(int*) * 20000);
    temp = malloc(sizeof(int) * graphSize);
    *returnColumnSizes = malloc(sizeof(int) * 20000);
    result = graphSize - 1; // 目标节点
    *returnSize = 0;
    dfs(graph, 0, 0, 0, returnSize, returnColumnSizes, graphColSize);
    return ret;
}
```





#### 695.岛屿的最大面积（[695. 岛屿的最大面积 - 力扣（LeetCode）](https://leetcode.cn/problems/max-area-of-island/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240423202542.png)

##### code:

```c
//dfs深搜
void dfs(int** grid, int i, int j, int gridSize, int* gridColSize, int* count) {
    if(i < 0 || i >= gridSize || j < 0 || j >= *gridColSize || grid[i][j] == 0) { //越界或者是海
        return;
    }
    grid[i][j] = 0; //已搜索，状态改变
    (*count)++;
    //上下左右继续深入
    dfs(grid, i-1, j, gridSize, gridColSize, count);
    dfs(grid, i+1, j, gridSize, gridColSize, count);
    dfs(grid, i, j-1, gridSize, gridColSize, count);
    dfs(grid, i, j+1, gridSize, gridColSize, count);
}

int maxAreaOfIsland(int** grid, int gridSize, int* gridColSize) {
    int count = 0, max = 0;
    for(int i = 0; i < gridSize; i++) {
        for(int j = 0; j < *gridColSize; j++) { 
            if(grid[i][j] == 1) { 
                count = 0;
                dfs(grid, i, j, gridSize, gridColSize, &count);//"1"岛屿进入搜索
                max = max > count ? max : count;
            }
        }
    }
    return max;
}
```


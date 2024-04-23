## 4月22日 Leetcode （陆辽旭）

#### 112.路径总合（[112. 路径总和 - 力扣（LeetCode）](https://leetcode.cn/problems/path-sum/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240422214847.png)

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
bool traversal(struct TreeNode* cur, int count) {
    if (!cur->left && !cur->right && count == 0) return true; // 遇到叶子节点，并且计数为0
    if (!cur->left && !cur->right) return false; // 遇到叶子节点直接返回

    if (cur->left) { // 左
        count -= cur->left->val; // 递归，处理节点;
        if (traversal(cur->left, count)) return true;
        count += cur->left->val; // 回溯，撤销处理结果
    }

    if (cur->right) { // 右
        count -= cur->right->val; // 递归，处理节点;
        if (traversal(cur->right, count)) return true;
        count += cur->right->val; // 回溯，撤销处理结果
    }
    return false;
}

bool hasPathSum(struct TreeNode* root, int targetSum) {
    if (root == NULL) {
        return false;
    }
    return traversal(root, targetSum - root->val);
}
```





#### 113.路径总合||（[113. 路径总和 II - 力扣（LeetCode）](https://leetcode.cn/problems/path-sum-ii/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240422205709.png)

##### code：

```c
void dfs(struct TreeNode * root, int targetSum, int** res, int* resSize, int* path, int pathSize, int ** returnColumnSizes)
{
    if(root == NULL)
        return ;
    targetSum -= root->val;
    path[pathSize++] = root->val;//入栈
    if(root->left == NULL && root->right == NULL && targetSum == 0)
    {
        res[(*resSize)] = (int *)malloc(sizeof(int) * (pathSize));
        memcpy(res[(*resSize)], path, sizeof(int) * (pathSize));
        (*returnColumnSizes)[(*resSize)++] = pathSize;
    }
    //往下深搜
    dfs(root->left, targetSum, res, resSize, path, pathSize, returnColumnSizes);
    dfs(root->right, targetSum, res, resSize, path, pathSize, returnColumnSizes);
    targetSum += root->val;//回溯
    return;
}

int** pathSum(struct TreeNode* root, int targetSum, int* returnSize, int** returnColumnSizes){
    int** res = (int **)malloc(sizeof(int *) * 2001);
    *returnColumnSizes = (int *)malloc(sizeof(int) * 2001);
    int * path = (int *)malloc(sizeof(int) * 2001);//栈
    *returnSize = 0;
    dfs(root, targetSum, res, returnSize, path, 0, returnColumnSizes);
    return res;
}
```





#### 106.从中序与后序遍历序列构造二叉树（[106. 从中序与后序遍历序列构造二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240422212832.png)

##### code：

```c
struct TreeNode* buildTree(int* inorder, int inorderSize, int* postorder, int postorderSize) {
    // 若中序遍历或后序遍历为空，则返回空指针
    if (inorderSize == 0 || postorderSize == 0) return NULL;
    // 获取根节点的值，后序遍历的最后一个节点即为根节点
    int rootValue = postorder[postorderSize - 1];
    // 根据根节点的值在中序遍历中找到根节点的位置
    int delimiterIndex;
    for (delimiterIndex = 0; delimiterIndex < inorderSize; ++delimiterIndex) {
        if (inorder[delimiterIndex] == rootValue) break;
    }
    // 创建根节点
    struct TreeNode* root = malloc(sizeof(struct TreeNode));
    root->val = rootValue;
    // 分割数组之后，in和post的左右分割数组大小都是相等的，左边大小为delimiterIndex，右边大小为数组长度-delimiterIndex-1
    // 递归构建左子树
    root->left = buildTree(inorder, delimiterIndex, postorder, delimiterIndex);
    // 递归构建右子树
    root->right = buildTree(inorder + delimiterIndex + 1, inorderSize - delimiterIndex - 1,
                            postorder + delimiterIndex, postorderSize - delimiterIndex - 1);

    return root;
}
```





#### 654.最大二叉树（[654. 最大二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-binary-tree/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240422213829.png)

##### code：

```c
struct TreeNode* constructMaximumBinaryTree(int* nums, int numsSize) {
    // 如果数组为空，则返回空指针
    if (numsSize == 0) {
        return NULL;
    }
    // 找到数组中的最大值及其索引
    int maxIndex = 0;
    for (int i = 1; i < numsSize; ++i) {
        if (nums[i] > nums[maxIndex]) {
            maxIndex = i;
        }
    }
    // 创建根节点，并赋值为数组中的最大值
    struct TreeNode* root = malloc(sizeof(struct TreeNode));
    root->val = nums[maxIndex];
    // 递归构建左子树
    root->left = constructMaximumBinaryTree(nums, maxIndex);
    // 递归构建右子树
    root->right = constructMaximumBinaryTree(nums + maxIndex + 1, numsSize - maxIndex - 1);
    return root;
}
```





#### 617.合并二叉树（[617. 合并二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-two-binary-trees/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240422214724.png)

##### code：

```c
struct TreeNode* mergeTrees(struct TreeNode* root1, struct TreeNode* root2) {
    struct TreeNode* root = malloc(sizeof(struct TreeNode));
    // 若两棵树的根节点都为空，则返回空指针
    if (root1 == NULL && root2 == NULL) {
        return NULL;
    }
    // 若两棵树的根节点均不为空
    else if (root1 != NULL && root2 != NULL) {
        root->val = root1->val + root2->val;
        root->left = mergeTrees(root1->left, root2->left);
        root->right = mergeTrees(root1->right, root2->right);
    }
    // 若第一棵树的根节点为空，第二棵树的根节点不为空
    else if (root1 == NULL && root2 != NULL) {
        root->val = root2->val;
        root->left = mergeTrees(root1, root2->left);
        root->right = mergeTrees(root1, root2->right);
    }
    // 若第一棵树的根节点不为空，第二棵树的根节点为空
    else if (root1 != NULL && root2 == NULL) {
        root->val = root1->val;
        root->left = mergeTrees(root1->left, root2);
        root->right = mergeTrees(root1->right, root2);
    }
    return root;
}
```


## 4月15日 Leetcode （陆辽旭）

#### 144.二叉树的前序遍历（[144. 二叉树的前序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-preorder-traversal/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240415170737.png)

##### code:

```c
/* 前序遍历 */
void preOrder(struct TreeNode* root, int* ans, int* count) {
    if (root == NULL) {
        return;
    }
    // 访问优先级：根节点 -> 左子树 -> 右子树
    ans[(*count)++] = root->val;
    preOrder(root->left, ans, count);
    preOrder(root->right, ans, count);
}
int* preorderTraversal(struct TreeNode* root, int* returnSize) {
    int* ans = (int*)malloc(100 * sizeof(int));
    int count = 0;
    preOrder(root, ans, &count);
    *returnSize = count;
    return ans;
}
```





#### 94.二叉树的中序遍历（[94. 二叉树的中序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240415171223.png)

##### code:

```c
 /* 中序遍历 */
void inOrder(struct TreeNode* root, int* ans, int* count) {
    if (root == NULL) {
        return;
    }
    //访问优先级： 左子树 -> 根节点 -> 右子树
    inOrder(root->left, ans, count);
    ans[(*count)++] = root->val;
    inOrder(root->right, ans, count);
}

int* inorderTraversal(struct TreeNode* root, int* returnSize) {
    int* ans = (int*)malloc(100 * sizeof(int));
    int count = 0;
    inOrder(root, ans, &count);
    *returnSize = count;
    return ans;
}
```





#### 145.二叉树的后序遍历（[145. 二叉树的后序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-postorder-traversal/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240415171535.png)

##### code：

```c
  /* 后序遍历 */
void postTraversal(struct TreeNode* root, int* ans, int* count) {
    if(root == NULL) {
        return;
    }
    //访问优先级： 左子树 -> 右子树 -> 根节点
    postTraversal(root->left, ans, count);
    postTraversal(root->right, ans, count);
    ans[(*count)++] = root->val;
}

int* postorderTraversal(struct TreeNode* root, int* returnSize) {
    int* ans = (int*)malloc(100 * sizeof(int));
    int count = 0;
    postTraversal(root, ans, &count);
    *returnSize = count;
    return ans;
}
```





#### 102.二叉树的层序遍历（[102. 二叉树的层序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-level-order-traversal/description/)）

```c
int** levelOrder(struct TreeNode* root, int* returnSize, int** returnColumnSizes){
    *returnSize = 0;// 数组指针
    if(root == NULL) {
        return NULL;
    }
    int **res = malloc(sizeof(int*)*2000);
    *returnColumnSizes = malloc(sizeof(int*)*2000);
    struct TreeNode *queue[2000];//辅助队列
    int rear = 0, front = 0, count = 0;// 队列指针
    queue[rear++] = root;// 加入根节点
    while(front != rear){
        int len = rear - front;//队列长度，即上一个父节点的子节点数
        count = 0;
        res[*returnSize] = malloc(sizeof(int)*(len));
        for(int i = 0; i < len; i++){
            struct TreeNode *temp = queue[front++];// 队列出队
            res[*returnSize][count++] = temp->val;// 保存节点值
            if(temp->left)
                queue[rear++] = temp->left;// 左子节点入队
            if(temp->right)
                queue[rear++] = temp->right;// 右子节点入队
        }
        (*returnColumnSizes)[*returnSize] = count;//每行的列数
        (*returnSize)++;
    }
    return res;
}
```


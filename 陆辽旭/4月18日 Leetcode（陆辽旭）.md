## 4月18日 Leetcode（陆辽旭）

#### 104.二叉树的最大深度（[104. 二叉树的最大深度 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240418200811.png)

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
void getdepth(struct TreeNode* node, int depth, int* result) {
    *result = depth > *result ? depth : *result; //深度更新
    if (node->left == NULL && node->right == NULL) {
        return;
    }
    if (node->left) { //左节点存在，递归
        getdepth(node->left, depth + 1, result);
    }
    if (node->right) { //右节点存在，递归
        getdepth(node->right, depth + 1, result);
    }
    return;
    
}

int maxDepth(struct TreeNode* root) {
    int result = 0;
    if (root == NULL) {
        return 0;
    }
    getdepth(root, 1, &result);
    return result;
}
```





#### 111.二叉树的最小深度（[111. 二叉树的最小深度 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-depth-of-binary-tree/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240418205334.png)

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
void getdepth(struct TreeNode* node, int depth, int* result) {
    if (node->left == NULL && node->right == NULL) {
        *result = depth < *result ? depth : *result; //不存在左右节点后判断深度
        return;
    }
    if (node->left) { //左节点存在，递归深度加一
        getdepth(node->left, depth + 1, result);
    }
    if (node->right) { //右节点存在，递归深度加一
        getdepth(node->right, depth + 1, result);
    }
    return;
    
}

int minDepth(struct TreeNode* root) {
    int result = 114514;
    if (root == NULL) {
        return 0;
    }
    getdepth(root, 1, &result);
    return result;
}
```





#### 1480.一维数组的动态和（[1480. 一维数组的动态和 - 力扣（LeetCode）](https://leetcode.cn/problems/running-sum-of-1d-array/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240418213118.png)

##### code：

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* runningSum(int* nums, int numsSize, int* returnSize){
    int* preSum = malloc(sizeof(int) * numsSize);
    *returnSize = numsSize;
    for (int i = 0; i < numsSize; ++i) {
        if (i == 0) {
            preSum[i] = nums[i];
        } else {
            preSum[i] = preSum[i - 1] + nums[i]; 
        }
    }
    return preSum;
}
```





#### 643.子数组最大平均数|（[643. 子数组最大平均数 I - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-average-subarray-i/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240418213138.png)

##### code：

```c
double findMaxAverage(int* nums, int numsSize, int k) {
    double preSum[numsSize + 1];
    preSum[0] = 0;
    double max = INT_MIN; 
    for (int i = 1; i <= numsSize; ++i) { 
        preSum[i] = preSum[i - 1] + nums[i - 1];
        if(i - k >= 0) {
            double average = (preSum[i] - preSum[i - k]) / k;
            max = max > average ? max : average; 
        }
    }
    return max;
}
```





#### 304.二维区域和检索-矩阵不可变

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240418220706.png)

##### code：

```c
typedef struct {
    int** preSumMatrix;
    int rowNum;
} NumMatrix;

NumMatrix* numMatrixCreate(int** matrix, int matrixSize, int* matrixColSize) {
    NumMatrix* obj = (NumMatrix*)calloc(1, sizeof(NumMatrix));
    obj->preSumMatrix = (int**)calloc(matrixSize + 1, sizeof(int*));
    obj->rowNum = matrixSize + 1;
    for (int i = 0; i <= matrixSize; i++) {
        obj->preSumMatrix[i] = (int*)calloc(matrixColSize[0] + 1, sizeof(int));
    }
    //计算前缀和数组S[i][j]
    for (int i = 0; i < matrixSize; i++) {
        for (int j = 0; j < matrixColSize[i]; j++) {
            obj->preSumMatrix[i + 1][j + 1] =
                obj->preSumMatrix[i][j + 1] + obj->preSumMatrix[i + 1][j] - obj->preSumMatrix[i][j] + matrix[i][j];
        }
    }
    return obj;
}
// res = S(x2, y2) - S(x1 - 1, y2) - S(x2, y1 - 1) +S(x1 - 1, y1 - 1)
int numMatrixSumRegion(NumMatrix* obj, int row1, int col1, int row2, int col2) {
    return obj->preSumMatrix[row2 + 1][col2 + 1] -obj->preSumMatrix[row2 + 1][col1] - obj->preSumMatrix[row1][col2 + 1] + 
            obj->preSumMatrix[row1][col1];
}

void numMatrixFree(NumMatrix* obj) {
    for (int i = 0; i < obj->rowNum; i++) {
        free(obj->preSumMatrix[i]);
    }
    free(obj->preSumMatrix);
}
/**
 * Your NumMatrix struct will be instantiated and called as such:
 * NumMatrix* obj = numMatrixCreate(matrix, matrixSize, matrixColSize);
 * int param_1 = numMatrixSumRegion(obj, row1, col1, row2, col2);

 * numMatrixFree(obj);
*/
```


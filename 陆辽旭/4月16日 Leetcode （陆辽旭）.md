## 4月16日 Leetcode （陆辽旭）

#### 62.不同路径（[62. 不同路径 - 力扣（LeetCode）](https://leetcode.cn/problems/unique-paths/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240416210540.png)

##### code：

```c
int uniquePaths(int m, int n) {
    int board[m][n];
    board[0][0] = 1;//初始化
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if ((j - 1) >= 0 && (i - 1) >= 0) {
                board[i][j] = board[i][j-1] + board[i-1][j];//因为机器人只能走右和下，因此当前路径就是左和上的和
            }
            else if (j - 1 >= 0) {
                board[i][j] = board[i][j-1];//没有上面
            }
            else if (i - 1 >= 0) {
                board[i][j] = board[i-1][j];//没有左边
            }
        }
    }
    return board[m-1][n-1];
}
```





#### 63.不同路径||（[63. 不同路径 II - 力扣（LeetCode）](https://leetcode.cn/problems/unique-paths-ii/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240416212233.png)

##### code：

```c
int uniquePathsWithObstacles(int** obstacleGrid, int obstacleGridSize, int* obstacleGridColSize) {
    int m = obstacleGridSize;
    int n = obstacleGridColSize[0];
    int board[m][n];
    board[0][0] = 1;//初始化
    if (obstacleGrid[0][0] == 1) {
        return 0;//起点存在障碍
    }
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (obstacleGrid[i][j] == 1) {
                board[i][j] = 0;//障碍物不可到达置0
            }
            else if (obstacleGrid[i][j] == 0 && (j - 1) >= 0 && (i - 1) >= 0 && obstacleGrid[i][j-1] == 0 && obstacleGrid[i-1][j] == 0){
                board[i][j] = board[i][j-1] + board[i-1][j];//因为机器人只能走右和下，因此当前路径就是左和上的和，前提是左和上没有障碍
            }
            else if (j - 1 >= 0 && obstacleGrid[i][j-1] == 0) {
                board[i][j] = board[i][j-1];//上面有障碍或者边界，左边没有障碍
            }
            else if (i - 1 >= 0 && obstacleGrid[i-1][j] == 0) {
                board[i][j] = board[i-1][j];//左边有障碍或者边界，上面没有障碍
            }
            else if ((j - 1 >= 0 && obstacleGrid[i][j-1] == 1) || (i - 1 >= 0 && obstacleGrid[i-1][j] == 1)){
                board[i][j] = 0;//存在左边或者上面，但是被挡住了
            }
        }
    }
    return board[m-1][n-1];
}
```





#### 226.翻转二叉树（[226. 翻转二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/invert-binary-tree/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240416215313.png)

##### code：

```c
void swap(struct TreeNode** left, struct TreeNode** right) {//接受指向左右子节点指针的指针，以便能够直接修改节点
    struct TreeNode* temp = *left;
    *left = *right;
    *right = temp;
}

struct TreeNode* invertTree(struct TreeNode* root) {
    if (root == NULL) {
        return NULL;
    }
    struct TreeNode* queue[100];//辅助数组
    int front = 0, rear = 0;
    queue[rear++] = root;
    while (front != rear) {
        int len = rear - front;
        for (int i = 0; i < len; i++) {
            swap(&(queue[front]->left), &(queue[front]->right)); //交换左右子节点
            if (queue[front]->left) {
                queue[rear++] = queue[front]->left;
            }
            if (queue[front]->right) {
                queue[rear++] = queue[front]->right;
            }
            front++;
        }
    }
    return root;
}
```


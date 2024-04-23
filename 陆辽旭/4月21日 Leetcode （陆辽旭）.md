#### 4月21日 Leetcode （陆辽旭）

#### 257.二叉树的所有路径（[257. 二叉树的所有路径 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-paths/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240421192557.png)

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
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
void construct_paths(struct TreeNode* root, char** paths, int* returnSize, int* stack, int top) {
    if (root != NULL) {
        if (root->left == NULL && root->right == NULL) {  // 当前节点是叶子节点
            char* tmp = (char*)malloc(1001);
            int len = 0;
            for (int i = 0; i < top; i++) {
                len += sprintf(tmp + len, "%d->", stack[i]);
            }
            sprintf(tmp + len, "%d", root->val);
            paths[(*returnSize)++] = tmp;  // 把路径加入到答案中
        } else {
            stack[top++] = root->val;  // 当前节点不是叶子节点，继续递归遍历
            construct_paths(root->left, paths, returnSize, stack, top);
            construct_paths(root->right, paths, returnSize, stack, top);
        }
    }
}

char** binaryTreePaths(struct TreeNode* root, int* returnSize) {
    char** paths = (char**)malloc(sizeof(char*) * 1001);
    *returnSize = 0;
    int stack[1001];
    construct_paths(root, paths, returnSize, stack, 0);
    return paths;
}
```





#### 513.找树左下角的值（[513. 找树左下角的值 - 力扣（LeetCode）](https://leetcode.cn/problems/find-bottom-left-tree-value/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240421195035.png)

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
int findBottomLeftValue(struct TreeNode* root) {
    struct TreeNode* queue[10001];
    int front = 0, rear = 0, result = 0;
    if (root == NULL) {
        return NULL;
    }
    queue[rear++] = root;
    while(front != rear) {
        int len = rear - front;
        for(int i = 0; i < len; i++) {
            struct TreeNode* temp = queue[front++];
            if (i == 0) result = temp->val;//更新result为队头，即左节点
            if(temp->left) {
                queue[rear++] = temp->left;
            }
            if(temp->right) {
                queue[rear++] = temp->right;
            }
        }
    }
    return result;
}
```





#### 2007.从双倍数组中还原原数组（[2007. 从双倍数组中还原原数组 - 力扣（LeetCode）](https://leetcode.cn/problems/find-original-array-from-doubled-array/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240421191411.png)

##### code：

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
typedef struct
{
	int key;
	int value;
} HashMap;

static int cmp(const void* a, const void* b)
{
	return *(int*)a - *(int*)b;
}

int* findOriginalArray(int* changed, int changedSize, int* returnSize) {
	*returnSize = 0; // 初始化返回大小为 0
	if (changedSize % 2 != 0) { // 如果输入数组长度为奇数，则直接返回 NULL
		return NULL;
	}
	qsort(changed, changedSize, sizeof(int), cmp); // 对输入数组进行排序
	int n = changedSize / 2; // 计算原始数组的长度
	HashMap* hash = (HashMap*)malloc(sizeof(HashMap) * (n + 1)); // 创建哈希表
	int j = 1;
	int k = 0;
	hash[0].key = changed[0]; // 初始化哈希表的第一个元素为输入数组的第一个元素
	for (int i = 1; i < changedSize; i++) {
		if (hash[k].key == changed[i] / 2.0 && k < j) { // 如果当前元素是前一个元素的两倍且未超出原始数组长度
			hash[k].value = changed[i]; 
			k++;
		} else if (hash[k].key != changed[i] / 2.0 && j < n) { // 如果当前元素不是前一个元素的两倍且未超出原始数组长度
			hash[j].key = changed[i];
			j++;
		}
	}
	if (j == k) { // 如果哈希表的长度和原始数组长度相等
		*returnSize = n;
		int* ans = (int*)malloc(sizeof(int) * n); 
		for (int i = 0; i < n; i++) {
			ans[i] = hash[i].key; 
		}
		return ans;
	} else {
		return NULL; // 如果哈希表的长度和原始数组长度不相等，则返回 NULL
	}
}
```


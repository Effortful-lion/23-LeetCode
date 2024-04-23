## 4月17日 Leetcode （陆辽旭）

#### 148.排序链表（[148. 排序链表 - 力扣（LeetCode）](https://leetcode.cn/problems/sort-list/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240417182340.png)

##### code：

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
 //  找到链表中间节点（876. 链表的中间结点）
struct ListNode* findMiddle(struct ListNode* head) {
    if (head == NULL || head->next == NULL) {
        return head;
    }
    struct ListNode* slow = head;
    struct ListNode* fast = head->next->next;
    while (fast != NULL && fast->next != NULL) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}
// 合并两个有序链表（21. 合并两个有序链表）
struct ListNode* mergeList(struct ListNode* left, struct ListNode* right) {
    struct ListNode* dummy = malloc(sizeof(struct ListNode));
    struct ListNode* temp = dummy;
    while (left != NULL && right != NULL) {
        if (left->val <= right->val) {
            temp->next = left;
            left = left->next;
        }
        else if (left->val > right->val) {
            temp->next = right;
            right = right->next;
        }
        temp = temp->next;
    }
    temp->next = left != NULL ? left : right;
    return dummy->next;
}

struct ListNode* sortList(struct ListNode* head) {
    //递归结束条件
    if (head == NULL || head->next == NULL) {
        return head;
    }
    //找到链表中间节点并断开链表 & 递归下探
    struct ListNode* middle = findMiddle(head);
    struct ListNode* rightHead = middle->next;
    middle->next = NULL;
    struct ListNode* left = sortList(head);
    struct ListNode* right = sortList(rightHead);
    //合并有序链表
    return mergeList(left, right);
}
```





#### 147.对链表进行插入排序（[147. 对链表进行插入排序 - 力扣（LeetCode）](https://leetcode.cn/problems/insertion-sort-list/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240417184217.png)

##### code：

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode *insertionSortList(struct ListNode *head) {
    if (head == NULL) { // 如果链表为空，直接返回
        return head;
    }
    // 创建哑节点
    struct ListNode *dummyHead = malloc(sizeof(struct ListNode));
    dummyHead->val = 0;
    dummyHead->next = head;
    // lastSorted 指向已经排序好的链表的最后一个节点
    struct ListNode *lastSorted = head;
    // curr 指向待排序的节点
    struct ListNode *curr = head->next;
    while (curr != NULL) {
        if (lastSorted->val <= curr->val) { // 如果待排序节点大于等于已排序链表的最后一个节点，直接将lastSorted向后移动一位
            lastSorted = lastSorted->next;
        } else { // 否则，需要找到插入的位置
            struct ListNode *prev = dummyHead;
            // 寻找插入位置，找到第一个大于当前节点值的节点的前一个节点
            while (prev->next->val <= curr->val) {
                prev = prev->next;
            }
            // 将curr插入到prev之后
            lastSorted->next = curr->next;
            curr->next = prev->next;
            prev->next = curr;
        }
        // 移动curr指针到下一个待排序节点
        curr = lastSorted->next;
    }
    // 返回排序后的链表
    return dummyHead->next;
}
```





#### 589.N叉树的前序遍历（[589. N 叉树的前序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/n-ary-tree-preorder-traversal/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240417192604.png)

##### code：

```c
void travesal(const struct Node* root, int* res, int* count) {
    if (root == NULL) {
        return;
    }
    res[(*count)++] = root->val;//先加入节点
    for (int i = 0; i < root->numChildren; i++) {//再遍历子树
        travesal(root->children[i], res, count);
    }
}

int* preorder(struct Node* root, int* returnSize) {
    int * res = (int *)malloc(sizeof(int) * 10000);
    int count = 0;
    travesal(root, res, &count);
    *returnSize = count;
    return res;
}
```





#### 590.N叉树的后序遍历（[590. N 叉树的后序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/n-ary-tree-postorder-traversal/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240417193533.png)

##### code：

```c
void travesal(const struct Node* root, int* res, int* count) {
    if (root == NULL) {
        return;
    }
    for (int i = 0; i < root->numChildren; i++) {//先遍历子树
        travesal(root->children[i], res, count);
    }
    res[(*count)++] = root->val;//再加入节点
}

int* postorder(struct Node* root, int* returnSize) {
    int * res = (int *)malloc(sizeof(int) * 10000);
    int count = 0;
    travesal(root, res, &count);
    *returnSize = count;
    return res;
}
```





#### 101.对称二叉树（[101. 对称二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/symmetric-tree/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240417210355.png)

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
bool compare(struct TreeNode* left, struct TreeNode* right) {
    // 首先排除空节点的情况
    if (left == NULL && right != NULL) return false;
    else if (left != NULL && right == NULL) return false;
    else if (left == NULL && right == NULL) return true;
    // 排除了空节点，再排除数值不相同的情况
    else if (left->val != right->val) return false;
    // 此时就是：左右节点都不为空，且数值相同的情况
    // 此时才做递归，做下一层的判断
    bool outside = compare(left->left, right->right);   // 左子树：左、 右子树：右
    bool inside = compare(left->right, right->left);    // 左子树：右、 右子树：左
    bool isSame = outside && inside;                    // 左子树：中、 右子树：中 （逻辑处理）
    return isSame;
}

bool isSymmetric(struct TreeNode* root) {
    if (root == NULL) return true;
    return compare(root->left, root->right);
}
```


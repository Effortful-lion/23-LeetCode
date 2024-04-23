## 4月9日 Leetcode  (陆辽旭)

#### 2529.正整数和负整数的最大值（[2529. 正整数和负整数的最大计数 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-count-of-positive-integer-and-negative-integer/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240409192358.png)

##### code:

```c
int binary_search(int *nums, int numsSize, int target) {//二分查找
    int left = 0;
    int right = numsSize - 1;   // 闭区间[left, right]
    int mid;
    while(left <= right) {
        mid = left + (right - left) / 2;
        if(nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return left;       
}

int maximumCount(int* nums, int numsSize){
    int i;
    int neg = 0, pos = 0;
    int tmp = binary_search(nums, numsSize, 0);//小于0的数
    neg = tmp;
    tmp = binary_search(nums, numsSize, 1);//小于1的数
    if(tmp == numsSize) {//全负数情况
        pos = 0;
    } else {
        pos = numsSize - tmp;
    }
    return (neg > pos) ? neg : pos;
}
```



#### 24.两两交换链表中的节点（[24. 两两交换链表中的节点 - 力扣（LeetCode）](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240409211305.png)

##### code:

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* swapPairs(struct ListNode* head) {
    if (head == NULL || head->next == NULL) {
        return head;
    }
    struct ListNode* curr = head;
    while (curr != NULL && curr->next != NULL) {
        struct ListNode* nextNode = curr->next;
        // 交换节点值
        int temp = curr->val;
        curr->val = nextNode->val;
        nextNode->val = temp;
        curr = curr->next->next;
    }
    return head;
}
```


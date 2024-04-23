## 4月12日 Leetcode （陆辽旭）

#### 347.前k个高频元素（[347. 前 K 个高频元素 - 力扣（LeetCode）](https://leetcode.cn/problems/top-k-frequent-elements/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240412151254.png)

##### code：

```c
struct hash_table {
    int key;
    int val;
    UT_hash_handle hh; // 哈希表节点，用于链接到哈希表中的下一个节点
};

typedef struct hash_table* hash_ptr; // 定义指向哈希表节点的指针类型

struct pair {
    int first;
    int second;
};

struct pair* heap; // 定义一个结构体pair的数组heap，用于实现最小堆
int heapSize; // 用于记录当前堆的大小

void swap(struct pair* a, struct pair* b) { // 定义一个交换两个pair结构体的函数
    struct pair t = *a;
    *a = *b, *b = t;
}

bool cmp(struct pair* a, struct pair* b) { // 定义一个比较两个pair结构体second成员的大小的函数
    return a->second < b->second;
}

struct pair top() { // 定义一个返回堆顶元素的函数
    return heap[1];
}

void push(hash_ptr x) { // 定义一个向堆中插入元素的函数
    heap[++heapSize].first = x->key;
    heap[heapSize].second = x->val;
    int p = heapSize, s;
    while (p > 1) {
        s = p >> 1;
        if (cmp(&heap[s], &heap[p])) return; // 如果父节点的值小于当前节点的值，则不需要调整，直接返回
        swap(&heap[p], &heap[s]); // 否则交换父节点和当前节点的值，并继续向上调整
        p = s;
    }
}

void pop() { // 定义一个从堆中删除堆顶元素的函数
    heap[1] = heap[heapSize--]; // 将堆顶元素替换为最后一个元素，并更新堆的大小
    int p = 1, s;
    while ((p << 1) <= heapSize) { // 当左孩子存在时
        s = p << 1;
        if (s < heapSize && cmp(&heap[s + 1], &heap[s])) s++; // 如果右孩子存在且比左孩子小，则指向右孩子
        if (cmp(&heap[p], &heap[s])) return; // 如果父节点的值小于孩子节点的值，则不需要调整，直接返回
        swap(&heap[p], &heap[s]); // 否则交换父节点和孩子节点的值，并继续向下调整
        p = s;
    }
}

int* topKFrequent(int* nums, int numsSize, int k, int* returnSize) { // 定义一个求解前k个高频元素的函数
    hash_ptr head = NULL; // 定义一个哈希表头节点
    hash_ptr p = NULL, tmp = NULL;

    // 遍历数组，统计每个元素出现的频率，并将其存储在哈希表中
    for (int i = 0; i < numsSize; i++) {
        HASH_FIND_INT(head, &nums[i], p); // 在哈希表中查找当前元素
        if (p == NULL) { // 如果当前元素不存在，则创建新节点
            p = malloc(sizeof(struct hash_table));
            p->key = nums[i];
            p->val = 1;
            HASH_ADD_INT(head, key, p); // 将新节点添加到哈希表中
        } else {
            p->val++; // 如果当前元素已存在，则更新其频率
        }
    }

    heap = malloc(sizeof(struct pair) * (k + 1)); // 分配存储堆的空间
    heapSize = 0;

    // 遍历哈希表，将前k个高频元素存储在最小堆中
    HASH_ITER(hh, head, p, tmp) {
        if (heapSize == k) { // 如果堆已满
            struct pair tmp = top();
            if (tmp.second < p->val) { // 如果当前元素的频率大于堆顶元素的频率
                pop(); // 弹出堆顶元素
                push(p); // 将当前元素插入堆中
            }
        } else {
            push(p); // 如果堆未满，则直接将当前元素插入堆中
        }
    }
    *returnSize = k; // 更新返回数组的大小
    int* ret = malloc(sizeof(int) * k); // 分配存储返回数组的空间
    for (int i = 0; i < k; i++) { // 依次将堆中的元素取出存入返回数组中
        struct pair tmp = top();
        pop();
        ret[i] = tmp.first;
    }
    return ret; // 返回结果数组
}
```


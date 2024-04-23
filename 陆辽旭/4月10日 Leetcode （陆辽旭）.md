## 4月10日 Leetcode （陆辽旭）

#### 739.每日温度（[739. 每日温度 - 力扣（LeetCode）](https://leetcode.cn/problems/daily-temperatures/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240410212811.png)

##### code：

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* dailyTemperatures(int* temperatures, int temperaturesSize, int* returnSize) {//找右边大左边小用单调栈
    int* answer = (int *)malloc(temperaturesSize * sizeof(int));
    // 初始化单调栈，这里使用数组模拟栈
    int stack[temperaturesSize];
    int top = -1; // 栈顶初始化为-1，表示栈为空
    for (int i = 0; i < temperaturesSize; i++) {
        //入栈元素大于栈头，栈头出栈，天数就等于下标之差
        while (top != -1 && temperatures[i] > temperatures[stack[top]]) {
            int index = stack[top--];// 取出栈顶元素
            answer[index] = i - index;
        }
        stack[++top] = i;
    }
    // 遍历结束后，栈中剩余的元素都是无法找到更高温度的天数，直接赋值为0
    while (top != -1) {
        int index = stack[top--];
        answer[index] = 0; // 无法找到更高温度，天数为0
    }
    *returnSize = temperaturesSize; // 返回结果数组的长度
    return answer;
}
```



#### 239.滑动窗口最大值（[239. 滑动窗口最大值 - 力扣（LeetCode）](https://leetcode.cn/problems/sliding-window-maximum/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240410212845.png)

##### code：

```c
int* maxSlidingWindow(int* nums, int numsSize, int k, int* returnSize) {
    *returnSize = numsSize - k + 1;
    int *res = (int *)malloc((*returnSize) * sizeof(int)); // 直接分配所需空间
    int front = 0, rear = -1; // 单调队列的头尾
    int maxIdx = 0; // 最大值在队列中的索引
    //用来存下标的队列，其中下标在数组中值，队列中呈现单调递减，如果有比队列最大值还大的元素要进队，那么证明最大值要更新
    int *deque = (int *)malloc(numsSize * sizeof(int));
    // 处理前 k 个元素，构建初始单调队列
    for (int i = 0; i < k; i++) {
        // 保持队列单调递减，如果当前元素大于等于队尾元素，则队尾元素出队，直到队列为空或当前元素小于队尾元素
        while (front <= rear && nums[i] >= nums[deque[rear]]) {
            --rear;
        }
        deque[++rear] = i;//队尾下标
        //如果超出窗口范围，则队头元素出队
        if (i - deque[front] >= k) {
            ++front;
        }
        //每次窗口滑动后，队头元素即为当前窗口的最大值的索引
        if (nums[i] >= nums[maxIdx]) {
            maxIdx = i;
        }
    }
    res[0] = nums[maxIdx];
    // 处理后续元素
    for (int i = k; i < numsSize; ++i) {
        // 保持队列单调递减
        while (front <= rear && nums[i] >= nums[deque[rear]]) {
            --rear;
        }
        deque[++rear] = i;
        // 更新当前窗口的最大值索引
        if (i - deque[front] >= k) {
            ++front;
        }
        // 更新最大值索引
        maxIdx = deque[front];
        res[i - k + 1] = nums[maxIdx];
    }
    free(deque);
    return res;
}
```


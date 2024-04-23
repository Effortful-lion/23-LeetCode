## 4月11日 Leetcode （陆辽旭）

#### 503.下一个更大元素||（[503. 下一个更大元素 II - 力扣（LeetCode）](https://leetcode.cn/problems/next-greater-element-ii/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240411192322.png)

##### code:

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
#include <stdlib.h>

int* nextGreaterElements(int* nums, int numsSize, int* returnSize) {
    int* answer = (int *)malloc(numsSize * sizeof(int));
    *returnSize = numsSize;
    // 初始化单调栈，这里使用数组模拟栈
    int stack[numsSize];
    int top = -1; // 栈顶初始化为-1，表示栈为空
    int count = 0;
    for(int i = 0; i < numsSize; i++) {
        while(top > -1 && nums[stack[top]] < nums[i]) {
            // 栈不为空且当前元素大于栈顶元素时，出栈并更新答案数组
            answer[stack[top]] = nums[i];
            --top;
        }
        // 当栈为空或当前元素小于等于栈顶元素时，将当前元素的索引入栈
        stack[++top] = i;
    }
    // 处理循环数组的情况
    for(int i = 0; i < numsSize && top > -1; i++) {
        while(top > -1 && nums[stack[top]] < nums[i]) {
            // 栈不为空且当前元素大于栈顶元素时，出栈并更新答案数组
            answer[stack[top]] = nums[i];
            --top;
        }
    }
    // 剩余元素没有更大的值，置为-1
    while(top > -1) {
        answer[stack[top--]] = -1;
    }
    return answer;
}
```



#### 746.使用最小花费爬楼梯（[746. 使用最小花费爬楼梯 - 力扣（LeetCode）](https://leetcode.cn/problems/min-cost-climbing-stairs/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240411213155.png)

##### code：

```c
int minCostClimbingStairs(int* cost, int costSize) {
    if(costSize == 2) {
        return cost[0] < cost[1] ? cost[0] : cost[1];
    }
    int dp[costSize+1];//动态规划数组
    dp[0] = dp[1] = 0;//前两层免费
    for(int i = 2; i < costSize; i++) {
        dp[i] = (dp[i-1] + cost[i-1]) < (dp[i-2] + cost[i-2]) ? (dp[i-1] + cost[i-1]) : (dp[i-2] + cost[i-2]);//公式
    }
    //判断到顶的最小花费
    dp[costSize] = (dp[costSize-1] + cost[costSize-1]) < (dp[costSize-2] + cost[costSize-2]) ? (dp[costSize-1] + cost[costSize-1]) : (dp[costSize-2] + cost[costSize-2]);
    return dp[costSize];
}
```



#### 84.柱状图中的最大矩形（[84. 柱状图中最大的矩形 - 力扣（LeetCode）](https://leetcode.cn/problems/largest-rectangle-in-histogram/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240411205218.png)

##### code：

```c
int largestRectangleArea(int* heights, int heightsSize) {//本题目矩形向右形成，在当前下标
    // 创建一个栈来保存柱形的索引
    int stack[heightsSize + 1];
    int top = -1;
    // 初始化栈，将哨兵元素入栈，确保所有柱形都能被处理到
    stack[++top] = -1;
    // 用于存储最大面积
    int maxArea = 0;
    // 遍历每个柱形
    for (int i = 0; i < heightsSize; i++) {
        // 如果当前柱形高度小于等于栈顶柱形高度，说明找到了右边界
        while (top > 0 && heights[i] <= heights[stack[top]]) {
            // 计算以栈顶柱形为高度的矩形面积
            int height = heights[stack[top--]]; // 取出栈顶柱形高度
            int width = i - stack[top] - 1; // 宽度为当前索引减去栈顶元素索引再减去1
            int area = height * width; // 计算矩形面积
            maxArea = area > maxArea ? area : maxArea; // 更新最大面积
        }
        // 当前柱形入栈
        stack[++top] = i;
    }
    // 处理栈中剩余的柱形
    while (top > 0) {
        int height = heights[stack[top--]]; // 取出栈顶柱形高度
        int width = heightsSize - stack[top] - 1; // 宽度为数组大小减去栈顶元素索引再减去1,最后一个元素width相当于heightsSize
        int area = height * width; // 计算矩形面积
        maxArea = area > maxArea ? area : maxArea; // 更新最大面积
    }
    return maxArea;
}
```


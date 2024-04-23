## 4月8日 Leetcode （陆辽旭）

#### 1047.删除字符串中的所有相邻重复项（[1047. 删除字符串中的所有相邻重复项 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240408185323.png)

##### code:

```c
char* removeDuplicates(char* s) {
    int stackSize = 5000; // 栈的原始尺寸
    char* stack = (char*)malloc(stackSize * sizeof(char)); // 动态分配
    int top = -1;
    int len = strlen(s);
    for (int i = 0; i < len; i++) {
        if (top >= stackSize - 1) { // 如果此时栈满了 就扩容
            stack = (char*)realloc(stack, (stackSize += 500) * sizeof(char));
        }
        char c = s[i];
        stack[++top] = c;
        // 如果当前字符与栈顶字符相同，并且栈顶不是刚刚入栈的字符
        if (top > 0 && stack[top - 1] == c) {
            top -= 2; // 移除重复字符及其前一个字符
        }
    }
    // 根据实际字符数量分配结果字符串的大小
    char* res = (char*)malloc((top + 2) * sizeof(char));
    for (int i = 0; i <= top; i++) {
        res[i] = stack[i];
    }
    // 释放栈内存
    free(stack);
    // 结果字符串的末尾添加'\0'
    res[top + 1] = '\0';
    return res;
}
```



#### 20.有效的括号（[20. 有效的括号 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-parentheses/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240408190400.png)

##### code：

```c
//为右括号返回为对应的左括号
char pairs(char a) {
    switch (a) {
        case '}': return '{';
        case ']': return '[';
        case ')': return '(';
        default: return 0; 
    }
}

bool isValid(char* s) {
    int n = strlen(s);
    if (n % 2 == 1) { // 如果长度是奇数，则无效
        return false;
    }
    int stk[n + 1]; // 定义栈
    int top = -1; // 初始化栈顶指针
    for (int i = 0; i < n; i++) { // 尝试将当前字符转换为对应的左括号
        char s1 = pairs(s[i]);
        if (s1) { // 如果转换成功，说明当前字符是右括号
            if (top == -1 || stk[top] != s1) { // 如果栈为空或者栈顶元素与当前右括号不匹配
                return false;
            }
            top--; // 弹出栈顶元素
        } else { // 如果当前字符是左括号就入栈
            stk[++top] = s[i]; 
        }
    }
    return top == -1; // 如果栈为空，则所有括号都匹配，字符串有效
}
```



#### 150.逆波兰表达式求值（[150. 逆波兰表达式求值 - 力扣（LeetCode）](https://leetcode.cn/problems/evaluate-reverse-polish-notation/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240408203428.png)

##### code：

```c
int evalRPN(char** tokens, int tokensSize) {
    int stack[tokensSize], top = -1;
    for(int i = 0; i < tokensSize; i++) {
        //如果是运算符
        if(strcmp(tokens[i], "+") == 0 || strcmp(tokens[i], "-" ) == 0 || strcmp(tokens[i], "*") == 0 || strcmp(tokens[i], "/") == 0) {
            int a=stack[top--];
            int b=stack[top--];
            if(strcmp(tokens[i], "+") == 0) stack[++top] = b+a;
            if(strcmp(tokens[i], "-") == 0) stack[++top] = b-a;
            if(strcmp(tokens[i], "*") ==0) stack[++top] = b*a;
            if(strcmp(tokens[i], "/") ==0) stack[++top] = b/a;
        }
        else {
            stack[++top] = atoi(tokens[i]);//转为long进行运算
        }
    }
    return stack[top];
}
```


## 4月14日 Leetcode （陆辽旭）

#### 459.重复的子字符串（[459. 重复的子字符串 - 力扣（LeetCode）](https://leetcode.cn/problems/repeated-substring-pattern/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240414195058.png)

##### code：

```c
// 使用KMP算法实现字符串匹配
bool kmp(char* query, char* pattern) {
    int n = strlen(query);  // 查询字符串长度
    int m = strlen(pattern);  // 模式字符串长度
    int fail[m];  // 失败函数数组
    memset(fail, -1, sizeof(fail));  // 初始化失败函数数组为-1
    // 构建失败函数数组
    for (int i = 1; i < m; ++i) {
        int j = fail[i - 1];  // j表示前一个字符的失败函数值
        // 如果失败，向前回溯
        while (j != -1 && pattern[j + 1] != pattern[i]) {
            j = fail[j];
        }
        // 更新失败函数值
        if (pattern[j + 1] == pattern[i]) {
            fail[i] = j + 1;
        }
    }
    int match = -1;  // 当前匹配的位置
    // 在查询字符串中匹配模式字符串
    for (int i = 1; i < n - 1; ++i) {
        // 如果匹配失败，向前回溯
        while (match != -1 && pattern[match + 1] != query[i]) {
            match = fail[match];
        }
        // 更新匹配位置
        if (pattern[match + 1] == query[i]) {
            ++match;
            // 如果匹配到了模式字符串的末尾，则返回true
            if (match == m - 1) {
                return true;
            }
        }
    }
    return false;  // 匹配失败，返回false
}

// 判断字符串是否由一个子字符串重复多次构成
bool repeatedSubstringPattern(char* s) {
    int n = strlen(s);  // 获取字符串长度
    char k[2 * n + 1];  // 创建一个长度为2*n+1的字符串数组
    k[0] = 0;  // 初始化字符串数组的第一个元素为0
    strcat(k, s);  // 将原字符串拼接到字符串数组中
    strcat(k, s);  // 再次将原字符串拼接到字符串数组中，相当于把原字符串复制了一遍
    return kmp(k, s);  // 调用KMP算法判断是否存在重复子字符串
}
```



#### 28.找出字符串中第一个匹配项的下标（[28. 找出字符串中第一个匹配项的下标 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)）

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240414205407.png)

##### code：

```c
int strStr(char* haystack, char* needle) {
    int n = strlen(haystack), m = strlen(needle); // 获取 haystack 和 needle 的长度
    if (m == 0) { // 如果 needle 为空，则直接返回 0
        return 0;
    }
    int fail[m]; // 定义一个长度为 m 的数组用于存储失配函数的值
    fail[0] = 0; // 初始化失配函数数组的第一个元素为 0
    // 构建失配函数数组
    for (int i = 1, j = 0; i < m; i++) {
        while (j > 0 && needle[i] != needle[j]) { // 如果字符不匹配，则回溯 j 到上一个可能的匹配位置
            j = fail[j - 1];
        }
        if (needle[i] == needle[j]) { // 如果字符匹配，则增加 j
            j++;
        }
        fail[i] = j; // 将计算得到的失配函数值存入数组中
    }
    // 在 haystack 中查找 needle
    for (int i = 0, j = 0; i < n; i++) {
        while (j > 0 && haystack[i] != needle[j]) { // 如果字符不匹配，则根据失配函数值回溯 j
            j = fail[j - 1];
        }
        if (haystack[i] == needle[j]) { // 如果字符匹配，则增加 j
            j++;
        }
        if (j == m) { // 如果 j 等于 needle 的长度 m，则说明找到了匹配的子串
            return i - m + 1; // 返回子串的起始位置
        }
    }
    return -1; // 没有找到匹配的子串，返回 -1
}

```



#### 686.重复叠加字符串匹配（[686. 重复叠加字符串匹配 - 力扣（LeetCode）](https://leetcode.cn/problems/repeated-string-match/description/))

![](https://gitee.com/knoci/picture/raw/master/QQ截图20240414213857.png)

##### code:

```c
int strStr(char* haystack, int n, char* needle, int m) {
    if (m == 0) { // 如果 needle 为空，则直接返回 0
        return 0;
    }
    int fail[m]; // 定义一个长度为 m 的数组用于存储失配函数的值
    fail[0] = 0; // 初始化失配函数数组的第一个元素为 0
    // 构建失配函数数组
    for (int i = 1, j = 0; i < m; i++) {
        while (j > 0 && needle[i] != needle[j]) { // 如果字符不匹配，则回溯 j 到上一个可能的匹配位置
            j = fail[j - 1];
        }
        if (needle[i] == needle[j]) { // 如果字符匹配，则增加 j
            j++;
        }
        fail[i] = j; // 将计算得到的失配函数值存入数组中
    }
    // b 开始匹配的位置是否超过第一个叠加的 a
    for (int i = 0, j = 0; i - j < n; i++) {
        while (j > 0 && haystack[i % n] != needle[j]) { // haystack 是循环叠加的字符串，所以取 i % n
            j = fail[j - 1];
        }
        if (haystack[i % n] == needle[j]) { // 如果字符匹配，则增加 j
            j++;
        }
        if (j == m) { // 如果 j 等于 needle 的长度 m，则说明找到了匹配的子串
            return i - m + 1; // 返回子串的起始位置
        }
    }
    return -1; // 没有找到匹配的子串，返回 -1
}

int repeatedStringMatch(char * a, char * b){
    int an = strlen(a), bn = strlen(b);
    int index = strStr(a, an, b, bn);// 在字符串 a 中查找字符串 b 的匹配位置
    if (index == -1) { // 如果没有找到匹配的子串，则返回 -1
        return -1;
    }
    if (an - index >= bn) { // 如果匹配位置之后的长度足够容纳字符串 b，则返回 1
        return 1;
    }
    // 计算需要重复多少次字符串 a 才能容纳整个字符串 b
    return (bn + index - an - 1) / an + 2;
}
```


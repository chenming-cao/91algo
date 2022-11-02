### [821. 字符的最短距离](https://leetcode-cn.com/problems/shortest-distance-to-a-character/)

### 解题思路
字符串遍历问题\
遍历两次字符串：\
第一次正向遍历从左至右，计算每个字符和左边出现的特殊字符的最近距离；\
第二次反向遍历从右至左，计算每个字符和右边出现的特殊字符的最近距离，并把最短距离写入整数数组中。

### 代码

```java
class Solution {
    public int[] shortestToChar(String s, char c) {
        int n = s.length();
        int[] res = new int[n];
        Arrays.fill(res, n); // fill initial values, distance cannot be longer than n
        // traverse from the left, calculate the distance between current letter and the nearest c on its left
        int left = -1;
        for (int i = 0; i < n; i++) {
            char cur = s.charAt(i);
            if (cur == c) {
                res[i] = 0;
                left = i;
            } else {
                if (left == -1) {
                    continue;
                } else {
                    res[i] = i - left;
                }
            }
        }
        // traverse from the right, calculate the distance between current letter and the nearest c on its right
        int right = n;
        for (int j = n - 1; j >= 0; j--) {
            char cur = s.charAt(j);
            if (cur == c) {
                res[j] = 0;
                right = j;
            } else {
                if (right == n) {
                    continue;
                } else {
                    res[j] = Math.min(res[j], right - j);
                }
            }
        }
        return res;
    }
}
```

### 复杂度分析
令n为字符串长度
- 时间复杂度：O(n)，循环运行次数为2倍字符串长度
- 空间复杂度：O(1)，没有额外开空间
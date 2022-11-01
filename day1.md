### [989. 数组形式的整数加法](https://leetcode-cn.com/problems/add-to-array-form-of-integer/)

### 解题思路
模拟加分运算规则，从低位往高位依次运算。\
从数组中获取数位信息，直接`num[index]`\
从数字`k`中获得数位信息，每次进行`k % 10`\
运算时注意进位，每次将数位计算结果存入链表前端。

### 代码（java）

```java
class Solution {
    public List<Integer> addToArrayForm(int[] num, int k) {
        List<Integer> res = new LinkedList<>();
        int carry = 0;
        int n = num.length;
        int index = n - 1; // record the index of array num that currently used in calculation
        int dk = 0; // record the digit from k
        int dn = 0; // record the digit from num

        while (k > 0) {
            dk = k % 10; // get the next digit from k
            dn = 0;
            if (index >= 0) {
                dn = num[index--];
            }
            res.add(0, (carry + dk + dn) % 10); // insert the result digit to the front of the linked list
            carry = (carry + dk + dn) / 10; // calculate carry digit
            k /= 10; // move left to one digit
        }
        // when all the digits in k are used in calculation, while there are still digits in num that are not used
        // continue to record the digits in num to res
        while (index >= 0) {
            res.add(0, (carry + num[index]) % 10);
            carry = (carry + num[index]) / 10;
            index--;
        }
        // if there is a carry not equal to 0, need to add one more digit for the carry
        if (carry != 0) {
            res.add(0, carry);
        }
        return res;
    }
}
```

### 复杂度分析
令n为nums数组长度，字符串数组的长度为logk
- 时间复杂度：O(max(n, logk)), 两个数组长度的最大值（最终循环次数）
- 空间复杂度：O(max(n, logk)), 创建链表储存结果，链表长度为两个长度最大值
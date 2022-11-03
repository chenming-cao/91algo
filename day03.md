### [1381. 设计一个支持增量操作的栈](https://leetcode-cn.com/problems/design-a-stack-with-increment-operation/)

### 解题思路
用数组来实现栈，设置变量`size`记录数组当前长度，用来快速判断栈是否已满，并且快速查找栈顶元素。\
用模拟实现`Increment`，更新数组中符合条件（下标在`0`到`min(k, size) - 1`)的所有元素

### 代码（方法1）

```java
class CustomStack {
    int[] stack;  // record the elements in the stack
    int size;

    public CustomStack(int maxSize) {
        stack = new int[maxSize];
        size = 0;
    }

    public void push(int x) {
        if (size < stack.length) {
            stack[size++] = x;
        }
    }

    public int pop() {
        if (size == 0) {
            return -1;
        } else {
            return stack[--size];
        }
    }

    public void increment(int k, int val) {
        k = Math.min(k, size);
        // update the elements in the stack by incrementing val
        for (int i = 0; i < k; i++) {
            stack[i] += val;
        }
    }
}
```

### 复杂度分析
- 时间复杂度：push和pop操作O(1)，increment操作O(k)
- 空间复杂度：O(maxSize)

### 代码（方法2）
`Increment`操作时间复杂度高，可优化。没有必要在`Increment`操作中更新所有的元素。\
可以用另一个数组`inc`记录每次`Increment`操作时的增加值，增加值只记录到从栈底开始最后一个被增加的位置。因为出栈操作在栈顶，每次我们可以求出栈顶元素被增加的值，并且顺势更新出栈后下一个栈顶元素的增加值，这样时间复杂度就可以降到O(1)

```java
class CustomStack {
    int[] stack;  // record the elements in the stack
    int[] inc;  // record the increment value
    int size;

    public CustomStack(int maxSize) {
        stack = new int[maxSize];
        inc = new int[maxSize];
        size = 0;
    }

    public void push(int x) {
        if (size < stack.length) {
            stack[size++] = x;
        }
    }

    public int pop() {
        if (size == 0) {
            return -1;
        } else {
            size--;
            int res = stack[size] + inc[size];
            // calculate the increment value for the element on the top of the stack after pop
            if (size > 0) {
                inc[size - 1] += inc[size];
            }
            // reset the increment value, cannot put inside if condition
            inc[size] = 0;
            return res;
        }
    }

    public void increment(int k, int val) {
        k = Math.min(k, size);
        if (k > 0) {
            // record the increment value at the position k - 1, all the elements with index 0 to k - 1 were incremented
            inc[k - 1] += val;
        }
    }
}
```

### 复杂度分析
- 时间复杂度：push和pop操作O(1)，increment操作O(1)
- 空间复杂度：O(maxSize)
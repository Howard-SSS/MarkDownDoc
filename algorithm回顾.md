[toc]

# 数组

## [26. 删除有序数组中的重复项 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)<u>完成下标与检索下标</u>

```java
public int removeDuplicates(int[] nums) {
    int index = 1;
    for (int i = 1; i < nums.length; i++) {
        if (nums[i - 1] != nums[i]) {
            nums[index++] = nums[i];
        }
    }
    return index;
}
```

## [88. 合并两个有序数组 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/merge-sorted-array/)

```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    int index = nums1.length - 1;
    m--;n--;
    while (index >= 0 && n >= 0) {
        if (m >= 0 && nums1[m] > nums2[n]) {// *
            nums1[index] = nums1[m];
            m--;
        } else {
            nums1[index] = nums2[n];
            n--;
        }
        index--;
    }
}
```

## [169. 多数元素 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/majority-element/)<u>对半消耗</u>

```java
public int majorityElement(int[] nums) {
    int res = nums[0], num = 1;
    for (int i = 1; i < nums.length; i++) {
        if (num == 0) {
            res = nums[i];
            num = 1;
        } else if (res == nums[i]) 
            num++;
        else if (res != nums[i])
            num--;
    }
    return res;
}
```

## [268. 丢失的数字 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/missing-number/)<u>异或取值</u>

```java
public int missingNumber(int[] nums) {
    int ret=0;
    for (int i=1; i <= nums.length; i++) {
        ret^=i;
        ret^=nums[i-1];
    }
    return ret;
}
```

## [724. 寻找数组的中心下标 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/find-pivot-index/)

```java
public int pivotIndex(int[] nums) {
    int sum = 0;
    for (int temp : nums) 
        sum += temp;
    int temp = 0;
    for (int i = 0; i < nums.length; i++) {
        sum -= nums[i];
        if (sum == temp)
            return i;
        temp += nums[i];
    }
    return -1;
}
```

## [1018. 可被 5 整除的二进制前缀 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/binary-prefix-divisible-by-5/submissions/)<u>求余缩减</u>

```java
public List<Boolean> prefixesDivBy5(int[] nums) {
    List res = new ArrayList<Boolean>();
    int temp = 0;
    for (int num : nums) {
        temp = (temp * 2 + num) % 5;
        res.add(temp == 0);
    }
    return res;
}
```

## [46. 全排列 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/permutations/)

```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    dfs(res, nums, 0);
    return res;
}
public void dfs(List res,int[] nums, int index) {
    if (index == nums.length) {
        ArrayList<Integer> l=new ArrayList<>();
        for (int a:nums)
            l.add(a);
        res.add(l);
    }
    for (int i = index; i < nums.length; i++) {
        swap(nums, index, i);
        dfs(res, nums, index + 1);
        swap(nums, index, i);
    }
}
public void swap(int[] nums,int a,int b) {
    int temp = nums[a];
    nums[a] = nums[b];
    nums[b] = temp;
}
```

## [75. 颜色分类 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/sort-colors/submissions/)

```java
public void sortColors(int[] nums) {
    int left = 0, right = nums.length - 1;
    for (int i = 0; i <= right; i++) {
        if (nums[i] == 2) {
            swap(nums, i, right);
            right--;
            i--;
        } else if (nums[i] == 0) {
            swap(nums, i, left);
            left++;
        }
    }
}
public void swap(int nums[], int a, int b) {
    int temp = nums[a];
    nums[a] = nums[b];
    nums[b]= temp;
}
```

## [134. 加油站 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/gas-station/submissions/)<u>奇技淫巧</u>

```java
public int canCompleteCircuit(int[] gas, int[] cost) {
    int res, sum, yu;
    res = sum = yu = 0;
    for (int i = 0; i < gas.length; i++) {
        sum = sum + gas[i] - cost[i];
        yu = yu + gas[i] - cost[i];
        if (yu < 0) {
            yu = 0;
            res = i + 1;
        }
    }
    if (sum < 0) 
        return -1;
    return res;
}
```

## [189. 轮转数组 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/rotate-array/)

```java
public void rotate(int[] nums, int k) {
    k = k % nums.length;
    reverse(nums, 0, k);
    reverse(nums, k + 1, nums.length - 1);
    reverse(nums, 0, nums.length - 1);
}
public void reverse(int[] nums, int start, int end) {
    while (start < end) {
        int temp = nums[start];
        nums[start] = nums[end];
        nums[end] = temp;
        start += 1;
        end -= 1;
    }
}
```

# 字符串

## [387. 字符串中的第一个唯一字符 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)<u>化简</u>

```java
public int firstUniqChar(String s) {
    int min = Integer.MAX_VALUE;
    for (int i = 'a'; i <= 'z'; i++) {
        int f = s.indexOf(i);
        if (f == -1)
            continue;
        if (f == s.lastIndexOf(i))
            min = Math.min(min, f);
    }
    return min == Integer.MAX_VALUE ? -1 : min;
}
```

# 链表

## [160. 相交链表 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/submissions/)<u>总会步长相等</u>

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if (headA == null || headB == null)
        return null;
    ListNode a = headA, b = headB;
    while (a != b) {
        a = a == null ? headB : a.next;
        b = b == null ? headA : b.next;
    }
    return a;
}
```

## [206. 反转链表 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/reverse-linked-list/submissions/)

```java
public ListNode reverseList(ListNode head) {
    if(head == null)
        return null;
    ListNode myHead = head;
    ListNode p = head.next;
    head.next = null;
    while(p != null) {
        ListNode last = p;
        p = p.next;
        last.next = myHead;
        myHead = last;
    }
    return myHead;
}
```

## [142. 环形链表 II - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/linked-list-cycle-ii/)<u>一步与两步</u>

```java
public ListNode detectCycle(ListNode head) {
    if(head == null || head.next == null)
        return null;
    ListNode slow = head.next, fast = slow.next;
    while(slow != fast) {
        if(fast == null || fast.next == null)
            return null;
        slow = slow.next;
        fast = fast.next.next;
    }
    while(slow != head) {
        slow = slow.next;
        head = head.next;
    }
    return head;
}
```

# 深度优先遍历

## [104. 二叉树的最大深度 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

```java
public int maxDepth(TreeNode root) {
    if(root == null)
        return 0;
    return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
}
```

## [543. 二叉树的直径 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

```java
int max = 0;
public int diameterOfBinaryTree(TreeNode root) {
    help(root);
    return max;
}
public int help(TreeNode root) {
    if (root == null) 
        return 0;
    int left = help(root.left), right = help(root.right);
    max = Math.max(max, left + right);
    return Math.max(left, right) + 1;
}
```

## [200. 岛屿数量 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/number-of-islands/)<u>向下抹除广度</u>

```java
public int numIslands(char[][] grid) {
    int r=grid.length;
    if(r==0)
        return 0;
    int c=grid[0].length,res=0;
    for(int i=0;i<r;i++){
        for(int j=0;j<c;j++){
            if(grid[i][j]=='1'){
                tran(grid,i,j);
                res++;
            } 
        }
    }
    return res;
}
public void tran(char[][] grid,int i,int j){
    int r=grid.length,c=grid[0].length;
    if(i>=r||i<0||j>=c||j<0||grid[i][j]=='0')
        return ;
    grid[i][j]='0';
    tran(grid,i+1,j);
    tran(grid,i-1,j);
    tran(grid,i,j+1);
    tran(grid,i,j-1);
}
```

## [695. 岛屿的最大面积 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/max-area-of-island/)<u>向上计数广度</u>

```java
public int maxAreaOfIsland(int[][] grid) {
    int max = 0;
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++)
            if (grid[i][j] == 1)
                max = Math.max(max, dfs(grid, i, j));
    }
    return max;
}
private int dfs(int[][] grid, int r, int c) {
    if (r < 0 || c < 0 || r == grid.length || c == grid[0].length || grid[r][c] != 1)
        return 0;
    grid[r][c] = 0;
    return 
        dfs(grid, r + 1, c) +
        dfs(grid, r - 1, c) + 
        dfs(grid, r, c + 1) + 
        dfs(grid, r, c - 1) + 1;
}
```

# 广度优先遍历

## [102. 二叉树的层序遍历 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> list = new ArrayList<>();
    Queue<TreeNode> queue = new ArrayDeque<>();
    if (root != null)
        queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        List<Integer> temp = new ArrayList<>();
        while (size-- > 0) {
            TreeNode node = queue.poll();
            if (node.left != null)
                queue.offer(node.left);
            if (node.right != null)
                queue.offer(node.right);
            temp.add(node.val);
        }
        list.add(temp);
    }
    return list;
}
```

## [542. 01 矩阵 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/01-matrix/)<u>蔓延</u>

```java
public int[][] updateMatrix(int[][] mat) {
    int[][] arr = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    int r = mat.length;
    if (r == 0)
        return mat;
    int c = mat[0].length;
    if (c == 0)
        return mat;
    ArrayDeque<Integer> queue = new ArrayDeque<>();
    for (int i = 0; i < r; i++) {
        for (int j = 0; j < c; j++) {
            if (mat[i][j] == 0)
                queue.offerLast(i * c + j);
            else
                mat[i][j] = -1;
        }
    }
    while (!queue.isEmpty()) {
        int code = queue.pollFirst();
        int i = code / c, j = code % c;
        for (int a = 0; a < 4; a++) {
            int x = i + arr[a][0], y = j + arr[a][1];
            if (x >= 0 && x < r && y >= 0 && y < c && mat[x][y] == -1) {
                mat[x][y] = mat[i][j] + 1;
                queue.offerLast(x * c + y);
            }
        }
    }
    return mat;
}
```

# 数学

## [509. 斐波那契数 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/fibonacci-number/submissions/)

```java
public int fib(int n) {
    double sq5 = Math.sqrt(5);
    double f1 = Math.pow((1 + sq5) / 2, n) - Math.pow((1 - sq5) / 2, n);
    return (int)(f1 / sq5);
}
```

## [172. 阶乘后的零 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/factorial-trailing-zeroes/)

```java
public int trailingZeroes(int n) {
    int res = 0;
    for (int i = 5; i <= n; i += 5) {
        int temp = i;
        while(temp % 5 == 0) {
            temp /= 5;
            res++;
        }
    }
    return res;
}
```

```java
public int trailingZeroes(int n) {
    int res = 0;
    long mut = 5;
    while (n >= mut) {
        res += n / mut;
        mut *= 5;
    }
    return res;
}
```

## [231. 2 的幂 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/power-of-two/)<u>以位定值</u>

```java
public boolean isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}
```

## [326. 3 的幂 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/power-of-three/submissions/)

```java
public boolean isPowerOfThree(int n) {
    return n > 0 && 1162261467 % n == 0;
}
```

## [342. 4的幂 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/power-of-four/)<u>以为定值</u>

```java
public boolean isPowerOfFour(int n) {
    return n > 0 && (n & (n - 1)) == 0 && (n & 0xaaaaaaaa) == 0;
}
```

# 位运算

## [191. 位1的个数 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/number-of-1-bits/submissions/)位与消1

```java
public int hammingWeight(int n) {
    int res = 0;
    while (n != 0) {
        n = n & (n - 1);
        res++;
    }
    return res;
}
```

## [201. 数字范围按位与 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/bitwise-and-of-numbers-range/submissions/)总结代表

```java
public int rangeBitwiseAnd(int left, int right) {
    int shift = 0;
    while (left != right) {
        left >>= 1;
        right >>= 1;
        shift++;
    }
    return left << shift;
}
```

## [面试题 16.07. 最大数值 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/maximum-lcci/)

```java
public int maximum(int a, int b) {
    long q = a;
    long p = b;
    return (int)(((q + p) + Math.abs(q - p)) / 2);
}
```



# 设计

## [705. 设计哈希集合 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/design-hashset/submissions/)

```java
class MyHashSet {
    private static final int BASE = 1000;
    private LinkedList[] list;

    public MyHashSet() {
        list = new LinkedList[BASE];
    }

    public void add(int key) {
        int h = hash(key);
        if (list[h] == null)
            list[h] = new LinkedList<Integer>();
        boolean isAdd = contains(key);
        if (!isAdd)
            list[h].offerFirst(key);
    }

    public void remove(int key) {
        int h = hash(key);
        if (list[h] == null)
            return;
        list[h].remove((Integer)key);
    }

    public boolean contains(int key) {
        int h = hash(key);
        if (list[h] == null)
            return false;
        Iterator<Integer> it = list[h].iterator();
        while (it.hasNext()) {
            Integer temp = it.next();
            if(temp == key)
                return true;
        }
        return false;
    }
    public static int hash(int key) {
        return key % BASE;
    }
}
```

## [706. 设计哈希映射 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/design-hashmap/submissions/)

```java
class MyHashMap {
    private class Pair extends Object {
        private int key;
        private int value;
        public Pair(int key, int value) {
            this.key = key;
            this.value = value;
        }
        public void setValue(int value) {
            this.value = value;
        }
        public int getKey() {
            return key;
        }
        public int getValue() {
            return value;
        }
        @Override
        public boolean equals(Object o) {
            if(o.getClass() != Pair.class)
                return false;
            return key == ((Pair) o).key;
        }
    }
    private static final int BASE = 1000;
    private LinkedList[] list;
    public MyHashMap() {
        list = new LinkedList[BASE];
    }
    public void put(int key, int value) {
        int h = hash(key);
        if (list[h] == null)
            list[h] = new LinkedList<Pair>();
        Iterator<Pair> it = list[h].iterator();
        while (it.hasNext()) {
            Pair temp = it.next();
            if (temp.getKey() == key) {
                temp.setValue(value);
                return;
            }
        }
        list[h].offerFirst(new Pair(key, value));
    }
    public int get(int key) {
        int h = hash(key);
        if (list[h] == null)
            return -1;
        Iterator<Pair> it = list[h].iterator();
        while (it.hasNext()) {
            Pair temp = it.next();
            if (temp.getKey() == key)
                return temp.getValue();
        }
        return -1;
    }
    public void remove(int key) {
        int h = hash(key);
        if (list[h] == null)
            return;
        list[h].remove(new Pair(key, 0));
    }
    public static int hash(int key) {
        return key % BASE;
    }
}
```

## [622. 设计循环队列 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/design-circular-queue/submissions/)

```java
class MyCircularQueue {
    private int[] array;
    private int head;// 指向的下个单元是第一个
    private int tail;// 指向的单元是最后一个
    public MyCircularQueue(int k) {
        array = new int[k + 1];
        head = tail = 0;
    }
    public boolean enQueue(int value) {
        if (isFull())
            return false;
        tail = (tail + 1) % array.length;
        array[tail] = value;
        return true;
    }
    public boolean deQueue() {
        if (isEmpty())
            return false;
        head = (head + 1) % array.length;
        return true;
    }
    public int Front() {
        if (isEmpty())
            return -1;
        return array[(head + 1) % array.length];
    }
    public int Rear() {
        if (isEmpty())
            return -1;
        return array[tail];
    }
    public boolean isEmpty() {
        return head == tail;
    }
    public boolean isFull() {
        return (tail + 1) % array.length == head;
    }
}
```

## [707. 设计链表 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/design-linked-list/)

```java
class MyLinkedList {
    private class ListNode {
        int val;
        ListNode next;
        public ListNode(int val) {
            this.val = val;
        }
    }
    private ListNode head;
    public MyLinkedList() { 
        head = new ListNode(-1);
    }
    public int get(int index) {
        if (index < 0) 
            return -1;
        ListNode node = head;
        while (index-- != 0) {
            if (node.next == null)
                return -1;
            node = node.next;
        }
        if (node.next == null)
            return -1;
        return node.next.val;
    }
    public void addAtHead(int val) {
        ListNode newHead = new ListNode(val);
        newHead.next = head.next;
        head.next = newHead;
    }
    public void addAtTail(int val) {
        ListNode newTail = new ListNode(val);
        ListNode node = head;
        while (node.next != null) 
            node = node.next;
        node.next = newTail;
    }
    public void addAtIndex(int index, int val) {
        if (index < 0)
            index = 0;
        ListNode newNode = new ListNode(val);
        ListNode node = head;
        while (index-- != 0) {
            if (node.next == null)
                return;
            node = node.next;
        }
        newNode.next = node.next;
        node.next = newNode;
    }
    public void deleteAtIndex(int index) {
        if (index < 0)
            return;
        ListNode node = head;
        while (index-- != 0) {
            if (node.next == null)
                return;
            node = node.next;
        }
        if (node.next != null)
            node.next = node.next.next;
    }
}
```

## [641. 设计循环双端队列 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/design-circular-deque/)

```java
class MyCircularDeque {
    private int[] array;
    private int head;
    private int tail;
    public MyCircularDeque(int k) {
        array = new int[k + 1];
        head = tail = 0;
    }
    // 先移后放
    public boolean insertFront(int value) {
        if (isFull())
            return false;
        head--;
        if (head == -1)
            head = array.length - 1;
        array[head] = value;
        return true;
    }
    // 先放后移
    public boolean insertLast(int value) {
        if (isFull())
            return false;
        array[tail] = value;
        tail = (tail + 1) % array.length;
        return true;
    }
    public boolean deleteFront() {
        if (isEmpty())
            return false;
        head = (head + 1) % array.length;
        return true;
    }
    public boolean deleteLast() {
        if (isEmpty())
            return false;
        tail--;
        if (tail == -1) 
            tail = array.length - 1;
        return true;
    }
    public int getFront() {
        if (isEmpty())
            return -1;
        return array[head];
    }
    public int getRear() {
        if (isEmpty())
            return -1;
        int temp = tail - 1;
        if (temp == -1)
            temp = array.length - 1;
        return array[temp];
    }
    public boolean isEmpty() {
        return head == tail;
    }
    public boolean isFull() {
        return (tail + 1) % array.length == head;
    }
}
```

## [面试题 03.04. 化栈为队 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/implement-queue-using-stacks-lcci/)<u>双栈做队列</u>

```java
class MyQueue {
    Stack<Integer> pushStack, pollStack;
    /** Initialize your data structure here. */
    public MyQueue() {
        pushStack = new Stack<Integer>();
        pollStack = new Stack<Integer>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        pushStack.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if (pollStack.isEmpty()) {
            while (!pushStack.isEmpty()) {
                pollStack.push(pushStack.pop());
            }
        }
        return pollStack.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        if (pollStack.isEmpty()) {
            while (!pushStack.isEmpty()) {
                pollStack.push(pushStack.pop());
            }
        }
        return pollStack.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return pushStack.isEmpty() && pollStack.isEmpty();
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```



# 动态规划

## [55. 跳跃游戏 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/jump-game/)

```java
public boolean canJump(int[] nums) {
    if (nums.length == 1 && nums[0] >=0) {
        return true;
    }
    int nextIndex = nums.length - 1;
    for (int i = nums.length - 2; i >= 0; i--) {
        if (i + nums[i] >= nextIndex) {
            nextIndex = i;
        }
    }
    return nextIndex == 0;
}
```

## [121. 买卖股票的最佳时机 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)<u>有小取小，终会遇到大</u>

```java
public int maxProfit(int[] prices) {
    int res = 0, buy = prices[0];
    for (int temp : prices) {
        res = Math.max(res, temp - buy);
        buy = Math.min(buy, temp);
    }
    return res;
}
```

## [198. 打家劫舍 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/house-robber/)<u>近水楼台先报警</u>

```java
public int rob(int[] nums) {
    if (nums.length == 1)
        return nums[0];
    else if (nums.length == 2)
        return Math.max(nums[0], nums[1]);
    else if (nums.length == 3)
        return Math.max(nums[1], nums[0] + nums[2]);
    nums[2] += nums[0];
    int max = Math.max(nums[1], nums[2]);
    for(int i = 3; i < nums.length; i++) {
        nums[i] += Math.max(nums[i - 2], nums[i - 3]);
        max = Math.max(max, nums[i]);
    }
    return max;
}
```

## [322. 零钱兑换 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/coin-change/)<u>从已完成中取值</u>

```java
public int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1);
    dp[0] = 0;
    for (int i = 0; i <= amount; i++) {
        for (int j = 0; j < coins.length; j++) {
            if (i >= coins[j]) {
                dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
            }
        }
    }
    return dp[amount] > amount ? -1 : dp[amount];
}
```



# 回溯

## [39. 组合总和 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/combination-sum/submissions/)<u>选与不选</u>

```java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> res = new ArrayList<>();
    Stack<Integer> temp = new Stack<>();
    help(res, candidates, target, temp, 0);
    return res;
}
private void help(List<List<Integer>> res, int[] array, int target, Stack<Integer> temp, int index) {
    if (index >= array.length)
        return;
    else if (target == 0) {
        res.add(new ArrayList<>(temp));
        return;
    }
    help(res, array, target, temp, index + 1);
    if (target - array[index] >= 0) {
        temp.push(array[index]);
        help(res, array, target - array[index], temp, index);
        temp.pop();
    }
}
```

# 单调栈

## [739. 每日温度 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/daily-temperatures/)<u>找最近</u>

```java
public int[] dailyTemperatures(int[] T) {
    int[] res = new int[T.length];
    Stack<Integer> stack = new Stack();
    stack.push(0);
    for (int i = 1; i < T.length; i++) {
        while (!stack.isEmpty() && T[i] > T[stack.peek()]) {
            int index = stack.pop();
            res[index] = i - index;
        }
        stack.push(i);
    }
    while (!stack.isEmpty()) {
        res[stack.pop()] = 0;
    }
    return res;
}
```

## [962. 最大宽度坡 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/maximum-width-ramp/)<u>找最远</u>

```java
public int maxWidthRamp(int[] nums) {
    Stack<Integer> stack = new Stack<>();
    int max = 0;
    stack.push(0);
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] < nums[stack.peek()])
            stack.push(i);
    }
    for (int i = nums.length - 1; i >= 0; i--) {
        while (!stack.isEmpty() && stack.peek() >= i) {
            stack.pop();
        }
        while (!stack.isEmpty() && nums[i] >= nums[stack.peek()]) {
            max = Math.max(max, i - stack.pop());
        }
        if (stack.isEmpty())
            break;
    }
    return max;
}
```

# 二分查找

## [35. 搜索插入位置 题解 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/search-insert-position/solution/)<u>二分变种</u>

```java
public int searchInsert(int[] nums, int target) {
    int left = 0, right = nums.length - 1, mid = -1;
    while (left <= right) {
        mid = (left + right) / 2;
        if (nums[mid] >= target) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

# 哈希表

## [1. 两数之和 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/two-sum/)<u>先比后存</u>

```java
public int[] twoSum(int[] nums, int target) {
    HashMap<Integer,Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        if (map.containsKey(target - nums[i])) {
            return new int[]{map.get(target - nums[i]), i};
        }
        map.put(nums[i], i);
    }
    return new int[]{};
}
```

## [剑指 Offer 03. 数组中重复的数字 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

```java
public int findRepeatNumber(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == i)
            continue;
        if (nums[nums[i]] == nums[i])
            return nums[i];
        int index = nums[i];
        nums[i] = nums[index];
        nums[index] = index;
        i--;
    }
    return -1;
}
```

# 滑动窗口

## [3. 无重复字符的最长子串 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

```java
public int lengthOfLongestSubstring(String s) {
    HashSet<Character> set = new HashSet<>();
    int left, right;
    left = right = 0;
    int ret = 0;
    while (right < s.length()) {
        char temp = s.charAt(right);
        if (set.contains(temp)) {
            while (s.charAt(left) != temp) {
                set.remove(s.charAt(left));
                left++;
            }
            left++;
        }
        ret = Math.max(ret, right - left + 1);
        set.add(temp);
        right++;
    }   
    return ret;
}
```

## [567. 字符串的排列 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/permutation-in-string/)

```java
public boolean checkInclusion(String s1, String s2) {
    int n = s1.length(), m = s2.length();
    if (n > m)
        return false;
    int[] arr = new int[26];
    for (int i = 0; i < n; i++) 
        ++arr[s1.charAt(i) - 'a'];
    for (int left = 0, right = 0; right < m; right++) {
        int x = s2.charAt(right) - 'a';
        --arr[x];
        while (arr[x] < 0) {
            ++arr[s2.charAt(left) - 'a'];
            ++left;
        }
        if (right - left + 1 == n)
            return true;
    }
    return false;
}
```


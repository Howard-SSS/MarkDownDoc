[toc]

# 双指针

## [1. 两数之和 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/two-sum/)

以target-num[i]为键访问值

若是值存在，说明匹配；若是值不存在说明暂时不匹配

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

## [26. 删除有序数组中的重复项 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

有序数组里判断num[i]与num[i-1]是否相等

若不相等说明值还未重复，可以赋值；若相等说明值重复出现，则跳过

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

## [46. 全排列 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/permutations/)

将数组划分为以归类和未归类，

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

定义`left`下标为0序列的结束位置，左侧均为0；定义`right`下标为2序列的开始，右侧均为2

下标`i`遍历`nums`：

每当`nums[i]`为2，`nums[i]`与`nums[right]`交换，交换后，`nums[i]`值未知待下次判断；

每当`nums[i]`为0，`nums[i]`与`nums[left]`交换，交换后，`nums[i]`值为1，跳过；

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

# 计数

## [169. 多数元素 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/majority-element/)

一个数的出现次数大于数组长度的一半，若是相互抵消，则最后出现的就是该数

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

# 求余

## [1018. 可被 5 整除的二进制前缀 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/binary-prefix-divisible-by-5/)

a=bc+d

10a+e=10(bc+d)+e

=10bc+10d+e=10bc+?bc+d^,^

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

# 搜索

## [387. 字符串中的第一个唯一字符 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

根据字母找最前和最后的下标：

若下标相等，说明是唯一的，找出最小的下标；

若下标不相等，说明不是唯一的；

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

# 步长

## [160. 相交链表 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

假设e是链表a和链表b的公共链表，链表a为q+e，链表b为p+e

使链表a变为q+e+p+e，链表b变为p+e+q+e，它们均需要走q+e+p次才到达e

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

# 快慢指针

## [142. 环形链表 II - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

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

# 部分展开

## [剑指 Offer 36. 二叉搜索树与双向链表 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

```java
public Node treeToDoublyList(Node root) {
    if (root == null)
        return null;
    ArrayDeque<Node> stack = new ArrayDeque<>();
    Node max = null;
    while (root != null) {
        stack.push(root);
        root = root.right;
    }
    max = stack.peek();
    Node last = null;
    while (!stack.isEmpty()) {
        root = stack.pop();
        Node temp = root.left;
        while (temp != null) {
            stack.push(temp);
            temp = temp.right;
        }
        if (last != null) {
            root.right = last;
            last.left = root;
        }
        last = root;
    }
    max.right = last;
    last.left = max;
    return last;
}
```

# 深度优先遍历

## [59. 螺旋矩阵 II - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/spiral-matrix-ii/)

确定方向，每当遇到该方向下不能再赋值，就更换方向

```java
public int[][] generateMatrix(int n) {
    int[][] arr = new int[n][n];
    int r = arr.length, c = arr[0].length;
    int i = 0, j = 0;
    int[][] help = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    int index = 0, num = 1;
    while (true) {
        arr[i][j] = num++;
        int tempI = i + help[index][0], tempJ = j + help[index][1];
        if (tempI >=0 && tempI < r && tempJ >= 0 && tempJ < c && arr[tempI][tempJ] == 0) {
            i = tempI;
            j = tempJ;
        } else {
            index = (index + 1) % 4;
            tempI = i + help[index][0];
            tempJ = j + help[index][1];
            if (tempI >= 0 && tempI < r && tempJ >= 0 && tempJ < c && arr[tempI][tempJ] == 0) {
                i = tempI;
                j = tempJ;
            } else
                break;
        }
    }
    return arr;
}
```

## [200. 岛屿数量 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/number-of-islands/)

`i,j`遍历`grid`，每当`grid[i][j]=='1'`：

就从该处深度遍历将岛屿填埋，并计数加一；

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

## [695. 岛屿的最大面积 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/max-area-of-island/)

`i,j`遍历`grid`，每当判断到`grid[i][j]`为1：

就从该处统计四周有多少个1+1，依次类推；

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

## [542. 01 矩阵 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/01-matrix/)<u>越前就越小</u>

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

## [117. 填充每个节点的下一个右侧节点指针 II - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii/)<u>横跳</u>

```java
public Node connect(Node root) {
    if (root == null)
        return null;
    Node head = root;
    Node l = null;
    // 找起始Node
    while (l == null && head != null) {
        if (head.left != null) {
            l = head.left;
            break;
        } else if (head.right != null) {
            l = head.right;
            break;
        }
        head = head.next;
    }
    Node nextFloor = l;
    // 给起始Node.next找Node
    while (head != null) {
        if (head.left != null && head.left != l) {
            l.next = head.left;
            l = l.next;
        }
        if (head.right != null && head.right != l) {
            l.next = head.right;
            l = l.next;
        }
        head = head.next;
    }
    connect(nextFloor);
    return root;
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

## [231. 2 的幂 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/power-of-two/)<u>1位</u>

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

## [342. 4的幂 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/power-of-four/)<u>1位</u>

```java
public boolean isPowerOfFour(int n) {
    return n > 0 && (n & (n - 1)) == 0 && (n & 0xaaaaaaaa) == 0;
}
```

## [43. 字符串相乘 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/multiply-strings/)<u>乘法规则</u>

```java
public String multiply(String num1, String num2) {
    if (num1.equals("0") || num2.equals("0"))
        return "0";
    char[] left = num1.toCharArray(), right = num2.toCharArray();
    ArrayList<Integer> list = new ArrayList<>();
    for (int i = left.length - 1; i >= 0; i--) {
        for (int j = right.length - 1; j >= 0; j--) {
            int index = left.length - 1 - i + right.length - 1 - j;
            int temp = (left[i] - '0') * (right[j] - '0');
            while (list.size() <= index)
                list.add(0);
            list.set(index, list.get(index) + temp);
        }
    }
    int temp = 0;
    StringBuilder builder = new StringBuilder("");
    for (int i = 0; i < list.size(); i++) {
        temp += list.get(i);
        builder.append(temp % 10);
        temp /= 10;
    }
    while (temp != 0) {
        builder.append(temp % 10);
        temp /= 10;
    }
    return builder.reverse().toString();
}
```

# 位运算

## [268. 丢失的数字 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/missing-number/)

```java
public int missingNumber(int[] nums) {
    int ret=0;
    for (int i = 1; i <= nums.length; i++) {
        ret ^= i;
        ret ^= nums[i - 1];
    }
    return ret;
}
```

## [191. 位1的个数 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/number-of-1-bits/submissions/)消后缀1

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

## [201. 数字范围按位与 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/bitwise-and-of-numbers-range/submissions/)

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

## [55. 跳跃游戏 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/jump-game/)<u>最远跳</u>

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

## [121. 买卖股票的最佳时机 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

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

## [198. 打家劫舍 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/house-robber/)

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

## [322. 零钱兑换 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/coin-change/)<u>已完成中取值</u>

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

## [139. 单词拆分 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/word-break/)<u>记忆</u>

```java
public boolean wordBreak(String s, List<String> wordDict) {
    HashSet<String> set = new HashSet<>(wordDict);
    boolean[] arr = new boolean[s.length() + 1];
    arr[0] = true;
    for (int i = 1; i <= s.length(); i++) {
        for (int j = 0; j < i; j++) {
            if (arr[j] && set.contains(s.substring(j, i))) {
                arr[i] = true;
                break;
            }
        }
    }
    return arr[s.length()];
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

## [784. 字母大小写全排列 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/letter-case-permutation/)<u>一分为二已完成和未完成</u>

```java
class Solution {
    private List<String> res = new ArrayList<>();
    public List<String> letterCasePermutation(String s) {
        StringBuilder builder = new StringBuilder(s.toLowerCase());
        dfs(builder, 0);
        return res;
    }
    private void dfs(StringBuilder s, int index) {
        if (s.length() == index) {
            res.add(s.toString());
            return;
        }
        // 小写或数字
        dfs(s, index + 1);
        // 大写
        char q = s.charAt(index);
        if (q >= 'a' && q <= 'z') {
            s.setCharAt(index, (char)(q - 32));
            dfs(s, index + 1);
            s.setCharAt(index, q);
        }
    }
}
```

## [90. 子集 II - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/subsets-ii/)

```java
public List<List<Integer>> subsetsWithDup(int[] nums) {
    Arrays.sort(nums);
    List res = new ArrayList<ArrayList<Integer>>();
    List temp = new ArrayList<Integer>();
    res.add(temp);
    dfs(nums, 0, res, temp);
    return res;
}
private void dfs(int[] nums, int index, List res, List temp) {
    if (index == nums.length)
        return;
    for (int i = index; i < nums.length;) {
        temp.add(nums[i]);
        res.add(new ArrayList<>(temp));
        dfs(nums, i + 1, res, temp);
        temp.remove(temp.size() - 1);
        int n = nums[i];
        while (i < nums.length && n == nums[i])
            i++;
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
    int[] map = new int[128];
    int max = 0;
    for (int left = 0, right = 0; right < s.length(); right++) {
        char rightChar = s.charAt(right);
        map[rightChar] += 1;
        while (map[rightChar] == 2) {
            map[s.charAt(left)]--;
            left++;
        }
        max = Math.max(max, right - left + 1);
    }
    return max;
}
```

## [424. 替换后的最长重复字符 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)

```java
public int characterReplacement(String s, int k) {
    int[] arr = new int[26];
    int maxCount = 0;
    int ret = 0;
    for (int left = 0, right = 0; right < s.length(); ) {
        arr[s.charAt(right) - 'A']++;
        maxCount = Math.max(maxCount, arr[s.charAt(right) - 'A']);
        right++;
        if (right - left > maxCount + k) {
            arr[s.charAt(left) - 'A']--;
            left++;
        }
        ret = Math.max(ret, right - left);
    }
    return ret;
}
```

# 树

## [144. 二叉树的前序遍历 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

```java
public List<Integer> preorderTraversal(TreeNode root) {
    Stack<TreeNode> stack = new Stack<>();
    List<Integer> list = new ArrayList<>();
    if (root != null)
        stack.push(root);
    while (!stack.isEmpty()) {
        TreeNode temp = stack.pop();
        if (temp.right != null)
            stack.push(temp.right);
        if (temp.left != null)
            stack.push(temp.left);
        list.add(temp.val);
    }
    return list;
}
```

## [94. 二叉树的中序遍历 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

```java
public List<Integer> inorderTraversal(TreeNode root) {
    Stack<TreeNode> stack = new Stack<>();
    List<Integer> list = new ArrayList<>();
    while (root != null || !stack.isEmpty()) {
        while (root != null) {
            stack.push(root);
            root = root.left;
        }
        root = stack.pop();
        list.add(root.val);
        root = root.right;
    }
    return list;
}
```

## [145. 二叉树的后序遍历 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/submissions/)<u>翻转树前序</u>

```java
public List<Integer> postorderTraversal(TreeNode root) {
    Stack<TreeNode> stack = new Stack<>();
    LinkedList<Integer> list = new LinkedList<>();
    if (root != null)
        stack.push(root);
    while (!stack.isEmpty()) {
        TreeNode temp = stack.pop();
        if (temp.left != null)
            stack.push(temp.left);
        if (temp.right != null)
            stack.push(temp.right);
        list.addFirst(temp.val);
    }
    return list;
}
```

## [199. 二叉树的右视图 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/binary-tree-right-side-view/)<u>覆盖</u>

```java
public List<Integer> rightSideView(TreeNode root) {
    List<Integer> ret = new ArrayList<>();
    dfs(root, 0, ret);
    return ret;
}
public void dfs(TreeNode root, int index, List<Integer> list) {
    if (root == null)
        return;
    if (list.size() == index) {
        list.add(root.val);
    } else {
        list.set(index, root.val);
    }
    dfs(root.left, index + 1, list);
    dfs(root.right, index + 1, list);
}
```

## [450. 删除二叉搜索树中的节点 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/delete-node-in-a-bst/)<u>代替</u>

```java
public TreeNode deleteNode(TreeNode root, int key) {
    if (root == null)
        return null;
    if (root.val == key) {
        TreeNode last = root, now = root.left;
        if (now == null)
            return root.right;
        while (now.right != null) {
            last = now;
            now = now.right;
        }
        if (root != last) {
            last.right = deleteNode(now, now.val);
        }
        now.right = root.right;
        if (now != root.left)
            now.left = root.left;
        return now;
    }
    root.left = deleteNode(root.left, key);
    root.right = deleteNode(root.right, key);
    return root;
}
```

# 前缀和

## [560. 和为 K 的子数组 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/subarray-sum-equals-k/)<u>差</u>

```java
public int subarraySum(int[] nums, int k) {
    HashMap<Integer, Integer> map = new HashMap<>();
    int ret = 0, sum = 0;
    map.put(0, 1);
    for (int temp : nums) {
        sum += temp;
        ret += map.getOrDefault(sum - k, 0);
        map.put(sum, map.getOrDefault(sum, 0) + 1);
    }
    return ret;
}
```

## [238. 除自身以外数组的乘积 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/product-of-array-except-self/)<u>左右横跳</u>

```java
public int[] productExceptSelf(int[] nums) {
    int[] ret = new int[nums.length];
    int temp = 1;
    for (int i = 0; i < nums.length; i++) {
        ret[i] = temp;
        temp *= nums[i];
    }
    temp = 1;
    for (int i = nums.length - 1; i >= 0; i--) {
        ret[i] *= temp;
        temp *= nums[i];
    }
    return ret;
}
```


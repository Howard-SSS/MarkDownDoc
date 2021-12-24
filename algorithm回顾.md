[toc]

# 数组

## [1. 两数之和 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/two-sum/)

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

## [169. 多数元素 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/majority-element/)

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

## [268. 丢失的数字 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/missing-number/)

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

## [1018. 可被 5 整除的二进制前缀 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/binary-prefix-divisible-by-5/submissions/)

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

## [134. 加油站 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/gas-station/submissions/)

奇技淫巧

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

# 链表

## [160. 相交链表 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/submissions/)

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

## [200. 岛屿数量 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/number-of-islands/)

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

# 数学

## [50. Pow(x, n) - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/powx-n/)

```

```


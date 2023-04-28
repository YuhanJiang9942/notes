### Java Notes

#### Array

```java
Boolean b = Arrays.equals(array1, array2);
```

###### array to list

```java
int[] arr = new int[5];
List<Integer> list = new ArrayList<>(Arrays.asList(arr));
```

###### List to Array

```java
List<List<Integer>> list = new ArrayList<>();
list.toArray(new int[list.size()][list.get(0).size()]);

```



#### Set

##### list to set

```java
Set<String> set = new HashSet<>(list);
```

#### String

```java
String s;
char[] arr = s.toCharArray();
String s1 = String.valueOf(arr);
```

#### char

```java
char c = '1';
int i = c - '0';
```



#### List

##### map to list

```java
ArrayList<List<String>>(map.values());
```



#### Stack

```java
Stack<Integer> stack1 = new Stack<>();
Deque<Integer> stack2 = new ArrayDeque<>();
// OR
Deque<Integer> stack2 = new LinkedList<>();

stack.push(1);
stack.pop();
stack.peek();
```

##### PriorityQueue

```java
PriorityQueue<Integer> p = new PriorityQueue<>((a, b) -> a-b);
p.add(1);
p.poll();
p.peek();
//默认小根堆
```



11. 盛最多水的容器

    贪心算法，双指针

    双指针从两端开始，重点是理解内在逻辑



1705. 吃苹果的最大数目

      贪心，优先队列

      首先理解，先吃掉最先过期的苹果

      根据此，想到使用优先队列



688. 骑士在棋盘上的概率

     首选动态规划

     想到dfs，但用时长，因为有位置被反复搜索。解决方法：存储走过的格子，是另类的动态规划

### Leetcode

#### stack

最小栈：

栈内单调递增，用于寻找右边第一个小于的数，也可以寻找左右第一个小于的数

84. 柱状图中最大的长方形

最后入栈一个0，可以达成全部出栈。

#### 回溯

本质是dfs

dfs(){

//结束条件

//操作cur

//dfs()

//cur恢复

}

93. 复原ip地址
94. 括号生成
95. 全排列

#### BFS

BFS 使用queue

```java
int level = 0;
Deque<Integer> queue = new LinkedList<>();
queue.offer(head);
while(!queue.isEmpty()){
    int size = queue.size();
    for (int i = 0; i < size; i++){
        Node cur = queue.poll();
        for (Node node : cur.){
            // if the node does not comply with the condition, do not offer to queue 
            if (!isVisited(node) && isValid(node)){
                queue.offer();
                visited(node);
            }
            // do something
        }
    }
    level++;
}
```



##### 图

对于图要注意要记录节点是否访问过

542. 01矩阵

     深搜会超时

     最短距离是典型的图的BFS

     是多起点的BFS，开始要将所有起点push

##### 树

#### DFS

##### 图

#### 图

133 clone graph

The deep copy of a graph. DFS BFS both fine. 

use a Map<oldNode, newNode> to record whether a node has been visited.



207 course schedule

判断一个有向图是否无环/是否可以遍历所有节点

dfs: 因为每个节点都需遍历，所以重点为判断是否有环，记录每个节点是否访问中，或访问过， 若遇到访问中的节点代表有环



bfs: 重点是判断是否访问过了所有节点，所以记录已访问的节点数量，多起点的bfs, 将indegree为0的节点作为起点

与普通bfs不同的是不记录每个节点是否访问过，因为只将indegree为0的节点入队所以不会重复访问

衍生

210 记录路径

1426 查询

#### DP

##### 优化

1. 滚动数组

   ```java
   int[] dp = new int[2][n];
   dp[i&1][j] = 
   ```

   

##### 01背包

完全背包

```java
class Solution {
    public int maxValue(int N, int C, int[] v, int[] w) {
        int[][] dp = new int[N][C + 1];
        
        // 先预处理第一件物品
        for (int j = 0; j <= C; j++) {
            // 显然当只有一件物品的时候，在容量允许的情况下，能选多少件就选多少件
            int maxK = j / v[0];
            dp[0][j] = maxK * w[0];
        }
        
        // 处理剩余物品
        for (int i = 1; i < N; i++) {
            for (int j = 0; j <= C; j++) {
                // 不考虑第 i 件物品的情况（选择 0 件物品 i）
                int n = dp[i - 1][j];
                // 考虑第 i 件物品的情况
                int y = 0;
                for (int k = 1 ;; k++) {
                    if (j < v[i] * k) {
                        break;
                    }
                    y = Math.max(y, dp[i - 1][j - k * v[i]] + k * w[i]);
                }
                dp[i][j] = Math.max(n, y);
            }
        }
        return dp[N - 1][C];
    }
}
```

一维优化

```java
dp[i][j] = Math.min(dp[i-1][j], dp[i][j - v[i]] + w[i])
```

##### 回文

```java
dp[i][j] = dp[i+1][j-1]
```

观察状态转移方程顺序，应先遍历 length (j-i)

此类问题也可使用中心扩散法

#### DP & 记忆化搜索

dp的思想和记忆化搜索类似，记忆化搜索为自上而下，dp为自下而上。

322. coin change   1575. 统计所有可行路径

记忆化搜索和dp皆可，类似01背包，遍历两个状态。

坐标降维：

(x,y) -> x * n + y

x = index / n

y = index % n



#### UnionFind

```java
class UnionFind{
        private int[] parent;
        public UnionFind(int n){
            parent = new int[n];
            for (int i = 0; i < n; i++){
                parent[i] = i;
            }
        }
        public int find(int n){
            if (parent[n] == n){
                return n;
            }
            else{
                return find(parent[n]);
            }
        }
        public void union(int a, int b){
            parent[find(b)] = find(a);
        }
    }
```

#### 排序

##### 荷兰国旗问题

双指针一次遍历

1. p0 & p1

   when swapping with p0, it is possible to swap a 1 swap, thus do another swap if necessary

2. p0 & p2

   swapping with p2 might get another 0 or 2, need to further swap until not 0 or 2.

#### 树

##### 二叉树

![](C:\Users\jiang\Pictures\typora\tree example.png)

- 层次遍历顺序：[1 2 3 4 5 6]
- 前序 (pre-order) 遍历顺序：[1 2 4 5 3 6]
- 中序 (in-order) 遍历顺序：[4 2 5 1 3 6]
- 后序 (post-order) 遍历顺序：[4 5 2 6 3 1]

前序/后序 + 中序 可以确定一棵二叉树

前序第一个是root，[root, left, right]

根据root，中序可以确定左右节点数量 [left, root, right]，

#### 二分法 （binary search）

```java
while(left < right){
    int mid = left + (right - left + 1)/2;
    if (...){
        left = mid;
    }
    else{
        right = mid-1;
    }
}
```

注意死循环的可能，mid取下界时left不能=mid;



#### TO DO

1303  hard 路径，DP

背包 https://leetcode.cn/problems/partition-equal-subset-sum/solution/gong-shui-san-xie-bei-bao-wen-ti-shang-r-ln14/



 9/19 boss

最小公约数

买卖股票3

加分二叉树

9/19 trend

stringbuffer toString outOfMemory

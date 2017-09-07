# LeetCode整理

## 基本算法
| **序号** | **题名** | **思路** | **分类** | 
| --- | --- | --- | --- |
| 1 | [Two Sum](https://leetcode.com/problems/two-sum/) | 字典保存差值 | Hash |
| 2 | [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/) | 进位，两个加数长短不同 | |
| 3 | [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | 局部和整体 | DP |
| 7 | [Reverse Integer](https://leetcode.com/problems/reverse-integer/) | 正负，处理溢出 | |
| 9 | [Palindrome Number](https://leetcode.com/problems/palindrome-number/) | 倒序后是否相等 | |
| 11 | [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)| 全局最大，双指针向中间移动 | DP |
| 12 | [Integer to Roman](https://leetcode.com/problems/integer-to-roman/) | 用列表先存储映射 | |
| 13 | [Roman to Integer](https://leetcode.com/problems/roman-to-integer/) | 用列表先存储映射 | |
| 14 | [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/) | 字符按长度排序，从最短的那个开始 | |
| 17 | [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) | 字典 | Hash |
| 19 | [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) | 虚拟头结点 | 链表 |
| 20 | [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) | 栈的使用 |  |
| 21 | [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) | 双指针齐头并进 | 链表 |
| 22 | [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/) | 括号生成的过程 |  |
| 24 | [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/) | 虚拟头结点，交换后两个 | 链表 |
| 26 | [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) | 慢指针，数据前移 |  |
| 27 | [Remove Element](https://leetcode.com/problems/remove-element/) | 同26 |  |
| 33 | [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) | 变种二分 | 二分 |
| 34 | [Search for a Range](https://leetcode.com/problems/search-for-a-range/) | 范围两侧分别二分 | 二分 |
| 35 | [Search Insert Position](https://leetcode.com/problems/search-insert-position/) | 基础二分 | 二分 |
| 46 | [Permutations](https://leetcode.com/problems/permutations/) | 插入式全排列 |  |
| 48 | [Rotate Image](https://leetcode.com/problems/rotate-image/) | 上下对称，对角线再对称 |  |
| 49 | [Group Anagrams](https://leetcode.com/problems/anagrams/) | 单词按字母顺序整理 |  |
| 50 | [Pow(x, n)](https://leetcode.com/problems/powx-n/) | 0，1，小于0，一半递归 |  |
| 53 | [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) | 全局，局部 | DP |
| 152 | [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/) | 乘法有负负得正，大小都要保存 | DP |
| 54 | [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/) | 四个指针，较难 |  |
| 55 | [Jump Game](https://leetcode.com/problems/jump-game/) | 逆推 |  |
| 61 | [Rotate List](https://leetcode.com/problems/rotate-list/) | 快慢指针 | 链表 |
| 62 | [Unique Paths](https://leetcode.com/problems/unique-paths/) | 经典动态规划，填表 | DP |
| 63 | [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/) | 62变种 | DP |
| 64 | [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/) | 62变种，每次只加小的那个 | DP |
| 69 | [Sqrt(x)](https://leetcode.com/problems/sqrtx/) | 二分或牛顿 |  |
| 70 | [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) | 斐波那契，如果可以走三步呢？ |  |
| 71 | [Simplify Path](https://leetcode.com/problems/simplify-path/) | 栈的使用 |  |
| 73 | [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/) | 用矩阵第一行第一列保存信息 |  |
| 74 | [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/) | 二分变种 | 二分 |
| 240 | [Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/) | 行迭代，每行的时候左右移动指针 |  |
| 78 | [Subsets](https://leetcode.com/problems/subsets/) | 循环新增 |  |
| 83 | [Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/) |  | 链表 |
| 82 | [Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/) | 83变种，虚拟头结点 | 链表 |
| 86 | [Partition List](https://leetcode.com/problems/partition-list/) | 两个新指针，往上拼 | 链表 |
| 206 | [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) | 三指针 | 链表 |
| 92 | [Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/) | 虚拟头结点，三指针 | 链表 |
| 136 | [Single Number](https://leetcode.com/problems/single-number/) | 异或 |  |
| 138 | [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/) | 复制一个，最后拆开 | 链表 |
| 141 | [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) | 快慢指针 | 链表 |
| 142 | [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) | 同141 | 链表 |
| 151 | [Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/) | 全翻转，然后翻转每个单词 |  |
| 160 | [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/) | 走到尾巴，转到另一条链表 | 链表 |
| 162 | [Find Peak Element](https://leetcode.com/problems/find-peak-element/) | 二分变种 | 二分 |
| 169 | [Majority Element](https://leetcode.com/problems/majority-element/) | 是加一，不是减一 |  |
| 179 | [Largest Number](https://leetcode.com/problems/largest-number/) | a+b比b+a，考虑0 |  |
| 189 | [Rotate Array](https://leetcode.com/problems/rotate-array/) | 全翻，翻左，翻右 |  |
| 191 | [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/) | 减一做与 |  |
| 203 | [Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/) | 虚拟头结点 | 链表 |
| 204 | [Count Primes](https://leetcode.com/problems/count-primes/) | 范围根号就够，按比例把后面都进行标记 |  |
| 215 | [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) | 快排思想，同理最大N个数 |  |
| 263 | [Ugly Number](https://leetcode.com/problems/ugly-number/) | 除2，除3，除5 |  |
| 264 | [Ugly Number II](https://leetcode.com/problems/ugly-number-ii/) | 三指针，三临时变量，保证有序 |  |
| 8 | [String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/) | 去空格，验证符号，检查溢出 |  |


## 理解思路
| **序号** | **题名** | **思路** |
| --- | --- | --- |
| 146 | [LRU Cache](https://leetcode.com/problems/lru-cache/?tab=Description) | 利用链表 |
| 155 | [Min Stack](https://leetcode.com/problems/min-stack/) | 双栈 |
| 225 | [Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/) | 双队列 |
| 232 | [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/) | 双栈 |


## 树相关
| **序号** | **题名** | **思路** |
| --- | --- | --- |
| 144 | [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/) | 前序 |
| 94 | [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/) | 中序 |
| 145 | [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/) | 后序 |
| 98 | [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/) | 理解BST |
| 100 | [Same Tree](https://leetcode.com/problems/same-tree/) | 左右递归 |
| 102 | [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/) | 用数组，队列保存 |
| 104 | [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) | 左右递归 |
| 111 | [Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/) | 左右递归，注意深度为0 |
| 112 | [Path Sum](https://leetcode.com/problems/path-sum/) |  |
| 113 | [Path Sum II](https://leetcode.com/problems/path-sum-ii/) | DFS |
| 105 | [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) | 理解前序中序遍历的关系 |
| 106 | [Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/) | 理解后序中序遍历的关系 |


> 好大一桶矿泉水 for TR ⊙ˍ⊙



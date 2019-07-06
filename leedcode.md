<!-- TOC -->

- [排序](#排序)
    - [](#)
- [LRU缓存机制](#lru缓存机制)
    - [法一：Java使用HashMap+双链表实现；](#法一java使用hashmap双链表实现)
    - [法二：利用LinkedHashMap 实现 LRU 算法](#法二利用linkedhashmap-实现-lru-算法)
- [链表](#链表)
    - [判断两个链表是否相交](#判断两个链表是否相交)
    - [在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序。](#在 on log n-时间复杂度和常数级空间复杂度下对链表进行排序)
- [树](#树)
    - [二叉树的序列化与反序列化](#二叉树的序列化与反序列化)
    - [二叉树的最近公共祖先](#二叉树的最近公共祖先)
- [数组](#数组)
    - [除自身以外数组的乘积](#除自身以外数组的乘积)
    - [递增的三元子序列](#递增的三元子序列)
    - [打乱数组](#打乱数组)
    - [移动零](#移动零)
    - [求众数](#求众数)
    - [乘积最大子序列](#乘积最大子序列)
- [堆、栈、队列](#堆栈队列)
    - [扁平化嵌套列表迭代器](#扁平化嵌套列表迭代器)
    - [基本计算器 II](#基本计算器-ii)
    - [数组中的第K个最大元素](#数组中的第k个最大元素)
- [字符串](#字符串)
    - [验证回文串](#验证回文串)
    - [分割回文串](#分割回文串)
    - [3、单词拆分](#3单词拆分)
    - [2、单词拆分2](#2单词拆分2)
    - [3、212. 单词搜索 II](#3212-单词搜索-ii)
- [动态规划](#动态规划)
    - [1、至少有K个重复字符的最长子串](#1至少有k个重复字符的最长子串)
    - [2、二叉树中的最大路径和](#2二叉树中的最大路径和)
    - [3、最长连续序列](#3最长连续序列)
    - [4、打家劫舍](#4打家劫舍)
    - [5、完全平方数](#5完全平方数)
    - [6、 最长上升子序列](#6-最长上升子序列)
    - [](#-1)
- [数学和位运算](#数学和位运算)
    - [1、直线上最多的点数](#1直线上最多的点数)
    - [2、分数到小数](#2分数到小数)
    - [3、给定一个整数 n，返回 n! 结果尾数中零的数量。](#3给定一个整数-n返回-n-结果尾数中零的数量)
        - [4、](#4)
        - [5、位 1 的个数](#5位-1-的个数)
        - [6、求一个数是不是质数](#6求一个数是不是质数)
        - [7、计数质数](#7计数质数)
        - [8、n个数的序列，找出 0 .. n 中没有出现在序列中的那个数。](#8n个数的序列找出-0--n-中没有出现在序列中的那个数)
        - [9、给定一个整数，写一个函数来判断它是否是 3 的幂次方。](#9给定一个整数写一个函数来判断它是否是-3-的幂次方)
- [图](#图)
    - [BFS](#bfs)
- [动态规划](#动态规划-1)

<!-- /TOC -->
# 排序
## 
* 法一：时间复杂度（O(nlongn)）,空间复杂度O(n)
* 【分析】：思路：这道题先对数组进行排序，然后把数组一分为二，分为左半部分left和右半部分right，每次取左半部分的最右边的元素和右半部分最右边的元素组成一队，然后一直到结束。解法很简单，但是问题来了，为什么必须取左右部分最右的元素呢，按道理说左半部分的元素都应该比右半部分小，所以随便取就行了，但是这样是不对的，我们注意数组不是严格递增的，可能会出现等于的情况，而一旦等于的情况跨越左右半边，其他的情况就不行了。比如：4,5,5,6。

其他情况包括：

A：取左半部分最左边和右半部分最左边     4,5     5,6

B：取左半部分最右边和右半部分最左边     5,5     4,6

C：取左半部分最左边和右半部分最右边     4,6     5,5

D：取左半部分最右边和右半部分最右边     5,6     4,5

只有第四种情况满足题意，因为提议要求：A<B>C<D，前三种情况都会出现C不能小于B（可能会有等于的情况出现）的情况，具体大家分析下情况就知道了，只有第四种严格满足要求。
```java
class Solution {
    //快排，错位放置
    public void wiggleSort(int[] nums) {
        int[] temp = nums.clone();
        Arrays.sort(temp);
        int k = (nums.length+1)/2;
        int j = nums.length;
        for(int i=0;i<nums.length;i++){
            nums[i] = i%2==0?temp[--k]:temp[--j];
        }
    }
}
```
* 法二：
三向切分

寻找第 k 大的元素，这里选择第 n/2 大元素，即寻找中位数，假设它的值是 mid，可以使用 O(n) 搞定它。因为 cpp 库已经提供了 nth_element 这个函数了，就不重复造轮子了，感兴趣的同学，参考 215 题：https://leetcode-cn.com/problems/kth-largest-element-in-an-array/comments/64977
三向切分，整个数组被切分成 [------,======,+++++]，即荷兰国旗问题，参考 75 题：https://leetcode-cn.com/problems/sort-colors/comments/64938 。三向切分注意，我们需要把比 mid 大的数安排到 mid 左边，比 mid 小的数安排到 mid 右边，最后数组大概像是这样 [++++++,=====,-------].
当你掌握了上面两个基础技术后（强烈建议去做一下 75、215)。

怎么把一个三向切分好的数组变成摆数组，可以使用“索引”映射这个方法（有人称为索引改写，有人称为虚拟索引，怎么说都行，我也是从大神那里学习过来的），假设数组大小是 6

 0 1 2 3 4 5
[1 1 1 3 4 5]

[3 1 4 1 5 1]
0->1
1->3
2->5
3->0
4->2
5->4
意思是说，做映射后，把位置 0 的元素安排到位置 1 上，把位置 1 安排到位置 3 上，位置 2 安排到位置 5 上，位置 3 安排到位置 0 上，位置 4 安排到位置 2 上，而位置 5 安排到位置 4 上。这样安排后，数组就变以了摆动数组。

实际上，我们可以把三向切分的过程，和数组索引映射合并到一起完成。在程序里，我们定义映射后的数组为 a(i) = nums[2*i+1] % (n|1)，在三向切分的时候，对 a(i) 进行操作即可。

* 这个代码是错误的，错在哪不知道**************************
```java
class Solution {
    //快排，错位放置
    public void wiggleSort(int[] nums) {
        int[] temp = nums.clone();
        int mid = (temp.length+1)/2;
        bfprt(temp, 0, temp.length - 1, mid);  //返回从小到大，位于 k-1 位置的数字，就是第 k 大的数
        int k = 0;
        int j = nums.length%2 ==0?mid+1:mid;
        for(int i=0;i<nums.length;i++){
            nums[i] = i%2==0?temp[k++]:temp[j++];
        }
    }
    // 在 l,r 范围上，找到从小到大排序为 i 的数，即为第 i+1 小的数
    public static void bfprt(int[] arr, int l, int r, int i) {
        if (l == r) {
            return ;
        }
        //法一：随机选择一个数来划分
        int num = arr[ l + (int)Math.random()*(r-l+1)];  // ** 不能少了 l
        int[] p = partition(arr, l, r, num);
        if (i >= p[0] && i <= p[1]) {
          return;
        } else if (i < p[0]) {
            bfprt(arr, l, p[0] - 1, i);
        } else {
           bfprt(arr, p[1] + 1, r, i);
        }
    }

    // 根据数 num 对arr[] 上l,r范围进行划分，（荷兰国旗）
    public static int[] partition(int[] arr, int l, int r, int num){
        int less = l-1;
        int more = r+1;
        int cur =l;
        while (cur < more){
            if(arr[cur] < num){
                swap(arr, ++less, cur++);
            }else if(arr[cur] > num){
                swap(arr, --more, cur);
            }else {
                cur++;
            }
        }
        return new int[]{less+1,more-1};
    }

    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```
# LRU缓存机制 
## 法一：Java使用HashMap+双链表实现； 
一，初始化时
维护一个双向链表，其中越近使用的数据越排在后面，越久没有使用的数据排在前面。
维护一个HashMap中，以便能在O(1)的时间找到数据

二，插入时，首先确认是否有这个数据？
如果有，直接在HashMap里进行更新。
如果没有，接着判断现在HashMap中是否还有剩余空间？
如果没有剩余空间，需要将双向链表中头结点指向的数据删除，同时把这个数据从HashMap里删除。
之后将要插入的数据放到双向链表的尾部并且加入到HashMap中。

三， 获取时，先查找HashMap中是否有这个数据？ 如果有这个数据， 我们首先应该将这个数据与它的前驱和后驱断开，然后将这个数据移动到尾结点。 如果没有这个数据，返回-1。

```java
public class LRUCache {
    class Node{
        Node pre;
        Node next;
        int key;
        int value;
        public Node(int key,int val){
            this.key = key;
            this.value = val;
        }
    }
    HashMap<Integer,Node> map = new HashMap<>();
    int capacity;
    Node head;
    Node tail;
    public LRUCache(int capacity){
        this.capacity = capacity;
        head = new Node(-1,-1);
        tail = new Node(-1,-1);
        head.next = tail;
        tail.pre = head;
    }

    public int get(int key) {
        if(map.containsKey(key)){
            Node node = map.get(key);
            node.next.pre = node.pre;
            node.pre.next = node.next;
            moveToTail(node);
            return node.value;
        }
        return -1;
    }


    public void put(int key, int value) {
        if(map.containsKey(key)){//如果已经存在，就是更新，记得要移动到最后面
            Node node = map.get(key);
            node.value = value;
            node.next.pre = node.pre;
            node.pre.next = node.next;
            moveToTail(node);
            return;
        }

        //移除最近最久未访问元素
        if(map.size() == capacity){
            Node node = head.next;
            map.remove(node.key);
            head.next = node.next;
            node.next.pre = head;
        }
        //插入元素
        Node node = new Node(key,value);
        map.put(key,node);
        moveToTail(node);
    }

    private void moveToTail(Node node) {
        node.next = tail;
        node.pre = tail.pre;
        tail.pre.next = node;
        tail.pre = node;
    }

}
```
## 法二：利用LinkedHashMap 实现 LRU 算法
* 3.3  LinkedListMap 与 LRU 小结【访问标志accessOrder是决定put和get时要不要按访问顺序，removeEldestEntry方法是决定何时删除最近最久未访问节点，默认是返回false，即不会删除，若要删除即要实现LRU，你只需要重写这个方法】
    * 1、使用 LinkedHashMap 实现 LRU 的必要前提是将 accessOrder 标志位设为 true 以便开启按访问顺序排序的模式。我们可以看到，无论是 put 方法还是 get 方法，都会导致目标 Entry 成为最近访问的 Entry，因此就把该 Entry 加入到了双向链表的末尾：get 方法通过调用 recordAccess 方法来实现。
    * 2、put 方法在插入新的 Entry 时，通过createEntry 中的 addBefore 方法来实现插入链表尾部；在覆盖已有 key 的情况下，通过 recordAccess 方法来实现将更新的entry放到链表尾部。get操作也通过recordAccess 方法将该entry放到链表尾部。多次操作后，双向链表前面的 Entry 便是最近没有使用的。
    * 3、在每次put插入新的Entry时，都会根据你重写的removeEldestEntry方法来决定是否要删除最近最久未访问元素（默认返回false，你可以重写成当节点个数大于多少时返回true），这样当节点个数大于某个数时，就会删除最前面的 Entry（head后面的那个Entry），因为它就是最近最久未使用的 Entry
如下所示，笔者使用 LinkedHashMap 实现一个符合 LRU 算法的数据结构，该结构最多可以缓存 6 个元素，但元素多于 6 个时，会自动删除最近最久没有被使用的元素，如下所示：
```java
public class LRU<K,V> extends LinkedHashMap<K, V> {
 
	private static final long serialVersionUID = 1L;
	
	public LRU(int initialCapacity, float loadFactor, boolean accessOrder){
		super(initialCapacity, loadFactor, accessOrder);
	}
	
	// 重写LinkdHashMap中的removeEldestEntry方法，当LRU中元素多于6个时，删除最不经常使用的元素
	@Override
	public boolean removeEldestEntry(Map.Entry<K, V> eldest){
		if(size() > 6){
			return true;
		}
		return false;
	}
}
```
# 链表
## 判断两个链表是否相交
* 【分析】：根据题目意思 如果两个链表相交，那么相交点之后的长度是相同的

我们需要做的事情是，让两个链表从同距离末尾同等距离的位置开始遍历。这个位置只能是较短链表的头结点位置。 为此，我们必须消除两个链表的长度差

指针pA指向A链表，指针pB指向B链表，依次往后遍历
如果pA到了末尾，则pA = headB 继续遍历
如果pB到了末尾，则pB = headA 继续遍历
比较长的链表指针指向较短链表head时，长度差就消除了
如此，只需要将最短链表遍历两次即可找到位置<br>
![](https://pic.leetcode-cn.com/e86e947c8b87ac723b9c858cd3834f9a93bcc6c5e884e41117ab803d205ef662-%E7%9B%B8%E4%BA%A4%E9%93%BE%E8%A1%A8.png)

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null) return null;
        ListNode pA = headA,pB = headB;
        while (pA != pB){
            pA = pA == null ? headB:pA.next;
            pB = pB == null ? headA:pB.next;
        }
        return pA;
    }
}
```
## 在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序。
```
在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序。

示例 1:

输入: 4->2->1->3
输出: 1->2->3->4
示例 2:

输入: -1->5->3->4->0
输出: -1->0->3->4->5
```
* 【分析】：题目要求排序算法的时间复杂度为 O(n log n) ，空间复杂度为 O(n) ，直接想到 归并排序。

归并排序 是采用分治法的一种排序算法：

分，就是把数列一分二，二分四...最后分成两两一组的最小子集
治，就是把分开后的子集，一一排序后归并在一起
最后由局部有序成为全部有序，和数组的归并排序相比，链表多出的一步就是如何找到链表的中点？

```java
class Solution {
    public ListNode sortList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode slow=head,fast=head.next;
        while (fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode mid = slow;
        ListNode right = sortList(mid.next);
        mid.next = null;
        ListNode left = sortList(head);
        return merge(left,right);
    }

    private ListNode merge(ListNode left, ListNode right) {
        ListNode temp = new ListNode(0);
        ListNode cur = temp;
        while (left != null && right != null){
            if(left.val <=right.val){
                cur.next = left;
                left = left.next;
            }else{
                cur.next = right;
                right = right.next;
            }
            cur = cur.next;
        }
        cur.next = left == null?right:left;
        return temp.next;
    }
}
```
# 树
## 二叉树的序列化与反序列化
```
序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。

请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

示例: 

你可以将以下二叉树：

    1
   / \
  2   3
     / \
    4   5

序列化为 "[1,2,3,null,null,4,5]"
提示: 这与 LeetCode 目前使用的方式一致，详情请参阅 LeetCode 序列化二叉树的格式。你并非必须采取这种方式，你也可以采用其他的方法解决这个问题。

说明: 不要使用类的成员 / 全局 / 静态变量来存储状态，你的序列化和反序列化算法应该是无状态的。
```
```java
//先序
public class Codec {
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if(root == null){
            return "#,";
        }
        String s = root.val+",";
        s+=serialize(root.left);
        s+=serialize(root.right);
        return s;
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] s = data.split(",");
        Queue<String> queue = new LinkedList<>();
        for(String i : s){
            queue.add(i);
        }
        return process(queue);
    }

    private TreeNode process(Queue<String> queue) {
        String s = queue.poll();
        if(s.equals("#")){
            return null;
        }
        TreeNode node = new TreeNode(Integer.valueOf(s));
        node.left = process(queue);
        node.right = process(queue);
        return node;
    }
}

```
```java
//层序
package test;

import java.util.LinkedList;
import java.util.Queue;

public class Codec {
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        Queue<TreeNode> q = new LinkedList<>();
        StringBuffer s = new StringBuffer();
        if(root == null){
            return "[]";
        }
        q.offer(root);
        s.append("["+root.val);
        while (!q.isEmpty()){
            TreeNode cur = q.poll();
            if(cur.left !=null){
                q.offer(cur.left);
                s.append(","+cur.left.val);
            }else {
                s.append(",null");
            }
            if(cur.right !=null){
                q.offer(cur.right);
                s.append(","+cur.right.val);
            }else {
                s.append(",null");
            }
        }
//        while(s.substring(s.length()-1).equals("null")){
//            s.delete(s.length()-1,s.length());
//        }
        s.append("]");
        return s.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if(data == null||data == "[]"){
            return null;
        }
        data = data.replace("[","").replace("]","");
        String[] s = data.split(",");
        Queue<TreeNode> q = new LinkedList<>();
        int i=0;
        TreeNode root = new TreeNode(Integer.valueOf(s[i++]));
        q.offer(root);
        while (!q.isEmpty()){
            TreeNode cur = q.poll();
            if(i<s.length&&!s[i].equals("null")){
                cur.left = new TreeNode(Integer.valueOf(s[i]));
                q.offer(cur.left);
            }
            i++;
            if(i<s.length&&!s[i].equals("null")){
                cur.right = new TreeNode(Integer.valueOf(s[i]));
                q.offer(cur.right);
            }
            i++;
        }
        return root;
    }

    public static void main(String[] args) {
        Codec codec = new Codec();
        String s = new String("[1,null,2]");
        System.out.println(codec.serialize(codec.deserialize(s)));

    }
}

```
## 二叉树的最近公共祖先
```
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]
```
![(https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/binarytree.png)]
```
示例 1:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
示例 2:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
 

说明:

所有节点的值都是唯一的。
p、q 为不同节点且均存在于给定的二叉树中。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```
```java
public class Solution {  
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {  
        //发现目标节点则通过返回值标记该子树发现了某个目标结点  
        if(root == null || root == p || root == q) return root;  
        //查看左子树中是否有目标结点，没有为null  
        TreeNode left = lowestCommonAncestor(root.left, p, q);  
        //查看右子树是否有目标节点，没有为null  
        TreeNode right = lowestCommonAncestor(root.right, p, q);  
        //都不为空，说明做右子树都有目标结点，则公共祖先就是本身  
        if(left!=null&&right!=null) return root;  
        //如果发现了目标节点，则继续向上标记为该目标节点  
        return left == null ? right : left;  
    }  
}  

```

* 自己的做法，错在哪里？
```java
class Solution {
    boolean[] find ;

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        find = new boolean[2];
        return process(root,p,q);

    }

    private TreeNode process(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null){
            return null;
        }
        TreeNode leftResult = process(root.left,p,q);
        if(leftResult != null){
            return leftResult;
        }
        TreeNode rightResutl = process(root.right,p,q);
        if(rightResutl != null){
            return rightResutl;
        }
        if(root == p){
            find[0] = true;
        }
        if(root == q){
            find[1] = true;
        }
        if(find[0] && find[1]){
            return root;
        }else {
            return null;
        }
    }
}
```
# 数组
## 除自身以外数组的乘积
```
给定长度为 n 的整数数组 nums，其中 n > 1，返回输出数组 output ，其中 output[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积。

示例:

输入: [1,2,3,4]
输出: [24,12,8,6]
说明: 请不要使用除法，且在 O(n) 时间复杂度内完成此题。

进阶：
你可以在常数空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组不被视为额外空间。）
```
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] res = new int[nums.length];
        int k = 1;
        for(int i = 0; i < res.length; i++){
            res[i] = k;
            k = k * nums[i]; // 此时数组存储的是除去当前元素左边的元素乘积
        }
        k = 1;
        for(int i = res.length - 1; i >= 0; i--){
            res[i] *= k; // k为该数右边的乘积。
            k *= nums[i]; // 此时数组等于左边的 * 该数右边的。
        }
        return res;
    }
}

```
* 【分析】：乘积 = 当前数左边的乘积 * 当前数右边的乘积
## 递增的三元子序列
* 【分析】：只需遍历一遍列表，同时维护两个值： 最小值 和 次小值 。首先将两个值都定义为超大的数字，先判断最小值，因为最小值必须保持在次小值之前，再判断次小值，若在最小值和次小值之间，则更新次小值，若大于次小值，则满足条件。

注： 会发现一个问题，若出现有一个位于次小值之后的值比最小值还小，那么更新后是否就违背了“次小值必须出现在最小值之后”。事实上，这并不影响最后的结果，因为若后面出现了大于次小值的数字，则与更新前的“最小值-次小值-当前值”构成三元子序列。这里更新最小值只是为了获得更小的“最小值-次小值”二元子序列而不放过可能的结果。
3个连续递增子序列 有3个槽位，a,b,c 满足条件 a < b < c，即可 需要将合适的元素填入这3个槽位

```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        int one = Integer.MAX_VALUE;
        int two = Integer.MAX_VALUE;
        for(int n: nums){
            if(n <= one){  //*** = 不能少
                one =n ;
            }else if( n <= two){ //*** = 不能少
                two = n;
            }else {
                return true;
            }
        }
        return false;
    }
}
```
##  打乱数组
```
打乱一个没有重复元素的数组。

示例:

// 以数字集合 1, 2 和 3 初始化数组。
int[] nums = {1,2,3};
Solution solution = new Solution(nums);

// 打乱数组 [1,2,3] 并返回结果。任何 [1,2,3]的排列返回的概率应该相同。
solution.shuffle();

// 重设数组到它的初始状态[1,2,3]。
solution.reset();

// 随机返回数组[1,2,3]打乱后的结果。
solution.shuffle();
```
* 【分析】
* 乱序算法不像排序算法，结果唯一可以很容易检验，因为「乱」可以有很多种，你怎么能证明你的算法是「真的乱」呢？

所以我们面临两个问题：

什么叫做「真的乱」？
设计怎样的算法来打乱数组才能做到「真的乱」？
这种算法称为「随机乱置算法」或者「洗牌算法」。

本文分两部分，第一部分详解最常用的洗牌算法。因为该算法的细节容易出错，且存在好几种变体，虽有细微差异但都是正确的，所以本文要介绍一种简单的通用思想保证你写出正确的洗牌算法。第二部分讲解使用「蒙特卡罗方法」来检验我们的打乱结果是不是真的乱。蒙特卡罗方法的思想不难，但是实现方式也各有特点的。

一、洗牌算法：此类算法都是靠随机选取元素交换来获取随机性

* **分析洗牌算法正确性的准则：产生的结果必须有 n! 种可能，否则就是错误的。**这个很好解释，因为一个长度为 n 的数组的全排列就有 n! 种，也就是说打乱结果总共有 n! 种。算法必须能够反映这个事实，才是正确的。
```java
class Solution {

    int[] origin ,temp;
    public Solution(int[] nums) {
        origin = nums;
        temp = new int[nums.length];
        for(int i=0; i<nums.length; i++){
            temp[i] = nums[i];
        }
    }

    /** Resets the array to its original configuration and return it. */
    public int[] reset() {
        return origin;
    }

    /** Returns a random shuffling of the array. */
    public int[] shuffle() {
        for (int i=temp.length-1 ; i>=0; i--){
            int index = (int) (Math.random()*(i+1)); //****int后面的要加括号
            int t = temp[index];
            temp[index] = temp[i];
            temp[i] = t;
        }
        return temp;
    }
}
```
## 移动零
```
给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

示例:

输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
说明:

必须在原数组上操作，不能拷贝额外的数组。
尽量减少操作次数。
```
* 【分析】：思路：可以先把所有非0的元素移到前面，然后将后面的位置补0。 使用指针i，指向需要插入的下标，使用指针j指向遍历的下标。遍历一遍，如果j指向的位置为0，则i不变，j++后移；如果j指向的位置不为0，则将j位置的元素值赋值到i位置，然后i++。

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int i=0;   //i:插入位置下标 ; j:查找位置下标
        for (int j=0; j<nums.length; j++){
            if(nums[j] != 0){
                nums[i] = nums[j];
                i++;
            }
        }
        for(; i<nums.length; i++){
            nums[i] = 0;
        }
    }
}
```
## 求众数
```
给定一个大小为 n 的数组，找到其中的众数。众数是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在众数。

示例 1:

输入: [3,2,3]
输出: 3
示例 2:

输入: [2,2,1,1,1,2,2]
输出: 2
```
* 【分析】：从第一个数开始count=1，遇到相同的就加1，遇到不同的就减1，减到0就重新换个数（作为众数的候选者）开始计数，总能找到最多的那个【最后剩下的一定是众数，因为>n/2，我们的算法是不相等的两个数两两抵消，不是众数的数一定会被抵消完，最后剩下的一定是众数】
```java
public class Solution {
    public int majorityElement(int[] nums) {
        int cand = nums[0];  //候选人
        int count = 1;
        for (int i =1 ;i<nums.length; i++){
            if(count != 0){
                if(cand == nums[i]){
                    count++;
                }else {
                    count--;
                }
            }else {
                cand = nums[i];
                count = 1;
            }
        }
        return cand;
    }
}
```
## 乘积最大子序列
```
给定一个整数数组 nums ，找出一个序列中乘积最大的连续子序列（该序列至少包含一个数）。

示例 1:

输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
示例 2:

输入: [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
```
* 【分析】：遍历数组时计算当前最大值，不断更新；令imax为当前最大值，则当前最大值为 imax = max(imax * nums[i], nums[i])；由于存在负数，那么会导致最大的变最小的，最小的变最大的。因此还需要维护当前最小值imin，imin = min(imin * nums[i], nums[i])；当负数出现时则imax与imin进行交换再进行下一步计算；前 n 位的乘积最大子序列分以下3种情况：

    * 1. 前n - 1位的乘积最小子序列的乘积 * A[n]最大

    * 2. 前n - 1位的乘积最大子序列的乘积 * A[n]最大

    * 3. 前n - 1位的乘积最大子序列和乘积最小子序列的乘积 * A[n]都不是最大，而A[n]本身最大。
```java
//动态规划版本
class Solution {
    public int maxProduct(int[] nums) {
        int max = Integer.MIN_VALUE;
        int cur = 1,imax=1,imin =1;
        for(int i=0;i <nums.length; i++){
            cur = nums[i];
            if(cur < 0){  //当当前数<0时，当前最大值在imin和cur之间产生。
                int temp = imax;
                imax = imin;
                imin = temp;
            }
            imax = Math.max(imax*cur,cur);
            imin = Math.min(imin*cur,cur);
            max = Math.max(max, imax);
        }
        return max;
    }
}
```


```java
//暴力版本

class Solution {
    public int maxProduct(int[] nums) {
        int max = Integer.MIN_VALUE;
        for(int i=0;i <nums.length; i++){
            int cur = nums[i];
            max = Math.max(cur,max);
            for(int j=i+1; j<nums.length; j++){
                cur *=nums[j];
                max = Math.max(cur,max);
            }
        }
        return max;
    }
}
```
# 堆、栈、队列

##  扁平化嵌套列表迭代器
```

给定一个嵌套的整型列表。设计一个迭代器，使其能够遍历这个整型列表中的所有整数。

列表中的项或者为一个整数，或者是另一个列表。

示例 1:

输入: [[1,1],2,[1,1]]
输出: [1,1,2,1,1]
解释: 通过重复调用 next 直到 hasNext 返回false，next 返回的元素的顺序应该是: [1,1,2,1,1]。
示例 2:

输入: [1,[4,[6]]]
输出: [1,4,6]
解释: 通过重复调用 next 直到 hasNext 返回false，next 返回的元素的顺序应该是: [1,4,6]。
```
* 【分析】：利用一个栈来实现。首先将所有的元素倒叙压入栈中（倒叙入栈，输出顺序才是正序）（也可以用队列，并没有什么区别），由于迭代过程是先判断hasNext然后再next获取值，这样我们就可以在hasNest中进行处理：
    * 首先判断栈顶元素是否为一个整数，若是，则证明next可以直接拿值，返回true；
    * 若不是，则是一个嵌套，我们将该元素弹出并转化成一个列表，将列表中元素倒叙入栈。
    * 重复上述过程直到栈顶元素是一个值为止。

```java

/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class NestedIterator implements Iterator<Integer> {
    Stack<NestedInteger> stack = new Stack<>();
    public NestedIterator(List<NestedInteger> nestedList) {
        for(int i=nestedList.size()-1; i>=0; i--){
            stack.push(nestedList.get(i));
        }
    }

    @Override
    public Integer next() {
        return stack.pop().getInteger();
    }

    @Override
    public boolean hasNext() {
        while (!stack.isEmpty()){
            NestedInteger n = stack.peek();
            if(n.isInteger()){
                return true;
            }
            List<NestedInteger> list = stack.pop().getList();
            for(int i = list.size()-1; i>=0; i--){
                stack.push(list.get(i));
            }
        }
        return false;
    }
}
```

##  基本计算器 II
```
实现一个基本的计算器来计算一个简单的字符串表达式的值。

字符串表达式仅包含非负整数，+， - ，*，/ 四种运算符和空格  。 整数除法仅保留整数部分。

示例 1:

输入: "3+2*2"
输出: 7
示例 2:

输入: " 3/2 "
输出: 1
示例 3:

输入: " 3+5 / 2 "
输出: 5
说明：

你可以假设所给定的表达式都是有效的。
请不要使用内置的库函数 eval。
```

* 【分析】：一般需要符号栈、数据栈，两个。但是，看到网上一个写的不错的算法，只用了一个数据栈。主要思想如下：

将减法转化为加法（取相反数）

由于乘除法优先级高，直接计算

整数不仅一位，会>10

表达式中没有括号

注意：

加减乘除空格的ASCII码都小于'0'，ASCII对照表如下：http://tool.oschina.net/commons?type=4

先做减法，避免int溢出

char类型，不能使用switch

```java
class Solution {
    static int i=0;
    public int calculate(String s) {
        char[] chars = s.toCharArray();
        Stack<Integer> opo = new Stack<>();
        i=0;  //*************一定要置为0哈，不然测试数据公用这个 i 出现问题。
        for (; i<chars.length; ){
            char c= chars[i];
            if(c == ' '){
                i++;
            }else if(c >= '0' && c<='9'){
                opo.push(getDigit(chars));
            }else if(c == '+' || c == '-'){
                i++;
                int n2 = getDigit(chars);
                if(c =='+'){
                    opo.push(n2);
                }else {
                    opo.push(-n2);
                }
            }else if(c == '*' || c=='/'){
                i++;
                int n1 = opo.pop();
                int n2 = getDigit(chars);
                if(c == '*'){
                    opo.push(n1*n2);
                }else {
                    opo.push(n1/n2);
                }
            }
        }
        int res =0;
        while (!opo.isEmpty()){
            res += opo.pop();
        }
        return res;
    }

    public int getDigit(char[] chars){  //取得 i 位置后面跟着的数字
        char ch2 = chars[i];
        while (ch2 == ' '){
            ch2 = chars[++i];
        }
        int n2 = 0;
        while (i<chars.length){
            ch2 = chars[i];
            if(ch2 >='0' && ch2<='9'){
                n2 = ch2-'0' + n2*10;
                i++;
            }else {
                break;
            }
        }
        return n2;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.calculate("3/2"));
    }
}

```
## 数组中的第K个最大元素
```
在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

示例 1:

输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
示例 2:

输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
说明:

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。
```

* 用快排的改进算法
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        QuickSort(nums,0,nums.length-1,k-1);
        return nums[k-1];
    }

    private void QuickSort(int[] nums, int l, int r, int k) {
        if(l<r){
            int p = partition(nums,l,r);
            if(p == k ){
                return;
            }else if(p < k){
                QuickSort(nums,p+1,r,k);
            }else {
                QuickSort(nums,l,p-1,k);
            }
        }
    }

    private int partition(int[] nums, int l, int r) {
        int pivot = nums[l];
        while (l < r){
            while (l<r && nums[r] <= pivot){
                r--;
            }
            nums[l] = nums[r];
            while (l<r && nums[l] >= pivot){
                l++;
            }
            nums[r] = nums[l];
        }
        nums[l] = pivot;
        return l;
    }
}
```

# 字符串
## 验证回文串
```
给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

说明：本题中，我们将空字符串定义为有效的回文串。

示例 1:

输入: "A man, a plan, a canal: Panama"
输出: true
示例 2:

输入: "race a car"
输出: false
```
* 【分析】：符号这些是不包含的哈
```
class Solution {
    public boolean isPalindrome(String s) {
        if(s == null){
            return false;
        }
        if(s.length() == 0){
            return true;
        }
        int i =0;
        int j=s.length()-1;
        while (i<j){
            //isLetterOrDigit方法用来判定是不是字母和数字
            while (i<j && !Character.isLetterOrDigit(s.charAt(i)))i++;
            while (i<j && !Character.isLetterOrDigit(s.charAt(j)))j--;
            if(Character.toLowerCase(s.charAt(i)) != Character.toLowerCase(s.charAt(j)))return false;
            i++;
            j--;
        }
        return true;
    }
}
```

## 分割回文串
```
给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。

返回 s 所有可能的分割方案。

示例:

输入: "aab"
输出:
[
  ["aa","b"],
  ["a","a","b"]
]
```
* 代码
```java
class Solution {
    List<List<String>> res = new ArrayList<>();
    List<String> temp = new ArrayList<>();
    public List<List<String>> partition(String s) {
        if(s == null){
            return null;
        }
        if(s.length() ==0){
            return res;
        }
        process(s,0);  //产生从某个位置开始分割回文串的所有可能结果
        return res;
    }

    private void process(String s, int i) {
        if(i == s.length()){ //这种分割方式是可行的一种
            res.add(new ArrayList<>(temp));
            return;
        }
        for(int j=i; j<s.length(); j++){
            if(isPalindrom(s,i,j)){
                temp.add(s.substring(i,j+1)); //能从这里分割，最后能不能成，就看之后能不能成功分割了
                process(s,j+1);
                temp.remove(temp.size()-1); //记得要移除
            }
        }
    }

    private boolean isPalindrom(String s, int i, int j) {
        while (i<j){
            if(s.charAt(i) != s.charAt(j)){
                return false;
            }
            i++;
            j--;
        }
        return true;
    }
}
```
##3、单词拆分
```
给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，判定 s 是否可以被空格拆分为一个或多个在字典中出现的单词。

说明：

拆分时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。
示例 1：

输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。
示例 2：

输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。
     注意你可以重复使用字典中的单词。
示例 3：

输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```
* 【分析】：判断以位置 i 开始能不能成功拆分。
* 递归版本
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        if(wordDict == null){
            return false;
        }
        if(s == null || s.length() == 0){
            return true;
        }
        Set<String> words = new HashSet<>(wordDict);
        return process(s,0,words);
    }

    private boolean process(String s, int index, Set<String> words) {
        if(index == s.length()){
            return true;
        }
        for (int j = index; j<s.length(); j++){
            String sub = s.substring(index,j+1);
            if(words.contains(sub)){
                if(process(s,j+1,words)){
                    return true;
                }
            }
        }
        return false;
    }
}
```
* 动态规划版本
```java
   public boolean wordBreak(String s, List<String> wordDict) {
        if(wordDict == null){
            return false;
        }
        if(s == null || s.length() == 0){
            return true;
        }
        Set<String> words = new HashSet<>(wordDict);
        boolean[] dp = new boolean[s.length()+1];
        dp[s.length()] = true;
        for(int index = s.length()-1; index>=0; index--){
            for (int j = index; j<s.length(); j++){
                String sub = s.substring(index,j+1);
                if(words.contains(sub)){
                    if(dp[j+1] == true) {
                        dp[index] = true; 
                        break;
                    }
                }
            }
        }

        return dp[0];
    }
```


## 2、单词拆分2
```java
给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，在字符串中增加空格来构建一个句子，使得句子中所有的单词都在词典中。返回所有这些可能的句子。

说明：

分隔时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。
示例 1：

输入:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
输出:
[
  "cats and dog",
  "cat sand dog"
]
示例 2：

输入:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
输出:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
解释: 注意你可以重复使用字典中的单词。
示例 3：

输入:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
输出:
[]
```
* 方法一，直接递归【回溯】：出现超时
    * 一些概念：https://blog.csdn.net/u014772862/article/details/51789015
        * 从问题的某一种可能出发, 搜索从这种情况出发所能达到的所有可能, 当这一条路走到” 尽头 “的时候, 再倒回出发点, 从另一个可能出发, 继续搜索. 这种不断” 回溯 “寻找解的方法, 称作” 回溯法 “。
        * 递归是一种算法结构，递归会出现在子程序中自己调用自己或间接地自己调用自己
        * 回溯是一种算法思想，可以用递归实现。通俗点讲回溯就是一种试探，类似于穷举，但回溯有“剪枝”功能，比如求和问题。给定7个数字，1 2 3 4 5 6 7求和等于7的组合，从小到大搜索，选择1+2+3+4 =10>7，已经超过了7，之后的5 6 7就没必要在继续了，这就是一种搜索过程的优化
```java
class Solution {
    List<String> res = new ArrayList<>();
    StringBuffer temp = new StringBuffer();
    public List<String> wordBreak(String s, List<String> wordDict) {
        if(wordDict == null){
            return res;
        }
        if(s == null || s.length() == 0){
            return res;
        }
        Set<String> words = new HashSet<>(wordDict);
        StringBuffer temp = new StringBuffer();
        process(s,0,words);
        return res;
    }

    private void process(String s, int index, Set<String> words) {
        if(index == s.length()){
            String s1 = temp.toString().trim();
            res.add(s1);
        }
        for (int j = index; j<s.length(); j++){
            String sub = s.substring(index,j+1);
            if(words.contains(sub)) {
                sub +=" ";
                temp.append(sub);
                process(s, j + 1, words);
                temp.delete(temp.length()-sub.length(),temp.length());
            }
        }
    }
}
```

* 方法二：利用动态规划先判断是否能成功拆分，能成功拆分才拆分
```java
class Solution {
    List<String> res = new ArrayList<>();
    StringBuffer temp = new StringBuffer();
    public List<String> wordBreak(String s, List<String> wordDict) {
        Set<String> words = new HashSet<>(wordDict);
        boolean[] dp = new boolean[s.length()+1];
        dp[s.length()] = true;
        for(int index = s.length()-1; index>=0; index--){
            for (int j = index; j<s.length(); j++){
                String sub = s.substring(index,j+1);
                if(words.contains(sub)){
                    if(dp[j+1] == true) {
                        dp[index] = true;
                        break;
                    }
                }
            }
        }
        if(!dp[0]){
            return res;
        }
        StringBuffer temp = new StringBuffer();
        process(s,0,words);
        return res;
    }

    private void process(String s, int index, Set<String> words) {
        if(index == s.length()){
            String s1 = temp.toString().trim();
            res.add(s1);
        }
        for (int j = index; j<s.length(); j++){
            String sub = s.substring(index,j+1);
            if(words.contains(sub)) {
                sub +=" ";
                temp.append(sub);
                process(s, j + 1, words);
                temp.delete(temp.length()-sub.length(),temp.length());
            }
        }
    }

}


```

## 3、212. 单词搜索 II
```
给定一个二维网格 board 和一个字典中的单词列表 words，找出所有同时在二维网格和字典中出现的单词。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。

示例:

输入: 
words = ["oath","pea","eat","rain"] and board =
[
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]

输出: ["eat","oath"]
说明:
你可以假设所有输入都由小写字母 a-z 组成。

提示:

你需要优化回溯算法以通过更大数据量的测试。你能否早点停止回溯？
如果当前单词不存在于所有单词的前缀中，则可以立即停止回溯。什么样的数据结构可以有效地执行这样的操作？散列表是否可行？为什么？ 前缀树如何？如果你想学习如何实现一个基本的前缀树，请先查看这个问题： 实现Trie（前缀树）。
```

* 【分析】：使用字典树来进行查询，提高查询效率。把String[] words 全部加入字典树，用一个boolean二维数组flag来记录是否被访问过的数据。在DFS遍历时，判断i、j是否越界，判断之前是否被访问过、判断前缀是否在字典树内 作为递归终止条件。

0、使用Set来去掉重复结果集

1、把String[] words 全部加入到字典树

2、遍历board二维数组，进行DFS。

3、编写DFS
 
递归返回：**判断i、j是否越界，判断之前是否被访问过、判断前缀是否在字典树内**

加入结果集，把flag状态标志修改，然后递归，上下左右，然后回溯，修改flag的标志

```java
class Solution {

    Set<String> res = new HashSet<>();
    public List<String> findWords(char[][] board, String[] words) {
        Trie trie = new Trie();
        for(String word : words){
            trie.insert(word);
        }
        int m = board.length;
        int n = board[0].length;
        boolean[][] flag = new boolean[m][n];
        for (int i=0; i< m; i++){
            for (int j=0; j<n; j++){
                dfs(board,i,j,"",trie,flag);
            }
        }
        return new ArrayList<>(res);
    }

    private void dfs(char[][] board, int i, int j, String s, Trie trie, boolean[][] flag) {
        if(i<0 || i>=board.length || j<0 || j>=board[0].length){
            return;
        }
        if(flag[i][j] == true){ //说明已经该位置已经被之前的访问过了，就没必要访问了
            return;
        }
        s +=board[i][j];
        if(!trie.startsWith(s)){
           return;
        }
        flag[i][j] = true; //我已经成为前缀的一部分了，所以必须把我标记为访问过
        if(trie.search(s)){
            res.add(s);
        }
        dfs(board,i+1,j,s,trie,flag);
        dfs(board,i-1,j,s,trie,flag);
        dfs(board,i,j-1,s,trie,flag);
        dfs(board,i,j+1,s,trie,flag);
        flag[i][j] = false; //包含我的遍历已经结束了，我不再是访问过的点了【我的上一层没有我的存在】
    }

}

//字典树
class Trie {

    Node root;
    static  class Node{
        int end;
        Node[] nexts;
        public Node(){
            nexts=new Node[26];
            end=0;
        }
    }
    /** Initialize your data structure here. */
    public Trie() {
        root = new Node();
    }

    /** Inserts a word into the trie. */
    public void insert(String word) {
        if(word == null){
            return;
        }
        Node node = root;
        for(int i=0; i<word.length(); i++){
            int index = word.charAt(i)-'a';
            if(node.nexts[index] == null){
                node.nexts[index] = new Node();
            }
            node = node.nexts[index];
        }
        node.end++;
    }

    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        if(word ==null){
            return false;
        }
        Node node = root;
        for(int i=0; i<word.length(); i++){
            int index = word.charAt(i)-'a';
            if(node.nexts[index] == null){
                return false;
            }
            node = node.nexts[index];
        }
        if(node.end !=0){
            return true;
        }
        return false;
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        if(prefix ==null){
            return false;
        }
        Node node = root;
        for(int i=0; i<prefix.length(); i++){
            int index = prefix.charAt(i)-'a';
            if(node.nexts[index] == null){
                return false;
            }
            node = node.nexts[index];
        }
        return true;
    }
}
```


# 动态规划
## 1、至少有K个重复字符的最长子串
```
找到给定字符串（由小写字符组成）中的最长子串 T ， 要求 T 中的每一字符出现次数都不少于 k 。输出 T 的长度。

示例 1:

输入:
s = "aaabb", k = 3

输出:
3

最长子串为 "aaa" ，其中 'a' 重复了 3 次。
示例 2:

输入:
s = "ababbc", k = 2

输出:
5

最长子串为 "ababb" ，其中 'a' 重复了 2 次， 'b' 重复了 3 次。
```
* 【分析】：首先找到字符串中数量小于k的字符的位置，然后根据这些位置将字符串划分为N个子串，递归求解【记得要处理最后一片】
```java
class Solution {
    public int longestSubstring(String s, int k) {
        if(s.length()<k){
            return 0;
        }
        if (k == 1){
            return s.length();
        }
        return longestSubstring(s,0,s.length()-1,k);
    }

    private int longestSubstring(String s, int left, int right, int k) {
        if(right-left+1 < k){
            return 0;
        }
        Map<Character,Integer> map = new HashMap<>();
        for(int i=left; i<=right;i++){
            char c = s.charAt(i);
            if(map.containsKey(c)){
                map.put(c,map.get(c)+1);
            }else {
                map.put(c,1);
            }
        }

        boolean isSplit = false;
        int max = 0;
        for(int i=left ; i<= right ;i++){
            char c = s.charAt(i);
            if(map.get(c) < k){ //要从这个字符拆分，需要分片
                isSplit = true;
                max = Math.max(max,longestSubstring(s,left,i-1,k));
                left = i+1;
            }else if(i == right && isSplit == true){ //用于处理拆分后的最后一片
                //必须加上后面一个条件，因为整个字符串都满足是一种basecase要直接返回结果的，isSplit==false,
                //如果你不加这个条件，那么basecase在下方，永远不能返回，你会一直在这个函数里
                max = Math.max(max,longestSubstring(s,left,i,k));
            }
        }
        if(isSplit == false){ //整个字符串都满足，没有经历分片
            return right-left+1;
        }
        return max;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.longestSubstring("bbaaacbd",3));
    }
}
```
## 2、二叉树中的最大路径和
```
给定一个非空二叉树，返回其最大路径和。

本题中，路径被定义为一条从树中任意节点出发，达到任意节点的序列。该路径至少包含一个节点，且不一定经过根节点。

示例 1:

输入: [1,2,3]

       1
      / \
     2   3

输出: 6
示例 2:

输入: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

输出: 42
```

* 【分析】：总结树形dp：（问自己求解流程是否能定成以每一个节点为头的答案，且答案就在其中）思路：小树计算完，再算父亲树。 1、考察以任意一个节点为头的子树的答案，分析其分析可能性-> 2、根据可能性列信息全集，定下返回值结构（我要我的子树返回给我什么信息）。-> 3、整合出自己的信息，向上返回。【basekey要单独考虑一下，作为最简单的情况，要给父返回啥，不至于让他干扰。】
    * 1、分析可能性：
        * 不含我：
            * 最大路径和只来自左树：我需要左子树返回最大路径和
            * 最大路径和只来自右树：我需要右子树返回最大路径和
        * 含我：
            * 返回包含我的最大路径和：
                *  我需要：左子树含左节点的单边最大路径和 及  右子树含右节点的单边最大路径和（必须是单边，即左子树里面没有任何节点同时含有左右子树，因为这样就不是一条路径了，无法从起点出发遍历完所有路径上的点）
       * 综合含我和不含我的最大值，就是我的最大路径和，同时我也需要返回包含我的单边最大路径和
```java
package test;

class Solution {
  static class ReturnData{
       int maxSum; //包括左子树最大，右子树最大值，包含自己的和左右子树的最大值
       int hasSelfSumSingleSide; //包含自己的单边（即我下面没有任何一个节点包含左右子树）的最大值
       public ReturnData(int maxSum,int hasSelfSum){
           this.maxSum = maxSum;
           this.hasSelfSumSingleSide = hasSelfSum;
       }
  }
  
    public int maxPathSum(TreeNode root) {
       if(root == null){
           return 0;
       }
       return process(root).maxSum;
    }
    

    private ReturnData process(TreeNode head) {
       if(head == null){ //因为可能整棵树都是负数，那么如果null是0，那么会导致在没有节点的情况下值最大
           return new ReturnData(Integer.MIN_VALUE,Integer.MIN_VALUE);
       }
       ReturnData leftReturn = process(head.left);
       ReturnData rightReturn = process(head.right);
       
       //包含自己的和左右子树的最大值
       int hasSelfSumDoubleSide= head.val; //*******
       if(leftReturn.hasSelfSumSingleSide >0){
           hasSelfSumDoubleSide+=leftReturn.hasSelfSumSingleSide;
       }
       if(rightReturn.hasSelfSumSingleSide >0){
           hasSelfSumDoubleSide+=rightReturn.hasSelfSumSingleSide;
       }
       int maxSum = Math.max(Math.max(leftReturn.maxSum,rightReturn.maxSum),hasSelfSumDoubleSide);

       //包含自己的单边的最大值
       int hasSelfSumSingleSide = head.val; //******
       int maxSingleSide= Math.max(leftReturn.hasSelfSumSingleSide,rightReturn.hasSelfSumSingleSide);
       if(maxSingleSide>0){
           hasSelfSumSingleSide +=maxSingleSide;
       }

       return new ReturnData(maxSum,hasSelfSumSingleSide);
    }
}
```     
## 3、最长连续序列
```
给定一个未排序的整数数组，找出最长连续序列的长度。

要求算法的时间复杂度为 O(n)。

示例:

输入: [100, 4, 200, 1, 3, 2]
输出: 4
解释: 最长连续序列是 [1, 2, 3, 4]。它的长度为 4。
```
* 【分析】：题目要求 O(n) 复杂度。

    * 用map保存数存在的区间的长度。 用一个区间头尾两个数来存区间的长度。 因为不在map中的数只会取头尾，只要根据前后两个数是否存在分情况处理就好。
    * 若数已在哈希表中：跳过不做处理
    * 若是新数加入：
        * 取出其左右相邻数已有的连续区间长度 left 和 right
        * 计算当前数的区间长度为：cur_length = left + right + 1
        * 根据 cur_length 更新最大长度 max_length 的值
        * 更新区间两端点的长度值
```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Map<Integer,Integer> map = new HashMap<>();
        int max = 0;
        for(Integer num : nums){
            if(map.containsKey(num)){ //出现重复的了
                continue;
            }
            int left = map.containsKey(num-1)?map.get(num-1):0;
            int right = map.containsKey(num+1)?map.get(num+1):0;
            int count = left + right + 1;
            max = Math.max(max,count);
            map.put(num,count);  //处理完一个数后要把该数放入map中***
            if(left > 0){
                map.put(num-left,count);
            }
            if(right > 0){
                map.put(num+right,count);
            }
        }
        return max;
    }
}
```

## 4、打家劫舍
```
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你在不触动警报装置的情况下，能够偷窃到的最高金额。

示例 1:

输入: [1,2,3,1]
输出: 4
解释: 偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
示例 2:

输入: [2,7,9,3,1]
输出: 12
解释: 偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```
* 【分析】：以 i 位置开始的最大金额
    * 要 i : 只能从 i+2 位置求最大金额
    * 不要 i ：就可以从 i+1 位置求最大金额
* 递归方法
```java
class Solution {
    public int rob(int[] nums) {
        if(nums == null || nums.length == 0){
            return 0;
    }
    return rob(nums,0);
}

    private int rob(int[] nums, int i) {
        if(i >= nums.length){
            return 0;
        }
        if(i == nums.length-1){
            return nums[i];
        }
        int max = 0;
        max = Math.max(max,nums[i]+rob(nums,i+2));// 含有我这个数
        max = Math.max(max,rob(nums,i+1));
        return max;
    }
}
```
## 5、完全平方数

* 递归求解：定义一个函数f(n)表示我们要求的解。f(n)的求解过程为：
    * f(n) = 1 + min{
    *    f(n-1^2), f(n-2^2), f(n-3^2), f(n-4^2), ... , f(n-k^2) //(k为满足k^2&lt;=n的最大的k)
    *     }
```java
class Solution {
   public int numSquares(int n) {
        if(n <= 3){
            return n;
        }
        int min = Integer.MAX_VALUE;
        for(int i=1; i*i <= n; i++){   //*** 因为一个完全平方数是可以重复利用的，这里的 n 是下面动态规划的 j 
            min = Math.min(min,1+numSquares(n-i*i));
        }
        return min;
    }
}
```
* 转为动态规划
```java
class Solution {
    public int numSquares(int n) {
        if(n <= 3){
            return n;
        }
        int[] dp = new int[n+1];
        for(int i =0; i<=3 ; i++){
            dp[i] = i;  //dp[0]=0,dp[1]=1,..dp[3]=3
        }
        for(int j = 4; j<=n; j++){
            int min = Integer.MAX_VALUE;
            for (int i = 1; i*i<=j; i++){  //****记住，我们的变量是 j ，n是常量
                min = Math.min(min,1+dp[j-i*i]);
            }
            dp[j] = min;
        }
        return dp[n];
    }
}
```
## 6、 最长上升子序列
```
给定一个无序的整数数组，找到其中最长上升子序列的长度。

示例:

输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
说明:

可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
你算法的时间复杂度应该为 O(n2) 。
进阶: 你能将算法的时间复杂度降低到 O(n log n) 吗?
```
* 【分析】：用dp[i] 来表示以 i 位置结尾的最长上升子序列的长度，那么我们就扫描i之前的元素，如果前面的元素比nums[i]小，我们就让该位置的长度=前面最大的最长上升子序列的长度加1，即“dp[i]=max{dp[j]+1}( 0< j < i, nums[j] < nums[i] )” 。需要注意的是必须是前面的最大的最长上升子序列的长度加1 ！！！【并不是第一个比 nums[i] 小的 j 位置结尾的上升子序列 ！！！！ 】
```java

//O(N^2)
 //【关键】将 dp 数组定义为：以 nums[i] 结尾的最长上升子序列的长度
 // 那么题目要求的，就是这个 dp 数组中的最大者
 // 以数组  [10, 9, 2, 5, 3, 7, 101, 18] 为例：
 // dp 的值： 1  1  1  2  2  3  4    4
// 注意实现细节。
class Solution {
    public int lengthOfLIS(int[] nums) {
        if(nums == null || nums.length == 0){
            return 0;
        }
       int[] dp = new int[nums.length];
        Arrays.fill(dp,1);  //***  // 如果只有 1 个元素，那么这个元素自己就构成了最长上升子序列，所以设置为 1 是合理的
        int max =1;
        for(int i = 0 ; i<dp.length; i++){
            for (int j=0; j<i; j++){  // 找出比当前元素小的哪些元素的最小值****
                if(nums[i] > nums[j]){  //说明之前以 j 结尾的序列可能与 i 组成以 i 结尾的最长
                    dp[i] = Math.max(dp[i],dp[j]+1);
                }
            }
            max = Math.max(dp[i],max);
        }
        return max;
    }
}
```
* O(N*logN) 的算法：二分：实质就是 相同长度的上升子序列，其数值更小的 在未来组成更大的上升子序列的可能性更大【tail即dp】
![](https://liweiwei1419.github.io/images/leetcode-solution/300-1.jpg)
![](https://liweiwei1419.github.io/images/leetcode-solution/300-2.jpg)
![](https://liweiwei1419.github.io/images/leetcode-solution/300-3.jpg)

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        /**
        dp[i]: 所有长度为i+1的递增子序列中, 最小的那个序列尾数.
        由定义知dp数组必然是一个递增数组, 可以用 maxL 来表示最长递增子序列的长度. 
        对数组进行迭代, 依次判断每个数num将其插入dp数组相应的位置:
        1. num > dp[maxL], 表示num比所有已知递增序列的尾数都大, 将num添加入dp
           数组尾部, 并将最长递增序列长度maxL加1
        2. dp[i-1] < num <= dp[i], 只更新相应的dp[i]
        **/
        int maxL = 0;
        int[] dp = new int[nums.length];
        for (int num : nums) {
            // 二分法查找, 也可以调用库函数如binary_search
            int lo = 0, hi = maxL - 1;
            while (lo <= hi) {  //找第一个 >= num的数，用num替换它
                int mid = lo + ((hi - lo) >> 1);   //*** 一定要加括号，你要被坑多少次啊，猪
                if (dp[mid] >= num) {
                    if (mid == 0 || dp[mid - 1] < num) {
                        dp[mid] = num;
                        break;
                    } else {
                        hi = mid - 1;
                    }
                } else {
                    lo = mid + 1;
                }
            }

            //说明没找到
            if (lo == maxL) { //说明num 比dp里的数都大，则最长序列长度 +1
                dp[lo] = num;
                maxL++;
            }
        }
        return maxL;
    }
}
```


* 错误解法：错误的认为 i 位置的值是第一个比 nums[i] 小的 j 位置结尾的上升子序列+1
```
class Solution {
    ////思路是错的**********************************
    public int lengthOfLIS(int[] nums) {
        if(nums == null || nums.length == 0){
            return 0;
        }
        return lengthOfLIS(nums,0,Integer.MIN_VALUE);
    }

    private int lengthOfLIS(int[] nums, int i, int preNum) {
        if(i == nums.length){
            return 0;
        }
        if(nums[i] < preNum){
            return Math.max(1+lengthOfLIS(nums,i+1,nums[i]),
                    lengthOfLIS(nums,i+1,preNum));
        }else {
            return lengthOfLIS(nums,i+1,preNum);
            //***种方法相当于认为前面离我最近的比我小的数 能和我组成最长序列，这是错误的认识
        }
    }
}
```

## 
* 自己的解法
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if(coins == null || coins.length == 0 || amount<0){
            return -1;
        }
       int[] dp = new int[amount+1];
        dp[0] = 0;
        for (int i = 1; i<amount+1; i++){
            int min = Integer.MAX_VALUE;
            for(int coin: coins){
                if(i-coin >= 0){
                    min = Math.min(min,dp[i-coin]);
                }
            }
            if(min == Integer.MAX_VALUE){
                dp[i] = Integer.MAX_VALUE;
            }else {
                dp[i] = min+1;
            }
        }
        if(dp[amount] == Integer.MAX_VALUE){
            return -1;
        }
        return dp[amount];
    }

}
```

* 别人的解法【没看】
```java
public int coinChange(int[] coins, int amount) {
    	if(coins.length == 0)
            return -1;
    	
    	//声明一个amount+1长度的数组dp，代表各个价值的钱包，第0个钱包可以容纳的总价值为0，其它全部初始化为无穷大
    	//dp[j]代表当钱包的总价值为j时，所需要的最少硬币的个数
    	int[] dp = new int[amount+1];
    	Arrays.fill(dp,1,dp.length,Integer.MAX_VALUE);
    	
    	//i代表可以使用的硬币索引，i=2代表只在第0个，第1个，第2个这三个硬币中选择硬币
    	for (int i = 0; i < coins.length; i++) {
    		/**
    		 * 	当外层循环执行一次以后，说明在只使用前i-1个硬币的情况下，各个钱包的最少硬币个数已经得到，
    		 * 		有些钱包的值还是无穷大，说明在仅使用前i-1个硬币的情况下，不能凑出钱包的价值
    		 * 	现在开始再放入第i个硬币，要想放如w[i]，钱包的价值必须满足j>=w[i]，所以在开始放入第i个硬币时，j从w[i]开始
    		 */
		for (int j = coins[i]; j <= amount; j++) {
			/**
			 * 	如果钱包当前的价值j仅能允许放入一个w[i]，那么就要进行权衡，以获得更少的硬币数
			 * 		如果放入0个：此时钱包里面硬币的个数保持不变： v0=dp[j]
			 * 		如果放入1个：此时钱包里面硬币的个数为：		v1=dp[j-coins[i]]+1
			 * 		 【前提是dp[j-coins[i]]必须有值，如果dp[j-coins[i]]是无穷大，说明无法凑出j-coins[i]价值的钱包，
			 * 	              那么把w[i]放进去以后，自然也凑不出dp[j]的钱包】
			 * 	所以，此时当钱包价值为j时，里面的硬币数目为 dp[j]=min{v0,v1}
			 * 	如果钱包当前价值j能够放入2个w[i]，就要再进行一次权衡
			 * 		如果不放人第2个w[i]，此时钱包里面硬币数目为，v1=dp[j]=min{v0,v1}
			 * 		如果放入第2个w[i],  此时钱包里面硬币数目为，v2=dp[j-coins[i]]+1
			 * 	所以，当钱包的价值为j时，里面的硬币数目为dp[j]=min{v1,v2}=min{v0,v1,v2}
			 * 	钱包价值j能允许放入3个，4个.........w[i]，不断更新dp[j]，最后得到在仅使用前i个硬币的时候，每个钱包里的最少硬币数目
			 */
			if(dp[j-coins[i]] != Integer.MAX_VALUE) {
				dp[j] = Math.min(dp[j], dp[j-coins[i]]+1);
			}
		}
	}
    	if(dp[amount] != Integer.MAX_VALUE)
    		return dp[amount];
    	return -1;
    }
```
# 数学和位运算
## 1、直线上最多的点数
```
给定一个二维平面，平面上有 n 个点，求最多有多少个点在同一条直线上。

示例 1:

输入: [[1,1],[2,2],[3,3]]
输出: 3
解释:
^
|
|        o
|     o
|  o  
+------------->
0  1  2  3  4
示例 2:

输入: [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
输出: 4
解释:
^
|
|  o
|     o        o
|        o
|  o        o
+------------------->
0  1  2  3  4  5  6
```
* 【分析】：除数为0；重复点；double(3/6) = 0.0 改为 double(3/6.0) = 0.5

## 2、分数到小数
```
分数到小数
给定两个整数，分别表示分数的分子 numerator 和分母 denominator，以字符串形式返回小数。

如果小数部分为循环小数，则将循环的部分括在括号内。

示例 1:

输入: numerator = 1, denominator = 2
输出: "0.5"
示例 2:

输入: numerator = 2, denominator = 1
输出: "2"
示例 3:

输入: numerator = 2, denominator = 3
输出: "0.(6)"
```
```java
class Solution {
    public String fractionToDecimal(int numerator, int denominator) {
        if(numerator == 0){  //必须有，因为，可能除数为负数，那么会导致sign不准
            return "0";
        }
        int sign1 = numerator>0?1:-1;
        int sign2 = denominator>0?1:-1;
        String sign = "";
        if(sign1 * sign2 < 0){  //要记录结果的正负
            sign = "-";
        }
        long n1 = Math.abs((long) numerator); //因为负数有-128
        long n2 = Math.abs((long) denominator);
        long Integ = n1/n2;
        long remain = n1 % n2;
        StringBuffer s = new StringBuffer();
        if(remain != 0){ //余数不为0，说明有小数
            s.append(Integ);
            s.append(".");
        }else { //只有整数
            return sign+String.valueOf(Integ);
        }
        Map<Long,Integer> map = new HashMap<>();
        int start =-1;
        while (remain != 0) {  //本次余数会得到什么结果
            if(map.containsKey(remain)){
                start = map.get(remain);
                String cycle = s.substring(start);
                String res = s.substring(0,start);
                return sign+res+"("+cycle+")";
            }else {
                map.put(remain,s.length());
                remain *=10;
                long cur = remain/n2;
                remain = remain % n2; //这次的余数
                s.append(cur);
            }
        }
        return sign+s.toString();
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.fractionToDecimal(0,3));
    }
}
```
## 3、给定一个整数 n，返回 n! 结果尾数中零的数量。
```
给定一个整数 n，返回 n! 结果尾数中零的数量。

示例 1:

输入: 3
输出: 0
解释: 3! = 6, 尾数中没有零。
示例 2:

输入: 5
输出: 1
解释: 5! = 120, 尾数中有 1 个零.
说明: 你算法的时间复杂度应为 O(log n) 。
```
* 【分析】：最简单粗暴的方法就是先乘完再说，然后一个一个数。事实上，你在使用暴力破解法的过程中就能发现规律： 这 9 个数字中只有 2（它的倍数） 与 5 （它的倍数）相乘才有 0 出现。所以，现在问题就变成了这个阶乘数中能配 多少对 2 与 5。
    * 举个复杂点的例子： 10！ = [2 3（ 2 * 2 ） 5 （ 2 * 3 ）7（ 2 * 2 * 2 ） 9 （ 2 * 5）] 在 10！这个阶乘数中可以匹配两对 2 * 5 ，所以10！末尾有 2 个 0。可以发现，一个数字进行拆分后 2 的个数肯定是大于 5 的个数的，所以能匹配多少对取决于 5 的个数。（好比现在男女比例悬殊，最多能有多少对异性情侣取决于女生的多少）。 那么问题又变成了 统计阶乘数里有多少个 5 这个因子。 需要注意的是，像 25，125 这样的不只含有一个 5 的数字的情况需要考虑进去。比如 n = 15。那么在 15! 中 有 15/5=3，【15<=3*5,2*5,1*5】， 所以计算 n/5 就可以 。但是比如 n=25，依旧计算 n/5 ，可以得到 5 个5，即1*5，2*5，3*5，4*5，5*5，被乘数5共有5个，但系数1*2*3*4*5里面还有5。 所以除了计算 n/5 ， 还要计算 n/5/5 , n/5/5/5 , n/5/5/5/5 , ..., n/5/5/5,,,/5直到商<5,说明系数没有5了，然后求和即可。
    *  
```java
class Solution {

    public int trailingZeroes(int n) {
        if(n<5){
            return 0;
        }
        return n/5+trailingZeroes(n/5);
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.trailingZeroes(10));
    }

```
### 4、
```
颠倒给定的 32 位无符号整数的二进制位。

 

示例 1：

输入: 00000010100101000001111010011100
输出: 00111001011110000010100101000000
解释: 输入的二进制串 00000010100101000001111010011100 表示无符号整数 43261596，
      因此返回 964176192，其二进制表示形式为 00111001011110000010100101000000。
示例 2：

输入：11111111111111111111111111111101
输出：10111111111111111111111111111111
解释：输入的二进制串 11111111111111111111111111111101 表示无符号整数 4294967293，
      因此返回 3221225471 其二进制表示形式为 10101111110010110010011101101001。
 

提示：

请注意，在某些语言（如 Java）中，没有无符号整数类型。在这种情况下，输入和输出都将被指定为有符号整数类型，并且不应影响您的实现，因为无论整数是有符号的还是无符号的，其内部的二进制表示形式都是相同的。
在 Java 中，编译器使用二进制补码记法来表示有符号整数。因此，在上面的 示例 2 中，输入表示有符号整数 -3，输出表示有符号整数 -1073741825。
 

进阶:
如果多次调用这个函数，你将如何优化你的算法？
```
```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int res =0;
        int i=32; //要处理32个位子
        while (i > 0){
            res <<=1;  //res向右移动一个位置，给 n 的最后一位挪个窝
            res += n&1; // n和1与，取出n的最后一位，放在ans的最后一位
            n >>>=1; //n右移一位，把已经挪到ans中的最后一位释放掉
            i--;
        }
        return res;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.reverseBits(-3));
        Integer.reverse(5);
    }

```
### 5、位 1 的个数
```
编写一个函数，输入是一个无符号整数，返回其二进制表达式中数字位数为 ‘1’ 的个数（也被称为汉明重量）。

 

示例 1：

输入：00000000000000000000000000001011
输出：3
解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
示例 2：

输入：00000000000000000000000010000000
输出：1
解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。
示例 3：

输入：11111111111111111111111111111101
输出：31
解释：输入的二进制串 11111111111111111111111111111101 中，共有 31 位为 '1'。
 

提示：

请注意，在某些语言（如 Java）中，没有无符号整数类型。在这种情况下，输入和输出都将被指定为有符号整数类型，并且不应影响您的实现，因为无论整数是有符号的还是无符号的，其内部的二进制表示形式都是相同的。
在 Java 中，编译器使用二进制补码记法来表示有符号整数。因此，在上面的 示例 3 中，输入表示有符号整数 -3。
 

进阶:
如果多次调用这个函数，你将如何优化你的算法？
```
```
    public int hammingWeight(int n) {
        int res =0;
        while ( n !=0){ //*** 不用移动32次来判断
            int cur = n &1;  //每次判断最后一个是不是1
            if(cur == 1){
                res++;
            }
            n >>>=1;
        }
        return res;
    }
```
### 6、求一个数是不是质数
* 因为素数是只能被1和本身整除的大于1的整数。如果能被小于或等于本身的平方根的数整除，该数就不是素数；如果这个数不能被小于或等于本身的平方根的数整除，假设能被大于本身平方根的数整除，其商应是小于本身平方根的整数，又同“不能被小于或等于本身的平方根的数整除”的前提相矛盾。所以，看N是否是素数 就是N/2一直除到N/根号
    * 素数的定义:对于一个自然数N，用大于1小于N的各个自然数都去除一下N，如果都除不尽，则N为素数，否则N为合数。
    * 每个合数都可以写成几个质数相乘的形式，其中每个质数都是这个合数的因数，把一个合数用质因数相乘的形式表示出来，叫做分解质因数
```
    //判断是否为素数
    public static boolean isPrime(int n){
      if(n<=1) return false;
 
      for(int i=2; i<=Math.sqrt(n); i++){
        if(n%i == 0)
              return false;
      }  
      return true;
    }

```
###  7、计数质数
```
统计所有小于非负整数 n 的质数的数量。

示例:

输入: 10
输出: 4
解释: 小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
```
* 【分析】：这题搜到一个非常牛逼的算法,叫做厄拉多塞筛法. 比如说求20以内质数的个数,首先0,1不是质数.2是第一个质数,然后把20以内所有2的倍数划去.2后面紧跟的数即为下一个质数3,然后把3所有的倍数划去.3后面紧跟的数即为下一个质数5,再把5所有的倍数划去.以此类推.【所有合数都可以分解为质数相乘，你把所有质数的倍数（合数）删除了，剩下的肯定就是质数了】
```java
public class Solution {
    public int countPrimes(int n) {
        int[] flag = new int[n]; //<n的质数，不包括n
        if(n < 2){
            return 0;
        }
        int count = n-2;//除去了0,1这两个不是质数的数【0-n-1】
        //为什么是 i*i<n，因为下面是从 i 倍算着走的，如果 j=i*i要<n，在这里先判断了，如果有更大的i也不用走下去了
        for(int i=2; i*i<n; i++){
            if(flag[i] == 0){  //如果是质数
                //为什么从 i*i 开始呢？因为 2...（i-2），(i-1)*i已经被标记过了，所以直接从i倍开始即可
                for(int j=i*i; j<n; j+=i){ 
                    //不能直接置为1 ，因为同一个位置的如果是合数，可能会被多次赋值，该位置记录了它被标记的次数
                    // 比如 7*8，之前会被2*28赋过一次值，只有第一次标记我们才减去它
                    flag[j] = flag[j]+1;
                    if(flag[j] == 1){
                        count--;
                    }
                }
            }
        }
        return count;
    }
}
```
### 8、n个数的序列，找出 0 .. n 中没有出现在序列中的那个数。
```
给定一个包含 0, 1, 2, ..., n 中 n 个数的序列，找出 0 .. n 中没有出现在序列中的那个数。

示例 1:

输入: [3,0,1]
输出: 2
示例 2:

输入: [9,6,4,2,3,5,7,0,1]
输出: 8
说明:
你的算法应具有线性时间复杂度。你能否仅使用额外常数空间来实现?
```
```java
//法一：利用加减
class Solution {
    public int missingNumber(int[] nums) {
        int total = nums.length; //***** 0+1+...+num.length（n个数）
        for(int i = 0; i<nums.length ;i++){ //减去存在的数，剩下的就是不存在的数
            total = total + i - nums[i];
        }
        return total;
    }
}

//法二：利用异或好像和以前的一道题（只出现一次的数字）有异曲同工之处。看了大家的题解，异或操作（^）是一种很好的方式，不用考虑sum越界问题。

举个例子：

0 ^ 4 = 4 //***与0异或为本身
4 ^ 4 = 0 //***同一个数异或为0
那么，就可以不用求和，直接使用异或运算^进行 抵消，剩下的数字就是缺失的了。


  public int missingNumber(int[] nums) {
        int total = nums.length;
        for(int i = 0; i<nums.length ;i++){
            total = total^i^nums[i];
        }
        return total;
    }

```
### 9、给定一个整数，写一个函数来判断它是否是 3 的幂次方。
```java
给定一个整数，写一个函数来判断它是否是 3 的幂次方。

示例 1:

输入: 27
输出: true
示例 2:

输入: 0
输出: false
示例 3:

输入: 9
输出: true
示例 4:

输入: 45
输出: false
进阶：
你能不使用循环或者递归来完成本题吗？
```
* 【分析】： 3的幂次质因子只有3，而整数范围内的3的幂次最大是1162261467
```java
class Solution {
    public boolean isPowerOfThree(int n) {
        return n > 0 && 1162261467%n == 0;
    }
}
```
# 图
## BFS
# 动态规划
```
给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少。

示例 1:

输入: n = 12
输出: 3 
解释: 12 = 4 + 4 + 4.
示例 2:

输入: n = 13
输出: 2
解释: 13 = 4 + 9.
```
* 【分析】：递归：numSqures(n)表示数为n 时返回最少的组成和的完全平方数个数
```java
class Solution {
    public int numSquares(int n) {
        if(n < 0) {
            return Integer.MAX_VALUE;
        }
        if(n == 0){
            return 0;
        }
        if(n == 1){
            return 1;
        }
        if(n == 2){
            return 2;
        }
        if(n == 3){
            return 3;
        }
        int min = Integer.MAX_VALUE;
        for(int i =1 ; n - i*i >=0; i++){
            min = Math.min(min,numSquares(n-i*i)+1);
        }
        return min;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.numSquares(12));
    }
}
```
* 改为动态规划
```java
class Solution {
    public int numSquares(int n) {  //先写一下递归
      //dp[n]代表和等于n时，所需要最少的完全平方数。
    //dp[n]=min(dp[n-i*i]+1),n-i*i>=0&&i>=1
    //这里的+1就是加上这个i*i平方数。n-i*i=0就是这个数就是个完全平方数（4）。
        if(n<=3)
            return n;
        int[]dp=new int[n+1];
        
        //1=1;2=1+1;3=1+1+1
        dp[1]=1;
        dp[2]=2;
        dp[3]=3;
        for(int i=4;i<=n;i++)
        {
            dp[i]=i;
            for(int t=1;i-t*t>=0;t++)
                dp[i]=Math.min(dp[i],dp[i-t*t]+1);
        }
        return dp[n];
    }
}
```
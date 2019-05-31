

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
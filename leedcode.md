# 数学和位运算
## 直线上最多的点数
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

<!-- TOC -->

- [一、复杂度估计和算法排序（上）](#一复杂度估计和算法排序上)
    - [时间复杂度](#时间复杂度)
    - [递归的时间复杂度估算-master公式](#递归的时间复杂度估算-master公式)
    - [排序对数器](#排序对数器)
    - [冒泡排序[O(N<sup>2</sup>), O(1)]](#冒泡排序onsup2sup-o1)
    - [选择排序[O(N<sup>2</sup>), O(1)]](#选择排序onsup2sup-o1)
    - [插入排序[O(N)-O(N<sup>2</sup>), O(1)]](#插入排序on-onsup2sup-o1)
    - [归并排序[O(NlogN), O(N)]](#归并排序onlogn-on)
    - [归并排序的变体——小和问题和逆序对问题](#归并排序的变体小和问题和逆序对问题)
- [二、复杂度估计和排序算法（下）](#二复杂度估计和排序算法下)
    - [荷兰国旗问题[O(N), O(1)]](#荷兰国旗问题on-o1)
    - [经典快排和随机快排（基于荷兰国旗问题）[O(NlogN), O(logN)]](#经典快排和随机快排基于荷兰国旗问题onlogn-ologn)
    - [堆结构](#堆结构)
    - [堆排序](#堆排序)
    - [桶排序 [O(N), O(N)]](#桶排序-on-on)
    - [计数排序](#计数排序)
    - [基数排序](#基数排序)
    - [数组排序后的最大差值问题](#数组排序后的最大差值问题)
    - [排序算法在工程中的应用](#排序算法在工程中的应用)
- [三、栈、队列、链表、数组、矩阵结构及相关常见面试题](#三栈队列链表数组矩阵结构及相关常见面试题)
    - [栈结构及面试](#栈结构及面试)
        - [题目一：用数组结构实现大小固定的队列和栈](#题目一用数组结构实现大小固定的队列和栈)
        - [题目二：能返回栈中最小元素的栈](#题目二能返回栈中最小元素的栈)
        - [题目三：仅用队列结构实现栈结构](#题目三仅用队列结构实现栈结构)
    - [队列结构及面试](#队列结构及面试)
        - [题目四：仅用栈结构实现队列结构](#题目四仅用栈结构实现队列结构)
        - [题目五：猫狗队列](#题目五猫狗队列)
    - [链表结构及面试](#链表结构及面试)
        - [题目八：反转单向和双向链表](#题目八反转单向和双向链表)
        - [题目十一：打印两个有序链表的公共部分](#题目十一打印两个有序链表的公共部分)
        - [题目十二：判断一个链表是否为回文结构](#题目十二判断一个链表是否为回文结构)
        - [题目十三：将单向链表按某值划分成左边小、中间相等、右边大的形式](#题目十三将单向链表按某值划分成左边小中间相等右边大的形式)
        - [题目十四：复制含有随机指针节点的链表](#题目十四复制含有随机指针节点的链表)
        - [题目十五：两个单链表相交的一系列问题](#题目十五两个单链表相交的一系列问题)
    - [数组结构及面试](#数组结构及面试)
    - [矩阵结构及面试——从宏观上实现（观察局部位置太复杂）](#矩阵结构及面试从宏观上实现观察局部位置太复杂)
        - [题目六：转圈打印矩阵【矩阵分圈处理】](#题目六转圈打印矩阵矩阵分圈处理)
        - [题目七：旋转正方形矩阵](#题目七旋转正方形矩阵)
        - [题目九：“之”字形打印矩阵](#题目九之字形打印矩阵)
        - [题目十：在行列都排好序的矩阵中找数](#题目十在行列都排好序的矩阵中找数)
    - [二分搜索的扩展](#二分搜索的扩展)

<!-- /TOC -->

# 一、复杂度估计和算法排序（上）
## 时间复杂度
  * 时间复杂度为一个算法流程中，常数操作数量的指标。常用O（读作big O）来表示。具体来说，在常数操作数量的表达式中，只要高阶项，不要低阶项，也不要高阶项的  系数，剩下的部分如果记为f(N)，那么时间复杂度为O(f(N))。  
  * 常数时间的操作：一个操作如果和数据量没有关系，每次都是固定时间内完成的操作，叫做常数操作,O(1)。<br>

## 递归的时间复杂度估算-master公式

  * 如果一个递归行为的时间复杂度公式为以下形式，即为master公式，可直接推出时间复杂度
     * T(N) = a*T(N/b) + O(N<sup>d</sup>)【master公式】
     * log<sub>b</sub>a > d -> 复杂度为O(N<sup>log<sub>b</sub>a</sup>)
     * log<sub>b</sub>a = d -> 复杂度为O(N<sup>d</sup> * logN) 
     * log<sub>b</sub>a < d -> 复杂度为O(N<sup>d</sup>)

  *  //比如归并排序T(N)=2T(N/2)+O(N)，b=2,a=2,d=1,因为log<sub>2</sub>2=1=d,所以时间复杂度为O(NlogN)<br>

## 排序对数器

  * 用于检查自己的排序是否有错误<br>
  1.有一个你想要测的方法a，<br>
  2.实现一个绝对正确但是复杂度不好的方法b，<br>
  3.实现一个随机样本产生器<br>
  4.实现比对的方法<br>
  5.把方法a和方法b比对很多次来验证方法a是否正确。<br>
  6.如果有一个样本使得比对出错，打印样本分析是哪个方法出错<br>
  7.当样本数量很多时比对测试依然正确，可以确定方法a已经正确。<br>
```Java
import java.util.Arrays;

public class Main {
    //排序对数器：测试所编写的排序方法是否正确
    public static void main(String[] args) {
        int testTime = 5000;                                    //测试次数
        int maxSize = 100;                                      //数组大小
        int maxValue = 100;                                     //值大小
        boolean succeed = true;                                 //若不正确则返回false
        for (int i = 0; i < testTime; i++) {
            int[] arr1 = generateRandomArray(maxSize, maxValue);//生成大小和值大小都随机的数组
            int[] arr2 = copyArray(arr1);
            BublleSort.bublleSort(arr1);                        //自己编写的方法将arr1排序
            comparator(arr2);                                   //一个绝对正确的方法将arr2排序
            if (!isEqual(arr1, arr2)) {
                succeed = false;
                break;
            }
        }
        System.out.println(succeed ? "Nice!" : "Fucking fucked!");

        int[] arr = generateRandomArray(maxSize, maxValue);
        printArray(arr);
        System.out.println();
        BublleSort.bublleSort(arr);
        printArray(arr);
    }


    //一个绝对正确的方法将arr2排序
    public static void comparator(int[] arr) {
        Arrays.sort(arr);
    }

    //生成大小和值大小都随机的数组
    public static int[] generateRandomArray(int maxSize, int maxValue) {
        int[] arr = new int[(int) ((maxSize + 1) * Math.random())];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = (int) ((maxValue + 1) * Math.random()) - (int) (maxValue * Math.random());
        }
        return arr;
    }

    //复制数组
    public static int[] copyArray(int[] arr) {
        if (arr == null) {
            return null;
        }
        int[] res = new int[arr.length];
        for (int i = 0; i < arr.length; i++) {
            res[i] = arr[i];
        }
        return res;
    }

    //查看自己写的方法和正确方法结构是否一样
    public static boolean isEqual(int[] arr1, int[] arr2) {
        if ((arr1 == null && arr2 != null) || (arr1 != null && arr2 == null)) {
            return false;
        }
        if (arr1 == null && arr2 == null) {
            return true;
        }
        if (arr1.length != arr2.length) {
            return false;
        }
        for (int i = 0; i < arr1.length; i++) {
            if (arr1[i] != arr2[i]) {
                return false;
            }
        }
        return true;
    }

    //打印数组
    public static void printArray(int[] arr) {
        if (arr == null) {
            return;
        }
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
    }
}
```


## 冒泡排序[O(N<sup>2</sup>), O(1)]

  * 时间复杂度:O(N<sup>2</sup>)<br>
  * 额外空间复杂度：O(1)
  * 原理：N个元素，相邻两个元素比较大小，前者大则交换位置，一轮结束后，最大的元素则到了本轮的最末位置，不再变动；再对前面N-1个元素再进行同样的操作，进而每轮把最大的元素扔在最后。<br>
```Java
public class BublleSort {
    public static void bublleSort(int[] arr) {
        if(arr == null || arr.length<2)   //当元素小于两个，即没有或只有一个，肯定不用排序了
            return;

        for(int i=arr.length-1 ; i>0 ; i--){
            for(int j=0 ; j<i;j++){   //遇到==不交换，所以可以保持稳定
                if(arr[j] > arr[j+1])
                swap(arr,j,j+1);
            }
        }

    }

    private static void swap(int[] arr, int j, int i) {
        int temp=arr[j];
        arr[j]=arr[i];
        arr[i]=temp;
    }
}

```
## 选择排序[O(N<sup>2</sup>), O(1)]

  * 时间复杂度：O(N<sup>2</sup>)<br>
  * 额外空间复杂度：O(1)
  * 原理：进行N轮，每轮把最小的元素放在最前面：0-N 找最小的元素放在0位置；1-N 找最小的元素放在1位置<br>

```Java
  public class SelectionSort {
    public static void selectionSort(int[] arr){
        if(arr==null || arr.length<2)
            return;

        for(int i=0; i<arr.length-1;i++){
            int minIndex=i;
            for(int j=i+1;j<arr.length;j++){
                if (arr[minIndex] > arr[j])
                    minIndex=j;
            }
            swap(arr,minIndex,i);
        }
    }

    private static void swap(int[] arr, int j, int i) {
        int temp=arr[j];
        arr[j]=arr[i];
        arr[i]=temp;
    }
}
```
 
## 插入排序[O(N)-O(N<sup>2</sup>), O(1)]

   * 时间复杂度：<br>
     1.如果本身已经排好序了，为O(N)<br>
     2.如果是逆序，则为O(N<sup>2</sup>)<br>
   * 额外空间复杂度：O(1)
   * 原理：相当于一副牌，每抓一张，往前面正确的位置插入。第一张牌因为只有一张所以不用排序，于是依次找第二张牌、第三张、第四张、、、的位置
 ```Java
   public class InsertionSort {
    public static void insertSort(int[] arr){
        if(arr==null || arr.length<2)
            return;

        for(int i=1; i<arr.length; i++){    //将下标为i的元素插入前面的有序数组
            int j=i;
            while(j>0 && arr[j]<arr[j-1]){  //遇到==不交换，所以可以稳定
                swap(arr,j,j-1);
                j--;
            }
        }
    }

    private static void swap(int[] arr, int j, int i) {
        int temp=arr[j];
        arr[j]=arr[i];
        arr[i]=temp;
    }
}
 ```
## 归并排序[O(NlogN), O(N)]

  * 时间复杂度：O(NlogN)：T(N)=2T(N/2)+O(N),其中O(N)为merge操作的时间复杂度，此递归算式符合master公式，直接得到时间复杂度为O(NlogN)
  * 额外空间复杂度：O(N)
  * 原理：利用递归的思想，分为左右分别排序，再进行整体归并 
 ```Java
package day1;
  
public class MergeSort {
    public  static  void mergeSort(int[] arr){
        if(arr==null || arr.length<2)
            return;
        mergeSort(arr,0,arr.length-1);           //递归进行归并排序
    }

    private static void mergeSort(int[] arr, int l, int r) {
        if (l==r)                               //说明已经到树梢了，只有一个元素，所以不用排序
            return;
        /** 右移一位相当于除了2，（l+r）/2 ,因为l和r都是下标，可能有溢出的风险,位运算比算数运算快*/
        int mid=l+((r-l)>>1);
        mergeSort(arr,l,mid);                  //对左边进行排序
        mergeSort(arr,mid+1,r);                //对右边进行排序
        merge(arr,l,mid,r);                    //进行归并排序
    }

    private static void merge(int[] arr, int l, int mid, int r) {
        int p1=l;                               //左边数组的起始下标
        int p2=mid+1;                            // 右边数组的起始下标
        int i=0;
        int[] help=new int[r-l+1];              //所需的额外空间，用于存放排好序的数组
        while (p1<=mid && p2<=r) {              //当左右数组都没有溢出各自范围时,除了循环就说明有一边已经全部放入help数组中了
            help[i++]=(arr[p1]<arr[p2])?arr[p1++]:arr[p2++];
        }
        while (p1<=mid){                      //说明溢出的是P2，那么就是右边已经全部放入help中了，所以现在要把左边全部放入数组
            help[i++]=arr[p1++];
        }
        while(p2<=r){                         //说明溢出的是P1
            help[i++]=arr[p2++];
        }
        /** 至此，左右两边数组的元素都排好序放入辅助数组help了，我们再将它放入arr即可 */
        for(i=0; i<help.length;i++) {            //切记不可以是i<arr.length,因为这是递归，每个子进程的范围都不一样
            arr[l+i]=help[i];                    //help和arr的数组大小是不一样的，所以只能这样
        }
    }
}

  ```
## 归并排序的变体——小和问题和逆序对问题

   * 小和问题
```
问题：
在一个数组中，每一个数左边比当前数小的数累加起来，叫做这个数组的小和。求一个数组的小和。
例子：
[1,3,4,2,5]
1左边比1小的数，没有；
3左边比3小的数，1；
4左边比4小的数，1、3；
2左边比2小的数，1；
5左边比5小的数，1、3、4、2；
所以小和为1+1+3+1+1+3+4+2=16
```

   * 原理： 实质就是对每个数找右边有多少个数比它大。设小和为res，对于左边数组的每一个数i，归并的时候看右边数组有多少比它大(不能再看左边本身比它大的了，因为之前已经归并过计算过了)，然后res+i*个数,直到最后全部归并完成，把下面的红色算式全部加起来就是小和<br>
     ![](https://images2018.cnblogs.com/blog/978657/201804/978657-20180404133717075-1766283535.png)<br>
 
```Java
package day1;

public class SmallSum {
    public static int smallSum(int[] arr){
        if(arr==null || arr.length<2)
            return 0;           //****只有一个或者是null的时候没有小和，即为0
        return mergeSort(arr,0,arr.length-1);
    }

    public static int mergeSort(int[] arr, int l, int r) {
        if(l==r)
            return 0;           //****到树梢了，只有一个元素，小和为0
        int mid=l+((r-l)>>1);
        return mergeSort(arr,l,mid)
                +mergeSort(arr,mid+1,r)
                +merge(arr,l,mid,r);    //****左边归并得到的小和+右边归并得到的小和+左右归并得到的小和
    }

    public static int merge(int[] arr,int l,int mid,int r){
        int p1=l;
        int p2=mid+1;
        int i=0;
        int[] help=new int[r-l+1];
        int res=0;               //****用于存放小和
        while (p1<=mid && p2<=r) {
            /**  //****左边数小，则左边数就是小和，右边数小，因为它在右边，所以不符合小和的定义 */
            res+=(arr[p1]<arr[p2])?(r-p2+1)*arr[p1]:0;
            help[i++]=(arr[p1]<arr[p2])?arr[p1++]:arr[p2++];
        }
        while (p1<=mid){
            help[i++]=arr[p1++];
        }
        while(p2<=r) {
            help[i++] = arr[p2++];
        }
        for(i=0; i<help.length;i++) {
            arr[l+i]=help[i];
        }
        return res;        //****返回小和
    }

    // for test，对数器=随机产生数组（generateRandomArray）+比较用的算法(comparator)
    public static int comparator(int[] arr) { }
    public static int[] generateRandomArray(int maxSize, int maxValue) { }
    public static int[] copyArray(int[] arr) {}
    public static boolean isEqual(int[] arr1, int[] arr2) { }
    public static void printArray(int[] arr) { }
    public static void main(String[] args) { }
}
```
   * 逆序对问题
        ```
        问题：
        在一个数组中，左边的数如果比右边的数大，则折两个数构成一个逆序对，请打印所有逆序对。
        ```
     * 原理：实质就是对每个数找右边有多少个数比它小，(可能maybe,没有验证过）是在小和那里改为：<br>
    
    ```Java
    if(arr[p1]>arr[p2]){
      for(int t=p1;t<=mid;t++)
      System.out.printf("(%d ,%d)",arr[t],arr[p2])
    }
    ```
    
# 二、复杂度估计和排序算法（下）

## 荷兰国旗问题[O(N), O(1)]

```
问题一：（荷兰国旗问题是分为三堆（遇到‘等于’不用动），这个问题只是分成两堆而已，小于等于————大于，遇到‘大于’时不用动）
给定一个数组arr，和一个数num，请把小于等于num的数放在数组的左边，大于num的数放在数组的右边。
要求额外空间复杂度O(1)，时间复杂度O(N)

问题二（荷兰国旗问题）（遇到‘等于’不用动）
给定一个数组arr，和一个数num，请把小于num的数放在数组的左边，等于num的数放在数组的中间，大于num的数放在数组的右边。
要求额外空间复杂度O(1)，时间复杂度O(N)
```
   * 时间复杂度：O(N)
   * 空间复杂度：O(1)
   * 原理:用了三个指针，less指示小于区域，more指示大于区域，cur指示当前需要判断的数
```Java
package day2;

public class NetherlandsFlag {
    public static int[] partition(int[] arr,int L,int R,int num){
        int less=L-1;           //左边小于区域
        int more=R+1;           //右边大于区域
        int cur=L;              //当前位置，要判断它是大于等于小于，然后放在合适的位置
        while(cur<more){        //more及其右边是已经分好的大于num的数，cur遇到more就意味着没有需要划分的数了
            if(arr[cur] < num){ //若小于num则放在小于区(swap)，小于区less++,然后判断下一个数cur++
                swap(arr,++less,cur++);
            }else if(arr[cur] > num){
                /** //若大于num则放在大于区(swap)，大于区扩大more--，从大于区交换来的cur位置的数没有判断过，所以cur不++ */
                swap(arr,--more,cur);
            }else cur++;        //若等于num,则不管它，然后继续判断下一个数cur++;
        }
        return new int[]{less+1,more-1};
        }

    private static void swap(int[] arr, int i, int j) {
        int temp=arr[i];
        arr[i]=arr[j];
        arr[j]=temp;
    }
}
```
  
## 经典快排和随机快排（基于荷兰国旗问题）[O(NlogN), O(logN)]
  * 时间复杂度：O(NlogN)
  * 空间复杂度：O(logN):因为递归的时候每一层都需要记录划分点（记录等于区域的位置）
  * 原理： 经典快排：利用最后一个数作为分界点，小的放左边，大的放右边，使用的是荷兰国旗问题的方法：数组分为小于，等于，大于三堆，再递归对小于，大于进行划分<br>
     随机快排：产生一个随机位置作为分界点
   * 随机快排的优点：经典快排在遇见有序数组时：1，2，3，4，5，6；每次只能排好最后一个数，要历时N轮，时间复杂度为O(N<sup>2</sup>),随机快排则不会出现此问题，其长期期望时间复杂度为O(NlogN)；<br>与归并排序相比：虽然时间复杂度都是O(NlogN),但归并排序有更多的while循环，常数时间比随机快排多，而且额外空间复杂度是O(N)，这个大于随机快排的O(logN),所以随机快排更优。
```Java
package day2;

import java.util.Arrays;

public class QuickSort {
    public static void quickSort(int[] arr){
        if(arr==null || arr.length<2)
            return;
        quickSort(arr,0,arr.length-1); //数组及其要排序的范围
    }
    
    private static void quickSort(int[] arr, int L, int R){
        if(L < R){      //L和R是需要排序的范围
            /** 荷兰国旗问题是按给的数来划分，随机快排是随机选择一个数作为num来划分,经典快排是一直以最后一个数来划分 */
            int num=arr[L+(int)Math.random()*(R-L+1)];
            /** partition荷兰国旗的代码，一摸一样 */
            int[] p=partition(arr,L,R,num);  //会返回中间"等于"区域的左边界和右边界放在P[]中
            quickSort(arr, L, p[0]-1);   //中间排好序后排两边
            quickSort(arr, p[1]+1, R);
        }
    }

    private static int[] partition(int[] arr, int L, int R, int num) {//此为荷兰国旗问题的代码
        int less=L-1;
        int more=R+1;
        int cur=L;
        while (cur < more){
            if(arr[cur] < num){
                swap(arr, ++less, cur++);
            }else if(arr[cur] > num){
                swap(arr,--more,cur);
            }else {
                cur++;
            }
        }
        return new int[]{less+1,more-1};
    }

    private static void swap(int[] arr, int i, int j) {
        int temp=arr[i];
        arr[i]=arr[j];
        arr[j]=temp;
    }
 }
```
## 堆结构
   * 堆：堆存储在`数组`中，完全二叉树只是为了帮助理解而想象出来的：<br>
   对结点i：左节点下标为(2*i）+1；右结点为(2* i）+2；父节点为（i-1）/2；<br>
   ![](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161217182857323-2092264199.png)<br>
   * 堆的分类：<br>
     * 大根堆（即优先队列）：堆中的每个结点都大于它的两个子结点
     * 小根堆：堆中的每个结点都小于它的两个子结点
     ![](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161217182750011-675658660.png)
   * 大根堆的构造（上浮swim）（在数组中构造）：实质就是每添加一个数，就将它上浮，直到它不再大于其父结点
   * 建立堆的时间复杂度：O(N)：log1+log2+log3+...logN-1【收敛于O(N)】
## 堆排序
   * 时间复杂度：O(NlogN)
   * 额外空间复杂度：O(1)
   * 堆排序（在数组中排序从小到大）：
      * 1、构造大根堆（上浮）；
      * 当while（heapSize > 0），循环执行2、3两步
      * 2、将顶点与尾部交换，堆大小heapSize减1（因为最大的那个数已经在末尾了，不需要再排序）
      * 3、将交换得来的顶点`下沉`到其应该的位置（即它比左右子结点都大），从而恢复堆有序
```Java
package day2;

import java.util.Arrays;

public class HeapSort {
    public static void heapSort(int[] arr){
        if(arr==null || arr.length<2)
            return;
        for (int i=0; i<arr.length ;i++){ //构造一个大根堆,依次将元素加入堆中
            swim(arr,i);                   //将位arr[i]上浮，直到它不再大于它的父节点
        }
        int heapsize=arr.length;
        while(heapsize > 0){
            swap(arr, 0, --heapsize);       //将堆的头部和尾部交换，堆大小减一，然后下沉头部
            sink(arr,0,heapsize);              //将0下沉，直到它比它的两边节点都大
        }
    }

    public static void swim(int[] arr, int i){
        while(arr[i] > arr[(i-1)/2]){
            swap(arr, i, (i-1)/2);
            i=(i-1)/2;
        }
    }

    public static void sink(int[] arr, int i, int heapSize){//将位于i的元素下沉
       int left = (2 * i)+1;
       int right =left+1;
       while(left < heapSize){
           int largest = (right < heapSize && arr[right] > arr[left])?right:left;
           largest = (arr[largest] > arr[i])?largest:i;    //找到i和其左右结点中最大的数，记录其位置
           if(largest == i){            //位于i的元素已经比它的左右子结点都大了，不用再下沉
               break;
           }else {
               swap(arr, i, largest);
               i=largest;
               left = (2 * i)+1;
               right =left+1;
           }
       }
    }

    private static void swap(int[] arr, int i, int j) {
        int temp=arr[i];
        arr[i]=arr[j];
        arr[j]=temp;
    }
 }
 ```


## 排序的稳定性
* 排序的稳定性:如果一个排序算法能够保留数组中重复元素的相对位置则可以被称为稳定的。
* 稳定性的意义：现实应用的需求，要求保留原始次序，比如：一个客户有身高和体重两个信息，对很多客户进行排序，先依据身高排序，再根据体重排序，如果具有稳定性，那么身高的排序信息能被保留（如果体重一样的话）
* 时间复杂度：
    * O(N<sup>2</sup>)：
      * 冒泡排序（稳定）
      * 选择排序（不稳定）
      * 插入排序（稳定）<br>
    * O(NlogN)：
      * 归并排序（可以稳定，只要merge的时候遇到相等，先拷贝左边，再拷贝右边即可）
      * 快速排序（不稳定，因为partition做不到稳定）
      * 堆排序（不稳定，因为交换过程中控制不住相等值）

## 比较器

* Array.sort(基础类型)：按值排序<br>
Array.sort(非基础类型)：按内存地址排序，所以无意义，无价值，因而有了比较器Comparator
* 可用的方法:<br>
     Arrays.sort(students,new IdComparator());  <br>
     PriorityQueue<Student> heap=new PriorityQueue<>(new IdComparator());//****用优先队列来排<br>
     TreeSet<Student> treeSet=new TreeSet<>(new IdComparator()); <br>
 
 ```Java
 package day2;
 
 public class ComparatorTest {
    public static class Student{        //静态内部类
        public String name;
        public int id;
        public int age;
        public Student(String name,int id,int age){
            this.name=name;
            this.id=id;
            this.age=age;
        }
        
        public String toString() {
            return name+' '+id+' '+age;
        }
    }
    public static class IdComparator implements Comparator<Student>{    //静态内部类，比较器
        public int compare(Student num1,Student num2){
            return num1.id-num2.id;
        }
    }
    public static void main(String[] args) {
        Student student1 = new Student("A", 1, 23);
        Student student2 = new Student("B", 2, 21);
        Student student3 = new Student("C", 3, 22);
        Student[] students = new Student[] { student3, student2, student1 };
        
        Arrays.sort(students,new IdComparator());        //****用数组自带的来排序，但还要给它一个比较器它才可以排序
        
        for(int i=0;i<students.length;i++){
            System.out.println(students[i]);
        }
    }

}
```

## 桶排序 [O(N), O(N)]

   * 此为非基于比较的排序，与被排序的样本的实际数据状况很有关系，所以实际中并不经常使用，是稳定的排序
   * 时间复杂度：O(N)
   * 额外空间复杂度：O(N)
   * 桶排序：分为计数排序和基数排序，计数排序就是N个数构造N个桶，遍历数组，对相应的桶不断+1

## 计数排序
## 基数排序
## 数组排序后的最大差值问题
```
 问题：
 给定一个数组，求如果排序之后，相邻两数的最大差值，要求时间复杂度O(N)，且要求不能用非基于比较的排序。
```

## 排序算法在工程中的应用

* 综合排序：
    * 数组长度短（<60）：直接用插入排序，因为插入排序的常数项极低
    * 数组长度长；先判断数据类型
        * 基础类型：用快速排序，因为基础类型不用区分原始顺序（不需要稳定，因为相同值无差异）
        * 引用类型：用归并排序，因为原始顺序是有用的

# 三、栈、队列、链表、数组、矩阵结构及相关常见面试题
## 栈结构及面试
### 题目一：用数组结构实现大小固定的队列和栈
   
```Java
package day3;   //用数组实现栈

public class Stack {
    int[] arr;
    int index;      //指向即将放入的位置

    public Stack(int initSize){
        if(initSize<0)
            throw new IllegalArgumentException("the init size is less than 0");
        arr=new int[initSize];
        index=0;
    }

    public void push(int obj){      //压入栈
        if(index==arr.length)
            throw new ArrayIndexOutOfBoundsException("the stack is full");
        arr[index++]=obj;
    }

    public int pop(){           //弹出栈
        if(index==0)
            throw new ArrayIndexOutOfBoundsException("the stack is empty");
        return arr[--index];
    }

    public int peek(){          //查看栈顶元素，但不移除
        if(index==0)
            throw new ArrayIndexOutOfBoundsException("the stack is empty");
        return arr[index-1];
    }
}
```
 ```Java
 package day3;  //用数组实现队列

public class Queue {
    int[] arr;
    int first;       //指向队头，要删除的位置
    int last;       //指向下一个要添加的位置
    int size;
    public Queue(int initSize){
        if(initSize<0)
            throw new IllegalArgumentException("the init size is less than 0");
        arr=new int[initSize];
        first=0;
        last=0;
        size=0;
    }

    public void push(int obj){
        if(size==arr.length)
            throw new ArrayIndexOutOfBoundsException("the queue is full");
        size++;
        arr[last]=obj;
        last=(last==arr.length-1)?0:last+1;
    }
    public int poll(){
        if(size==0)
            throw new ArrayIndexOutOfBoundsException("the queue is empty");
        size--;
        int tmp = first;
        first = (first == arr.length - 1 )? 0 : first + 1;
        return arr[tmp];
    }
}

 ```
 
### 题目二：能返回栈中最小元素的栈
* 实现一个特殊的栈，在实现栈的基本功能的基础上，再实现返回栈中最小元素的操作<br>
【要求】<br>
1．pop、push、getMin操作的时间复杂度都是O(1)。<br>
2．设计的栈类型可以使用现成的栈结构。<br>

```Java
  package day3;

import java.util.Stack;

public class MyStack{
  private Stack<Integer> stackData;
  private Stack<Integer> stackMin;

  public MyStack(){
      stackData=new Stack<Integer>();
      stackMin =new Stack<Integer>();
  }

  public void push(int obj){
      stackData.push(obj);
      if(stackMin.isEmpty()){
          stackMin.push(obj);
      }else if(obj <= stackMin.peek()){
          stackMin.push(obj);
      }else {
          stackMin.push(stackMin.peek());
      }
  }

  public int pop(){
      stackMin.pop();
      return stackData.pop();
  }

  public int getMin(){
      if(stackMin.isEmpty())
          throw new ArrayIndexOutOfBoundsException("the stack is empty");
      return stackMin.peek();
  }

```

### 题目三：仅用队列结构实现栈结构

* 如何仅用队列结构实现栈结构？<br>
* 原理：可以用两个队列（queue、help）来实现栈,加元素时加在queue，删除时，把queue最后一位前的元素全部弹出放入help队列中，然后再弹出返回queue的最后一位元素（这就达成栈后入先出的要求了），然后交换help和queue指针即可
* 队列：poll(移除并返回队列的头部)，add(添加一个元素到队列尾部)，peek（返回队列的头部，不删除）

```java
package day3;
import java.util.LinkedList;
import java.util.Queue;

public class TwoQueueStack {
    private Queue<Integer> queue = new LinkedList<Integer>();  //LinkedList实现了Queue接口
    private Queue<Integer> help = new LinkedList<Integer>();   //不能使用原始类型int,而应该用Integer

    public void push(int obj){
        queue.add(obj);
    }

    public int pop(){
        if(queue.isEmpty())
            throw new RuntimeException("stack is empty");
        while (queue.size()>1){
            help.add(queue.poll());
        }
        int res=queue.poll();
        swap();
        return res;
    }

    public int peek(){
        if (queue.isEmpty())
            throw new RuntimeException("stack is empty");
        while (queue.size()!=1)
            help.add(queue.poll());
        int res=queue.poll();
        help.add(res);
        return res;
}
    public void swap(){
        Queue<Integer> temp=help;
        help=queue;
        queue=temp;
    }
}
```

## 队列结构及面试
### 题目四：仅用栈结构实现队列结构

* 如何仅用栈结构实现队列结构？<br>图片地址：(https://img-blog.csdn.net/20180527092623978)<br>
* 原理：可以用两个栈（stack1和stack2）来实现队列 ，进入时放入stack1栈，出栈时从stack2栈出，这样就能把顺序变为先进先出,( 栈：push,pop,peek)
* 图（1）：将队列中的元素“abcd”压入stack1中，此时stack2为空；<br>
图（2）：将stack1中的元素pop进stack2中，此时pop一下stack2中的元素，就可以达到和队列删除数据一样的顺序了；<br>
图（3）：可能有些人很疑惑，就像图3，当stack2只pop了一个元素a时，satck1中可能还会插入元素e,这时如果将stack1中的元				素e插入stack2中，在a之后出栈的元素就是e了，显然，这样想是不对的，我们必须规定当stack2中的元素pop完之后，也就是satck2为空时，再插入stack1中的元素。<br>

	  
```Java
package day3;

import java.util.Stack;

public class TwoStackQueue {
    Stack<Integer> stack1=new Stack<Integer>();
    Stack<Integer> stack2=new Stack<Integer>();

    public void add(int obj){
        stack1.push(obj);
    }

    public int poll(){
        if (stack2.isEmpty() && stack1.isEmpty()){
            throw new RuntimeException("queue is empty");
        }else if(stack2.isEmpty()){
            while (!stack1.isEmpty()){      //只有当stack2为空时，才能将stack1中的元素放进来
                stack2.push(stack1.pop());
            }
        }
        return stack2.pop();       //如果stack2中有元素，那么直接弹出，要stack2中没有元素了才能从stack1中重新放
    }

    public int peek(){
        if (stack2.isEmpty() && stack1.isEmpty()){
            throw new RuntimeException("queue is empty");
        }else if(stack2.isEmpty()){
            while (!stack1.isEmpty()){
                stack2.push(stack1.pop());
            }
        }
        return stack2.peek();       //和poll前面一样的判断，只是最后只需返回不需删除
    }
}
```

### 题目五：猫狗队列
```
宠物、狗和猫的类如下：
public class Pet { private String type;
public Pet(String type) { this.type = type; }
public String getPetType() { return this.type; }
}
public class Dog extends Pet { public Dog() { super("dog"); } }
public class Cat extends Pet { public Cat() { super("cat"); } }

实现一种狗猫队列的结构，要求如下： 
用户可以调用add方法将cat类或dog类的实例放入队列中； 
用户可以调用pollAll方法，将队列中所有的实例按照进队列的先后顺序依次弹出； 
用户可以调用pollDog方法，将队列中dog类的实例按照进队列的先后顺序依次弹出； 
用户可以调用pollCat方法，将队列中cat类的实例按照进队列的先后顺序依次弹出； 
用户可以调用isEmpty方法，检查队列中是否还有dog或cat的实例；
用户可以调用isDogEmpty方法，检查队列中是否有dog类的实例； 
用户可以调用isCatEmpty方法，检查队列中是否有cat类的实例。
```
   * 分析：1、建一个猫狗队列，这个队列里包含 DogQ 和 CatQ 两个队列，用于分别加猫和加狗，但这个会导致在pollAll时无法判断之前猫狗的顺序，所以有了2<br>
   2、建一个CatDog类，里面包含pet和count两个成员，pet用于记录CatDog类的这个实例是猫还是狗，count用于记录当前pet的顺序，那么就能判断猫狗的顺序了
   ```Java
   package day4;

import java.util.*;

public class CatDogQueue {

    //用户给的猫和狗的类定义，定义为静态类或许只是为了代码不冗杂
    public static class Pet {
        private String type;

        public Pet(String type) {
            this.type = type;
        }

        public String getPetType() {
            return this.type;
        }
    }

    public static class Dog extends Pet {
        public Dog() {
            super("dog");
        }
    }

    public static class Cat extends Pet {
        public Cat() {
            super("cat");
        }
    }

    public static class CatDog{
        private Pet pet;       //用于记录是猫还是狗
        private long count;    //用于记录当前pet的顺序
        public CatDog(Pet pet ,long count){
            this.pet=pet;
            this.count=count;
        }
        public Pet getPet(){
            return this.pet;
        }
        public long getCount(){
            return this.count;
        }
    }


    /** CatDogQueue的正式代码 */
    private Queue<CatDog> catQ = new LinkedList<CatDog>(); //Queue只是一个接口，其具体实现为LinkedList
    private Queue<CatDog> dogQ = new LinkedList<CatDog>();
    private long count = 0;

    public void add(Pet pet){
        if (pet.getPetType().equals("cat")){
            catQ.add(new CatDog(pet,count++));
        }else if(pet.getPetType().equals("dog")){
            dogQ.add(new CatDog(pet,count++));
        }else throw new RuntimeException("err,this is not dog or cat");
    }

    public Pet pollAll(){
        if(!catQ.isEmpty()&& !dogQ.isEmpty()){
            if (catQ.peek().count<dogQ.peek().count){
                return catQ.poll().getPet();
            }else {
                return dogQ.poll().getPet();
            }
        }else if(!catQ.isEmpty()){
            return catQ.poll().getPet();
        }else if(!dogQ.isEmpty()){
            return dogQ.poll().getPet();
        }else {
            throw new RuntimeException("err, queue is empty!");
        }
    }

    public Dog pollDog(){
        if(!dogQ.isEmpty()){
            return (Dog)dogQ.poll().getPet();
        }else {
            throw new RuntimeException("the dog queue is empty");
        }
    }

    public Cat pollCat(){
        if(!catQ.isEmpty()){
            return (Cat) catQ.poll().getPet();
        }else {
            throw new RuntimeException("the cat queue is empty");
        }
    }

    public boolean isAllEmpty(){
        return dogQ.isEmpty()&&catQ.isEmpty();
    }

    public boolean isDogEmpty(){
        return dogQ.isEmpty();
    }

    public boolean isCatEmpty(){
        return catQ.isEmpty();
    }

   ```
	

## 链表结构及面试
### 题目八：反转单向和双向链表
* 【题目】 分别实现反转单向链表和反转双向链表的函数。<br>
【要求】 如果链表长度为N，时间复杂度要求为O(N)，额外空间
复杂度要求为O(1)

### 题目十一：打印两个有序链表的公共部分
* 【题目】 给定两个有序链表的头指针head1和head2，打印两个链表的公共部分。

### 题目十二：判断一个链表是否为回文结构
* 【题目】 给定一个链表的头节点head，请判断该链表是否为回文结构。 例如： 1->2->1，返回true。 1->2->2->1，返回true。15->6->15，返回true。 1->2->3，返回false。<br>
进阶： 如果链表长度为N，时间复杂度达到O(N)，额外空间复杂度达到O(1)。

### 题目十三：将单向链表按某值划分成左边小、中间相等、右边大的形式
【题目】 给定一个单向链表的头节点head，节点的值类型是整型，再给定一个
整 数pivot。实现一个调整链表的函数，将链表调整为左部分都是值小于 pivot
的节点，中间部分都是值等于pivot的节点，右部分都是值大于 pivot的节点。
除这个要求外，对调整后的节点顺序没有更多的要求。 例如：链表9->0->4->5-
>1，pivot=3。 调整后链表可以是1->0->4->9->5，也可以是0->1->9->5->4。总
之，满 足左部分都是小于3的节点，中间部分都是等于3的节点（本例中这个部
分为空），右部分都是大于3的节点即可。对某部分内部的节点顺序不做 要求。
进阶： 在原问题的要求之上再增加如下两个要求。
在左、中、右三个部分的内部也做顺序要求，要求每部分里的节点从左 到右的
顺序与原链表中节点的先后次序一致。 例如：链表9->0->4->5->1，pivot=3。
调整后的链表是0->1->9->4->5。 在满足原问题要求的同时，左部分节点从左到
右为0、1。在原链表中也 是先出现0，后出现1；中间部分在本例中为空，不再
讨论；右部分节点 从左到右为9、4、5。在原链表中也是先出现9，然后出现4，
最后出现5。
如果链表长度为N，时间复杂度请达到O(N)，额外空间复杂度请达到O(1)。

### 题目十四：复制含有随机指针节点的链表
【题目】 一种特殊的链表节点类描述如下：
public class Node { public int value; public Node next; public
Node rand;
public Node(int data) { this.value = data; }
}
Node类中的value是节点值，next指针和正常单链表中next指针的意义
一 样，都指向下一个节点，rand指针是Node类中新增的指针，这个指
针可 能指向链表中的任意一个节点，也可能指向null。 给定一个由
Node节点类型组成的无环单链表的头节点head，请实现一个 函数完成
这个链表中所有结构的复制，并返回复制的新链表的头节点。 进阶：
不使用额外的数据结构，只用有限几个变量，且在时间复杂度为 O(N)
内完成原问题要实现的函数。

### 题目十五：两个单链表相交的一系列问题
【题目】 在本题中，单链表可能有环，也可能无环。给定两个
单链表的头节点 head1和head2，这两个链表可能相交，也可能
不相交。请实现一个函数， 如果两个链表相交，请返回相交的
第一个节点；如果不相交，返回null 即可。 要求：如果链表1
的长度为N，链表2的长度为M，时间复杂度请达到 O(N+M)，额外
空间复杂度请达到O(1)。

## 数组结构及面试
## 矩阵结构及面试——从宏观上实现（观察局部位置太复杂）
### 题目六：转圈打印矩阵【矩阵分圈处理】
* 【题目】 给定一个整型矩阵matrix，请按照转圈的方式打印它。例如：
```
 1  2  3  4 
 5  6  7  8
 9 10 11 12 
 13 14 15 16 
 打印结果为：1，2，3，4，8，12，16，15，14，13，9，5，6，7，11， 10
【要求】 额外空间复杂度为O(1)。
``` 
* 【分析】：这是一个宏观的调度问题，使用分圈的处理方式，每次对一个矩阵的外围圈进行处理（记得考虑当圈只是一行或者一列或者一个点的情况），如图所示：
![](https://github.com/zhaojing5340126/interview/blob/master/picture/%E8%BD%AC%E5%9C%88%E6%89%93%E5%8D%B0%E7%9F%A9%E9%98%B5.JPG?raw=true)

```Java
package day4;

public class PrintMatrixSpiral {
    public static void printMatrixSpiral(int[][] matrix){
        int a=0;        //左上角坐标(a,b),右下角坐标c(c,d)
        int b=0;
        int c=matrix.length-1;  //matrix.length是行长度
        int d=matrix[0].length-1; //matrix[0].length是列长度
        while(a<=c && b<=d){       //一个圈一个圈地打印
            printCircle(matrix,a++,b++,c--,d--);
        }
    }

    private static void printCircle(int[][] m, int a, int b, int c, int d) {
        int i,j;
        if(a==c){       //当只有一行时（包括只有一个元素的情况）
            for (j=b ; j<=d ;j++){
                System.out.print(m[a][j]+" ");
            }
        }else if(b==d){ //当只有一列时
            for (i=a; i<=c; i++){
                System.out.print(m[i][b]+" ");
            }
        }else{      //当是一个正常的矩形时
           for (j=b; j<d; j++){
               System.out.print(m[a][j]+" ");
           }
           for (i=a; i<c; i++){
               System.out.print(m[i][d]+" ");
           }
           for (j=d; j>b; j--){
               System.out.print(m[c][j]+" ");
           }
           for (i=c; i>a; i--){
               System.out.print(m[i][b]+" ");
           }
        }
    }

    //测试
    public static void main(String[] args) {
        int[][] matrix = { { 1, 2, 3, 4 }, { 5, 6, 7, 8 }, { 9, 10, 11, 12 }, { 13, 14, 15, 16 } };
        printMatrixSpiral(matrix);
    }
}

```

### 题目七：旋转正方形矩阵
* 【题目】 给定一个整型`正方形`矩阵matrix，请把该矩阵调整成顺时针旋转90度的样子。
【要求】 额外空间复杂度为O(1)。<br>
![](https://github.com/zhaojing5340126/interview/blob/master/picture/%E6%97%8B%E8%BD%AC%E6%AD%A3%E6%96%B9%E5%BD%A2%E7%A4%BA%E4%BE%8B.png?raw=true)


* 【分析】：这个也是一个宏观的调度问题，还是使用分圈的处理方式，每次对一个矩阵的外围进行处理，我们发现只要替换四个数，替换n-1次，就把外面的边界替换好了，然后我们缩小范围(左上角的(ar,ac)和右下角的(br,bc)分别减一)，就可以完成操作，如下图：<br>
![](https://github.com/zhaojing5340126/interview/blob/master/picture/%E6%97%8B%E8%BD%AC%E6%AD%A3%E6%96%B9%E5%BD%A2.png?raw=true)

```Java
package day4;

public class RotateMatrix {
    public static void rotateMatrix(int[][] matrix){
        int a=0;        //左上角坐标(a,b),右下角坐标c(c,d)
        int b=0;
        int c=matrix.length-1;  //matrix.length是行长度
        int d=matrix[0].length-1; //matrix[0].length是列长度
        while(a<c){
            rotateEdge(matrix,a++,b++,c--,d--);
        }
    }

    private static void rotateEdge(int[][] m, int a, int b, int c, int d) {
        int times=d-b;      //第一行有几个数据需要旋转，即一个圈要旋转几次
        int temp;
        for(int i=0; i<times; i++){ //自己画个图，标一下各个位置的下标就晓得了
            temp =m[a][b+i];
            m[a][b+i]=m[c-i][b];
            m[c-i][b]=m[c][d-i];
            m[c][d-i]=m[a+i][d];
            m[a+i][d]=temp;
        }
    }
}
```

### 题目九：“之”字形打印矩阵
* 【题目】 给定一个矩阵matrix，按照“之”字形的方式打印这个矩阵，例如： 
```
1  2  3  4  
5  6  7  8  
9  10 11 12
“之”字形打印的结果为：1，2，5，9，6，3，4，7，10，11，8，12
【要求】 额外空间复杂度为O(1)。
```
![](https://github.com/zhaojing5340126/interview/blob/master/picture/%E4%B9%8B%E5%AD%97%E5%BD%A2%E6%89%93%E5%8D%B0%E7%9F%A9%E9%98%B5.png?raw=true)

* 【分析】：同样要从宏观来看，看成不断打印斜线，每打印了一条斜线就换方向来打印下一条斜线
   * 刚开始时A（row2，coll2）、B(row ,coll)位于（0,0）处，然后A往右走，走到尽头就往下面走，同时B往下走，走到尽头就往右走，当两者走到末尾右下角时所有的打印都结束了。
```Java
package day4;

public class PrintZigZagMatrix {
    public static void printZigZag(int[][] matrix){
        int a=0;        //A(a,b),B(c,d)
        int b=0;
        int c=0;
        int d=0;
        int endRow=matrix.length-1;     //矩阵右下角的下标（endRow,endCol)
        int endCol=matrix[0].length-1;
        boolean dire=true;
        while (a<=endRow && b<=endCol){   //移动到最末尾打印后就结束，
                printSlash(matrix,a,b,c,d,dire);
            dire=!dire;
            d = c >= endRow ? d+1 : d;  //d和c的顺序,a和b的顺序不能反
            c = c < endRow ? c+1 : c;   //即c这一行和b那一行不能在前面，因为它们如果改变了值会直接影响后面的d和a
            a = b >=endCol ? a+1:a;
            b=b < endCol ? b+1:b;

        }
    }

    private static void printSlash(int[][] m, int a,int b,int c, int d,boolean dire) {
        if(dire){
            for(; a<=c; c--,d++){
                System.out.print(m[c][d]+" ");
            }
        }else{
            for(;a<=c;a++,b--){
                System.out.print(m[a][b]+" ");
            }
        }

    }
}
```

### 题目十：在行列都排好序的矩阵中找数
* 【题目】 给定一个有N*M的整型矩阵matrix和一个整数K，
matrix的每一行和每一列都是排好序的。实现一个函数，判断K
是否在matrix中。 例如： 0 1 2 5 2 3 4 7 4
4 4 8 5 7 7 9 如果K为7，返回true；如果K为6，返
回false。
【要求】 时间复杂度为O(N+M)，额外空间复杂度为O(1
## 二分搜索的扩展

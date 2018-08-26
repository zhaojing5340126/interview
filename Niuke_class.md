
* [一 、复杂度估计和排序算法（上）](#一、复杂度估计和排序算法（上）)
  * [时间复杂度](#时间复杂度)
  * [递归的时间复杂度估算-master公式](#递归的时间复杂度估算-master公式)
  * [排序对数器](#排序对数器)
  * [冒泡排序 [O(N<sup>2</sup>), O(1)]](#冒泡排序)
  * [选择排序 [O(N<sup>2</sup>), O(1)]](#选择排序)
  * [插入排序 [O(N)-O(N<sup>2</sup>), O(1)]](#插入排序)
  * [归并排序 [O(NlogN), O(N)]](#归并排序)
  * [归并排序的变体——小和问题和逆序对问题](#小和问题和逆序对问题)
* [二 、复杂度估计和排序算法（下）](#二、复杂度估计和排序算法（下）)
  * [荷兰国旗问题 [O(N), O(1)]](#荷兰国旗问题)
  * [经典快排和随机快排（基于荷兰国旗问题）[O(NlogN), O(logN)]](#经典快排和随机快排)
  * [堆结构](#堆结构)
  * [堆排序](#堆排序)
  * [排序算法的稳定性](#排序算法的稳定性)
  * [比较器](#比较器)
  * [桶排序 [O(N), O(N)]](#桶排序)
  * [计数排序](#计数排序)
  * [基数排序](#基数排序)
  * [桶排序思想的应用：数组排序后的最大差值问题](#数组排序后的最大差值问题)
  * [排序算法在工程中的应用](#排序算法在工程中的应用)
* [三、栈、队列、链表、数组、矩阵结构及相关常见面试题](#三、栈、队列、链表、数组、矩阵结构及相关常见面试题)
  * [栈结构及面试](#栈结构及面试)
    * [题目一：用数组结构实现大小固定的队列和栈](#题目一)
    * [题目二：实现一个特殊的栈，具有基本栈功能和返回栈中最小元素的操作](#题目二)
    * [题目三：如何仅用队列结构实现栈结构](#题目三)
    * [题目四：如何仅用栈结构实现队列结构](#题目四)


# 一、复杂度估计和算法排序（上）<br>
* #### 时间复杂度<br>
  * 时间复杂度为一个算法流程中，常数操作数量的指标。常用O（读作big O）来表示。具体来说，在常数操作数量的表达式中，只要高阶项，不要低阶项，也不要高阶项的  系数，剩下的部分如果记为f(N)，那么时间复杂度为O(f(N))。  
  * 常数时间的操作：一个操作如果和数据量没有关系，每次都是固定时间内完成的操作，叫做常数操作,O(1)。<br>
* #### 递归的时间复杂度估算-master公式<br>
  * 如果一个递归行为的时间复杂度公式为以下形式，即为master公式，可直接推出时间复杂度
     * T(N) = a*T(N/b) + O(N<sup>d</sup>)【master公式】
     * log<sub>b</sub>a > d -> 复杂度为O(N<sup>log<sub>b</sub>a</sup>)
     * log<sub>b</sub>a = d -> 复杂度为O(N<sup>d</sup> * logN) 
     * log<sub>b</sub>a < d -> 复杂度为O(N<sup>d</sup>)

  *  //比如归并排序T(N)=2T(N/2)+O(N)，b=2,a=2,d=1,因为log<sub>2</sub>2=1=d,所以时间复杂度为O(NlogN)<br>
* #### 排序对数器<br>
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

* #### 冒泡排序<br>
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
* #### 选择排序<br>
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
 
* #### 插入排序<br>
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
* #### 归并排序
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
 * #### 小和问题和逆序对问题
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
* #### 荷兰国旗问题
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
  
* #### 经典快排和随机快排
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
* #### 堆结构
   * 堆：堆存储在`数组`中，完全二叉树只是为了帮助理解而想象出来的：<br>
   对结点i：左节点下标为(2*i）+1；右结点为(2* i）+2；父节点为（i-1）/2；<br>
   ![](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161217182857323-2092264199.png)<br>
   * 堆的分类：<br>
     * 大根堆（即优先队列）：堆中的每个结点都大于它的两个子结点
     * 小根堆：堆中的每个结点都小于它的两个子结点
     ![](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161217182750011-675658660.png)
   * 大根堆的构造（上浮swim）（在数组中构造）：实质就是每添加一个数，就将它上浮，直到它不再大于其父结点
   * 建立堆的时间复杂度：O(N)：log1+log2+log3+...logN-1【收敛于O(N)】
* #### 堆排序
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
* #### 排序算法的稳定性
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
* #### 比较器
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

* #### 桶排序
   * 此为非基于比较的排序，与被排序的样本的实际数据状况很有关系，所以实际中并不经常使用，是稳定的排序
   * 时间复杂度：O(N)
   * 额外空间复杂度：O(N)
   * 桶排序：分为计数排序和基数排序，计数排序就是N个数构造N个桶，遍历数组，对相应的桶不断+1
* #### 计数排序
* #### 基数排序
* #### 数组排序后的最大差值问题
```
 问题：
 给定一个数组，求如果排序之后，相邻两数的最大差值，要求时间复杂度O(N)，且要求不能用非基于比较的排序。
```

* #### 排序算法在工程中的应用
   * 综合排序：
      * 数组长度短（<60）：直接用插入排序，因为插入排序的常数项极低
      * 数组长度长；先判断数据类型
         * 基础类型：用快速排序，因为基础类型不用区分原始顺序（不需要稳定，因为相同值无差异）
         * 引用类型：用归并排序，因为原始顺序是有用的
# 三、栈、队列、链表、数组、矩阵结构及相关常见面试题
* ### 栈结构及面试
    * #### 题目一
      * 用数组结构实现大小固定的队列和栈
   
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
 
   * #### 题目二
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

   * #### 题目三
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
   * #### 题目四
        * 如何仅用栈结构实现队列结构？<br>
          * 原理：可以用两个栈（stack1和stack2）来实现队列 ，进入时放入stack1栈，出栈时从stack2栈出，这样就能把顺序变为先进先出,( 栈：push,pop,peek)
	  		* 图（1）：将队列中的元素“abcd”压入stack1中，此时stack2为空；<br>
				图（2）：将stack1中的元素pop进stack2中，此时pop一下stack2中的元素，就可以达到和队列删除数据一样的顺序了；<br>
				图（3）：可能有些人很疑惑，就像图3，当stack2只pop了一个元素a时，satck1中可能还会插入元素e,这时如果将stack1中的元				素e插入stack2中，在a之后出栈的元素就是e了，显然，这样想是不对的，我们必须规定当stack2中的元素pop完之后，也就是satck2为空时，再插入stack1中的元素。<br>
![](https://img-blog.csdn.net/20180527092623978)<br>
	  
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
* #### 队列结构及面试
* #### 链表结构及面试
* #### 数组结构及面试
* #### 矩阵结构及面试
* #### 二分搜索的扩展

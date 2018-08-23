
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
  * [堆结构与堆排序](#堆结构与堆排序)
  * [排序算法的稳定性](#排序算法的稳定性)
  * [比较器](#比较器)
  * [桶排序](#桶排序)
  * [计数排序](#计数排序)
  * [基数排序](#基数排序)
  * [数组排序后的最大差值问题](#数组排序后的最大差值问题)
  * [排序算法在工程中的应用](#排序算法在工程中的应用)


# 一、复杂度估计和算法排序（上）<br>
* #### 时间复杂度<br>
  * 时间复杂度为一个算法流程中，常数操作数量的指标。常用O（读作big O）来表示。具体来说，在常数操作数量的表达式中，只要高阶项，不要低阶项，也不要高阶项的  系数，剩下的部分如果记为f(N)，那么时间复杂度为O(f(N))。  
  * 常数时间的操作：一个操作如果和数据量没有关系，每次都是固定时间内完成的操作，叫做常数操作,O(1)。<br>
* #### 递归的时间复杂度估算-master公式<br>
```
    如果一个递归行为的时间复杂度公式为以下形式，即为master公式，可直接推出时间复杂度
    T(N) = a*T(N/b) + O(N^d)
    1) log(b,a) > d -> 复杂度为O(N^log(b,a))
    2) log(b,a) = d -> 复杂度为O(N^d * logN) 
    3) log(b,a) < d -> 复杂度为O(N^d)
```
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
            for(int j=0 ; j<i;j++){
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
            while(j>0 && arr[j]<arr[j-1]){
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
* #### 堆结构与堆排序
* #### 排序算法的稳定性
* #### 比较器
* #### 桶排序
* #### 计数排序
* #### 基数排序
* #### 数组排序后的最大差值问题
* #### 排序算法在工程中的应用

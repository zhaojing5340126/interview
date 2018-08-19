
* [一 、复杂度估计和排序算法（上）](#一、复杂度估计和排序算法（上）)
  * [时间复杂度](#时间复杂度)
  * [排序对数器](#排序对数器)
  * [冒泡排序 [O(N<sup>2</sup>),O(1)]](#冒泡排序)
  * [选择排序 [O(N<sup>2</sup>),O(1)]](#选择排序)
  * [插入排序 [O(N)-O(N<sup>2</sup>),O(1)]](#插入排序)
  * [归并排序 [O(NlogN),O(N)]](#归并排序)
* [二 、复杂度估计和排序算法（下）](#二、复杂度估计和排序算法（下）)

# 一、复杂度估计和算法排序（上）<br>
* #### 时间复杂度<br>
  * 时间复杂度为一个算法流程中，常数操作数量的指标。常用O（读作big O）来表示。具体来说，在常数操作数量的表达式中，只要高阶项，不要低阶项，也不要高阶项的  系数，剩下的部分如果记为f(N)，那么时间复杂度为O(f(N))。  
  * 常数时间的操作：一个操作如果和数据量没有关系，每次都是固定时间内完成的操作，叫做常数操作,O(1)。<br>
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
  * 时间复杂度：O(NlogN)
  * 额外空间复杂度：O(N)
  * 原理：利用递归的思想，分为左右分别排序，再对左右进行归并
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

# 二、复杂度估计和排序算法（下）

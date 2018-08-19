
* [一 、复杂度估计和排序算法（上）](#一、复杂度估计和排序算法（上）)
  * [时间复杂度](#时间复杂度)
  * [排序对数器](#排序对数器)
  * [冒泡排序](#冒泡排序)
  * [选择排序](#选择排序)
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
        int testTime = 5000;//测试次数
        int maxSize = 100;//数组大小
        int maxValue = 100;//值大小
        boolean succeed = true;//若不正确则返回false
        for (int i = 0; i < testTime; i++) {
            int[] arr1 = generateRandomArray(maxSize, maxValue);//生成大小和值大小都随机的数组
            int[] arr2 = copyArray(arr1);
            BublleSort.bublleSort(arr1);//自己编写的方法将arr1排序
            comparator(arr2);//一个绝对正确的方法将arr2排序
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
  * 冒泡排序时间复杂度为N的平方级<br>
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
  * 时间复杂度：N的平方
  * 原理：进行N轮，每轮把最小的元素放在最前面：0~N 找最小的元素放在0位置；1~N 找最小的元素放在1位置
  ```Java
  
  ```

# 二、复杂度估计和排序算法（下）

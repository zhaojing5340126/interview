
<!-- TOC -->

- [一、复杂度估计和算法排序（上）](#一复杂度估计和算法排序上)
    - [时间复杂度](#时间复杂度)
    - [递归的时间复杂度估算-master公式](#递归的时间复杂度估算-master公式)
    - [排序对数器](#排序对数器)
    - [冒泡排序[O(N<sup>2</sup>), O(1)，稳定]](#冒泡排序onsup2sup-o1稳定)
    - [选择排序[O(N<sup>2</sup>), O(1)，不稳定]](#选择排序onsup2sup-o1不稳定)
    - [插入排序[O(N)-O(N<sup>2</sup>), O(1)，稳定]](#插入排序on-onsup2sup-o1稳定)
    - [归并排序[O(NlogN), O(N)，可以稳定]](#归并排序onlogn-on可以稳定)
    - [归并排序的变体——小和问题和逆序对问题](#归并排序的变体小和问题和逆序对问题)
- [二、复杂度估计和排序算法（下）](#二复杂度估计和排序算法下)
    - [荷兰国旗问题[O(N), O(1)]](#荷兰国旗问题on-o1)
    - [经典快排和随机快排（基于荷兰国旗问题）[O(NlogN), O(logN)，不稳定]](#经典快排和随机快排基于荷兰国旗问题onlogn-ologn不稳定)
    - [堆结构](#堆结构)
    - [堆排序【O(NlogN)，O(1)，不稳定】](#堆排序onlogno1不稳定)
    - [桶排序 [O(N), O(N)]](#桶排序-on-on)
    - [计数排序](#计数排序)
    - [基数排序](#基数排序)
    - [数组排序后的最大差值问题](#数组排序后的最大差值问题)
    - [排序算法在工程中的应用](#排序算法在工程中的应用)
    - [排序的稳定性](#排序的稳定性)
    - [比较器](#比较器)
- [三、栈、队列、链表、数组、矩阵结构及相关常见面试题](#三栈队列链表数组矩阵结构及相关常见面试题)
    - [栈结构及面试](#栈结构及面试)
        - [题目一：用数组结构实现大小固定的队列和栈](#题目一用数组结构实现大小固定的队列和栈)
        - [题目二：能返回栈中最小元素的栈](#题目二能返回栈中最小元素的栈)
        - [题目三：仅用队列结构实现栈结构](#题目三仅用队列结构实现栈结构)
    - [队列结构及面试](#队列结构及面试)
        - [题目四：仅用栈结构实现队列结构](#题目四仅用栈结构实现队列结构)
        - [题目五：猫狗队列](#题目五猫狗队列)
    - [链表结构及面试](#链表结构及面试)
        - [基础：构建链表【包括基本的增删等】](#基础构建链表包括基本的增删等)
        - [题目八：反转单向和双向链表](#题目八反转单向和双向链表)
        - [题目十一：打印两个有序链表的公共部分【类似外排，荷兰国旗也用了外排】](#题目十一打印两个有序链表的公共部分类似外排荷兰国旗也用了外排)
        - [题目十二：判断一个链表是否为回文结构](#题目十二判断一个链表是否为回文结构)
        - [题目十三：将单向链表按某值划分成左边小、中间相等、右边大的形式【荷兰国旗】](#题目十三将单向链表按某值划分成左边小中间相等右边大的形式荷兰国旗)
        - [题目十四：复制含有随机指针节点的链表](#题目十四复制含有随机指针节点的链表)
        - [题目十五：两个单链表相交的一系列问题](#题目十五两个单链表相交的一系列问题)
            - [(1) 单链表是否有环](#1-单链表是否有环)
            - [(2) 两无环单链表是否相交](#2-两无环单链表是否相交)
            - [(3) 两有环单链表是否相交](#3-两有环单链表是否相交)
    - [数组结构及面试](#数组结构及面试)
    - [矩阵结构及面试——从宏观上实现（观察局部位置太复杂）](#矩阵结构及面试从宏观上实现观察局部位置太复杂)
        - [题目六：转圈打印矩阵【矩阵分圈处理】](#题目六转圈打印矩阵矩阵分圈处理)
        - [题目七：旋转正方形矩阵【矩阵分圈处理】](#题目七旋转正方形矩阵矩阵分圈处理)
        - [题目九：“之”字形打印矩阵【矩阵分斜线处理】](#题目九之字形打印矩阵矩阵分斜线处理)
        - [题目十：在行列都排好序的矩阵中找数【注意`都排好序`这个条件】](#题目十在行列都排好序的矩阵中找数注意都排好序这个条件)
- [四、二叉树](#四二叉树)
    - [题目一：实现二叉树的先序、中序、后序遍历，包括递归方式和非递归方式](#题目一实现二叉树的先序中序后序遍历包括递归方式和非递归方式)
        - [1 综： 递归思想**](#1-综-递归思想)
        - [1.1  先序、中序、后序遍历的递归版本](#11--先序中序后序遍历的递归版本)
        - [1.2先序、中序、后序遍历的非递归版本](#12先序中序后序遍历的非递归版本)
            - [1.2.1 先序【中左右】](#121-先序中左右)
            - [1.2.2 后序【左右中】【两个栈，由先序变来，先序中左右->中右左->左右中】](#122-后序左右中两个栈由先序变来先序中左右-中右左-左右中)
            - [1.2.3 中序【左中右】](#123-中序左中右)
    - [题目二：在二叉树中找到一个节点的后继节点](#题目二在二叉树中找到一个节点的后继节点)
    - [题目三、介绍二叉树的序列化和反序列化](#题目三介绍二叉树的序列化和反序列化)
    - [题目四、折纸问题](#题目四折纸问题)
    - [题目五、判断一棵二叉树是否是平衡二叉树](#题目五判断一棵二叉树是否是平衡二叉树)
        - [5.1 二叉树大套路：递归很好用【每个结点会来三次，不要去想什么先、中、后序，有没有打印递归都是存在的】](#51-二叉树大套路递归很好用每个结点会来三次不要去想什么先中后序有没有打印递归都是存在的)
        - [5.2 平衡二叉树：对任何一棵树，其左右子树高度差不超过1](#52-平衡二叉树对任何一棵树其左右子树高度差不超过1)
    - [题目六、判断一棵树是否是搜索二叉树、判断一棵树是否是完全二叉树](#题目六判断一棵树是否是搜索二叉树判断一棵树是否是完全二叉树)
        - [6.1 搜索二叉树：没有重复结点，对任何一节点，左子树都比它小，右子树都比它大](#61-搜索二叉树没有重复结点对任何一节点左子树都比它小右子树都比它大)
        - [6.2 完全二叉树：从左往右堆齐](#62-完全二叉树从左往右堆齐)
    - [题目七、已知一棵完全二叉树，求其节点的个数](#题目七已知一棵完全二叉树求其节点的个数)
- [五、哈希](#五哈希)
    - [模块一：认识哈希函数和哈希表](#模块一认识哈希函数和哈希表)
        - [1. 哈希函数：传入一个字符串，返回一个哈希码【十六进制，十六位，FFFF FFFF FFFF FFFF】](#1-哈希函数传入一个字符串返回一个哈希码十六进制十六位ffff-ffff-ffff-ffff)
            - [1.1 要求：](#11-要求)
            - [1.2 哈希函数特征](#12-哈希函数特征)
            - [1.3 如何实现1000个相互独立的哈希函数【规律不一样】 h= h1 + i * h2](#13-如何实现1000个相互独立的哈希函数规律不一样-h-h1--i--h2)
        - [2. 哈希表 【put(key,value), get(key), remove(key)】【复杂度数学上不是O(1)，但我们默认是 O(1),因为扩容次数很少，而且可以离线扩容】](#2-哈希表-putkeyvalue-getkey-removekey复杂度数学上不是o1但我们默认是-o1因为扩容次数很少而且可以离线扩容)
            - [2.1 离线扩容：用户有新数据，同步往新老结构放，使用get时，从老结构拿，直到扩容完毕，就启用新结构，废掉老结构【所以不需要让用户等】](#21-离线扩容用户有新数据同步往新老结构放使用get时从老结构拿直到扩容完毕就启用新结构废掉老结构所以不需要让用户等)
            - [2.2 开放地址法，拉链法，实际实现的时候是数组+红黑树](#22-开放地址法拉链法实际实现的时候是数组红黑树)
            - [2.3 应用：一个100T大文件，里面含字符串，要求你打印出所有重复的字符串](#23-应用一个100t大文件里面含字符串要求你打印出所有重复的字符串)
    - [题目一：设计RandomPool结构【默认哈希表的增、删、改、查都是 O(1)】](#题目一设计randompool结构默认哈希表的增删改查都是-o1)
    - [模块二：认识布隆过滤器【比特类型的一个Map，可以把所需内存压得很小】：解决爬虫驱虫问题、黑名单问题【检查是否处于这个集合的问题】](#模块二认识布隆过滤器比特类型的一个map可以把所需内存压得很小解决爬虫驱虫问题黑名单问题检查是否处于这个集合的问题)
        - [1. 举例：黑名单问题](#1-举例黑名单问题)
            - [1.1 将某个URL加入黑名单，将100亿个URL加入黑名单](#11-将某个url加入黑名单将100亿个url加入黑名单)
            - [1.2. 查某个URL是否在黑名单](#12-查某个url是否在黑名单)
            - [1.3 至少准备多大的容器：m = - (n * lnP) / (ln2)<sup>2</sup> 【由样本量和预期失误率决定】](#13-至少准备多大的容器m----n--lnp--ln2sup2sup-由样本量和预期失误率决定)
            - [1.4 计算需要多少个哈希函数 ：k = ln2 * (m / n)](#14-计算需要多少个哈希函数-k--ln2--m--n)
            - [1.5 我设计了 m 大小容器，k 个哈希函数，这种设计的真实失误率： （1 - e<sup>-(n*k)/m）</sup>）<sup>k</sup>](#15-我设计了-m-大小容器k-个哈希函数这种设计的真实失误率-1---esup-nkmsupsupksup)
    - [模块三：认识一致性哈希【是一种服务器设计】和虚拟节点技术](#模块三认识一致性哈希是一种服务器设计和虚拟节点技术)
        - [3.1 经典的服务器的抗压结构](#31-经典的服务器的抗压结构)
            - [3.1.1 场景：放数据，查数据，这个操作很频繁，怎样让它负载均衡](#311-场景放数据查数据这个操作很频繁怎样让它负载均衡)
            - [3.1.2 问题：当我想要加机器或者减机器的时候，这个结构就死了，跟哈希表扩容一样，数据需要全部重新分布【因为m变了，取余m 就肯定不一样了】](#312-问题当我想要加机器或者减机器的时候这个结构就死了跟哈希表扩容一样数据需要全部重新分布因为m变了取余m-就肯定不一样了)
        - [3.2 升级：一致性哈希结构 + 虚拟结点技术：在实现负载均衡的同时，把数据迁移的代价变得很低【有效地解决分布式存储结构下动态增加和删除节点所带来的问题】](#32-升级一致性哈希结构--虚拟结点技术在实现负载均衡的同时把数据迁移的代价变得很低有效地解决分布式存储结构下动态增加和删除节点所带来的问题)
            - [3.2.1 一致性哈希原理：](#321-一致性哈希原理)
            - [3.2.2 当缓存的节点有增加或删除的时候，一致性哈希的优势就显现出来了。](#322-当缓存的节点有增加或删除的时候一致性哈希的优势就显现出来了)
            - [3.2.2 一致性哈希算法引入了 虚拟节点 【迁移代价很低+负载均衡】](#322-一致性哈希算法引入了-虚拟节点-迁移代价很低负载均衡)
    - [模块四：认识并查集结构【时间复杂度：O(1)】](#模块四认识并查集结构时间复杂度o1)
        - [4.1 并查集功能【isSameSet，union】](#41-并查集功能issamesetunion)
            - [1） isSameSet:非常快的检查两个元素是否属于一个集合](#1-issameset非常快的检查两个元素是否属于一个集合)
            - [2） union: 将两个元素所属的集合合并在一起](#2-union-将两个元素所属的集合合并在一起)
            - [3） findHead时优化：任何一次查找头结点过程结束后，把这一条路径上的每个结点的父节点都改为头结点](#3-findhead时优化任何一次查找头结点过程结束后把这一条路径上的每个结点的父节点都改为头结点)
            - [4） 注意：](#4-注意)
                - [1.并查集结构必须一次性把数据给它，不支持流操作](#1并查集结构必须一次性把数据给它不支持流操作)
                - [2.并查集结构用两个HashMap来实现是最优的](#2并查集结构用两个hashmap来实现是最优的)
                - [3.样本为N，只要查询次数+合并次数 逼近于O(N)，则单次查询或合并的平均时间复杂度为O(1),即常数时间。](#3样本为n只要查询次数合并次数-逼近于on则单次查询或合并的平均时间复杂度为o1即常数时间)
        - [4.2 实现并查集](#42-实现并查集)
    - [题目二：岛问题 【矩阵很大时，用分治】](#题目二岛问题-矩阵很大时用分治)
- [六、前缀树、堆结构、贪心算法](#六前缀树堆结构贪心算法)
    - [1、前缀树Prefix Tree）,又叫字典树、Trie树，单词查找树或键树，是一种多叉树结构](#1前缀树prefix-tree又叫字典树trie树单词查找树或键树是一种多叉树结构)
        - [1.1 前缀树题目举例：一个字符串类型的数组arr1，另一个字符串类型的数组arr2。](#11-前缀树题目举例一个字符串类型的数组arr1另一个字符串类型的数组arr2)
            - [1.1.1 arr2中有哪些字符，是arr1中出现的？请打印](#111-arr2中有哪些字符是arr1中出现的请打印)
            - [1.1.2 arr2中有哪些字符，是作为arr1中某个字符串前缀出现的？请打印](#112-arr2中有哪些字符是作为arr1中某个字符串前缀出现的请打印)
            - [1.1.3 arr2中有哪些字符，是作为arr1中某个字符串前缀出现的？请打印arr2中出现次数最大的前缀。](#113-arr2中有哪些字符是作为arr1中某个字符串前缀出现的请打印arr2中出现次数最大的前缀)
        - [1.2 前缀树的insert、delete、search、prefixNum方法代码:代码相似度很高，前半部分基本一样，都是从root开始遍历](#12-前缀树的insertdeletesearchprefixnum方法代码代码相似度很高前半部分基本一样都是从root开始遍历)
    - [2、堆结构的扩展与应用](#2堆结构的扩展与应用)
    - [3、介绍贪心算法及相关题目](#3介绍贪心算法及相关题目)
        - [题目七：按最低字典序拼接字符串](#题目七按最低字典序拼接字符串)
    - [4、在面试中如何快速地尝试出贪心策略](#4在面试中如何快速地尝试出贪心策略)
- [七、暴力递归和动态规划](#七暴力递归和动态规划)
    - [1. 递归](#1-递归)
    - [2. 动态规划](#2-动态规划)
    - [3. 如何把暴力递归套路变为动态规划](#3-如何把暴力递归套路变为动态规划)

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


## 冒泡排序[O(N<sup>2</sup>), O(1)，稳定]

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
## 选择排序[O(N<sup>2</sup>), O(1)，不稳定]

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
 
## 插入排序[O(N)-O(N<sup>2</sup>), O(1)，稳定]

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
## 归并排序[O(NlogN), O(N)，可以稳定]

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
  
## 经典快排和随机快排（基于荷兰国旗问题）[O(NlogN), O(logN)，不稳定]
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
## 堆排序【O(NlogN)，O(1)，不稳定】
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


## 桶排序 [O(N), O(N)]

   * 此为非基于比较的排序，与被排序的样本的实际数据状况很有关系，所以实际中并不经常使用，是稳定的排序
   * 时间复杂度：O(N)
   * 额外空间复杂度：O(N)
   * 桶排序：分为计数排序和基数排序，计数排序就是N个数构造N个桶，遍历数组，对相应的桶不断+1

## 计数排序
* 暂未
## 基数排序
* 暂未
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
### 基础：构建链表【包括基本的增删等】
### 题目八：反转单向和双向链表
* 【题目】 分别实现反转单向链表和反转双向链表的函数。<br>
【要求】 如果链表长度为N，时间复杂度要求为O(N)，额外空间
复杂度要求为O(1)
* 反转单向链表
    * 【分析】从头到尾一个结点一个结点的挨个处理：将当前结点（head） 和下一个结点断开，指向前一个结点
```Java
package day4;

public class ReverseLikedlist {
    public static class Node{       //结点的定义
        public int value;
        public Node next;
        public Node(int value){
            this.value=value;
        }
    }
    public static Node revserSingleList(Node head){ //反转单向链表
        Node pre = null;
        Node next = null;
        while (head != null){
            next = head.next;
            head.next=pre;  //将当前结点（head）与下一个结点断开指向前一个结点
            pre = head;    //当前结点就是下一个当前结点的前一个结点
            head = next;       //头指针指向下一个结点，开始处理下一个结点
        }
        return pre;
    }

    public static void printLinkedlist(Node head){//打印单项链表
        System.out.println("Linkedlist: ");
        while (head != null){
            System.out.print(head.value + " ");
            head = head.next;
        }
        System.out.println();
    }

   public static void main(String[] args) {//测试
        Node head=new Node(1);
        head.next = new Node(2);
        head.next.next = new Node(3);
        printLinkedlist(head);
        head=revserSingleList(head);
        printLinkedlist(head);
    }
}

```
* 反转双向链表
    * 【分析】从头到尾一个结点一个结点的挨个处理：对每个结点，交换其next和last即可，并记录当前的引用（仅仅为了最后的返回）
```Java
package day4;

public class ReverseDoubleLinkedlist {
    public static class DoubleNode{
        int value;
        DoubleNode last;
        DoubleNode next;
        public DoubleNode(int value){
            this.value = value;
        }
    }

    public static DoubleNode reverseDoubleLinkedList(DoubleNode head){
        DoubleNode temp = null;
        DoubleNode pre = null;
        while (head != null ){
            temp = head.next;
            head.next = head.last;
            head.last = temp;
            pre = head;     //pre的作用仅仅是记录head，因为我们要返回head，但最后head是null，所以需要一个变量来记录
            head = temp;
        }
        return pre;
    }

    public static void printDoubleLinkedList(DoubleNode head) {
        System.out.print("Double Linked List: ");
        DoubleNode end = null;
        while (head != null) {
            System.out.print(head.value + " ");
            end = head;
            head = head.next;
        }
        System.out.print("| ");
        while (end != null) {
            System.out.print(end.value + " ");
            end = end.last;
        }
        System.out.println();
    }

    public static void main(String[] args) {
        DoubleNode head2 = new DoubleNode(1);
        head2.next = new DoubleNode(2);
        head2.next.last = head2;
        head2.next.next = new DoubleNode(3);
        head2.next.next.last = head2.next;
        head2.next.next.next = new DoubleNode(4);
        head2.next.next.next.last = head2.next.next;
        printDoubleLinkedList(head2);
        printDoubleLinkedList(reverseDoubleLinkedList(head2));

    }
}

```
### 题目十一：打印两个有序链表的公共部分【类似外排，荷兰国旗也用了外排】
* 【题目】 给定两个有序链表的头指针head1和head2，打印两个链表的公共部分。
```Java
package day4;

public class PrintCommonPart {
    public static class Node{       //结点定义
        int value;
        Node next;
        public Node(int value){
            this.value = value;
        }
    }
    public static void printCommonPart(Node head1,Node head2){
        System.out.println("print common part: ");
        while (head1 !=null && head2 != null){  //外排
            if(head1.value < head2.value){
                head1=head1.next;
            }else if(head1.value > head2.value){
                head2=head2.next;
            }else {
                System.out.print(head1.value + " ");
                head1=head1.next;
                head2=head2.next;
            }
        }
    }
}
```

### 题目十二：判断一个链表是否为回文结构
* 【题目】 给定一个链表的头节点head，请判断该链表是否为回文结构。 例如： 1->2->1，返回true。 1->2->2->1，返回true。15->6->15，返回true。 1->2->3，返回false。<br>
进阶： 如果链表长度为N，时间复杂度达到O(N)，额外空间复杂度达到O(1)。
* 【分析】回文：理解1：原序和逆序一样 。  理解2：能从中间对折
    * isPalindrome1非进阶版：因为不需要空间O(1),所以可以增加一个额外的空间O(N)，我们先遍历一次链表，把每个结点的值放入一个栈中，那么值弹出时就是原链表的逆序，所以只需要每弹出一个比较一个即可，若有不一样的，则直接返回false，否则就是回文
    * isPalindrome2进阶版：空间复杂度O(1),总体上是先找到链表中点，再把后半部分反转，然后前后比较，一样则是回文。
        * 找中点用了快指针（一次两步）和慢指针（一次一步），若为偶数，慢指针指向两个中点的前一个，若为奇数，慢指针指向正中位置。
        * 判断完回文后要把后半部分再反转回去，不能说别人给你的数据，你把结构给别人改变了
```Java
package day4;

import java.util.Stack;

public class IsPalindromList {
    public static class Node{
        int value;
        Node next;
        public Node(int value){
            this.value=value;
        }
    }

    public static boolean isPalindrome1(Node head){    //用了栈，空间复杂度O(N)
        Node cur = head ;
        Stack<Node> stack = new Stack<>();
        while (cur != null){        //第一次遍历链表，存放于stack中
            stack.push(cur);
            cur=cur.next;
        }
        cur = head;
        while (cur != null){        //第二次遍历链表，与stack弹出【逆序】的进行比较
            if(cur.value != stack.pop().value){
                return false;
            }
            cur = cur.next;
        }
        return true;
    }

    public static boolean isPalindrome2(Node head){
        if(head == null || head.next==null){   //没有或只有一个时为回文
            return true;
        }
        Node cur = head;
        Node fast = head;
        Node slow = head;
        /** //慢指针一次一步，最后会走到中间，若为偶数，走到中间的前一位，若为奇数，走到正中间*/
        while (fast.next !=null && fast.next.next!=null){
            fast = fast.next.next;  //快指针一次走两步,若fast.next!=null,那么说明这是偶数个，快指针最后达到的位置是末尾的前一位
            slow = slow.next;
        }
        Node end = revserSingleList(slow); //slow到达的是中点位置，反转后半部分，反转后中点指向的是null
        fast = end;
        while (cur != null && fast != null){  //将前半部分与后半部分折叠对比
            if(cur.value != fast.value){
                return false;
            }
            cur = cur.next;
            fast = fast.next;
        }
        cur = revserSingleList(end); //不能改变原来给的数据结构，所以最后还要把后半部分反转回去
        return true;
    }

    public static Node revserSingleList(Node head){
        Node pre = null;
        Node next = null;
        while (head != null){
            next = head.next;
            head.next=pre;
            pre = head;
            head = next;
        }
        return pre;
    }
}
```

### 题目十三：将单向链表按某值划分成左边小、中间相等、右边大的形式【荷兰国旗】
* 【题目】 给定一个单向链表的头节点head，节点的值类型是整型，再给定一个
整数pivot。实现一个调整链表的函数，将链表调整为左部分都是值小于 pivot
的节点，中间部分都是值等于pivot的节点，右部分都是值大于 pivot的节点。
除这个要求外，对调整后的节点顺序没有更多的要求。 例如：链表9->0->4->5->1，pivot=3。 调整后链表可以是1->0->4->9->5，也可以是0->1->9->5->4。总
之，满足左部分都是小于3的节点，中间部分都是等于3的节点（本例中这个部
分为空），右部分都是大于3的节点即可。对某部分内部的节点顺序不做要求。
* 进阶： 在原问题的要求之上再增加如下两个要求。
    * 在左、中、右三个部分的内部也做顺序要求，要求每部分里的节点从左到右的
顺序与原链表中节点的先后次序一致。 例如：链表9->0->4->5->1，pivot=3。
调整后的链表是0->1->9->4->5。 在满足原问题要求的同时，左部分节点从左到
右为0、1。在原链表中也是先出现0，后出现1；中间部分在本例中为空，不再
讨论；右部分节点 从左到右为9、4、5。在原链表中也是先出现9，然后出现4，
最后出现5。
    * 如果链表长度为N，时间复杂度请达到O(N)，额外空间复杂度请达到O(1)。

* 【分析】：
    * listPartition1非进阶版：不需要稳定性和空间复杂度。所以可以遍历一遍链表存放在数组中，然后就是荷兰国旗问题，将数组划分为三段less，equal，more，再将数据转移到链表中。
    * listPartition2进阶版：要求稳定性和空间复杂度O(1)，遍历链表时less,equal,more指向它们第一次出现的结点，用endless,endequal,endmore来添加结点，属于哪一个就加在哪一个的尾巴后面【保证了稳定性，因为less，equal，more指向的是各自范围出现的第一个】，最后再将less，equa，more三个链表链接在一起就可以了。
```Java
package day4;

public class SmallEqualBigger {
    public static class Node{
        int value;
        Node next;
        public Node(int value){
            this.value=value;
        }
    }

    public static Node listPartition1(Node head , int num){  //先放入数组再进行划分的方法
        while (head==null || head.next==null)
            return head;
        int i=0;
        Node cur = head;
        while (cur != null){  //计算有多少个结点
            i++;
            cur = cur.next;
        }
        int[] arr = new int[i]; //建立和链表一样大的数组
        cur = head;
        i = 0;
        while (cur != null){    //将链表的值复制到数组
            arr[i++] = cur.value;
            cur = cur.next;
        }
        arrPartiton(arr , num); //在数组中用荷兰国旗的方法对值进行小、等于、大的划分
        cur = head;
        i=0;
        while (cur != null){    //将划分好的数值放入链表
            cur.value = arr[i++];
            cur = cur.next;
        }
        return head;            //返回划分好的链表
    }

    private static void arrPartiton(int[] arr, int num){
        int less = -1;
        int more = arr.length;
        int cur = 0;
        while (cur != more){       //less指向小于区最前方的位置，more指向大于区最前方的位置，cur=more，说明所有的数都排过了
            if(arr[cur] < num ){
                swap(arr , ++less, cur++);
            }else if(arr[cur] > num){
                swap(arr , cur , --more);
            }else {
                cur++;      //等于时不动
            }
        }
    }

    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```
* 进阶版
```Java

    public static Node listPartition2(Node head, int num ) {
        Node less = null;      //存放小于num的结点
        Node equal = null;
        Node more = null;
        Node endless = null;       //指向各自那条链表的最后一位
        Node endequal = null;
        Node endmore = null;
        Node next = null;
        while (head != null) {
            next = head.next;
            head.next = null;   //*****那么每次加入的结点都是指向null，就是less，equal，more末尾结点
            if (head.value < num) {     //若小于num，则放在less链表中
                if (less == null) {
                    less = head;
                    endless = head;
                } else {
                    endless.next = head;
                    endless = head;
                }
            } else if (head.value > num) {    //若大于num，则放在more链表中
                if (more == null) {
                    more = head;
                    endmore = head;
                } else {
                    endmore.next = head;
                    endmore = head;
                }
            } else {                         //若等于num，则放在equal链表中
                if (equal == null) {
                    equal = head;
                    endequal = head;
                } else {
                    endequal.next = head;
                    endequal = head;
                }
            }
            head = next;
        }

        /** 返回划分好的链表 */
        if (less != null){      //less链表存在
            endless.next = equal;
            if(equal != null){  //equal链表存在
                endequal.next = more;
            }else {             //less存在，equal链表不存在
                endless.next = more;
            }
            return less;
        }else {                 //less不存在
            if (equal != null){ //less不存在，equal存在
                endequal.next = more;
                return equal;    //返回equal
            }else {             //less不存在，equal不存在
                return more;      //返回more
            }
        }
    }
```
### 题目十四：复制含有随机指针节点的链表
* 【题目】 一种特殊的链表节点类描述如下：
```
public class Node { 
    public int value; 
    public Node next; 
    public Node rand;
    public Node(int data) {
         this.value = data;
     }
}
    Node类中的value是节点值，next指针和正常单链表中next指针的意义一样，都指向下一个节点，rand指针是Node类中新增的指针，这个指针可能指向链表中的任意一个节点，也可能指向null。 给定一个由Node节点类型组成的无环单链表的头节点head，请实现一个函数完成这个链表中所有结构的复制，并返回复制的新链表的头节点。 
进阶：不使用额外的数据结构，只用有限几个变量，且在时间复杂度为 O(N)内完成原问题要实现的函数。
```
* 【分析】：
    * copyListWithRand1非进阶版：利用一个hashmap实现原链表结点和复制结点的映射，然后就可以把结构关系复制下来了。
    * copyListWithRand2进阶版：因为要求不使用额外的数据结构，即不能用hashmap，只用链表，步骤如下：
        * 1、复制结点到链表，成为1->1'->2->2'->3->3'->...->null形式；
        * 2、复制rand结构
        * 3、将链表拆分出来，得到原链表和复制链表。

```Java
import java.util.HashMap;

public class CopyListWithRandom {
    public static class Node{
        int value;
        Node rand;
        Node next;
        public Node(int value){
            this.value = value;
        }
    }
    public static Node copyListWithRand1(Node head){     //利用hashmap来进行元列表结点和复制结点的映射
        HashMap<Node,Node> map = new HashMap<Node ,Node>();
        Node cur = head;
        while (cur != null){        //第一次遍历，形成结点与复制结点的映射
            map.put(cur, new Node(cur.value));
            cur = cur.next;
        }
        cur = head;
        while (cur != null){        //第二次遍历，复制结点间的关系，包括next和rand
            map.get(cur).next = map.get(cur.next);
            map.get(cur).rand = map.get(cur.rand);
            cur = cur.next;         //****
        }
        return map.get(head);       //返回复制列表的头结点
    }
}
```
* 进阶版
```Java
public static Node copyListWithRand2(Node head){
        if(head == null){
            return null;
        }
        Node cur = head;
        Node temp = null;
        while (cur != null){  //复制结点构建为1->1'->2->2'->3->3'->...->null形式
            temp = cur.next;
            cur.next = new Node(cur.value);
            cur.next.next=temp;
            cur = cur.next.next;
        }
        cur = head;
        Node curCopy = head.next;
        while (cur != null){        //复制rand结构
            curCopy = cur.next;     //****不要遗忘了
            curCopy.rand = (cur.rand == null) ? null : cur.rand.next;
            cur = cur.next.next;
        }
        Node headCopy = head.next;
        cur = head;
        while (cur != null){        //拆分链表
            curCopy = cur.next;
            cur.next = cur.next.next;
            curCopy.next = curCopy.next == null ? null :  curCopy.next.next;
            cur = cur.next;
        }
        return headCopy;
    }
```
### 题目十五：两个单链表相交的一系列问题
```
【题目】 在本题中，单链表可能有环，也可能无环。给定两个单链表的头节点 head1和head2，这两个链表可能相交，也可能不相交。请实现一个函数， 如果两个链表相交，请返回相交的第一个节点；如果不相交，返回null 即可。 <br>

进阶：要求：如果链表1的长度为N，链表2的长度为M，时间复杂度请达到 O(N+M)，额外空间复杂度请达到O(1)。

```
* 【分析】：这道题实际是三道题的综合，要解决以下三个问题：【以下是基础版，进阶版（不使用HashSet）比较玄幻，咱策略性放弃】
    * (1)、单链表是否有环，有环则返回入环结点，否则null
    * (2)、两无环单链表是否相交，相交则返回相交的第一个结点，否则null
    * (3)、两有环单链表是否相交，相交则返回相交的第一个结点，否则null
* 主函数：
```Java
import sun.java2d.pipe.LoopBasedPipe;

import java.util.HashSet;

public class FindFirstIntersectNode {
    public static class Node{
        int value;
        Node next;
        public Node(int value){
            this.value = value;
        }
    }

    /*主函数*/
    public static Node findFirstIntersectNode(Node head1,Node head2){
        if (head1 == null || head2 == null){
            return null;
        }
        Node loop1 = getLoopNode(head1);    //判断单链表是否有环：若有环，则返回入环结点，无环则返回null
        Node loop2 = getLoopNode(head2);
        if(loop1 == null && loop2 == null){ //两个都是无环单链表
            return noLoop(head1 , head2);       //无环单链表是否相交：相交返回相交的第一个结点，不相交返回null
        }else if (loop1 != null && loop2 != null){  //两个都是有环单链表
            return bothLoop(head1,loop1,head2,loop2); //两个有环单链表是否相交：相交返回结点，不相交返回null
        }
        return null;        //一个有环，一个无环，不可能相交，返回null
    }
}
```
#### (1) 单链表是否有环
```Java
/*单链表是否有环，有环则返回入环结点，否则null*/
public static Node getLoopNode(Node head){
        HashSet<Node> set = new HashSet<>();
        while (head != null){
            if (!set.contains(head)){   //若结点不在set里，则放入set
                set.add(head);      //*****记住是add，不是push
                head = head.next;
            }else {             //结点在set里面说明之前已经遇见过了，即有环，此为入环结点
                return head;
            }
        }
        return null;    //直到遍历完后都没有遇到重复的，说明无环
    }
```
#### (2) 两无环单链表是否相交
```Java
    /** 判断两个无环单链表是否相交：相交返回相交的第一个结点，不相交返回null*/
    public static Node noLoop(Node head1 ,Node head2){
        HashSet<Node> set = new HashSet<>();
        while (head1 != null){  //将head1链表放入set中
            set.add(head1);
            head1 = head1.next;
        }
        while (head2 != null){  //遍历head2，看是否与head1有重复的结点，有则返回，即为第一个相交的结点
            if (set.contains(head2)){
                return head2;
            }
            head2 = head2.next;
        }
        return null;        //遍历完head2都没有与head1有重复的结点，说明不相交
    }
```

#### (3) 两有环单链表是否相交
* 【分析】：两有环单链表相交的可能情况如下，若不相交，则是不相干的两个 6 6 。
![](https://github.com/zhaojing5340126/interview/blob/master/picture/%E6%9C%89%E7%8E%AF.gif?raw=true)
```Java
    /** 两个有环单链表是否相交： */
    public static Node bothLoop(Node head1, Node loop1, Node head2, Node loop2){
        // 即情况二，那么在环的上方可归结为无环单链表找相交点
        if (loop1 == loop2){ 
            HashSet<Node> set = new HashSet<>();
            while (head1 != loop1){ //将head1环上部分放入set
                set.add(head1);
                head1 = head1.next;
            }
            while (head2 != loop2){ //将head2环上部分遍历，与head1比较
                if (set.contains(head2)){   //在head1中找到一样的，则即为第一个相交点
                    return head2;
                }
                head2 = head2.next;
            }
            return loop1;   //若直到遍历完环上部分都没有重复的，说明入环点loop1==loop2即为第一个相交点
        }
        
        //即情况一或者情况三【6 6 形式】
        else {
            Node cur = loop1.next;
            while (cur != loop1){       //cur从loop1开始向下遍历，若直到再次返回loop1都没有遇见loop2，说明二者不相交
                if (cur == loop2){
                    return loop1;       //遇见loop2，则说明相交，为情况一
                }
                cur = cur.next;
            }
            return null;        //cur遍历完它自己那个环都没遇见loop2 ，说明不相交，为6 6 这种形式
        }
    }
```

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

### 题目七：旋转正方形矩阵【矩阵分圈处理】
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

### 题目九：“之”字形打印矩阵【矩阵分斜线处理】
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

### 题目十：在行列都排好序的矩阵中找数【注意`都排好序`这个条件】
![](https://github.com/zhaojing5340126/interview/blob/master/picture/%E8%A1%8C%E5%88%97%E9%83%BD%E6%8E%92%E5%A5%BD%E5%BA%8F%E7%9F%A9%E9%98%B5.jpg?raw=true)
* 【分析】：从第一行的最后一个数开始查询，当前查询的数记作a(i,j)，因为是排好序的，所以：
    * 若a<k，则a所在行左边的数都一定小于k，所以a向下移动；
    * 若a>k，则a所在列的下方的数一定都大于k，所以a向左移动;
    * 否则，a=k，返回true;
    * 若查完了矩阵都没有返回，说明k不在矩阵中，返回false。

```Java
package day4;

public class SearchInSortedMatrix {
    public static boolean isContains(int[][] matrix, int k){
        int i=0;
        int j=matrix[0].length-1;
        while (i<matrix.length && j>-1){
            if (matrix[i][j] < k){
                i++;
            }else if(matrix[i][j] > k){
                j--;
            }else {
                return true;
            }
        }
        return false;
    }

    //测试
    public static void main(String[] args) {
        int[][] matrix=new int[][]{{1,2,3,4},{6,67,543,765},{4543,454,35,66}};      //***记住数组是怎么创建的
        int k=7;
        System.out.println(isContains(matrix,k));
    }
}
    
```
# 四、二叉树
## 题目一：实现二叉树的先序、中序、后序遍历，包括递归方式和非递归方式
### 1 综： 递归思想**
* 【分析】：必须想清楚你这一层要返回给你的上一层什么东西，这些信息需要同样的格式
    * 左交给我信息
    * 右交给我信息
    * 我要交给上一层什么信息

* 实际遍历的时候三者是一样的，每个结点会遇见3次，先序、中序、后序遍历只是选择在哪里打印
### 1.1  先序、中序、后序遍历的递归版本
```Java
    public static class Node{
        public int value;
        public Node left;
        public Node right;
        public Node(int value){
            this.value = value;
        }
    }

    //递归版本【递归打印】
        public static void preOrderRecur(Node head){
        if(head == null)
            return;
        System.out.print(head.value + " ");//先序
        preOrderRecur(head.left);
        preOrderRecur(head.right);
    }

    public static void inOrderRecur(Node head){
        if (head == null)
            return;
        inOrderRecur(head.left);
        System.out.print(head.value + " "); //中序
        inOrderRecur(head.right);
    }

    public static void posOrderRecur(Node head){
        if (head == null)
            return;
        posOrderRecur(head.left);
        posOrderRecur(head.right);
        System.out.print(head.value + " " );//后序
    }
```
### 1.2先序、中序、后序遍历的非递归版本
#### 1.2.1 先序【中左右】
* 【分析】：处理当前结点，有右先压右，有左再压左，那么这样弹出就会是先左，再右
    * 为什么使用栈：二叉树只有从上到下的路径，所以需要一个结构让他回去，只有栈【队列只能从上到下，回不去】
```Java
 public static void preOrderUnRecur(Node head){
        System.out.println("pre - order:");
        if (head == null)
            return;
        Stack<Node> stack = new Stack<Node>();
        stack.push(head);
        while (!stack.isEmpty()){
            head = stack.pop(); //先处理当前结点
            System.out.print(head.value + " ");
            if (head.right != null){ //有右先压右，有左再压左，那么弹出的时候就会是先左，再右了
                stack.push(head.right);
            }
            if(head.left != null){
                stack.push(head.left);
            }
        }
        System.out.println();
    }
```
#### 1.2.2 后序【左右中】【两个栈，由先序变来，先序中左右->中右左->左右中】
* 【分析】：先序中左右，先处理当前结点，然后改为先压左孩子，再压右孩子，那么弹出的顺序就成为了右左【中右左】，利用一个栈即可变为左右中
```Java
  public static void posOrderUnRecur(Node head){//在先序遍历【中左右】的基础上改，中左右->中右左->利用一个新栈变成左右中
        System.out.println("post-order:");
        if (head == null)
            return;
        Stack<Node> s1= new Stack<Node>();
        Stack<Node> s2= new Stack<Node>(); //用于将 中右左 变为 左右中【后序】
        s1.push(head);
        while (!s1.isEmpty()){
            head = s1.pop();
            s2.push(head);  //***不是直接打印，而是放入栈中
            if (head.left != null) //***和先序相反
                s1.push(head.left);
            if (head.right != null) //**
                s1.push(head.right);
        }
        while (!s2.isEmpty())
            System.out.print(s2.pop().value + " ");
    }
```

#### 1.2.3 中序【左中右】
* 【分析】：压一绺左边界，再再从尾端依次往外弹，弹出每个结点，再去遍历其右孩子【压右孩子的左边界】的过程就模拟了左中右这个过程 
![](https://github.com/zhaojing5340126/interview/blob/master/picture/%E4%B8%AD%E5%BA%8F.jpg?raw=true)
```Java
   public static void inOrderUnRecur(Node head){
        System.out.println("in-Order:");
        if (head == null)
            return;
        Stack<Node> stack = new Stack<Node>();
        /** 压一绺左边界，再再从尾端依次往外弹，弹出每个结点，再去遍历其右孩子的过程就模拟了左中右这个过程 */
        while (!stack.isEmpty() || head != null) { //head!=null只是用于刚开始stack为空的时候，因为它没有像别的那样在循环前就push了head
            if (head != null){
                while (head != null) {//当前结点不为空，当前结点压入栈，当前结点往左移动【一压就压一斜绺】
                    stack.push(head);
                    head = head.left;
                }
            }else {  //当前结点为空说明上面已经压完一绺了，弹出结点（中点），再处理右边
                head = stack.pop();
                System.out.print(head.value + " ");
                head = head.right;
            }
        }
        System.out.println();

    }
```

## 题目二：在二叉树中找到一个节点的后继节点
```
【题目】 现在有一种新的二叉树节点类型如下：
public class Node { 
    public int value; 
    public Node left;
    public Node right;
    public Node parent;
    public Node(int data) { this.value = data; }
}
二叉树节点结构多了一个指向父节点的parent指针。假设有一 棵Node类型的节点组成的二叉树，
树中每个节点的parent指针都正确地指向 自己的父节点，头节点的parent指向null。
只给一个在二叉树中的某个节点 node，请实现返回node的后继节点的函数。
在二叉树的中序遍历的序列中，node的下一个节点叫作node的后继节点。
```
![](https://github.com/zhaojing5340126/interview/blob/master/picture/%E5%90%8E%E7%BB%A7%E7%BB%93%E7%82%B9.png?raw=true)

```Java
package day5;

public class SuccessorNode {
    public static class Node{ //带parent指针的结点定义
        private Node left;
        private Node right;
        private Node parent;
        private int value;
        public Node(int value){
            this.value = value;
        }
    }

    public static Node getSuccessorNode(Node node){
        if (node == null) 
            return null; //不能是return；因为要求有返回值
        if (node.right != null){
            //情况一，如果有右子树，后继节点是右子树最左边的结点
            return getLeftMost(node.right);
        }else {  
            //情况二，没有右子树，向上查找我属于哪个结点的左子树，该节点即为后继节点，整棵树只有最后一个结点没有后继结点，会查找到根节点的父节点null
            Node parent = node.parent;
            while (parent != null && parent.left != node){ 
            //parent！=null是为最后一个结点设置的，若parent==null，说明是最后一个结点，其后继结点为null
                node = parent;
                parent = node.parent;
            }
            return parent;
        }
    }

    private static Node getLeftMost(Node node) {
        if (node == null)
            return node;  
        while (node.left != null)
            node = node.left;
        return node;
    }
    
}
```
## 题目三、介绍二叉树的序列化和反序列化
* 【分析】：序列化：怎么记录下来（包括结构这些），反序列化：怎么还原结构。
    * “_”：用于分开字符
    * “#”：用于表示尾端null
* 【技巧】：利用递归，怎么序列化就用同样的方式反序列化【先序、中序、后序都是一个套路】
```Java
package day5;

import testTools.Node;
import testTools.PrintTree;
import java.util.LinkedList;
import java.util.Queue;

public class SerializeAndReconstruct {
    
    public static String serialByPre(Node head){ //利用先序来进行序列化
        if (head == null)
            return "#_";
        String res = head.value + "_";
        res += serialByPre(head.left);
        res += serialByPre(head.right);
        return res;
    }

    public static Node reconByPreString(String s){ //反序列化
        String[] values = s.split("_"); //*****
        Queue<String> queue = new LinkedList<String>();
        for (String i: values){
            queue.add(i);
        }
        return reconByPreString( queue);
    }

    private static Node reconByPreString(Queue<String> queue) {
        String s = queue.poll();//queue利用poll
        if (s.equals("#")){ //不能用s==“#”
            return null;
        }
        Node node = new Node(Integer.valueOf(s));//*****
        node.left = reconByPreString(queue);
        node.right = reconByPreString(queue);
        return node;
    }
    
}
```
## 题目四、折纸问题
【题目】 请把一段纸条竖着放在桌子上，然后从纸条的下边向
上方对折1次，压出折痕后展开。此时 折痕是凹下去的，即折痕
突起的方向指向纸条的背面。如果从纸条的下边向上方连续对折
2 次，压出折痕后展开，此时有三条折痕，从上到下依次是下折
痕、下折痕和上折痕。
给定一 个输入参数N，代表纸条都从下边向上方连续对折N次，
请从上到下打印所有折痕的方向。 例如：N=1时，打印： down
N=2时，打印： down down up
## 题目五、判断一棵二叉树是否是平衡二叉树
### 5.1 二叉树大套路：递归很好用【每个结点会来三次，不要去想什么先、中、后序，有没有打印递归都是存在的】
* 【套路】：
    * 1.列出所有可能性
    * 2.整理出返回值的类型ReturnData【整个递归要按照同样的返回值的结构】
    * 3.得到左右子树的信息
    * 4.整合子树的信息
    * 5.返回我的信息
### 5.2 平衡二叉树：对任何一棵树，其左右子树高度差不超过1
* 【分析】：只要每个结点的树都是平衡的，则整棵树就是平衡的
    * 1.左子树不平衡？
    * 2.右子树不平衡？
    * 3.都平衡，高度差超过1？
    * 4.都平衡，高度差不超过1，则说明是平衡树

```Java
package day5;

import testTools.Node;

public class IsBalance {

    public static class ReturnData{ //返回值的结构
        public boolean isB;
        public int h;
        public ReturnData(boolean isB,int h){
            this.h = h;
            this.isB = isB;
        }
    }

    public static boolean isBalance(Node head){ //主调用
        return process(head).isB;
    }

    private static ReturnData process(Node head){ //递归从把自己看作是一棵子树的开头来看
        if (head == null){
            return new ReturnData(true,0);
        }
        ReturnData l = process(head.left); //得到左子树的高度和是否平衡信息
        if (l.isB == false){ //左子树不平衡，则整棵树不平衡
            return new ReturnData(false, -1);//可以只用h就表示高度和是否平衡，加上isB只是方便记忆递归套路
        }
        ReturnData r = process(head.right);
        if (r.isB == false){ //右子树不平衡，则整棵树不平衡
            return new ReturnData(false,-1);
        }

        if (Math.abs(r.h-l.h) > 1){ //左右子树平衡，但高度差超过1，整棵树也不平衡
           return new ReturnData(false,-1);
        }

        //左右子树都平衡，且高度差小于等于1，则此节点所引导的子树平衡，高度为左右子树最高的高度+1
        return new ReturnData(true,Math.max(r.h,l.h) + 1);
    }
    
}
```

## 题目六、判断一棵树是否是搜索二叉树、判断一棵树是否是完全二叉树
### 6.1 搜索二叉树：没有重复结点，对任何一节点，左子树都比它小，右子树都比它大
* 【分析】：判断方法：实际就是中序遍历的结果如果是依次升序，就是搜索二叉树【非递归版好改一些】
```Java
package day5;

import testTools.Node;
import testTools.PrintTree;
import java.util.Stack;

public class IsBinarySearchTree {

    public static Boolean isBinarySearchTree (Node head){
        //基于中序遍历
        if (head == null)
            return true; //空树是二叉搜索树
        int pre = Integer.MIN_VALUE; //*******不要忘记设置值，否则默认为0，而树里第一个数可能小于0，导致错误
        Stack<Node> stack = new Stack<Node>();
        /** 压一绺左边界，再再从尾端依次往外弹，弹出每个结点，再去遍历其右孩子的过程就模拟了左中右这个过程 */
        while (!stack.isEmpty() || head != null) { //head!=null只是用于刚开始stack为空的时候，因为它没有像别的那样在循环前就push了head
            if (head != null){
                while (head != null) {//当前结点不为空，当前结点压入栈，当前结点往左移动【一压就压一斜绺】
                    stack.push(head);
                    head = head.left;
                }
            }else {  //当前结点为空说明上面已经压完一绺了，弹出结点（中点），再处理右边，对右边下一次一压就压一斜绺
                head = stack.pop();
                if (pre > head.value){ //*****【位置不能放错了哈】前一个数大于后一个，则一定不是二叉搜索树
                    return false;
                }
                pre = head.value; // ****前一个数小于当前，则pre记录当前，即为下一个数的前一个数
                head = head.right;
            }
        }
        return true; //中序遍历完了都没有返回false，说明是二叉搜索树
    }
}
```

### 6.2 完全二叉树：从左往右堆齐
* 【分析】：进行层序遍历每个结点：
    * 情况1：左右双全，则看下一个结点
    * 情况2：如果一个结点不是左右双全
        * 2.1 如果一个结点无左结点，有右节点，则一定不是完全二叉树
        * 2.2 如果一个结点有左结点，无右节点，则后面遇到的结点必须都是叶节点才能使完全二叉树，否则false
        * 2.3 如果一个结点无左结点，无右结点，依然则后面遇到的结点必须都是叶节点才能使完全二叉树，否则false

```Java
package day5;

import testTools.Node;
import testTools.PrintTree;
import java.util.LinkedList;
import java.util.Queue;

public class IsCompleteBT {
    public static Boolean isCompleteBT(Node head){
        if (head == null){ //永远的第一步
            return true;
        }
        Boolean afterMustLeaf = false;
        Queue<Node> queue = new LinkedList<Node>();
        queue.offer(head);
        while (! queue.isEmpty()){
            head = queue.poll();
            if (afterMustLeaf && (head.left!=null || head.right!=null)){
                return false; //在这结点前已经开启后续结点必须是叶节点的要求后，此节点却不是叶节点，则一定不是完全二叉树
            }
            //没开启afterMustLeaf，说明之前的结点都正常，是不是完全二叉树就看我及之后的结点了
            if (head.left == null && head.right != null){ //情况2.1，无左结点，有右节点，则一定不是完全二叉树
                return false;
            }
            //压入左右结点【为了层序遍历】，并且开始判断之后是否要开启afterMustLeaf
            if (head.left != null){
                queue.offer(head.left);
            }
            if (head.right != null){
                queue.offer(head.right);
            }else { // 前面所有结点都是左右双全，但自己少了右结点【左结点可能有，可能无，情况2.2和2.3】，后续结点都必须是叶节点
                afterMustLeaf = true;
            }
        }
        return true; //层序遍历完了都没出现问题返回false，则说明是完全二叉树
    }
}
```
## 题目七、已知一棵完全二叉树，求其节点的个数
* 已知一棵完全二叉树，求其节点的个数。 要求：时间复杂度低于O(N)，N为这棵树的节点个数
* 【分析】：如果是按照遍历，复杂度会是O(N)，所以不可以用遍历整棵树来求，所以要利用满二叉树的性质：结点个数为2^h-1.
    * 1）如果树为空，返回0；
    * 2）不为空，遍历左子树左边界，得到左子树高度
    * 3）遍历右子树左边界得到右子树高度
        * 若右子树的高度等于左子树，说明左子树一定是满二叉树，总结点数 = 2^hl-1【左子树结点数】 +1【是父结点】+ 递归得到的右子树结点个数
        * 若右子树的高度不等于（少1）左子树，说明右子树一定是满二叉树，总结点数 = 2^hr-1【右子树结点数】 +1【是父结点】+ 递归得到的左子树结点个数
* 【时间复杂度分析】：总共要找到 logN 个结点【每层一个】，每个结点要找它的左子树高度+右子树高度 logN，所以时间复杂度是O[(logN)^2]
    * 如果树有2^32个结点，用遍历是2^32，而我们的算法得到的是32^2，所以 O[(logN)^2] 小于 O(N) 

```Java
package day5;

import testTools.Node;

public class NodeNum {

    public static int nodeNum(Node head){
        if (head == null){
            return 0;
        }
        int hl = high(head.left);   //得到左子树的高度
        int hr = high(head.right);  //得到右子树的高度
        if (hr == hl){
            //如果右子树的高度等于左子树，说明左子树一定是满二叉树，结点数2^hl-1，+1加的是父结点，然后再加右子树结点个数
            return ((1<<hl) -1 +1 +nodeNum(head.right));
        }else { //如果不等于，肯定是少了1层【完全二叉树的性质】，则右子树一定是满二叉树
            return ((1<<hr) -1 +1 + nodeNum(head.left));
        }
    }

    private static int high(Node node) { //求树的高度
        if (node == null)
            return 0;
        int h = 0;
        while (node != null){
            h++;
            node = node.left;
        }
        return h;
    }

}
```
#  五、哈希
## 模块一：认识哈希函数和哈希表
### 1. 哈希函数：传入一个字符串，返回一个哈希码【十六进制，十六位，FFFF FFFF FFFF FFFF】
#### 1.1 要求：
 * 1）输入域无穷大
 * 2）输出是有穷尽的
 * 3）同一个输入，返回值一定一样
 * 4）当输入不一样，也可能得到一样的值
 * 5）给我很多不同的输入，我将在整个输出域S上均匀分布【离散型】
#### 1.2 哈希函数特征
* 1）和输入规律没有关系【所以可以用来打乱输入规律】
* 2）哈希函数的推论：如果哈希函数返回值在整个S域上均匀分布，则返回值%m，可以把输出域限定在0~m-1，且在这个范围上也是均匀分布的
    * 面试官不会让你实现一个哈希函数的
#### 1.3 如何实现1000个相互独立的哈希函数【规律不一样】 h= h1 + i * h2
* 你只有一个哈希函数，你把得到的16位结果分为两半。
    * 把前八位作为第一个哈希函数 H1，把后八位作为第二个哈希函数 H2
        * 因为在哈希函数内部，16位哈希码的每一位的结果也是独立的
        * 当然如果你本身就有两个哈希函数，你直接按公式就可以
    * H = H1 + i * H2 【i = 1,2,3,4......,可以做无数个哈希函数】


### 2. 哈希表 【put(key,value), get(key), remove(key)】【复杂度数学上不是O(1)，但我们默认是 O(1),因为扩容次数很少，而且可以离线扩容】
#### 2.1 离线扩容：用户有新数据，同步往新老结构放，使用get时，从老结构拿，直到扩容完毕，就启用新结构，废掉老结构【所以不需要让用户等】
#### 2.2 开放地址法，拉链法，实际实现的时候是数组+红黑树

#### 2.3 应用：一个100T大文件，里面含字符串，要求你打印出所有重复的字符串
* 【分析】：这是一个经典的大数据的问题，用哈希来分流。
    * 问面试官给多少台机器，大文件存在哪，按行读有没有很快的工具？
        * 1000台机器，标好号0——999，大文件存在分布式文件系统上，有很快的工具，你直接说怎么处理吧
    * 每行作为一个文本读出来，利用哈希函数算出一个hashcode，然后hashcode%1000，就可以把这行文本扔到该机器，我们可以知道，相同的文本一定在相同的机器上。会把n种字符串均匀地分布到这1000 台机器上。然后单独在一台机器上是统计哪些是重复的【1000台机器运行，并发】
        * 如果这1000 台机器每台的数据你还是觉得多，就可以在每台机器上再通过哈希函数分成小文件，每台机器又可以并行？



## 题目一：设计RandomPool结构【默认哈希表的增、删、改、查都是 O(1)】

```
【题目】 设计RandomPool结构

设计一种结构，在该结构中有如下三个功能：

insert(key)：将某个key加入到该结构，做到不重复加入。

delete(key)：将原本在结构中的某个key移除。 

getRandom()：等概率随机返回结构中的任何一个key。

【要求】 Insert、delete和getRandom方法的时间复杂度都是 O(1)
```
* 【分析】：准备两张哈希表和一个size，size用于记录含有多少个元素【且0 ~ size-1 一定含有东西】
```Java
package day6;

import java.util.HashMap;

/** 可以加入数据，删除数据，且等概率随机返回结构中的任何一个数据 */
public class RandomPool<K> { //此处的K不能忘，泛型

    private HashMap<K,Integer> map1; //用Integer是因为下标是int
    private HashMap<Integer,K> map2;
    private int size; //用于记录有多少个元素，且保证 0~size-1 里面一定是有元素的

    public RandomPool(){
        map1 = new HashMap<K, Integer>();
        map2 = new HashMap<Integer, K>();
        size = 0;
    }

    public void insert(K key){ //将某个key加入到该结构，做到不重复加入。
        if (! map1.containsKey(key)){ //**因为如果加入重复的键，则会改变value值，而不是添加一个新的，所以要判断
            map1.put(key,size); // key->index
            map2.put(size,key); //此表是用于等概率随机返回的,index->key
            size++;
        }
    }


    public void delete(K key){ //将原本在结构中的某个key移除。
        //删除原本key对儿,lastIndex对儿,加入lastKey->deleteIndex这一对儿
        if ( map1.containsKey(key)){ //如果包含才删除
            int deleteIndex = map1.get(key); //要删除的元素的标号，我们要将最后一个元素和这个标号连在一起
            int lastIndex = --size; //同时数据个数减1了
            K lastKey = map2.get(lastIndex);
            //加入已经存在的键就是更新其值，记住key才是真正的数据，index是你自己定义的，为了随机返回
            map1.put(lastKey, deleteIndex); //加入lastKey->deleteIndex这一对儿
            map2.put(deleteIndex,lastKey);  //加入lastKey->deleteIndex这一对儿
            map1.remove(key); //删除元素
            map2.remove(lastIndex); //删除元素
        }
    }

    public K getRandom(){ //等概率随机返回结构中的任何一个key
        if (size == 0)
            return null; // *****不要忘记
        //不要忘记（int），否则返回类型是double，也不要忘记 (Math.random() *size)的括号，否则出大问题
        int index =(int) (Math.random() *size);
        return map2.get(index);
    }

}
```

## 模块二：认识布隆过滤器【比特类型的一个Map，可以把所需内存压得很小】：解决爬虫驱虫问题、黑名单问题【检查是否处于这个集合的问题】
* 假如有一个100亿个URL的黑名单。你给我一个URL，如果属于我返回true，否则false。
    * 如果用哈希集合HashSet,一个URL假设是64字节，那么需要6400亿字节【640G】，需要的空间太大了【只有一台机器做不到】，所以不能用此方法
    * 如果你用哈希表来分流到很多机器，还是需要代价
* 所以此种类型的题应该用布隆过滤器，布隆过滤器可以只用一台机器实现查某个东西是否在集合中。
     * 缺点：布隆过滤器一定有失误率【宁可错杀三千，不放过一个】: 一个URL在黑名单里，它一定会报true，但一个URL不在黑名单里，它也可能报true。失误率万分之一，可以容忍
### 1. 举例：黑名单问题
#### 1.1 将某个URL加入黑名单，将100亿个URL加入黑名单

* 布隆过滤器先根据 样本大小和要求的失误率 准备一个大小为 m 的容器，比如int[] arr = new int[1000];//此容器可以表示m=32*1000个位置，因为一个int是4字节，32位
* 然后准备 K 个相互独立的哈希函数
* 该URL通过这 K 个哈希函数得到 K 个hashcode【这是取模后的，%m】（可能有重复）,在容器中将这些位置描黑（比特位置为1），则这个URL就进入到布隆过滤器里面来了，描黑步骤如下，比如其中一个位置index = 30000：
    * 先 intIndex = index/32得到这个比特位来第几个整数
    * 再 bitIndex = intIndex%32 得到来自这个整数的哪个比特位
    * 再 arr[intIndex] =( arr[intIndex] | (1 << bitInt) ); 将该比特位改为1 ，就算是这个位置被标记了，以后查这个URL是否在黑名单就看它所属的位置是否被标记为1了
* 重复100亿次就将这100亿个URL加入黑名单了，所以这个容器要大，否则基本所有的位置都被描黑了，那么即使不在里面，你也会查出描黑返回true，出现失误
* 所以空间 m 越大，失误率越低


#### 1.2. 查某个URL是否在黑名单
* 经过 K 个哈希函数找到那 K 个位置【可能小于k，因为可能存在哈希函数得到了一样的值】
    * 如果所有这些位置都被描黑，则返回true，表示在黑名单里
    * 如果这些位置存在一个及以上位置没有被描黑，则一定不在黑名单里【因为如果在黑名单，进黑名单的时候这些位置就肯定全部描黑了】

#### 1.3 至少准备多大的容器：m = - (n * lnP) / (ln2)<sup>2</sup> 【由样本量和预期失误率决定】
* n 是样本量，p 是要求的预期失误率 ，比如：n=100亿 ，p=0.0001
    * 代入公式得 m = 131571428572 大小，但这是比特，所以还要除以8 ，才能得到需要多少字节数，得到23G (而原本不用布隆过滤器要640G，所以布隆过滤器很省空间)
#### 1.4 计算需要多少个哈希函数 ：k = ln2 * (m / n)
* 通过1.3得到 m ,一般我们向上取整（因为越大，失误率越低），然后如果K 是带小数的，我们也向上取整【因为哈希函数不可能 几点几个】，但这样的话，失误率又变了，所以要通过 1.5 来计算失误率是否小于我们期望的值

#### 1.5 我设计了 m 大小容器，k 个哈希函数，这种设计的真实失误率： （1 - e<sup>-(n*k)/m）</sup>）<sup>k</sup>
* 计算出来大约是多少，是否小于0.0001，我的设计是否满足要求


## 模块三：认识一致性哈希【是一种服务器设计】和虚拟节点技术 

### 3.1 经典的服务器的抗压结构

#### 3.1.1 场景：放数据，查数据，这个操作很频繁，怎样让它负载均衡
* 有很多前端，接受request；有一个后端集群组含m台机器，比如 机器1，2,3
* 怎样放入 【“左”，1】这条数据？
    * 所有前端都带同一个哈希函数，得到hashcode%m，是哪台机器就放在哪台机器，以此实现负载均衡，查数据也是找到这台机器
#### 3.1.2 问题：当我想要加机器或者减机器的时候，这个结构就死了，跟哈希表扩容一样，数据需要全部重新分布【因为m变了，取余m 就肯定不一样了】

### 3.2 升级：一致性哈希结构 + 虚拟结点技术：在实现负载均衡的同时，把数据迁移的代价变得很低【有效地解决分布式存储结构下动态增加和删除节点所带来的问题】

#### 3.2.1 一致性哈希原理：

* 假设哈希函数得到的hashcode范围是 ：0~2^32-1；将这个范围想象成一个环 【2^32-1 的下一个位置为0】

* 1.我们的每台机器【缓存节点node】，根据IP得到hashcode，映射到环形空间当中。

* 2.一个数据key来，用同样的哈希函数计算出hashcode，打到环上，顺时针找到离它最近的缓存节点，就扔到里面去。所以图中key1存储于node1，key2，key3存储于node2，key4存储于node3。

* 顺时针找的实现方法：将这些机器的hashcode从小到大排成数组，前端每台机器都有一个这个数组，来了一个request，计算其哈希值hr，在数组里找到第一个大于等于hr的值，这个值就是该request应该去的机器【优化：使用二分法来找这个机器】

![](https://upload-images.jianshu.io/upload_images/5838771-58db181eddaec8a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/640/format/webp)

#### 3.2.2 当缓存的节点有增加或删除的时候，一致性哈希的优势就显现出来了。

* 1.增加节点

    * 当缓存集群的节点有所增加的时候，整个环形空间的映射仍然会保持一致性哈希的顺时针规则，所以有一小部分key的归属会受到影响。
    * 图中加入了新节点node4，处于node1和node2之间，按照顺时针规则，从node1到node4之间的缓存不再归属于node2，而是归属于新节点node4。因此受影响的key只有key2。
    * 最终把key2的缓存数据从node2迁移到node4，就形成了新的符合一致性哈希规则的缓存结构。
![](https://upload-images.jianshu.io/upload_images/5838771-15dd06d340b56334.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/640/format/webp)


* 2.删除节点

    * 当缓存集群的节点需要删除的时候（比如节点挂掉），整个环形空间的映射同样会保持一致性哈希的顺时针规则，同样有一小部分key的归属会受到影响。
    * 图中删除了原节点node3，按照顺时针规则，原本node3所拥有的缓存数据就需要“托付”给node3的顺时针后继节点node1。因此受影响的key只有key4。
    * 最终把key4的缓存数据从node3迁移到node1，就形成了新的符合一致性哈希规则的缓存结构。
![](https://upload-images.jianshu.io/upload_images/5838771-941765990b326bcf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/640/format/webp)

说明：这里所说的迁移并不是直接的数据迁移，而是在查找时去找顺时针的后继节点，因缓存未命中而刷新缓存。?

#### 3.2.2 一致性哈希算法引入了 虚拟节点 【迁移代价很低+负载均衡】
* 引入虚拟节点的原因：
    * 因为机器节点太少，所以可能出现分布不均衡的情况【因为机器结点的位置是由IP计算hashcode得到的，哈希算法能在很多结点的时候让它们均匀分布在整个空间，但很少的时候可能让两台机器距离很近或很远，导致负载不均衡】。
    * 即使一开始做到均匀分布，但加入和删除机器都会破坏这种均匀，所以引入了 虚拟节点 的概念。
* 所谓虚拟节点，就是基于原来的物理节点映射出 N 个子节点，最后把所有的子节点映射到环形空间上。

* 真实机器是：node1,node2,node3，准备3000个虚拟结点，每台机器分配1000个虚拟机结点，将机器和其对应的虚拟结点记录在路由表中【可以查真实的机器有哪些虚拟结点，虚拟节点属于哪个真实机器】，此时环形空间中不再有物理节点node1，node2，node3，只有虚拟节点node1-1，node1-2,...node2-1，node2-2...node3-1....。由于虚拟节点数量较多，缓存key与虚拟节点的映射关系也变得相对均衡了。将虚拟节点负责的域都扔给它属于的真实机器来处理即可。
* 增加机器的时候，就分别从node1,2,3含有的虚拟节点取出一部分分给node4，那么只需要迁移这部分虚拟结点的数据即可【直接迁移到node4】。

* 虚拟节点的实现：假如node1的ip是192.168.1.109，那么原node1节点在环形空间的位置就是hash（“192.168.1.109”）。我们基于node1构建两个虚拟节点，node1-1 和 node1-2，虚拟节点在环形空间的位置可以利用（IP+后缀）计算，例如：hash（“192.168.1.109#1”），hash（“192.168.1.109#2”）


## 模块四：认识并查集结构【时间复杂度：O(1)】

### 4.1 并查集功能【isSameSet，union】

#### 1） isSameSet:非常快的检查两个元素是否属于一个集合
* a,b的头结点一样，则是一个集合，否则不是
#### 2） union: 将两个元素所属的集合合并在一起
* 合并，谁的元素多，就挂在谁下。
#### 3） findHead时优化：任何一次查找头结点过程结束后，把这一条路径上的每个结点的父节点都改为头结点
#### 4） 注意：
##### 1.并查集结构必须一次性把数据给它，不支持流操作
##### 2.并查集结构用两个HashMap来实现是最优的
##### 3.样本为N，只要查询次数+合并次数 逼近于O(N)，则单次查询或合并的平均时间复杂度为O(1),即常数时间。
### 4.2 实现并查集
* 【分析】：
* 第一步：初始化所有结点，将每个结点作为一个集合，一个头结点，指向自己
* 第二步到第n步：合并或者查询，即isSameSet，或union

![](https://images2015.cnblogs.com/blog/715283/201609/715283-20160917221738289-52951535.png)
```Java
package day7;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Stack;

public class UnionFindSet { //并查集利用两个hashmap实现
    public static class Node{ //随便你放什么都可以，并查集和结点无关
        int i;
        public Node(int i){
            this.i = i;
        }
    }

    HashMap<Node,Node> fatherMap;  //结点->father结点，father只是上一个结点，不代表就是头结点哈
    HashMap<Node,Integer> sizeMap; //结点->集合size，实际我们会用到的只有头结点对应的集合size
    public UnionFindSet(List<Node> list){
        initialSet(list);
    }

    private void initialSet(List<Node> nodes) { //初始化并查集
        fatherMap = new HashMap<Node,Node>();
        sizeMap = new HashMap<Node,Integer>();
        for (Node node:nodes){  //初始化，将每个结点作为一个集合，一个头结点，指向自己
            fatherMap.put(node,node);
            sizeMap.put(node,1);
        }
    }

    /** 查看a,b是否属于同一个集合 */
    public boolean isSameSet(Node a,Node b){
        return findHead(a)==findHead(b); //比较的是地址？
    }

    /** 合并a,b所在的集合 */
    public void union(Node a,Node b){
        if (a == null || b == null){ 
            return;
        }
        Node aHead = fatherMap.get(a);
        Node bHead = fatherMap.get(b);
        if (aHead != bHead){ //两个不在一个集合才需要合并
            int aSize = sizeMap.get(aHead);
            int bSize = sizeMap.get(bHead);
            if (aSize <= bSize){ //即a集合小，则a挂在b集合下
                fatherMap.put(aHead,bHead);
                sizeMap.put(bHead,aSize+bSize);
            }else {
                fatherMap.put(bHead,aHead);
                sizeMap.put(aHead,aSize + bSize);
            }
        }
    }

    /** 查找头节点 */
    private Node findHead(Node cur) {
        if (cur == null){ 
            return null;
        }
        Stack<Node> stack = new Stack<>();
        Node father = fatherMap.get(cur);
        while (cur != father){//找头结点
            stack.push(cur);  //不是头结点就存起来，方便之后优化：将它们的父节点直接指向头
            cur = father;
            father = fatherMap.get(cur);
        }
        while (!stack.isEmpty()){ //进行优化，将该路径上所有节点都指向头节点
            fatherMap.put(stack.pop(),father);
        }
        return father;
    }
}
```

## 题目二：岛问题 【矩阵很大时，用分治】

```
一个矩阵中只有0和1两种值，每个位置都可以和自己的上、下、左、右四个位置相连，
如果有一片1连在一起，这个部分叫做一个岛，求一个矩阵中有多少个岛？举例：
0 0 1 0 1 0
1 1 1 0 1 0
1 0 0 1 0 0
0 0 0 0 0 0
这个矩阵中有三个岛。

进阶：这个矩阵巨大无比，但你有多个CPU，怎么让它快速得出结果，
设计一个分治的思路，能并行的算法，就是你把它分成几块来处理，然后合并出岛的信息
```

* 基础版（不分治，和并查集没有任何关系，利用的是递归）：
    * 【分析】：利用dfs思想，遇到一个未被感染的区域，就递归感染其周围相连的（将数值变为2）

```Java
package day7;

public class CountIslands {
    public static int countIslands(int[][] m){
        if (m==null || m[0]==null) //maybe因为m.length和m[0].length
            return 0;
        int row = m.length; //***行数
        int column = m[0].length; //***列数
        int countOfIslands = 0;
        for (int i = 0;i<row ; i++){
            for (int j = 0; j< column; j++){
                if (m[i][j] == 1){//说明遇到了一个新的未被感染的区域
                    countOfIslands++;
                    inject(m,i,j,row,column);
                }
            }
        }
        return countOfIslands;
    }

    private static void inject(int[][] m, int i, int j, int row, int column) {
        if (i<0 || i>=row || j<0 || j>=column || m[i][j]!=1){
            return; //当超出边界，和遇到被感染区或者不相连的地方（!= 1）,就不再需要感染
        }
        m[i][j] = 2; //否则感染它
        inject(m,i-1,j,row,column);//感染上面
        inject(m,i+1,j,row,column);//感染下面
        inject(m,i,j-1,row,column);//感染左边
        inject(m,i,j+1,row,column);//感染右边
    }
}
```


* 进阶版【利用并查集】：矩阵可以分成几块，放在不同的CPU上处理，重点在于合并的逻辑。

![](https://github.com/zhaojing5340126/interview/blob/master/picture/%E5%B2%9B%E9%97%AE%E9%A2%98.jpg?raw=true)



#  六、前缀树、堆结构、贪心算法
## 1、前缀树Prefix Tree）,又叫字典树、Trie树，单词查找树或键树，是一种多叉树结构

* 前缀树的功能很强大，比如有一个字符串数据，我们要从查找其中以“hell”开头的(设置一个passN)，或者以"ive"结尾的字符的个数(?)等等操作。我们只需要在定义前缀树的时候加上相应得数据项就OK了。下面来简单通俗易懂的解释一下前缀树。
### 1.1 前缀树题目举例：一个字符串类型的数组arr1，另一个字符串类型的数组arr2。
#### 1.1.1 arr2中有哪些字符，是arr1中出现的？请打印
* /** 返回树中有多少个要求查找的单词 */ public int search(String word) 的变体
#### 1.1.2 arr2中有哪些字符，是作为arr1中某个字符串前缀出现的？请打印
* /** 有多少单词以pre为前缀 */ public int prefixNumber(String pre)的变体
#### 1.1.3 arr2中有哪些字符，是作为arr1中某个字符串前缀出现的？请打印arr2中出现次数最大的前缀。
*  /** 有多少单词以pre为前缀 */ public int prefixNumber(String pre)，找最大的个

### 1.2 前缀树的insert、delete、search、prefixNum方法代码:代码相似度很高，前半部分基本一样，都是从root开始遍历
* 假设刚开始我们，有一个空节点，现在我们有一个操作，往这个空的节点上insert字符串“abc”, 那么我们按照下面的步骤insert：

* process: 1、首先看当前节点有没有指向字符'a'的路径，没有的话就创建指向'a'的路径，否则滑过到下一个字符，同样是看看有没有到该字符的路径。一直遍历完字符，并且都创建好了路径。如下图所示：
![](https://github.com/zhaojing5340126/interview/blob/master/picture/%E5%89%8D%E7%BC%80%E6%A0%91.png?raw=true)

![](https://github.com/zhaojing5340126/interview/blob/master/picture/%E5%89%8D%E7%BC%80%E6%A0%912.jpg?raw=true)

```Java
package day7;

class TrieNode{
     public int passN; //表示有多少个字符串到达过
     public int endN; //表示有多少个字符串以它结尾
     public TrieNode[] paths; //指路径，paths[0]!=null代表该节点到下一个结点的路径 a 存在

     public TrieNode(){
         passN = 0;
         endN = 0;
         paths = new TrieNode[26];  //若只认同小写字母，则一个结点拥有26条路：a-z
     }
}

//字母在路径上，是否为null表示路径的有无
//结点是该字母的记录结点，下面的路径从该节点发出
//要对某条边做处理应该是在它的记录节点上进行处理，有多少个字符串到达过等
//将字母（路径）和该字母的记录结点看作 “一体”*******
public class TrieTree {

     private TrieNode root; //每次不管什么操作都要从根节点开始，所以要记录根节点
     public TrieTree(){  //前缀树的初始化
         root = new TrieNode();
    }

    /** 往树中加入一个单词 */
    public void insert(String word){
         if (word == null)
             return;
         char[] chars = word.toCharArray();
         TrieNode node = root;
         int index = 0; //0-25对应路径a-z
        for (int i=0; i<chars.length; i++){ //把整个单词插入树中，字母在路径上
            index =chars[i]-'a'; //计算该字母应该在哪条路径上，不能是“”，因为这样代表字符串
            if (node.paths[index] == null){ //==null，说明这条路径不存在，就要创建它
                node.paths[index] = new TrieNode(); //路径存在了，连接着该路径的记录节点
            }
            node = node.paths[index];  //到达了此路径（字母）的记录结点，即将走向下一条路径
            node.passN++; //又有一个单词滑过了
        }
        node.endN++; //又有一个单词以该字母结尾
    }

    /** 从树中删除一个单词 */
    public void delete(String word){
         if (search(word) == 0 )
             return; //树中没有此单词 ，自然不用删除
         char[] chars= word.toCharArray();
         TrieNode node = root;
         int index =0;
         for (int i=0; i<chars.length; i++){
             index = chars[i] -'a'; //计算该字母应该在哪条边上
             if (-- node.paths[index].passN == 0){ //要对某条边做处理应该是在它的记录节点上进行处理，将到达过的字符串数量减1，等于0说明没有字符串经过了
                 node.paths[index] = null;
                 return;
             }
             node = node.paths[index]; //***
         }
         node.endN--;  //删除了整个单词，所以将该单词的数量减1
    }

    /** 返回树中有多少个查找的单词 */
    public int search(String word) {
        if (word == null)
            return 0;
        char[] chars = word.toCharArray();
        TrieNode node = root;
        int index = 0;
        for (int i=0; i< chars.length; i++){
            index = chars[i] -'a';
            if (node.paths[index] == null){ //这个字母所在的路不存在，说明单词不在树中
                return 0;
            }
            node = node.paths[index];//到达了此路径（字母）的记录结点，即将走向下一条路径
        }
        //整个单词的所有字母都在，说明单词在树中，返回最后一个字母记录结点中的endN:有多少个字符串以它结尾
        return node.endN;
    }

    /** 有多少单词以pre为前缀 */
    public int prefixNumber(String pre){
        if (pre == null)
            return 0;
        char[] chars = pre.toCharArray();
        TrieNode node = root;
        int index = 0;
        for (int i=0; i<chars.length; i++){
            index = chars[i]-'a';
            if (node.paths[index] == null){ //这个字母所在的路不存在，说明以pre为前缀的单词不在树中
                return 0;
            }
            node = node.paths[index];//到达其记录结点，将去判断下一条路了
        }
        return node.passN;
    }

}


```

## 2、堆结构的扩展与应用
## 3、介绍贪心算法及相关题目
* 所谓贪心算法是指，在对问题求解时，总是做出在当前看来是最好的选择。也就是说，不从整体最优上加以考虑，他所做出的仅是在某种意义上的局部最优解。 
* 贪心策略适用的前提是：局部最优策略能导致产生全局最优解。 【不要去理论证明你选择的贪心策略的正确性，因为太复杂，你最好直接看结果对不对就好】
* 3.1 贪心算法的基本思路： 
    * 1.建立数学模型来描述问题。 
    * 2.把求解的问题分成若干个子问题。 
    * 3.对每一子问题求解，得到子问题的局部最优解。 
    * 4.把子问题的解局部最优解合成原来解问题的一个解。

### 题目七：按最低字典序拼接字符串
* 题目：给定一个字符串类型的数组strs，找到一种拼接方式，使得把所有字符串拼起来之后形成的字符串具有最低的字典序。
* 字典序：对于两个字符串
    * 长度不一样，将短的个补0（0是最小ASCII码值），补成长度一样
    * 先按首字符排序，如果首字符相同，再按第二个字符排序，以此类推。如aa,ab,ba,bb,bc就是一个字典序，从小到大
* 【分析】贪心：你定一个指标，按这个指标来，对每个样本分出个优先，优先级大的先处理
    * 本题的贪心策略就是你选择的比较策略
        * str1.str2<=str2.str1，则str1放前面，否则str2放前面【根据两个字符串拼接的结果的大小来决定排序】

```Java
package day7;

import java.util.Arrays;
import java.util.Comparator;

public class Lowest {

    private static class MyComparator implements Comparator<String>{
        @Override
        public int compare(String a, String b) {
            return (a+b).compareTo(b+a); //ab<ba，则返回-1，说明a小，b大
        }
    }

    public static String lowestString(String[] strs){
        if (strs==null || strs.length==0){ //**指针为null和数组为空
            return "";
        }
        Arrays.sort(strs,new MyComparator());
        String res ="";
        for (String s:strs){
            res+=s;
        }
        return res;
    }

    public static void main(String[] args) {
        String[] strs2 = { "ba", "b" };  //正确排序为bab,而非bba
        System.out.println(lowestString(strs2));

    }

}

```
## 4、在面试中如何快速地尝试出贪心策略

 

# 七、暴力递归和动态规划
## 1. 递归
## 2. 动态规划
## 3. 如何把暴力递归套路变为动态规划
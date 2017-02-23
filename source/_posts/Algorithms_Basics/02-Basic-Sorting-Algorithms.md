---
title: C#算法与数据结构（二）：简单的排序算法
categories:
  - Algorithms
tags:
  - Algorithms
  - Data Structure
date: 2016-08-03 08:32:41
updated: 2017-02-17 18:30:00
---

**概述：**对于数据常见的操作是**`排序`**和**`搜索`**。这种现象从计算行业开始就存在了，而这也意味着它们也是计算机科学研究较多的两种操作。本系列文章所讨论的数据结构主要围绕着如何使得排序和或搜索更加容易和高效来设计的。

本文将向你介绍一些基本的排序和查找算法，这些算法仅依赖数组作为数据结构，而且所采用的“高级”编程 技术只是递归。本章还介绍了用来非正式分析不同算法之间速度与效率的方法。
<!--more-->

<!-- TOC -->

- [另外一种算法效率测试方法](#另外一种算法效率测试方法)
- [排序算法](#排序算法)
    - [冒泡排序](#冒泡排序)
        - [现实世界的案例：](#现实世界的案例)
        - [语言描述：](#语言描述)
        - [代码实现：](#代码实现)
    - [选择排序](#选择排序)
        - [现实世界的案例：](#现实世界的案例-1)
        - [语言描述：](#语言描述-1)
        - [代码实现：](#代码实现-1)
    - [插入排序](#插入排序)
        - [现实世界的案例：](#现实世界的案例-2)
        - [语言描述：](#语言描述-2)
        - [代码实现：](#代码实现-2)
- [效率比较](#效率比较)
- [总结](#总结)
- [思考](#思考)
- [参考](#参考)

<!-- /TOC -->

## 另外一种算法效率测试方法

## 排序算法

我们日常生活中常见的数据往往都是排序好的。例如根据字母表查字典，电话里查找联系人的时候我们会迅速定位到姓的首字母，邮局会按照邮编结合省，市，街道，门牌号，姓名给信件排序。排序是日常活动常见的行为，值得我们好好学习一下。

我们之前讨论过，对于不同的排序算法做了一些研究。尽管已经发明了一些非常复杂的算法，不过我们还是要先学习一些简单的排序算法，它们是：插入排序、冒泡排序和插入排序。这些算法都易于理解和实现。虽然对于任意情况而言这些算法不是最好的算法，但是对于少量数据集合或者其他特殊情况而言，它们是可用的最好算法。

> 首先需要了解算法思想，能在现实生活中找到对应的案例，然后用自己的语言描述出来，最后用代码实现才能算掌握了该算法。

排序算法首先要注意是升序还是降序，在本文中，我们假设所有的排序要求都是**升序**。 我们使用的数据结构是数组，命名为`nums`，其中有$i$个数值，其中的数值以$x_0$，$x_1$，…… $x_(i-1)$，$x_i$来表示。

### 冒泡排序

这种排序算法的得名是由于数据“像气泡一样”从列表的一端浮动到另一端。
假设现在要把一列数按**升序**方式进行排序，即较大数值浮动到列的右侧，而较小数值则浮动到列的左侧。这种效果可以通过下列操作来实现:多次遍历整个列表，并且比较相邻的数值，如果左侧的数值大于右侧数值就进行交换。

#### 现实世界的案例：

#### 语言描述：

前面我们提到过，算法的组成结构有三个：顺序结构、选择结构和循环结构，而循环结构是算法中的关键，要符合算法的**可停止性**，就必然要求我们在循环中找到结束条件和监测的变量。所以当我们使用语言来描述的时候，我们尤其需要强调该结束条件。

1. 既然是要将数组中的相邻的两个元素进行比较，那么我们将数组中的第1个元素与第2个元素比较，一直比较到最后一个元素。
  将 $x_j$ 与 $x_(j+1)$ 进行比较，其中 $ j+1 <= i $
    * $x_j > x_(j+1)$，那么，$x_j$ 与 $x_(j+1)$ 互换位置，执行下一步
    * $x_j < x_(j+1)$，执行下一步
  总共比较了 $i-1$ 次，由于数组中的最大值一直往右侧排，那么当比较了$i-1$ 次之后，$x_i$就是最大值，下次就没有比较再参与比较了。

2. 继续重复第1步，只不过这比较次数变为 $i-2$。 
3. 继续比较，交换位置直到最后比较两个数。

#### 代码实现：

```cs
public void BubbleSort(ref int[] nums){
  
  for(int i= nums.Length-1; i>=1; i--){
    for(int j=0; j<=i-1; j++){
      if(nums[j] > nums[j+1]){
        int temp = nums[j+1];
        nums[j+1] = nums[j];
        nums[j] = temp;
      }
    }
  }
}
```

简单的总结一下：这里可以将数组分为两个部分，已排序和未排序两个部分，在算法开始执行之前，未排序的部分的数值个数是$i$，当我们比较第一次之后，未排序的部分为$i-1$，也就说，还未进行排序的数值个数就是我们的循环结构的控制变量。

小技巧：先写内循环，再写外循环。

### 选择排序

从整个数组中找出最大的值，放到最后一个，然后再从剩下的里面找出最大的值，放到倒数第二个，以此类推。

#### 现实世界的案例：

新学期第一节体育课，或者做广播操的时候，同学们都按个子高矮进行排序，一眼看过去最高的排最后，然后再找……
当然，实际情况可能更快点儿，因为也会看到最矮的，然后让他排到最前面，但核心思想就是，不断从还没有排序的学生中找到个子最高或最矮的。

#### 语言描述：

之前我们写过从一组数值中找到最大或最小值的代码，这里我们会用到它的思路，就是随便挑一个值，假设它是最大或最小的，然后拿它和数组中的其他值进行比较。

按照上面的思路，我们将数组分为未排序和已排序两个部分。我们不断假设未排序部分中的第一个值为最大值。

这里的未排序的数值的个数就是循环结构的控制变量。

#### 代码实现：

```cs
public void SelectionSort(ref int[] nums)
{
  int temp, maxIndex;

  for(int i= nums.Length-1; i>0; i--){
    
    maxIndex = 0;  
    
    for(int j=0; j<=i; j++){
      if(nums[j] > nums[maxIndex]){
        maxIndex = j;
      }
    }

    temp = nums[i];
    nums[i]= nums[maxIndex];
    nums[maxIndex] = temp;
  }
}

```

### 插入排序

插入排序的思想也很简单，通过假设第一个数值的位置是正确的，然后将第二个值与第一个值进行比较。根据条件换序，这样就假设第一个值和第二值的顺序是正确的，然后将第三个值分别和第一个值、第二个值比较……


#### 现实世界的案例：

假设手里现在有A到K的扑克牌，只不过顺序是乱的，如果我们想按照从A，1，2，3，……，J，Q，K把牌整理好。比较习惯的做法就是，看牌，如果下一张牌应该在前面，插到前面。

#### 语言描述：


#### 代码实现：

```cs
public void InsertionSort(ref int[] nums){
  int temp;

  for(int i=1; i<=nums.Length-1; i++){
    for(int j=0; j < i; j++){
      if(nums[i]<nums[j]){
        temp = nums[j];
        nums[j] = nums[i];
        nums[i] = temp;      
      }
    }
  }
}
```

或者可以使用其他循环的写法：


```cs
public void InsertionSort() 
{
  int inner, temp;
  
  for(int outer = 1; outer <= upper; outer++) 
  {
    temp = arr[outer];
    inner = outer;
    while(inner > 0 && arr[inner-1] >= temp) 
    {
      arr[inner] = arr[inner-1];
      inner -= 1;
    }
    arr[inner] = temp; 
  }
}
```

## 效率比较

These three sorting algorithms are very similar in complexity and theoretically, at least, should perform similarly when compared with each other. We can use the Timing class to compare the three algorithms to see if any of them stand out from the others in terms of the time it takes to sort a large set of numbers.
To perform the test, we used the same basic code we used earlier to demonstrate how each algorithm works. In the following tests, however, the array sizes are varied to demonstrate how the three algorithms perform with both smaller data sets and larger data sets. The timing tests are run for array sizes of 100 elements, 1,000 elements, and 10,000 elements. Here’s the code:

```cs
static void Main() 
{
  Timing sortTime = new Timing();
  Random rnd = new Random(100);
  int numItems = 1000;
  CArray theArray = new CArray(numItems); 
  
  for(int i = 0; i < numItems; i++)
  {
    theArray.Insert((int)(rnd.NextDouble() * 100));
  }
  sortTime.startTime();
  theArray.SelectionSort();
  sortTime.stopTime();
  Console.WriteLine("Time for Selection sort: " + sortTime.getResult().TotalMilliseconds);
  theArray.Clear();

  for(int i = 0; i < numItems; i++)
  {
    theArray.Insert((int)(rnd.NextDouble() * 100));
  }
  sortTime.startTime();
  theArray.BubbleSort();
  sortTime.stopTime();
  Console.WriteLine("Time for Bubble sort: " + sortTime.getResult().TotalMilliseconds);
  theArray.Clear();

  for(int i = 0; i < numItems; i++)
  {
    theArray.Insert((int)(rnd.NextDouble() * 100));
  }
  sortTime.startTime();
  theArray.InsertionSort();
  sortTime.stopTime();
  Console.WriteLine("Time for Selection sort: " + sortTime.getResult().TotalMilliseconds);
 }

```
The output from this program is:

showing that the Selection and Bubble sorts perform at the same speed and the Insertion sort is about half as fast (or twice as slow).

Now let’s compare the algorithms when the array size is 1,000 elements:
  Here we see that the size of the array makes a big difference in the performance of the algorithm. The Selection sort is over 100 times faster than the Bubble sort and over 200 times faster than the Insertion sort.
When we increase the array size to 10,000 elements, we can really see the effect of size on the three algorithms:
 The performance of all three algorithms degrades considerably, though the Selection sort is still many times faster than the other two. Clearly, none of these algorithms is ideal for sorting large data sets. There are sorting algo- rithms, though, that can handle large data sets more efficiently. We’ll examine their design and use in Chapter 16.

## 总结

In this chapter, we discussed three algorithms for sorting data—the Selection sort, the Bubble sort, and the Insertion sort. All of these algorithms are fairly easy to implement and they all work well with small data sets. The Selec- tion sort is the most efficient of the algorithms, followed by the Bubble sort and the Insertion sort. As we saw at the end of the chapter, none of these algorithms is well suited for larger data sets (i.e., more than a few thousand elements).


## 思考

1. 请创建一个至少由 100 个字符串值组成的数据文件。大家可以自行输入字符串来创建这个列表，也可以从某些类 型的文本文件中复制内容，甚至可以通过随机生成字符串来创建文件。请使用本章讨论过的三种排序算法的每一种 对文件进行排序。还请创建一个程序来测试每种算法，并且类似于本章最后一节的输出，也要输出三种算法的时间。
2. 请创建一个由 1000 个整数组成的数组，其中的整数是按数值大小排序的(即从小到大)。请编写一个程序能够运 行三种排序算法来处理此数组，而且测试和比较每种算法的时间。最后还请把这些时间与随机排序的整数数组所用 三种算法的时间进行比较。
3. 请创建一个由 1000 个整数字组成的数组，其中的整数是按数值大小反向顺序的(即从大到小)。请编写一个程序 能够运行三种排序算法来处理此数组，而且测试和比较每种算法的时间。


## 参考

* Data Structure and Algorithms using C#
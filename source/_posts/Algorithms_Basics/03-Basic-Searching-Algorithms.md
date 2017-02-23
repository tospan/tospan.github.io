---
title: C#算法与数据结构（三）：简单的搜索算法
categories:
  - Algorithms
tags:
  - Algorithms
  - Data Structure
date: 2016-08-04 08:32:41
updated: 2017-02-17 18:30:00
---

**概述：** 搜索是非常基础的编程操作，本文将介绍如何在一个(数值)数组中进行搜索特定的内容的算法。它们是顺序查找(数组中内容未排序)和二分查找(数组中内容已排序)。

<!-- TOC -->

- [按序搜索(Sequential Searching)](#按序搜索sequential-searching)
    - [让按序搜索更快一点：子组织数据](#让按序搜索更快一点子组织数据)
- [二分搜索(Binary Search)](#二分搜索binary-search)
    - [递归二分搜索算法](#递归二分搜索算法)
- [总结](#总结)
- [参考](#参考)

<!-- /TOC -->
<!--more-->
## 按序搜索(Sequential Searching)

最常见的搜索方式就是从记录集的开始处按序遍历每条记录，直到找到所要的记录或者是到达数据集的末尾。
这就是所谓的顺序查找。 顺序查找(也被称为线性查找)是非常容易实现的。从数组的起始处开始，把每个访问到的数组元素依次和所
要查找的数值进行比较。如果找到匹配的数据项，就结束查找操作。如果遍历到数组的末尾仍没有产生匹配，那么 就说明此数值不在数组内。

顺序搜索方法代码如下:

```cs
bool SeqSearch(int[] arr, int sValue) {
for (int index = 0; index < arr.Length-1; index++)
      if (arr[index] == sValue)
        return true;
    return false;
}
```

如果发现匹配，那么函数会立刻返回 `true` 并且退出。如果到达数组的末尾，函数还没有返回`true`，那么要搜索的的数值就不在数组内，而函数则会返回 `false`。

当然，用户也可以这样写按序搜索函数。这样当找到要查找的数值时，函数就会返回此数值的在数组中的索引，当没有找到要数值时，函数就会返回-1。首先来看一看新函数:

```cs
static int SeqSearch(int[] arr, int sValue)
{
  for (int index = 0; index < arr.Length -1 ; index++)
    if (arr[index] == sValue)
      return index; 
  
  return -1;
}
```

### 让按序搜索更快一点：子组织数据

从上面的方法我们可以知道，如果搜索的元素位于数组的开始处位置附近，那么返回结果的速度就快。

如果需要频繁搜索，为了提升搜索效率，我们可以在找到数据项后把它移动到数组开始处，从而减少搜索时遍历数组的次数以提高效率。最终的结果就是所有最频繁查找的数据项都会被放置在数据集合的开始部分。

之所以叫自组织的原因，是数组中的数据位置不是由程序员在程序运行之前设定的，而是程序运行过程中程序自己组织的。

那么怎么移动呢？

可以根据帕累托的2/8定律，从我们这个角度上讲，对数组的80%的搜索操作都是为了找到其中20%的数据，那么就可以将数组长度 $L * 20%$ 作为分界线。

如果找到的数据的索引值$I > L * 20% $，那我们就该数据的位置与它前一个值的位置进行交换。

示例代码如下：

```cs
static int SeqSearch(int sValue) 
{
  for(int index = 0; i < arr.Length-1; i++)
  {
    if (arr[index] == sValue && index > (arr.Length * 0.2)) 
    {
        swap(index, index-1);
        return index; 
    } else if (arr[index] == sValue)
    {
        return index;      
    }
    
    return -1;
  }
}

static void swap(ref int item1, ref int item2) { 
  int temp = arr[item1];
  arr[item1] = arr[item2];
  arr[item2] = temp;
}
```
以上的算法适用于对无序且长度变化较少的数据集进行搜索。接下来的将讨论一种只处理有序数据但比任何已提到的顺序查找算法都要更加高效的查找算法，即二分查找。

## 二分搜索(Binary Search) 

二分查找法对于已经排序的数据集的查找效率非常高。

其核心思想可以通过这个例子说明：请假设你正试图猜测由朋友选定的一个在 1 至 100 之间的数字。对于每次你所做的猜测，朋友都会告诉你是猜对了，还是猜大了，或是猜小了。最好的策略是第一次猜 50。如果猜大了，那么就 应该再猜 25。如果猜 50 猜小了，则应该再猜 75。在每次猜测的时候，你都应该根据调整的数的较小或较大范围(这 依赖于你猜测的数是偏大还是偏小)选择一个新的中间点作为下次要猜测的数。只要遵循这个策略，你最终一定会猜出正确的数。

要实现运用该策略可以按照如下的步骤：

1. 首先需要把数据按顺序(最好是升序方式)存储到数组内(当然，其他数据结构也可行)。
2. 找到该数组或列表的边界。

在查找 的开始，这就意味着是数组的上限和下限。然后，通过把上限和下限相加后除以 2 的操作就可以计算出数组的中间 点。接着把存储在中间点上的数组元素与要查找的数值进行比较。如果两者相同，那么就表示找到了该数值，同时 查找算法也就此结束。如果要查找的数值小于中间点的值，那么就通过从中间点减去一的操作计算出新的上限。否 则，若是要查找的数值大于中间点的值，那么就把中间点加一求出新的下限。此算法反复执行直到下限和上限相等 时终止，这也就意味着已经对数组全部查找完了。如果发生这种情况，那么就返回-1，这表示数组中不存在要查找 的数值。
这里把算法作为 C#语言函数进行了编写:

```cs
static int binSearch(int value) 
{
  int upperBound, lowerBound, mid; 
  upperBound = arr.Length-1; 
  lowerBound = 0;
  while(lowerBound <= upperBound) 
  {
    mid = (upperBound + lowerBound) / 2;
    if (arr[mid] == value)
        return mid;
    else if (value < arr[mid])
        upperBound = mid - 1;
    else
      lowerBound = mid + 1; 
  }
  
  return -1;
}
```

Here’s a program that uses the binary search method to search an array:
```cs
static void Main(string[] args)
{ 
  Random random = new Random(); 
  CArray mynums = new CArray(9); 
  for(int i = 0; i <= 9; i++)
      mynums.Insert(random.next(100));
  mynums.SortArr();
  mynums.showArray();
  int position = mynums.binSearch(77, 0, 0);
  if (position >= -1)
  { 
    Console.WriteLine("found item"); mynums.showArray();
  } 
  else{
    Console.WriteLine("Not in the array");    
  }
  
  Console.Read();
}
```

### 递归二分搜索算法

Although the version of the binary search algorithm developed in the previ- ous section is correct, it’s not really a natural solution to the problem. The binary search algorithm is really a recursive algorithm because, by constantly subdividing the array until we find the item we’re looking for (or run out of room in the array), each subdivision is expressing the problem as a smaller version of the original problem. Viewing the problem this ways leads us to discover a recursive algorithm for performing a binary search.
In order for a recursive binary search algorithm to work, we have to make some changes to the code. Let’s take a look at the code first and then we’ll discuss the changes we’ve made:

```cs
public int RbinSearch(int value, int lower, int upper) 
{ 
  if (lower > upper)
    return -1; 
  else
  {
      int mid;
      mid = (int)(upper+lower) / 2;
      if (value < arr[mid])
        RbinSearch(value, lower, mid-1);
      else if (value = arr[mid])
        return mid;
      else
        RbinSearch(value, mid+1, upper)
  }
}
```

The main problem with the recursive binary search algorithm, as compared to the iterative algorithm, is its efficiency. When a 1,000-element array is sorted using both algorithms, the recursive algorithm is consistently 10 times slower than the iterative algorithm:


Of course, recursive algorithms are often chosen for other reasons than effi- ciency, but you should keep in mind that anytime you implement a recursive algorithm, you should also look for an iterative solution so that you can compare the efficiency of the two algorithms.
Finally, before we leave the subject of binary search, we should mention that the Array class has a built-in binary search method. It takes two arguments,

an array name and an item to search for, and it returns the position of the item in the array, or -1 if the item can’t be found.
To demonstrate how the method works, we’ve written yet another binary search method for our demonstration class. Here’s the code:

```cs
public int Bsearh(int value) {
  return Array.BinarySearch(arr, value)
}
```
When the built-in binary search method is compared with our custom- built method, it consistently performs 10 times faster than the custom-built method, which should not be surprising. A built-in data structure or algorithm should always be chosen over one that is custom-built, if the two can be used in exactly the same ways.

## 总结
对数据集中的某个元素进行查找是常见的操作，最简单的方法就是从数组或列表的开始处遍历数组直到找到想要的元素或到数组的结尾，这种搜索方法使用与数据量比较少，且未排序的数据集。

如果该数据集排过序，二分查找法则是比较好的算法。二叉查找会持续划分数据集合直到找到所要查找的数据项为止。大家可以采用迭代方式或递归方式编写二分查找算法。C#语言的 `Array` 类包括有内置的二叉查找方法。

## 参考

更多有关数据集合中概率分布的知识请参阅 Knuth 的书 (1998, pp. 399–401)。
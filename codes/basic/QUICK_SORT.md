![quickSort](/assets/codes/basic/quickSort.webp "quickSort")

### 前言

这么多排序算法里面，我最喜欢考快速排序，也是我出去面试时被问到最多的，快速排序代码量适中，并且考察你设计函数和递归的能力，是非常适合用来面试的。

如果你还不明白快速排序的思路，可以看下上面的图，快速排序核心思想就是找到一个点，然后通过交换，让点左边的数都小于等于这个点的值，点右边的数都大于等于这个点的值，再在左右进行递归。

核心思想虽然一致，但是快速排序的写法还是很多的，这篇文章来介绍几种。

### 框架

首先我们明确命名，在快速排序中，我们找到的这个点一般叫做pivot，中文叫支点或者叫枢轴。我个人在考察别人代码的时候，命名是非常重要的考察点，我自己写代码也非常注重命名，也希望大家能够重视起来。

接下来我们来规划一下快排中需要哪些函数。首先swap最好还是抽成一个函数，就用来交换数组中的两个数。然后因为要递归，我们需要一个innerQuickSort(int[] arr, int start, int end)，用来进行从start到end的一个排序，最后我们需要一个innerFindPivot(int[] arr, int start, int end)，用于在start和end间找到一个pivot，并且使得pivot左边的数都小于等于pivot，右边的数都大于等于pivot。

接下来我们来看一下大体框架。

```java
public static void swap(int[] arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}

public static int innerFindPivot(int[] arr, int start, int end) {
    // TODO
}

public static void innerQuickSort(int[] arr, int start, int end) {
    int pivot = innerFindPivot(arr, start, end);
    if(start < pivot - 1) {
        innerQuickSort(arr, start, pivot - 1);
    }
    if(pivot + 1 < end) {
        innerQuickSort(arr, pivot + 1, end);
    }
}

public static void quickSort(int[] arr) {
    innerQuickSort(arr, 0, arr.length - 1);
}
```

真正的函数quickSort直接调用innerQuickSort，而innerQuickSort调用innerFindPivot找到pivot后，递归地调用innerQuickSort就能完成排序了。

所以真正核心的地方就是innerFindPivot。

### 三种方式

我们先看第一种方法，我们先将arr[start]作为pivot，然后处理后面的数，处理的时候我们使用双指针的思想，废话不多说，看下代码：

```java
public static void swap(int[] arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}

public static int innerFindPivot(int[] arr, int start, int end) {
    int left = start, right = end;
    int pivot = arr[start];
    while(left < right) {
        while(left < right && arr[right] > pivot) {
            right--;
        }
        while(left < right && arr[left] <= pivot) {
            left++;
        }
        if(left < right) {
            swap(arr, left, right);
        }
    }
    swap(arr, start, left);
    return left;
}

public static void innerQuickSort(int[] arr, int start, int end) {
    int pivot = innerFindPivot(arr, start, end);
    if(start < pivot - 1) {
        innerQuickSort(arr, start, pivot - 1);
    }
    if(pivot + 1 < end) {
        innerQuickSort(arr, pivot + 1, end);
    }
}

public static void quickSort(int[] arr) {
    innerQuickSort(arr, 0, arr.length - 1);
}
```

这个方法需要注意的细节还是比较多的，首先必须先right--，然后再left++，否则会有问题，具体为啥读者可以自行模拟一下。另外=是在left这边，最后返回也是返回left。

还有一个问题是pivot的选择，可以选第一个数，也可以选择一个随机数，选择随机数就先和第一个数交换，然后再进行后续流程就行。

还有一种双边的方法是可以不用选择pivot的，我们一起看一下。

```java
public static void swap(int[] arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}

public static int innerFindPivot(int[] arr, int start, int end) {
    int left = start, right = end;
    boolean isLeft = true;
    while(left < right) {
        if(arr[left] <= arr[right]) {
            if(isLeft) {
                left++;
            } else {
                right--;
            }
        } else {
            swap(arr, left, right);
            isLeft = !isLeft;
        }
    }
    return left;
}

public static void innerQuickSort(int[] arr, int start, int end) {
    int pivot = innerFindPivot(arr, start, end);
    if(start < pivot - 1) {
        innerQuickSort(arr, start, pivot - 1);
    }
    if(pivot + 1 < end) {
        innerQuickSort(arr, pivot + 1, end);
    }
}

public static void quickSort(int[] arr) {
    innerQuickSort(arr, 0, arr.length - 1);
}
```

这种是双边交换之后自动产生pivot的位置，不过你仔细模拟一下会发现，它其实也是选择了最后一个元素作为pivot，但是这种方法细节就少多了，而且left和right是对称的，所以我更喜欢这种方法。

还有一种单边的方法，也一起看一下，这种单边的方法相对双边会更简单一些。

```java
public static void swap(int[] arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}

public static int innerFindPivot(int[] arr, int start, int end) {
    int mark = start;
    int pivot = arr[start];
    for(int i = start + 1; i <= end; i++) {
        if(arr[i] < pivot) {
            mark++;
            swap(arr, mark, i);
        }
    }
    swap(arr, start, mark);
    return mark;
}

public static void innerQuickSort(int[] arr, int start, int end) {
    int pivot = innerFindPivot(arr, start, end);
    if(start < pivot - 1) {
        innerQuickSort(arr, start, pivot - 1);
    }
    if(pivot + 1 < end) {
        innerQuickSort(arr, pivot + 1, end);
    }
}

public static void quickSort(int[] arr) {
    innerQuickSort(arr, 0, arr.length - 1);
}
```

单边的思想就是保证mark的位置就是pivot的位置，这里也要注意一下，首先i一直是>=mark的，其次mark的位置是小于pivot的数的边界。

关于快排大家还是要细细体会，如果还没理解的话自己画图模拟一下，实在不行背也要背下来，我建议大家至少要能够手写其中一种方法。

### 练习题

学习完快速排序，大家可以去做一下以下的习题：

leetcode912 排序数组

leetcode147 链表排序

[原文链接](https://mp.weixin.qq.com/s?__biz=MzIzMTE1ODkyNQ==&mid=2649414095&idx=1&sn=eeb4abb9a6c0bb28b02f73f91958b3f0&chksm=f0b61c2dc7c1953be53135c828a2203f0f7cef565440ab17fb822fbb9cbcc9072a99d4224802&token=1400467286&lang=zh_CN#rd)

![follow](/assets/follow.webp "follow")

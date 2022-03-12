![mergeSort](/assets/codes/basic/mergeSort.webp "mergeSort")

### 前言

归并排序也是面试中问得比较多的，代码量适中，也需要使用递归来实现，而且理解起来比快速排序要简单，实现方式也较为单一，所以也广受面试官的喜爱。

还没有了解归并排序原理的可以看一下上面的图，它其实就是先把要排序的数组分为左右两部分，左边和右边分别先递归排好序，然后再将两个有序的数组合并为一个数组。

### 实现

这里需要注意的就是我们需要一个innerMergeSort的函数实现递归，具体可以直接看下代码。

```java
public static void innerMergeSort(int[] arr, int start, int end) {
    int mid = (end - start) / 2 + start;
    if(start < mid) {
        innerMergeSort(arr, start, mid);
    }
    if(mid + 1 < end) {
        innerMergeSort(arr, mid + 1, end);
    }
    int[] tmp = new int[end - start + 1];
    int index = 0, index1 = start, index2 = mid + 1;
    while(index1 <= mid && index2 <= end) {
        if(arr[index1] <= arr[index2]) {
            tmp[index++] = arr[index1++];
        } else {
            tmp[index++] = arr[index2++];
        }
    }
    while(index1 <= mid) {
        tmp[index++] = arr[index1++];
    }
    while(index2 <= end) {
        tmp[index++] = arr[index2++];
    }
    for(int i = 0; i < end - start + 1; i++) {
        arr[start + i] = tmp[i];
    }
}

public static void mergeSort(int[] arr) {
    innerMergeSort(arr, 0, arr.length - 1);
}
```

先找到mid，然后对start到mid和mid+1到end分别排序，最后将两个有序数组merge。在merge的时候，需要定义一个额外空间tmp，同时定义三个指针，分别指向两个有序数组和我们的目标数组。最后再将tmp的元素拷贝到原数组中。

### 应用

其实归并排序有一个特点，它不需要一次性将数据全部加载到内存，也就是说用它的思想可以排序很大的文件。这其实也就是外部排序的思想。

所以如果有面试官问你有一个10亿整数数据的文件，但是内存有限，你该怎么排序，你应该想到归并排序。

### 练习题

学习完归并排序，大家可以去做一下以下的习题：

leetcode912 排序数组

leetcode147 链表排序

[原文链接](https://mp.weixin.qq.com/s?__biz=MzIzMTE1ODkyNQ==&mid=2649414108&idx=1&sn=1d793ead117fbd18a1bea89427407b30&chksm=f0b61c3ec7c1952844b6da15c023f8f057424f164f1ab2b011e9b3819f7e93b9478f803f53f6&token=1400467286&lang=zh_CN#rd)

![follow](/assets/follow.webp "follow")

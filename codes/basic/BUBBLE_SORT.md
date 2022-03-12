![bubbleSort](/assets/codes/basic/bubbleSort.webp "bubbleSort")

### 前言

只要学过编程，多多少少会接触到冒泡排序，这是一个非常直观的排序算法，不太需要费脑筋就能理解。

但是题目简单不代表面试简单，面试不仅仅是要把题目做出来，还要比别的面试候选人做得更好，这就得对冒泡排序研究一番。

### 90%人都能写出来的代码

不出意外，学过一点编程的人，稍微准备一下，都能写出来这个版本。

```java
public static void swap(int[] arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}

public static void bubbleSort(int[] arr) {
    for(int i = 0; i < arr.length - 1; i++) {
        for(int j = arr.length - 2; j >= i; j--) {
            if(arr[j] > arr[j + 1]) {
                swap(arr, j, j + 1);
            }
        }
    }
}
```

其实实现的关键就是计算需要排多少趟，以及每趟需要交换多少次，只要这个计算没问题，代码实现还是比较简单的。

这是把最小的往上冒泡，当然你也可以把最大的往下“沉”，但是这时候如果面试官问你还能不能优化，你会怎么办？

### 优化

大部分的教科书提供了上面这种实现就没有再往下讲了。如果你平时根本没思考过怎么优化，那被问到的时候，可能一时半会儿还真想不出来。

首先你可以这样想，上面的实现其实每次都要排arr.length - 1趟，假如第1趟就已经有序，后面的不是都白白浪费了吗？所以第一个优化点就是定义一个变量，来判断每一趟排序后是否已经有序。

```java
public static void swap(int[] arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}

public static void bubbleSort(int[] arr) {
    for(int i = 0; i < arr.length - 1; i++) {
        boolean isSorted = true;
        for(int j = arr.length - 2; j >= i; j--) {
            if(arr[j] > arr[j + 1]) {
                swap(arr, j, j + 1);
                isSorted = false;
            }
        }
        if(isSorted) {
            return;
        }
    }
}
```

到这里，应该可以甩掉70%的人了，但是面试官还想继续压榨，问了句还能再优化吗？

到这里想再优化不是一件容易的事情，不过你再仔细想想排序的细节，我们每次都是从后往前比较交换，但是假如上一趟交换从某个点开始，前面的都已经有序了，下一趟的时候是不是就可以不用再去比较和交换那个点前面的数据了？

所以这里我们可以再定义一个变量来完成这件事情。

```java
public static void swap(int[] arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}

public static void bubbleSort(int[] arr) {
    int lastExchangeIndex = 0;
    for(int i = 0; i < arr.length - 1; i++) {
        boolean isSorted = true;
        int sortBorder = lastExchangeIndex;
        for(int j = arr.length - 2; j >= sortBorder; j--) {
            if(arr[j] > arr[j + 1]) {
                swap(arr, j, j + 1);
                isSorted = false;
                lastExchangeIndex = j;
            }
        }
        if(isSorted) {
            return;
        }
    }
}
```

到这里已经向面试官展示了你的深入思考能力，这道题大概率能过了。

### 拓展

如果面试官还是那句话，还能继续优化吗？其实还有优化的手段，有一种双向冒泡的方法，但是代码量会增加一倍，一般面试不会让写，如果真的被问到，可以说一下思路，读者也可以想想双向冒泡能解决什么问题，作为拓展阅读吧。

### 练习题

学习完冒泡排序，大家可以去做一下以下的习题：

leetcode912 排序数组

leetcode147 链表排序

[原文链接](https://mp.weixin.qq.com/s?__biz=MzIzMTE1ODkyNQ==&mid=2649414092&idx=1&sn=53207436661add280ac052aace4f13ff&chksm=f0b61c2ec7c19538a3167b64378ab5b37e57f29fa81f5a02dcd8431a3c91b66628d2c63c969c&token=1115583065&lang=zh_CN#rd)

![follow](/assets/follow.webp "follow")

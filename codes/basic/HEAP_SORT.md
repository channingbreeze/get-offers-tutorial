![heapSort](/assets/codes/basic/heapSort.webp "heapSort")

### 前言

堆这种结构实在是用得太多了，而且一般考到的话都是中等或者困难的题，关键是在没有提示的情况下，初学者很难想到可以用堆来进行优化。

如果对堆还没概念，我这里简单描述一下。堆是一种二叉树，分大顶堆和小顶堆，大顶堆就是根元素要大于左右两边的元素，并且左右两边都是大顶堆。小顶堆相反，根元素小于左右两边的元素，并且左右两边都是小顶堆。

所以对于堆来说，堆顶元素是整个堆的最值。堆排序就是利用了堆的这个性质，每次将堆顶元素取出，然后剩余元素调整形成新的堆，再次将堆顶元素取出，这样最终就能排序了。

### 外援

首先我们可以用语言自己提供的堆的实现来实现堆排序，看下代码。

```java
public static void heapSort(int[] arr) {
    PriorityQueue<Integer> heap = new PriorityQueue<>();
    for(int i = 0; i < arr.length; i++) {
        heap.offer(arr[i]);
    }
    for(int i = 0; i < arr.length; i++) {
        arr[i] = heap.poll();
    }
}
```

在java中，PriorityQueue就是堆（也叫优先队列），offer方法是把一个元素加入到堆里面，并且内部进行调整，使它重新满足堆的条件，poll方法是将堆顶元素取出，并且内部进行调整，使其他元素重新满足堆的条件。

有了上面的知识我们很简单就能理解了，我们直接把数组放到堆里，然后一个个从堆顶取出就好，这样就自动排好序了。

如果考堆排序，面试官一般不会让你用PriorityQueue，但是我们也得会，因为如果其他题用到堆，一般是可以直接用PriorityQueue的。

### 内功

自己实现，我们其实就是去实现调整堆的逻辑。先看一下代码，我们对着代码讲。

```java
public static void swap(int[] arr, int i, int j) {
    int tmp = arr[i];
    arr[i] = arr[j];
    arr[j] = tmp;
}

public static void downAdjust(int[] arr, int parent, int end) {
    int left = parent * 2, right = parent * 2 + 1;
    if(left > end) {
        return;
    }
    if(right <= end && arr[parent] < arr[right] && arr[left] < arr[right]) {
        swap(arr, parent, right);
        parent = right;
    } else if(arr[parent] < arr[left]) {
        swap(arr, parent, left);
        parent = left;
    } else {
        return;
    }
    downAdjust(arr, parent, end);
}

public static void heapSort(int[] arr) {
    for(int i = arr.length / 2; i >= 0; i--) {
        downAdjust(arr, i, arr.length - 1);
    }
    for(int i = arr.length - 1; i > 0; i--) {
        swap(arr, 0, i);
        downAdjust(arr, 0, i - 1);
    }
}
```

其实主要分两步，第一步先把原来的数组变成一个堆，我们从arr.length/2开始，因为再往后都是叶子节点，没必要调整了。而每次调整，我们调用的是downAdjust，这个是核心方法。

在downAdjust里面，一定要注意定义一个end作为边界，然后找到左右孩子，将parent和左右孩子中较大的交换，交换之后将parent设为孩子的位置，再继续递归。

这样全部调整完之后，就会形成一个大顶堆，然后从后往前，每次把堆顶和数组元素交换，这样数组就变成有序数组啦。

说实话这个实现还是有一定技巧的，面试中出现的也不多，但是堆这个数据结构一定要掌握，面试很有帮助。

### 练习题

学习完堆排序，大家可以去做一下以下的习题：

leetcode912 排序数组

leetcode147 链表排序

[原文链接](https://mp.weixin.qq.com/s?__biz=MzIzMTE1ODkyNQ==&mid=2649414109&idx=1&sn=8ddda988a526028c9545f82a4f9d0fea&chksm=f0b61c3fc7c19529d3053d631933562689e63c5a068e64bea2c534ac61cc634e5b4aba77f60f&token=1400467286&lang=zh_CN#rd)

![follow](/assets/follow.webp "follow")

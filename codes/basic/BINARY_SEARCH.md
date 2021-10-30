### 前言

写这篇文章前，我看了很多讲二分查找的文章，包括公众号和知乎的，在我看来，讲得都比较差，说几点原因：

1、要么只讲最简单的情况

2、要么讲了稍复杂的情况，但是一会儿闭区间一会儿开区间，一会儿加一减一，一会儿又不加减，一会儿还要打个补丁

二分法细节确实比较多，但是真正理解之后，是可以有一个通用模板的，根本不需要考虑到底是开区间还是闭区间，加一还是减一还是不加减，更不需要打补丁。

本文只讲最基础的情况，但是我认为最基础的情况，至少也包括五种，而我看到的目前市面上的文章，很少讲全了五种。

我们先假设数组是升序排列，再给你一个数target。

题目总共可能有五种情况，这五种情况分别是：

1、最基础版，只要找到target就行，返回下标，找不到返回-1

2、进阶版一，找到等于target的左边界(数组中可能有多个数都等于target)，找不到返回-1

3、进阶版二，找到等于target的右边界(数组中可能有多个数都等于target)，找不到返回-1

4、进阶版三，找到小于target的最大值，找不到返回-1

5、进阶版四，找到大于target的最小值，找不到返回-1

### 最基础版二分法

首先你需要知道最基础版应该怎么写，二分法的思想其实很简单，就是先取中间的数，然后和target比较，如果小了就往右找，大了就往左找。然后再取中间数，如此循环，直到中间没有数了。

思路我相信大家都能理解，但是在写法上，实际上还有一个双指针的思想，也就是我们需要left和right两个指针，然后不断调整两个指针的位置，最终得到答案。

废话不多说，我们看一下代码。

```java
public static int commonBinarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    while(left <= right) {
        int mid = left + (right - left) / 2;
        if(arr[mid] == target) {
            return mid;
        }
        if(arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;
}
```

代码其实很简单我就不多解释了。

我相信可能有同学看过其他版本，比如所谓的开区间版本，但是我告诉大家，没有必要去看去记这么多版本，你只要会一种就能行走天下，而我建议大家就写上面这种。

这种也叫闭区间版本，每一个数都会搜索到，需要注意的几个点

1、right初始是arr.length-1，很好理解，因为这是数组右边界下标

2、while里面是left<=right，也好理解，因为我们每个数都要搜索，当left=right时，我们当然要进里面再判断一下

3、区间范围缩小时，left=mid+1或者right=mid-1，也好理解，因为arr[mid]已经不是我们要找的数，所以范围需要加一或者减一

之所以要推荐闭区间版本，是因为它最符合我们的直观逻辑，而且它是对称的，是把left和right一视同仁，而不会像有的版本left需要加一，但是right却不需要减一。

### 进阶版

有了基础模板，我们怎么去做进阶版呢？大家可以先思考一下第二个问题。

说实话，如果是第一次碰到这个问题，还真不一定能找到最优解，第一个直观感受可能是我找到mid后，在往左一个一个地看，看看哪里是边界，但是这样，时间复杂度可能退化成o(n)。

其实还是应该用二分的思想找，关键在于arr[mid]==target时候的处理。

我们来看一下我的解法。

```java
public static int firstBinarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    int res = -1;
    while(left <= right) {
        int mid = left + (right - left) / 2;
        if(arr[mid] == target) {
            res = mid;
            right = mid - 1;
        } else if(arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return res;
}
```

这里的一个技巧就是当arr[mid]==target时，因为要找左边界，我们把right=mid-1，也就是改变右边界从而缩小范围。这时候其实存在两种情况，要么答案已经是res(左边界)，[left,right]里面已经没有答案，while再也不会更新res，最终返回res是正确的，要么res还不是最终答案，那么答案就会在[left,right]里面，而在while中，我们总能找到它从而更新res，最终返回res也是正确的。

这里我们额外定义了一个res表示结果，这一步是整个解法的精髓。相对于很多其他教程在考虑到底是返回left还是left+1还是right还是right-1，直接定义一个res要好理解得多。

这里也要注意几点：

1、res一定是符合条件的，比如在这里，我们只有当arr[mid]==target的时候，我们才会更新res。

2、res会随着搜索范围减小越来越接近答案，当搜索结束时，res就是答案。

3、如果一次都没有更新res，说明整个数组里没有满足条件的数，res应该为-1，也就是初始值。

理解了第二问你再做第三问，我相信你直接看代码就行，都不需要我解释。

```java
public static int lastBinarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    int res = -1;
    while(left <= right) {
        int mid = left + (right - left) / 2;
        if(arr[mid] == target) {
            res = mid;
            left = mid + 1;
        } else if(arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return res;
}
```

同样，你做第四问和第五问，只需要考虑什么时候更新res，因为第四问要找的数是小于target的，所以应该在arr[mid]<target时更新res，同理，第五问应该在arr[mid]>target时更新res。

第四问：

```java
public static int lowerBiggest(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    int res = -1;
    while(left <= right) {
        int mid = left + (right - left) / 2;
        if(arr[mid] < target) {
            res = mid;
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return res;
}
```

第五问：

```java
public static int higherSmallest(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    int res = -1;
    while(left <= right) {
        int mid = left + (right - left) / 2;
        if(arr[mid] <= target) {
            left = mid + 1;
        } else {
            res = mid;
            right = mid - 1;
        }
    }
    return res;
}
```

### 题外话

通过这个问题，我也说说题外话。我记得曾经看过linus的一个[视频](https://m.weibo.cn/status/4216882070442401?wm=3333_2001&sourcetype=weixin&featurecode=newtitle&from=timeline&isappinstalled=0)，其中一个点我印象很深刻，他举了一个单链表删除节点的例子，他说所有的教科书都会告诉我们要分情况，删除头节点和删除一个中间节点是不一样的逻辑。但是他觉得不是，他说要从另外一个角度看，这两种情况可以统一，而一个逻辑统一的代码才是好代码。


### 练习题

学习完二分查找，大家可以去做一下以下的习题：

leetcode704 二分查找

leetcode34 在排序数组中查找第一个和最后一个元素

leetcode35 搜索插入位置

leetcode33 搜索旋转排序数组

leetcode162 寻找峰值

还不会的话关注我，后续讲解。

[原文链接](https://mp.weixin.qq.com/s?__biz=MzIzMTE1ODkyNQ==&mid=2649414053&idx=1&sn=f8e4acbaaa15de712540e2d2d7a76186&chksm=f0b61c47c7c19551d3cf6eabadee922c491bcc230e3def24212dcb401c86e9f8960966d048e1&token=1678213786&lang=zh_CN#rd)

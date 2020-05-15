# 简单排序
主要操作：比较两个数据项、交换两个数据项或复制其中一项，具体操作要看排序的类型。
### 1.冒泡排序
运行效率较低, 但是在概念上它是排序算法中最简单的，最适合初学的一种排序，具体思路为：
- 对未排序的各元素从头到尾依次比较相邻的两个元素大小关系
- 如果左边的队员高, 则两队员交换位置
- 向右移动一个位置, 比较下面两个队员
- 当走到最右端时, 最高的队员一定被放在了最右边
- 继续从最左边开始排序，把第二高的队员排到倒数第二位置上
- 依此类推，完成排序

```javascript
ArrayList.prototype.bubbleSort = function () {
    // 1.获取数组的长度
    var length = this.array.length

    // 2.反向循环, 因此次数越来越少
    for (var i = length - 1; i >= 0; i--) {
        // 3.根据i的次数, 比较循环到i位置
        for (var j = 0; j < i; j++) {
            // 4.如果j位置比j+1位置的数据大, 那么就交换
            if (this.array[j] > this.array[j+1]) {
                // 交换
                this.swap(j, j+1)
            }
        }
    }
}

ArrayList.prototype.swap = function (m, n) {
    var temp = this.array[m]
    this.array[m] = this.array[n]
    this.array[n] = temp
}
```
冒泡排序的效率为O(N²)，比较次数为N²，交换次数也为N²
### 2.选择排序
思路：
- 选定第一个索引位置，然后和后面元素依次比较
- 如果后面的队员, 小于第一个索引位置的队员, 则交换位置
- 经过一轮的比较后, 可以确定第一个位置是最小的
- 然后使用同样的方法把剩下的元素逐个比较即可
- 可以看出选择排序，第一轮会选出最小值，第二轮会选出第二小的值，直到最后

```javascript
ArrayList.prototype.selectionSort = function () {
    // 1.获取数组的长度
    var length = this.array.length

    // 2.外层循环: 从0位置开始取出数据, 直到length-2位置
    for (var i = 0; i < length - 1; i++) {
        // 3.内层循环: 从i+1位置开始, 和后面的内容比较
        var min = i
        for (var j = min + 1; j < length; j++) {
            // 4.如果i位置的数据大于j位置的数据, 那么记录最小的位置
            if (this.array[min] > this.array[j]) {
                min = j
            }
        }
        // 5.交换min和i位置的数据
        this.swap(min, i)
    }
}
```
选择排序虽然效率也是O(N²)，但在一定程度上优化了冒泡排序的效率，比较次数跟选择排序的效率一样都是O(N²)，交换次数是O(N)。
### 3.插入排序
> 插入排序是简单排序中效率最好的一种，也是学习其他高级排序的基础, 比如希尔排序/快速排序, 所以也非常重要。

思路：
- 从第一个元素开始，该元素可以认为已经被排序
- 取出下一个元素，在已经排序的元素序列中从后向前扫描
- 如果该元素（已排序）大于新元素，将该元素移到下一位置
- 重复上一个步骤，直到找到已排序的元素小于或者等于新元素的位置
- 将新元素插入到该位置后, 重复上面的步骤

```javascript
ArrayList.prototype.insertionSort = function () {
    // 1.获取数组的长度
    var length = this.array.length

    // 2.外层循环: 外层循环是从1位置开始, 依次遍历到最后
    for (var i = 1; i < length; i++) {
        // 3.记录选出的元素, 放在变量temp中
        var j = i
        var temp = this.array[i]

        // 4.内层循环: 内层循环不确定循环的次数, 最好使用while循环
        while (j > 0 && this.array[j-1] > temp) {
            this.array[j] = this.array[j-1]
            j--
        }

        // 5.将选出的j位置, 放入temp元素
        this.array[j] = temp
    }
}
```
插入排序对已经有序或基本有序的数据来说，效率高得多，它的效率还是O(N²)，它的比较次数是选择排序的一半，所以这个算法效率是高于选择排序的。
# 高级排序
### 1. 希尔排序

> 希尔排序是插入排序的一种高效的改进版, 并且效率比插入排序要更快

插入排序的问题：当某个很小的数据比如1在最右端，要想把它放到正确的索引为0的位置，必须把每个前面的数据都向右移动一位，即比较和交换的次数都是N。

希尔排序就是在此基础上提出了一种不需要一个个移动所有中间数据项的算法，大致思路就是先将数据分成间隔比较大的分组进行排序，接着分成间隔小的分组进行排序，一步步缩小间隔，直到间隔为1。分完间隔排序后每次都让数据离自己正确位置更近了，间隔为1的排序就是插入排序了，此时大家都离自己正确的位置很近了，就不需要再对比交换那么多次数了，比如：
- 有数组 81, 94, 11, 96, 12, 35, 17, 95, 28, 58, 41, 75, 15
- 先让间隔为5, 进行排序. (35, 81), (94, 17), (11, 95), (96, 28), (12, 58), (35, 41), (17, 75), (95, 15)
- 排序后的新序列, 一定可以让数字离自己的正确位置更近一步
- 再让间隔位3, 进行排序. (35, 28, 75, 58, 95), (17, 12, 15, 81), (11, 41, 96, 94)
- 排序后的新序列, 一定可以让数字离自己的正确位置又近了一步
- 最后, 让间隔为1, 也就是正确的插入排序. 这个时候数字都离自己的位置更近, 那么需要复制的次数一定会减少很多
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200515085112286.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjU5Nzg4MA==,size_16,color_FFFFFF,t_70)
所以，希尔排序间隔的选取非常重要，大致的选择思路为：
- 希尔排序的原稿中, 建议的初始间距是N / 2, 简单的把每趟排序分成两半
- 希尔排序的效率很增量是有关系的，但是, 它的效率证明非常困难, 甚至某些增量的效率到目前依然没有被证明出来
- 但是经过统计, 希尔排序使用原始增量, 最坏的情况下时间复杂度为O(N²), 通常情况下都要好于O(N²)

```javascript
ArrayList.prototype.shellSort = function () {
    // 1.获取数组的长度
    var length = this.array.length

    // 2.根据长度计算增量
    var gap = Math.floor(length / 2)

    // 3.增量不断变量小, 大于0就继续排序
    while (gap > 0) {
        // 4.实现插入排序
        for (var i = gap; i < length; i++) {
            // 4.1.保存临时变量
            var j = i
            var temp = this.array[i]

            // 4.2.插入排序的内层循环
            while (j > gap - 1 && this.array[j - gap] > temp) {
                this.array[j] = this.array[j - gap]
                j -= gap
            }

            // 4.3.将选出的j位置设置为temp
            this.array[j] = temp
        }
      
        // 5.重新计算新的间隔
        gap = Math.floor(gap / 2)
    }
}
```

总之, 我们使用希尔排序大多数情况下效率都高于简单排序, 甚至在合适的增量和N的情况下, 还好好于快速排序
### 2.快速排序

> 快速排序几乎可以说是目前所有排序算法中, 最快的一种排序算法。当然希尔排序确实在某些情况下可能好于快速排序.，但是大多数情况下, 快速排序还是比较好的选择.
> 快速排序非常重要，是面试中提升水平的排序算法

快速排序是冒泡排序的升级版，冒泡排序要交换很多次才能将最大值放到正确的位置上，而快速排序可以在一次循环中(其实是递归调用)找出某个元素的正确位置, 并且该元素之后不需要任何移动。

快速排序最重要的思想是分而治之，比如：
- 先选择其中一个数字，将大于数字的放到右边，小于数字的放到左边
- 这个数字就被放到了自己肯定正确的地方
- 接下来再对左右两堆数据进行递归

这里的程序如何实现将选择的枢纽放到正确的位置也是需要需要技巧的，不可能每次对比完枢纽和每个值都交换位置，所以可以采取下面的思路实现将枢纽放到正确的位置：
- 首先将枢纽和最后一个数交换，即把枢纽放到最后位置 length-1
- 接着用两个指针 left 指向0，right指向length-2
- 循环left指针直到 left > 枢纽
- 循环right指针直到 right  < 枢纽
- 交换left和right位置
- 循环上述过程，直到 left <= right

所以如何选择这个数字也就是枢纽非常重要，常用的方法是取数组头、中、尾的中位数，例如 8、12、3的中位数就是8。

```javascript
// 选择枢纽
// 输入数据左右边界
// swap函数是交换数组两个数据位置
ArrayList.prototype.median = function (left, right) {
    // 1.求出中间的位置
    var center = Math.floor((left + right) / 2)

    // 2.判断并且进行交换
    if (this.array[left] > this.array[center]) {
        this.swap(left, center)
    }
    if (this.array[center] > this.array[right]) {
        this.swap(center, right)
    }
    if (this.array[left] > this.array[right]) {
        this.swap(left, right)
    }

    // 3.巧妙的操作: 将center移动到right - 1的位置.
    // 因为前面已经对比交换过，最右边的数是三个数中最小的，所以把中位数放到倒数第二
    this.swap(center, right - 1)

    // 4.返回pivot
    return this.array[right - 1]
}
```

```javascript
// 快速排序实现
ArrayList.prototype.quickSort = function () {
    this.quickSortRec(0, this.array.length - 1)
}

ArrayList.prototype.quickSortRec = function (left, right) {
    // 0.递归结束条件
    if (left >= right) return

    // 1.获取枢纽
    var pivot = this.median(left, right)

    // 2.开始进行交换
    // 2.1.记录左边开始位置和右边开始位置
    var i = left
    var j = right - 1
    // 2.2.循环查找位置
    while (true) {
        while (this.array[++i] < pivot) { }
        while (this.array[--j] > pivot) { }
        if (i < j) {
              // 2.3.交换两个数值
            this.swap(i, j)
        } else {
            // 2.4.当i<j的时候(一定不会=, 看下面解释中的序号3), 停止循环因为两边已经找到了相同的位置
            break
        }
    }

    // 3.将枢纽放在正确的位置
    this.swap(i, right - 1)

    // 4.递归调用左边
    this.quickSortRec(left, i - 1)
    this.quickSortRec(i + 1, right)
}
```
快速排序的最坏效率就是每次选择的枢纽都是最左边或最右边，此时效率等同于冒泡排序，但选择中位数的枢纽选择方法是不可能遇到最坏情况的，所以快速排序的平均效率是O(N * logN)，其他某些算法的效率也可以达到O(N * logN), 但是快速排序是最好的。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020051508593098.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjU5Nzg4MA==,size_16,color_FFFFFF,t_70)

## 一、概念
队列和堆都是一种数据结构，优先队列是用二叉堆实现的。优先队列和堆常用于辅助实现其它算法，例如数据压缩赫夫曼编码算法、Dijkstra最短路径算法、Prim最短生成树算法，优先队列还可以实现事件模拟、选择问题，操作系统的线程调度也使用优先队列。
#### 优先队列
**特点**：元素正常入队，按照优先级大小出队。
**实现方式**：有序表或无序表（即数组，或顺序表和链表）、二叉树、AVL平衡二叉树、堆和二叉堆。使用普通的表实现优先队列的性能比较低，有序表插入为O(1)，无序表取出为O(n)，另外使用普通二叉树实现优先队列只能保证平均时间为O(log n)，但最差情况是O(n)，AVL树则需要复杂的无关操作，而堆可以实现性能较好的优先队列，取出效率可达到O(1)，一般的操作都能达到O(log n)。
#### 二叉堆
**特点**：二叉堆是完全二叉树，分为最大堆和最小堆两种类型，最小堆中根结点的关键字是其它所有结点关键字的最小值，最大堆则是最大值。
**表示方式**：完全二叉树是二叉堆的逻辑表示方式，数组是二叉堆的物理表示，也是计算机中的实际形式。
**应用**：堆排序复杂度O(nlogn)、优先队列

```javascript
// 二叉堆构造函数，capacity: 容量（决定二叉堆大小），compare: 比较函数（决定是最大堆还是最小堆），array: 数组
function BinaryHeap(capacity, compare, array){
    if(array){
        this.array = array.concat();
        this.size = array.length;
        
        if(capacity < 3)
            this.capacity = this.size * 2;
        else
            this.capacity = capacity;
        this.compare = compare;
    }
    else{
        if(capacity < 3)
            return null;
        this.array = new Array(capacity);
        this.size = 0;
        this.capacity = capacity;
        this.compare = compare;
    }
}
// 检查二叉堆是否已空
BinaryHeap.prototype.isEmpty = function(){
    return this.size == 0;
}
// 获取结点i的父结点
BinaryHeap.prototype.parent = function(i){
    return Math.floor((i - 1) / 2);
}

// 获取结点i的左儿子
BinaryHeap.prototype.left = function(i){
    return 2 * i + 1;
}

// 获取结点i的右儿子
BinaryHeap.prototype.right = function(i){
    return 2 * i + 2;
}
// 根据数组元素的索引进行交换
BinaryHeap.prototype.swap = function(i, j){
    var temp = this.array[i];
    this.array[i] = this.array[j];
    this.array[j] = temp;
}
// 二叉堆push操作，往堆中添加一个新元素
BinaryHeap.prototype.push = function(T){
	// 没有容量了
    if(this.size == this.capacity){
        console.log("overflow: could not push key.");
        return;
    }
    // 错位，数组0索引处是树的根节点，也可以不给0赋值，把数组索引和树的位置对应
    // 只要把数组长度加1
    this.size++;
    var i = this.size - 1;
    this.array[i] = T;
    // 上浮操作，由下而上检查，新元素和它的父节点比较
    while(i != 0 && this.compare(this.array[i], this.array[this.parent(i)])){
    	// 交换新元素和父节点位置
        this.swap(i, this.parent(i));
        // 把父节点索引给i，继续向上比较
        i = this.parent(i);
    }
}
// 下沉操作，修复二叉堆的性质，从根节点自上而下检查，需要比较父节点和它两个子节点
BinaryHeap.prototype.heapify = function(i){
    var left = this.left(i);
    var right = this.right(i);
    // small存储值比较小的节点
    var small = i;
    // 顺序比较 父元素 左子元素 右子元素
    // 比较父元素和左子元素
    if(left < this.size && this.compare(this.array[left], this.array[i]))
        small = left;
    // 比较左子元素和右子元素
    if(right < this.size && this.compare(this.array[right], this.array[small]))
        small = right;
    // 父元素的值改变，交换
    if(small != i){
        this.swap(i, small);
        // 继续下浮，恢复堆结构
        this.heapify(small);
    }
}
// 获取堆顶元素
BinaryHeap.prototype.top = function(){
    return this.array[0];
}
// 释放堆顶元素，将最后一个元素替换堆顶元素，然后进行下沉堆化操作
BinaryHeap.prototype.pop = function(){
    if(this.size <= 0){
        console.log("heap is empty.");
        return null;
    }
    if(this.size == 1){
        this.size--;
        return this.array[0];
    }
    var root = this.array[0];
    this.array[0] = this.array[this.size - 1];
    this.size--;
    // 从顶部下浮恢复结构
    this.heapify(0);
    return root;
}
// 改变第i个结点的值
BinaryHeap.prototype.decreaseKey = function(i, NT){
    this.array[i] = NT;
    // 判断改变过后是否还符合二叉堆性质
    while(i != 0 && this.compare(this.array[i], this.array[this.parent(i)])){
        this.swap(i, this.parent(i));
        i = this.parent(i);
    }
}
// 删除元素
BinaryHeap.prototype.delete = function(i){
    if(this.size <= 0){
        console.log("heap is empty.");
        return;
    }
    if(this.size == 1){
        this.size--;
        return;
    }
    // 把最后一个值替换到要删除的位置上
    this.array[i] = this.array[this.size - 1];
    // 然后数组长度减一
    this.size--;
    // 再下沉，恢复结构
    this.heapify(i);
}
// 构建堆，用于使用一个数组构建堆的时候进行堆化操作
BinaryHeap.prototype.buildHeap = function(){
	// 从数组中间的索引位置开始下浮
    for(var i = Math.floor(this.size / 2);i >= 0;i--)
        this.heapify(i);
}
```

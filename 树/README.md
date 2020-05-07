## 二、树的遍历
树的遍历非常重要，分为两类
- 深度优先遍历：前序、中序、后序，遍历方法有两种，递归方法最为直观易懂，但效率不高，可用栈迭代方法提供效率，但代码复杂。
- 广度优先遍历：中序，遍历用队列辅助
#### 1.前序遍历
```javascript
// 递归
var preorderTraversal = function(root) {
    let res = []
    let preorder = function(node) {
        if(!node) return
        res.push(node.val)
        preorder(node.left)
        preorder(node.right)
    }
    preorder(root)
    return res
};
```
用栈模拟递归遍历
```javascript
// 迭代遍历
var preorderTraversal = function(root) {
    if(!root) return []
    let res = []
    stack = [root]
    while(stack.length) {
        let node = stack.pop()
        res.push(node.val)
        
        if(node.right) stack.push(node.right)
        if(node.left) stack.push(node.left)
    }
    return res
};
```
#### 2. 中序遍历
[力扣：中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
```javascript
// 递归
var inorderTraversal = function(root) {
    if(!root) return []
    let res = []
    let inorder = function(node) {
        if(!node) return
        inorder(node.left)
        res.push(node.val)
        inorder(node.right)
    }
    inorder(root)
    return res
};
```
迭代的方法有很多，这一种是标记访问过节点的
```javascript
// 迭代
var inorderTraversal = function(root) {
    if(!root) return []
    let res = []
    let stack = [root]
    // 用指针指向当前遍历的节点
    let cur = root
    // 标记是否被访问过，不能一开始就把root当作已访问过
    let visited = new Set()
    while(stack.length) {
        // 只要还有左节点
        while(cur.left && !visited.has(cur)) {
            visited.add(cur)
            // 存入左节点
            stack.push(cur.left)
            // 指向下一个左节点
            cur = cur.left
        }
        // 弹出最左边节点
        cur = stack.pop()
        res.push(cur.val)
        // 有右节点就压入
       if(cur.right) {
            stack.push(cur.right)
            cur = cur.right
       }
    }
    return res
};
```
也可以不用标记节点
```javascript
// 迭代
var inorderTraversal = function(root) {
    if(!root) return []
    let res = []
    let stack = []
    // 用指针指向当前遍历的节点
    let cur = root
    // 栈里还有值或者当前节点不为空就要遍历，
    // 当前节点不为空是防止根节点取不到
    while(stack.length || cur) {
        // 所有不为空的节点都来这里入栈
        while(cur) {           
            stack.push(cur)
            // 中序遍历先循环左节点
            cur = cur.left
        }
        // 弹出节点
        cur = stack.pop()
        res.push(cur.val)
        // 不管右节点有没有值都先让指针指向
        cur = cur.right   
    }
    return res
};
```
#### 3.后序遍历
[力扣：后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

```javascript
// 递归
var postorderTraversal = function(root) {
    let res = []
    let traversal = function(node) {
        if(!node) return
        traversal(node.left)
        traversal(node.right)
        res.push(node.val)
    }
    preorder(root)
    return res
};
```
树的后序后序迭代最难想，用层序遍历和栈实现
```javascript
// 迭代
var postorderTraversal = function(root) {
    // 搞不懂为什么 return root 会报错
    if(!root) return []
    let res = []
    let stack = [root]

    while(stack.length) {
    	// 取出每个要遍历的节点
        let cur = stack.pop()
        res.push(cur.val)
        // 若有左节点、右节点，先后入栈
        if(cur.left) stack.push(cur.left)
        if(cur.right) stack.push(cur.right)
    }
    // 记得最后将层序遍历结果反转
    return res.reverse()
};
```
#### 4.通用深度遍历模板
可以利用图的深度遍历思路来进行树的通用深度遍历
- 用颜色标记节点的状态，未访问为白色（0），已访问的节点为灰色（1）。
- 如果遇到的节点为白色（0），则将其标记为灰色（1），然后将其右子节点、自身、左子节点依次入栈（改变入栈顺序就改变了遍历顺序）。
- 如果遇到的节点为灰色（1），加入结果。
```javascript
var postorderTraversal = function(root) {
    if(!root) return []
    let res = []
    // 0代表未访问过
    let stack = [[root, 0]]
    while(stack.length) {
        let node = stack.pop()
        if(!node[0]) continue
        // 未访问过
        if(node[1] == 0) {
        	// 先序
            stack.push([node[0].right, 0])
            stack.push([node[0].left, 0])
            // 1代表已访问过，下次不需要再访问
            stack.push([node[0], 1])
        } else {
            // 已访问节点，压入结果栈
            console.log(node[0].val)
            res.push(node[0].val)
        }
    }
    return res
};
```
改变节点压入顺序，就改变了遍历顺序

```javascript
// 中序
stack.push([node[0].right, 0])
stack.push([node[0], 1])
stack.push([node[0].left, 0])  
// 后序
stack.push([node[0], 1])
stack.push([node[0].right, 0])
stack.push([node[0].left, 0])  
```
#### 5.层序遍历
借助队列实现，转自[力扣题解](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/jie-zhu-queuezai-jie-guo-zhong-ti-xian-chu-ceng-ci/)


```javascript
var levelOrder = function(root) {
    if (!root) return [];
    const queue = [root];
    const res = []; // 存放遍历结果
    let level = 0; // 代表当前层数
    while (queue.length) {
        res[level] = []; // 第level层的遍历结果

        let levelNum = queue.length; // 第level层的节点数量
        while (levelNum--) {
            const front = queue.shift();
            res[level].push(front.val);
            if (front.left) queue.push(front.left);
            if (front.right) queue.push(front.right);
        }

        level++;
    }
    return res;
};
```
## 三、树的应用

#### 1.判断二叉树是否相同
#### 2. 树的深度

```javascript
// 深度
var maxDepth = function(root) {
    if(!root) return 0
    return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1
};
```


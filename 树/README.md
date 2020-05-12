# 一、二叉树
二叉树是每个结点至多只有两棵子树（度小于等于2）的树，且二叉树的子树有左右之分，次序不能颠倒。

> 二叉树和度为2的树区别：度为2的树至少有三个节点，而二叉树可以为空

### 1.满二叉树
满二叉树的每一层都含有最多的结点，且除了叶子结点之外的每个结点度数均为2，对满二叉树编号如下，根据这些号可以求出每一层的最大宽度等信息。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200510160313650.png)
### 2.完全二叉树
当有n个结点的二叉树每个结点都与满二叉树的编号为1~n的结点一一对应时，就是完全二叉树
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200510160329391.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjU5Nzg4MA==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200510160935379.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjU5Nzg4MA==,size_16,color_FFFFFF,t_70)
看出满二叉树和完全二叉树的区别了吗？其实完全二叉树就是叶子结点未填满的满二叉树。
### 3.二叉树的存储结构
- 顺序存储结构

按照满二叉树的编号将二叉树每个节点`i`存储在数组对应下标为`i-1`的位置上，一般二叉树也这样存，即使有空余，因此顺序存储多用于完全二叉树中
- 链式存储结构

用链表表示元素的逻辑关系，一个数据域和两个指针域，分别指向左孩子和右孩子
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200510161059179.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjU5Nzg4MA==,size_16,color_FFFFFF,t_70)
### 4.二叉搜索树
二叉搜索树，也叫二叉排序树、二叉查找树，或BST，其特点为：
- 若左子树非空，则左子树上所有结点值均小于根结点
- 若右子树非空，则右子树上所有结点值均大于根结点
- 左、右子树本身也是二叉搜索树

二叉搜索树的最重要性质为：**中序遍历序列是递增的**，利用这一个性质可以做许多有趣的事情。
# 二、树的遍历
树的遍历非常重要，分为两类
- 深度优先遍历：前序、中序、后序，遍历方法有两种，递归方法最为直观易懂，但效率不高，可用栈迭代方法提供效率，但代码复杂。
- 广度优先遍历：中序，遍历用队列辅助

递归通常有两种，一种是直接在整个函数中递归（可以直接使用函数的形参），一种是用辅助函数递归（不能直接使用形参，需要处理再调用辅助函数）

```javascript
// 函数递归
function isTree(root) {
	// 终止条件，一般都是判断节点是否为空
	if(!root) return true 
	// 否则，递归
	return isTree(root.right) && isTree(root.left)
}
```

```javascript
// 辅助函数递归
function Tree(root) {
	// 调用辅助函数
	return isTree(root)
}
function isTree(root) {
	// 终止条件，一般都是判断节点是否为空
	if(!root) return true 
	// 否则，递归
	return isTree(root.right) && isTree(root.left)
}
```

### 1.前序遍历
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
### 2. 中序遍历
[力扣：中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)
```javascript
// 递归
var inorderTraversal = function(root) {
    if(!root) return ［］
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
迭代的方法有很多，这一种是标记访问过节点的，依次加入左节点到栈中，直到没有左节点，再依次弹出左节点加入右节点。
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
### 3.后序遍历
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
树的后序后序迭代最难想，用层序遍历和栈实现，注意最后要反转数组
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
### 4.通用深度遍历模板
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
### 5.层序遍历
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
# 三、树的应用

### 1.树的递归
[力扣：对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)
```javascript
// 递归
function isMirror(leftroot, rightroot) {
    // 左右子树均为空，说明相等
    if(!leftroot && !rightroot) return true
    // 左右子树有一个不为空，说明不相等
    if(!leftroot) return false
    if(!rightroot) return false
    // 若两个节点值相同，且左子树的左子树等于右子树的右子树，左子树的右子树等于右子树的左子树
    if(leftroot.val == rightroot.val && isMirror(leftroot.left, rightroot.right) && isMirror(leftroot.right, rightroot.left)) return true
    else return false
}
var isSymmetric = function(root) {
    if(!root) return true
    return isMirror(root.left, root.right)
};
```
很明显可以看出来迭代的原理还是递归，只不过用while和栈模拟了递归
```javascript
// 迭代
var isSymmetric = function(root) {
    if(!root) return true
    let stack = [root.left, root.right]
    while(stack.length) {
        let left = stack.pop()
        let right = stack.pop()
        if(!left && !right) continue
        if(!left) return false
        if(!right) return false
        if(left.val == right.val) {
            stack.push(left.left)
            stack.push(right.right)
            stack.push(left.right)
            stack.push(right.left)
        } else {
            return false
        }
    }
    return true
};
```

### 2. 树的深度
树的最大深度可以用递归或广度优先遍历
```javascript
// 递归
var maxDepth = function(root) {
    if(!root) return 0
    return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1
};
// 迭代也可以求，用广度优先遍历
```
[树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)
这道题一定要理解清楚题意，求的是根节点到叶子节点的最短距离，所以要判断现在到达的节点是否为叶子节点。
```javascript
// 第一次见到这么多的递归条件，每个节点都有五种可能情况
var minDepth = function(root) {
    // (1)该节点为空，不记数
    if(!root) return 0
    // (2)该节点的左右节点均为空，说明是叶子节点，结束递归
    if(!root.left && !root.right) return 1
    // (3)左节点为空，但右节点不为空
    if(!root.left) return minDepth(root.right)+1
    // (4)右节点为空，但左节点不为空
    if(!root.right) return minDepth(root.left)+1
    // (5)左右节点均不为空，返回最小深度
    return Math.min(minDepth(root.left), minDepth(root.right)) + 1
};
```
### 3.回溯在树中的应用
[力扣：路径总和II](https://leetcode-cn.com/problems/path-sum-ii/)

```javascript
var pathSum = function(root, sum) {
    if(!root) return []
    let res = []
    let path = [root.val]
    let lookpath = function(root, sum, path) {
        // 到子节点
        if(!root.left && !root.right) {
            // 判断这条路是否满足条件
            if(root.val == sum) {
                res.push(path.slice())
            }
            return
        }
        // 剪枝条件，注意一定要考虑负数的情况
        // if(Math.abs(root.val) >= Math.abs(sum)) return
        // 不能剪枝，因为没说全是负数或者全是正数
        // 还是考虑问题不全面啊！！！！答错了两次，心痛！
        if(root.left) {
            path.push(root.left.val)
            lookpath(root.left, sum-root.val, path)
            path.pop()
        }
        if(root.right) {
            path.push(root.right.val)
            lookpath(root.right, sum-root.val, path)
            path.pop()
        }
    }
    lookpath(root, sum, path)
    return res
};
```
### 4.满二叉树的应用
根据满二叉树的层序序号，	求[力扣：二叉树的最大宽度](https://leetcode-cn.com/problems/maximum-width-of-binary-tree/)

```javascript
var widthOfBinaryTree = function(root) {
    if(!root) return 0
    let queue = [root]
    let idx = [1]
    // 根节点所在的第一层宽度肯定是1
    let max = 1
    while(queue.length) {
        const size = queue.length

        for(let i = 0; i < size; i++) {
            let node = queue.shift()
            let id = idx.shift()
            if(node.left) {
                queue.push(node.left)
                idx.push(2*id)
            }
            if(node.right) {
                queue.push(node.right)
                idx.push(2*id+1)
            }
        }
        // 只有一个值活着没有值不用考虑
        if(idx.length > 1) max = Math.max(max, idx[idx.length-1] - idx[0] + 1)
    }
    return max
};
```
### 5.由遍历序列构造二叉树
各个遍历序列的特点：
- 先序遍历中，第一个结点是二叉树的根节点，左子序列的第一个结点是左子树的根节点，右子序列的第一个结点是右子树的根节点
- 中序遍历中，根节点将序列分割成两个子序列，前一个序列就是根节点的左子树的中序序列，前一个序列就是根节点的右子树的中序序列
- 后序遍历中，最后一个结点是二叉树的根节点，左子序列的第一个结点是左子树的根节点，右子序列的第一个结点是右子树的根节点

可以由以下几个遍历序列构造二叉树
- 先序和中序
- 后序和中序
- 层序和中序
- 先序和后序

注意：这类题目都要`if(!root) return null`，因为返回类型是树的节点
**[力扣：从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)**

```javascript
var buildTree = function(preorder, inorder) {
    if(!preorder.length) return null
    let root = new TreeNode(preorder[0])
    if(preorder.length==1) return root
    // 因为树中没有重复的元素，如果有了就不能用这种方法分割中序序列了
    let mid = inorder.indexOf(preorder[0])
           
    root.left = buildTree(preorder.slice(1,mid+1),inorder.slice(0,mid))
    root.right = buildTree(preorder.slice(mid+1),inorder.slice(mid + 1))
    return root
};
```
[力扣：从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
[力扣：根据前序和后序遍历构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)
### 6.二叉搜索树
[力扣：将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

```javascript
var sortedArrayToBST = function(nums) {
    const len = nums.length   
    if(!len) return null
    const r = Math.floor(len/2) 
    let root = new TreeNode(nums[r]) 
    root.left = sortedArrayToBST(nums.slice(0, r))
    root.right = sortedArrayToBST(nums.slice(r+1, len))
    return root
};
```
[力扣：验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)
根据性质二叉搜索树的中序序列是递增序列，在二叉树中序遍历的代码中加一个判断即可。

```javascript
var isValidBST = function(root) {
    let stack = [];
    // js中最小值的表示方法
    let inorder = -Infinity;
    let res = []

    while (stack.length || root !== null) {
        while (root !== null) {
            stack.push(root);
            root = root.left;
        }
        root = stack.pop();
        res.push(root.val)
        // 如果中序遍历得到的节点的值小于等于前一个 inorder，说明不是二叉搜索树
        if (root.val <= inorder) return false;
        inorder = root.val;
        root = root.right;
    }
    return true;
};
```
### 7.二叉树的祖先
[二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

```javascript
var lowestCommonAncestor = function(root, p, q) {
    if(!root || root==p || root==q) return root
    let left = lowestCommonAncestor(root.left, p, q)
    let right = lowestCommonAncestor(root.right, p, q)
    if(left && right) return root
    if(!left && !right) return
    if(!left) return right
    if(!right) return left
};
```

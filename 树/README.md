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
迭代
```javascript
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

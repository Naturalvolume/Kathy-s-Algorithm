栈的应用
[力扣739:每日温度](https://leetcode-cn.com/problems/daily-temperatures/submissions/)

递减栈的使用
```javascript
let Node = function(idx, val) {
    this.idx = idx;
    this.val = val;
}
var dailyTemperatures = function(T) {
    const len = T.length;
    if(!len) return [];
    let stack = [];
    let list = new Array(len);
    for(let i = 0; i < len; i++) {
        // 当递减栈为空，就把元素加入栈中
        if(stack.length==0) {
            let node = new Node(i, T[i])
            stack.push(node)
            continue;
        }
        // 元素小于递减栈的栈顶
        
        while(stack.length>0 && T[i] > stack[stack.length-1].val) {
            // 弹出栈顶元素            
            let node = stack.pop();       
            list[node.idx] = i - node.idx;
        } 
        // 不管大于小于都要,压入新元素
        let node = new Node(i, T[i])
        stack.push(node)
        
    }
    // 栈内剩余的都是不会升高的了
    
    while(stack.length) {
        let node = stack.pop();
        list[node.idx] = 0;
    }
    return list;
};
```
深度优先遍历（DFS）
1. 使用递归
2. 使用栈代替递归
##### [岛屿数量](https://leetcode-cn.com/problems/number-of-islands/solution/pythonjavascript-tao-lu-dfs200-dao-yu-shu-liang-by/)
在队列中用BFS实现过，现在用DFS实现
##### [克隆图](https://leetcode-cn.com/problems/clone-graph/)

递归实现
```javascript
var cloneGraph = function(node) {
    if(!node) return node
    // 不能用集合做映射，这样只能存储新创建节点的地址
    // 而每次是通过旧的节点地址判断是否访问过
    // 所以会出错
    // let visited = new Set()
    // 使用字典，用原地址做键，新地址做值，将新旧地址映射成键值对即可
    let dict = new Map();
    let clone = function(node) {
        // 死循环 无法判断
        if(dict.has(node)) return dict.get(node);
        let val = node.val
        let n = new Node(val);
        dict.set(node, n)
        for(let item of node.neighbors) {
            n.neighbors.push(clone(item))
        }      
        return n;
    }
    return clone(node);
};
```
## 栈的应用
[力扣739:每日温度](https://leetcode-cn.com/problems/daily-temperatures/submissions/)

#### 1.递减栈的使用
```javascript
let Node = function(idx, val) {
    this.idx = idx;
    this.val = val;
}
var dailyTemperatures = function(T) {
    const len = T.length;
    if(!len) return [];
    let stack = [];
    let list = new Array(len);
    for(let i = 0; i < len; i++) {
        // 当递减栈为空，就把元素加入栈中
        if(stack.length==0) {
            let node = new Node(i, T[i])
            stack.push(node)
            continue;
        }
        // 元素小于递减栈的栈顶
        
        while(stack.length>0 && T[i] > stack[stack.length-1].val) {
            // 弹出栈顶元素            
            let node = stack.pop();       
            list[node.idx] = i - node.idx;
        } 
        // 不管大于小于都要,压入新元素
        let node = new Node(i, T[i])
        stack.push(node)
        
    }
    // 栈内剩余的都是不会升高的了
    
    while(stack.length) {
        let node = stack.pop();
        list[node.idx] = 0;
    }
    return list;
};
```
#### 2.深度优先遍历（DFS）
- 使用递归
- 使用栈代替递归
##### [岛屿数量](https://leetcode-cn.com/problems/number-of-islands/solution/pythonjavascript-tao-lu-dfs200-dao-yu-shu-liang-by/)
在队列中用BFS实现过，现在用DFS实现
##### [克隆图](https://leetcode-cn.com/problems/clone-graph/)

递归实现
```javascript
var cloneGraph = function(node) {
    if(!node) return node
    // 不能用集合做映射，这样只能存储新创建节点的地址
    // 而每次是通过旧的节点地址判断是否访问过
    // 所以会出错
    // let visited = new Set()
    // 使用字典，用原地址做键，新地址做值，将新旧地址映射成键值对即可
    let dict = new Map();
    let clone = function(node) {
        // 死循环 无法判断
        if(dict.has(node)) return dict.get(node);
        let val = node.val
        let n = new Node(val);
        dict.set(node, n)
        for(let item of node.neighbors) {
            n.neighbors.push(clone(item))
        }      
        return n;
    }
    return clone(node);
};
```
栈辅助实现

```javascript
var cloneGraph = function(node) {
    if(!node) return node;
    let hashmap = new Map();
    // 辅助遍历栈
    let stack = [node];
    // 克隆图
    let clonenode = new Node(node.val);
    hashmap.set(node, clonenode)
    while(stack.length) {
        // 与BFS不同，DFS从最后面的点开始取
        let temp = stack.pop(); 
        // 对应的克隆节点
        let k = hashmap.get(temp); 
            
        for(let item of temp.neighbors) {
            // 若哈西map中没有新节点
            if(!hashmap.has(item)) {
                let newn = new Node(node.val)
                hashmap.set(node, newn)
            }
            // 加入新节点的邻居
            k.neighbors.push(hashmap.get(item));
        }
    }
    return clonenode
};
```

## 一、栈
栈是一种先进后出的数据结构，关于栈的js实现：[我的git仓库](https://github.com/Naturalvolume/Kathy-s-Algorithm/blob/master/%E6%A0%88%E5%92%8CDFS/satck.html)

## 二、栈的应用
#### 2.1 后缀表达式
后缀表达式也叫逆波兰表达式，平时我们使用的表达式如 `1+2*3` 是中缀表达式符合人民的阅读习惯，为了让计算机也能够理解表达式的意思，就需要使用后缀表达式 `1 2 3 * +`
##### 2.1.1 用栈计算后缀表达式
- 创建两个栈，一个为stack动态存放运算符，一个为post存放后缀表达式
- 从左到右扫描中缀表达式，若都到操作数，直接存入post栈中，若读到的是运算符：
	1.  栈为空时或左括号`(`直接存入stack
	2. 遇到右括号`)`将stack中与左括号间的运算符全部出栈，存入post栈中，注意：左右括号`()`直接消除，不需要存入栈中
	3. 非括号且栈不为空，将该运算符和stack栈顶运算符做比较：高于或等于栈顶运算符就直接存入stack，否则将栈顶元素出栈，存入post，然后存入该运算符到stack
##### 2.1.2 用二叉树
- 将中缀表达式转换为表达式树
	1. 表达式树的根节点为优先级最低且最靠右的操作数
	2. 表达式树的树叶为操作数，其它节点是操作符
- 后序遍历表达式树得到后缀表达式
参考：[表达式树](https://www.pianshen.com/article/7532296917/)
 ##### 2.1.3 [加括号法](https://blog.csdn.net/qq_29462849/article/details/93649198)

 - 按运算符优先级对中缀表达式加括号，原先的括号不用管
 - 将运算符移到括号的后面
 - 去掉所有括号
##### 2.1.4 后缀表达式转中缀表达式
- 从左到右遍历后缀表达式
- 用辅助栈temp存储结果
	1. 将操作数放入辅助栈temp
	2. 遇到操作符，弹出temp栈中后两个操作数，与操作符计算后重新放入temp
	3. 直到遍历完，temp中的数就是最后的结果
##### [力扣：逆波兰表达式求值](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/)

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
递减栈也叫单调栈，这个大神把单调栈讲解的非常清晰：[单调栈](https://labuladong.gitbook.io/algo/shu-ju-jie-gou-xi-lie/dan-tiao-zhan)
#### 2.深度优先遍历（DFS）
- 使用递归，虽然没有直接使用栈，但实际上使用但是由系统提供的隐式栈，也称为调用栈（Call Stack）。

```javascript
// 递归DFS的模板
function DFS(node, target, visited) {
	// 图为空
	if(!node) return false
	// 找到目标节点
	if(node == target) return true
	for(let item of node.neighbor) {
		if(!visited.has(item)) {
			visited.add(item)
			retrun DFS(item, target, visited)
		}
	}
}
```

- 使用显式栈代替递归，但递归深度太高时，会堆栈溢出，此时可使用BFS或显式栈

```javascript
// 显式栈模板，使用 while 循环和栈来模拟递归期间的系统调用栈。
function DFS(node, target) {
	// 集合防止重复访问
	let visted = new Set()
	// 辅助栈，跟BFS一样要先加入根节点
	let stack = [root]
	while(stack.length) {
		let node = stack.pop()
		// 找到目标点
		if(node = target) return true
		for(let item of node.neighbors) {
			// 遍历当前节点的每个邻接节点
			if(!visited.has(item)) {
				visited.add(item)
				stack.push(item)
			}
		}
	}
	return false
}
```

##### [岛屿数量](https://leetcode-cn.com/problems/number-of-islands/solution/pythonjavascript-tao-lu-dfs200-dao-yu-shu-liang-by/)
在队列中用BFS实现过，现在用DFS实现
##### [克隆图](https://leetcode-cn.com/problems/clone-graph/)
在克隆链表或图这种一个节点会指向多个节点的结构时，要使用字典做原节点和克隆节点的映射，防止重复访问。
- 递归实现
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
            	// 加入未遍历的新节点
            	stack.push(item)
                let newn = new Node(item.val)
                hashmap.set(item, newn)
            }
            // 加入新节点的邻居
            k.neighbors.push(hashmap.get(item));
        }
    }
    return clonenode
};
```

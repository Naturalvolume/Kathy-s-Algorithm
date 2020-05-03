## 一、队列的特点
先进先出
队列的应用
## 二、广度优先搜索（DFS）
通常 用来找最短路径。
```javascript
// 模板
function BFS(root, target) {
	// 定义队列，存入根节点
	let queue = [root];
	// 每一层到根节点的距离
	let step = 0;
	// 使用哈西表防止重复访问同一个节点
	let hash = new Set();
	hash.set(root);
	// 队列不为空就说明没有遍历完
	while(queue.length) {
		// 深度加一
		step += 1;
		// 当前层节点的数量
		let size = queue.length;
		// 循环遍历当前层每个节点
		for(let i = 0; i < size; i++) {
			// 提取第一个节点
			let cur = queue.shift();
			// 找到目标节点，返回距离
			if(cur == target) return step;
			// 保存这个节点对应的每个子节点
			for(找到每个子节点node  && !hash.get(node)) {
				queue.push(node);
				hash.set(node);
			}
		}
	}
	// 没有目标节点
	return false;
}
```
#### 1. BFS的特点
（1）每一层的节点齐头并进，像是面一样往前走，所以在某一层找到的目标就是最短路径。
（2）BFS的空间复杂度比DFS高，对于DFS空间复杂度就是递归堆栈，最坏情况下顶多就是树的高度，也就是O(logN)；对于BFS存储每一层的所有节点，最坏情况下空间复杂度是树的最底层节点的数量N/2，即O(N)。
###### [力扣200:岛屿数量](https://leetcode-cn.com/problems/number-of-islands/submissions/)
用矩阵表示的哈西表
```javascript
var Node = function(i, j) {
    this.i = i;
    this.j = j;
}
var numIslands = function(grid) {
    const row = grid.length;
    if(!row) return 0;
    const col = grid[0].length;
    let queue = [];
    let count = 0;
    // 用矩阵表示访问过的哈西表，一定要完整定义矩阵
    let hash = new Array(row);
    for(let i = 0; i < row; i++) {
        hash[i] = new Array(col).fill(false);
    }
    // 定义每个节点的访问的四个方向
    let direction = [[-1,0], [0,-1], [1,0], [0,1]];
    // 遍历矩阵的每个格子
    for(let i = 0; i < row; i++) {
        for(let j = 0; j < col; j++) {
            // 遇到为1且未访问过的就找它的连通区域，且标记为已访问
            if(grid[i][j] == 1 && !hash[i][j]) {
                // 岛屿数加1
                count += 1;
                queue.push(new Node(i, j));
                hash[i][j] = true;
                while(queue.length) {
                    let node = queue.shift();
                    
                    for(item of direction) {
                        let new_i = node.i + item[0];
                        let new_j = node.j + item[1];
                        // 不越界、节点为1、没有被访问过就加入队列
                        if(new_i>=0 &&new_i<row && new_j>=0 && new_j<col && grid[new_i][new_j] == 1 && !hash[new_i][new_j]) {
                            queue.push(new Node(new_i, new_j));
 
                            hash[new_i][new_j] = true;
                        }
                    }                   
                }
            }
        }
    }
    return count
};
```
注意：一定要完整定义矩阵，不然后面赋值赋错。
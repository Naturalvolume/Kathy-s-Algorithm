## 1.摩尔投票
在一些候选人中选出得票最多的那个，分为两个阶段：

 1. 对抗阶段：分属两个候选人的票数进行两两对抗抵消
 2. 计数阶段：计算对抗结果中最后留下的候选人票数是否有效
可看例题：[力扣169:多数元素](https://leetcode-cn.com/problems/majority-element/solution/tu-jie-mo-er-tou-piao-fa-python-go-by-jalan/)

```javascript
// 伪代码
major ＝ arr[0]  // 保存当前票数最多候选人，初值为第一个候选人
count ＝ 1   // 当前最多候选人票数
循环剩下的候选人：
	arr[i]当前候选人 != major:  
		count--
	arr[i]当前候选人 == major:
	    count++

	最多候选人票数 count == 0:
		major = arr[i]		// 更新候选人
		count = 1			// 更新票数
```
## 2.双指针的使用
#### [移除元素](https://leetcode-cn.com/problems/remove-element/)
要在原地修改或查找数组时，可以使用双指针，根据两个指针在不同的情况下改变，实现修改或查找数组。因为规定了不能使用额外的辅助数组，且不能使用splice删除数组那样下标值就改变了，也就是说此时遍历中的索引i已经是下一个了，会跳过一个，对于这一点不能理解的，建议自己先使用遍历＋splice试一下。

```c
var removeElement = function(nums, val) {
    // 使用两个指针一起移动，快指针遇到val时先走一步，慢指针不要动
    let slow = fast = 0
    // 
    while(fast < nums.length) {
        // 碰到val时，fast跳过它，作用是在下面的往前赋值过程中不移动它，让它被后面的值覆盖掉
        if(nums[fast] == val) fast++
        else {
            // 不是val，fast向slow移动数值
            // 移动数组
            nums[slow] = nums[fast]
            // 两个同时移动
            fast++
            slow++
        }
    }
    return slow
};
```

#### [两个数组的交集 II](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)
这是双指针在两个数组中的应用，分别指向两个数组。
```javascript
var intersect = function(nums1, nums2) {
    // 先排序
    nums1.sort((a,b) => a-b)
    nums2.sort((a,b) => a-b)
    // 双指针
    let p1 = 0
    let p2 = 0
    let res = []
    // 两个指针边走边判断，每次都是指向小数的指针走，相等就一起走
    while(p1 < nums1.length && p2 < nums2.length) {
        if(nums1[p1] == nums2[p2]) {
            res.push(nums2[p2])
            p1++
            p2++
        } else if(nums1[p1] < nums2[p2]) {
            p1++
        } else {
            p2++
        }
    }
    return res
};
```
## 3. 构建字典
字典就是"键值对"的集合，即js的`Map()`数据结构，有一些题目就可以利用键值－数值间的对应关系做，比如[力扣：罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)

```c
var romanToInt = function(s) {
    const map = {
        I : 1,
        IV: 4,
        V: 5,
        IX: 9,
        X: 10,
        XL: 40,
        L: 50,
        XC: 90,
        C: 100,
        CD: 400,
        D: 500,
        CM: 900,
        M: 1000
    };
    let ans = 0;
    for(let i = 0;i < s.length;) {
        if(i + 1 < s.length && map[s.substring(i, i+2)]) {
            ans += map[s.substring(i, i+2)];
            i += 2;
        } else {
            ans += map[s.substring(i, i+1)];
            i ++;
        }
    }
    return ans;
};
```


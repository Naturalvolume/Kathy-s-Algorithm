#  二分查找模板
二分查找常用在有序数组中找到某个元素或插入某个元素，关于二分查找方法的详细介绍和分析可以看力扣上这一篇大神级题解：[用「排除法」（减治思想）写二分查找问题、与其它二分查找模板的比较](https://leetcode-cn.com/problems/search-insert-position/solution/te-bie-hao-yong-de-er-fen-cha-fa-fa-mo-ban-python-/)。对区间的选取不同会有很多种不同的模板，这里只把我最常用的思路模板写一下：
```javascript
left = 0
right = arr.lenght-1
while(left < right) {
	// 注意这里是向下取整
	mid = Math.floor((left + right)/2)
	if(target > arr[mid]) {
		left = mid+1
	} else {
		right = mid
	}
}
// 这里 return left 是因为循环到最后left代表两种意思
// 1. 数组中有target元素，此时left就是target元素的下标
// 2. 数组中没有target元素，此时left就是本来target元素的位置，即应该插入的位置
return left
```
对于这里返回left的意义，可以做一下力扣的这一题：[搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)，把这个模板改一下可以解决很多问题。
### 判断数组中是否有target元素
要判断是否有`target`，首先要在`if..else`中添加一个`target == arr[mid]`的判断，找到了就直接`return mid`，但是这里要注意一个特殊情况，比如在数组`[5,7,8,10] `中找到`10`，最后你会发现当`left=mid+1`后`left 会等于 right`直接跳出`while`循环，没办法再判断是否等于`target`，为了解决这个bug，在循环完成后再加个判断。

```javascript
// 没有找到对应的题目，就写个模版吧
left = 0
right = arr.lenght-1
while(left < right) {
	// 注意这里是向下取整
	mid = Math.floor((left + right)/2)
	if(target > arr[mid]) {
		left = mid+1
	} else if(target == arr[mid]) {
		return mid
	} else {
		right = mid
	}
}
// 这里再加个判断，注意啦，是使用left判断
if(arr[left] == target) return left
// 没有找到返回 -1
return -1
```
其实，还有一个更简单的解决办法，不需要再添加判断条件，就是在最开始赋值的时候，直接让`right = arr.lenght`，多给right加一个长度，就能避免现有问题，且永远也取不到`arr[arr.length]`，不会有`undefined`情况。
### [统计数字在排序数组中出现的次数](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)
```javascript
var search = function(nums, target) {
    let left = 0;
    // 避免出现无法判断最后一次情况
    let right = nums.length;
    let count = 0;
    while(left < right) {
        mid = Math.floor((right + left)/2);
        if(nums[mid] > target) {
            right = mid;
        } else if(nums[mid] < target){
            left = mid+1;
        } else {
            // 找到目标元素，判断有几个
            let a = mid;
            let b = mid-1;
            // 往左边找
            while(nums[mid] == nums[a]) {
                a++;
                count++;
            }
            // 往右边找
            while(nums[mid] == nums[b]) {
                b--;
                count++;
            }
            return count;
        }
        
    }
    return count;
};
```
### 在部分排序数组中的应用
有些数组只是部分有序，也能使用二分查找寻找元素位置，比如[搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)


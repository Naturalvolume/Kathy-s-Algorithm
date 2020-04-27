### 贪心、分治、动态规划的区别
从一道经典题说明：力扣42:连续子数组的最大和

```javascript
// 贪心
var maxSubArray = function(nums) {
    const len = nums.length;
    let max = nums[0];
    let sum = nums[0];
    for(let i = 1; i < len; i++) {
        if(sum < 0) {
            sum = nums[i];
        } else {
            sum += nums[i];
        }
        if(sum > max) max = sum;
    }
    return max;
};
```

```javascript
// 动态规划
var maxSubArray = function(nums) {
    const len = nums.length;
    if(len==0) return 0;
    let dp = new Array(len);
    let max = (dp[0] = nums[0]);
    for(let i = 1; i < len; i++) {
        dp[i] = Math.max(dp[i-1]+nums[i], nums[i]);   
        max = Math.max(max, dp[i]);   
    }
    return max;
};
```

```javascript
// 在原地进行的动态规划，用nums[i]表示dp[i]
var maxSubArray = function(nums) {
    const len = nums.length;
    if(len==0) return 0;
    let max = nums[0];
    for(let i = 1; i < len; i++) {
        if (nums[i - 1] > 0) {
            nums[i] += nums[i - 1];
        }
        max = Math.max(max, dp[i]);   
    }
    return max;
};
```

### 一、动态规划
动态规划其实是运筹学的一种最优化方法，只不过在计算机问题上应用比较多，比如说让你求最长递增子序列呀，最小编辑距离呀等等。
动态规划 ＝ 穷举 ＋ 剪枝，一定要记住**穷举**，先找出所有的状态，再找到所有状态对应的所有选择，接着对比选出符合条件的选择，这个过程中用**dp table**存储之前的选择或**备忘录**减少递归次数。
##### 解题步骤
 1. 建立dp数组，根据题意可以建立一维、二维甚至三维的数组，dp数组的索引值就是影响每个状态的值，比如两个字符串问题，有了字符串的改变才有了状态的改变，存储的值就是题目要求的值；背包问题中背包的体积和物体的类别限制了背包中物体的总价值和能放的总体积。
 2. 定义base case，dp数组的初始条件，方便后来迭代求解下一个值
 3. 找状态转移方程，其实我觉得这才是第一步，当读懂了题目后才能思考如何建立dp数组和base case。所谓的状态就是对不同的选择形成的状态，也就是在这里穷举所有的可能性，选出最符合题意的最大或最小值等。
 4. 遍历穷举，一般是几维的dp数组就有几层循环，比较所有选择的值，找出符合题意的，遍历策略也是非常需要注意的。
##### 动态规划的时间复杂度
- 动态规划算法的时间复杂度就是**子问题个数 × 函数本身的复杂度**。
##### 动态规划的base case
- Number.MAX_VALUE — JavaScript可表示的最大值，最大值为1.7976931348623157e+308

- -Number.MAX_VALUE — JavaScript可表示的最小值

- Number.NEGATIVE_INFINITY — 负无限接近于0，溢出时返回该值
#### 优化方法
- 二分查找：通过二分查找减少遍历次数，如[鸡蛋掉落问题](https://leetcode-cn.com/problems/super-egg-drop/)


关于动态规划如何理解，为什么需要动态规划，可以看看这个大神的题解：[动态规划套路详解](https://leetcode-cn.com/problems/coin-change/solution/dong-tai-gui-hua-tao-lu-xiang-jie-by-wei-lai-bu-ke/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427204204372.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjU5Nzg4MA==,size_16,color_FFFFFF,t_70)

#### 1.两个字符串问题
非常经典，典型的二维动态规划，大部分比较困难的字符串问题都可以用这个方法。



##### [（1）最长公共子序列LCS问题](https://leetcode-cn.com/problems/longest-common-subsequence/)
先是列出所有可能的情况，从顶向下递归解决。

```javascript
// 暴力解法，从顶向下
// 理解起来简单，但时间复杂度高，会超出时间限制
var longestCommonSubsequence = function(text1, text2) {
    let dp = function(i, j) {
        if(i==-1 || j==-1) return 0;
        if(text1[i] == text2[j]) {
            console.log(1);
            return dp(i-1, j-1) + 1;
        } else {
            return Math.max(dp(i-1, j), dp(i, j-1));
        }
    } 
    return dp(text1.length-1, text2.length-1);
};
```
用动态规划把递归的从上到下转换成从下到上，减少时间复杂度。
```javascript
// 动态规划解法
// 使用状态机 dp table 保存之前的所有状态，推测出新的状态
var longestCommonSubsequence = function(text1, text2) {
    // 用table保存之前的每个状态
    // 后一个状态取决于前一个状态
    const len1 = text1.length;
    const len2 = text2.length;
    // 建立 dp table 保存状态
    // 同时给基础状态 base case 赋值
    let table = Array.from(new Array(len1+1),() => new Array(len2+1).fill(0));
	// 循环，寻找状态
    for(let i = 1; i < len1+1; i++) {
        for(let j = 1; j < len2+1; j++) {
            if(text1[i-1] == text2[j-1]) {
                
                table[i][j] = table[i-1][j-1] + 1;
            } else {
                table[i][j] = Math.max(table[i][j-1], table[i-1][j]);
            }
        }
    }

    return table[len1][len2];
};
```
##### [（2）编辑距离](https://leetcode-cn.com/problems/edit-distance/)
暴力解决，从顶向下。
```javascript
var minDistance = function(word1, word2) {
    // 字符串类的动态规划，穷举策略都是用两个指针分别指向两个字符串，然后遍历两个字符串
    // 穷举
    const l1 = word1.length;
    const l2 = word2.length;
    let dp = function(i, j) {
        if(i==-1) return j+1;
        if(j==-1) return i+1;

        if(word1[i] == word2[j]) {
            return dp(i-1, j-1);
        } else {
            return Math.min(dp(i-1, j-1)+1, dp(i-1, j)+1, dp(i, j-1)+1);
        }
    }
    return dp(l1, l2);
};
```
使用备忘录记录已经求过的状态。
```javascript
var minDistance = function(word1, word2) {
    // 字符串类的动态规划，穷举策略都是用两个指针分别指向两个字符串，然后遍历两个字符串
    // 穷举
    const l1 = word1.length;
    const l2 = word2.length;
    // 加备忘录
    let table = Array.from(new Array(l1+1),() => new Array(l2+1).fill(0));
    console.log(table);
    let dp = function(i, j) {
        // base case
        if(i==-1) return j+1;
        if(j==-1) return i+1;
        // 如果之前dp[i][j]的值之前计算过，就直接取它的值
        // 添加备忘录，减少搜索时间
        if(table[i][j] != 0) {
            return table[i][j];
        }

        if(word1[i] == word2[j]) {
            table[i][j] = dp(i-1, j-1);
        } else {
            table[i][j] = Math.min(dp(i-1, j-1)+1, dp(i-1, j)+1, dp(i, j-1)+1);
        }
        return table[i][j];
    }
    return dp(l1, l2);
};
```
动态规划，从下到上。
```javascript
var minDistance = function(word1, word2) {
    // 字符串类的动态规划，穷举策略都是用两个指针分别指向两个字符串，然后遍历两个字符串
    // 穷举
    const l1 = word1.length;
    const l2 = word2.length;
    // 创建新数组时最好不要调用函数，会增加时间复杂度
    let dp = Array.from(new Array(l1+1), () => new Array(l2+1).fill(0));
    // base case
    for (let i = 1; i <= l1; i++)
        dp[i][0] = i;
    for (let j = 1; j <= l2; j++)
        dp[0][j] = j;
    for(let i = 1; i < l1+1; i++) {
        for(let j = 1; j < l2+1; j++) {
            if(word1[i-1] == word2[j-1]) {
                dp[i][j] = dp[i-1][j-1];
            } else {
                dp[i][j] = Math.min(dp[i-1][j-1]+1, dp[i-1][j]+1, dp[i][j-1]+1);
            }
        }
    }
    console.log(dp);
    return dp[l1][l2];
};
```
注意：创建新数组时最好不要调用函数，会增加时间复杂度
##### 2. 背包问题
背包问题非常经典，可惜力扣上没有对应的题目

##### [3. 力扣系列动态规划问题：股票买卖问题](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)
股票买卖问题总有六道题，难度从简到难，归根结底每道题都可以用动态规划来做，这个大神讲解的非常全面细致：[一个通用方法团灭 6 道股票问题](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/solution/yi-ge-tong-yong-fang-fa-tuan-mie-6-dao-gu-piao-wen/)
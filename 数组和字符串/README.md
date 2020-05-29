## 一、数组Array
#### 1. Array构造函数方法
- `Array.from` 常用指数：两颗星

从类似数组或迭代器对象中构造函数，包括dom元素数组、集合Set、字典Map、arguments等等。
#### 2. 原地修改数组方法
这些方法不会返回新数组
- 五颗星
(1) `arr.push()`：向数组末尾添加一个或多个元素，添加多个元素要用展开语法 `arr.push(...[1,2,3])`
(2) `arr.pop()`：弹出末尾元素，并返回它
- 四颗星
	(1) `arr.reverse()`：反转数组
	(2) `arr.sort()`：排序数组，注意啦！默认的是按编码排序的，要想升序或降序排序，要添加函数参数，如`arr.sort((a, b) => a-b)`是升序，换成`arr.sort((a, b) => b-a)`是降序
	(3) `arr.shift()`：删除并返回第一个元素
	(4) `arr.splice(index, howmany, item1, item2, item3)`：增加、删除或替换某些元素，起始位置由index决定，多少个由howmany决定，item是要增加或替换成的元素，具体使用请看[MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)的详细讲解。
- 三颗星
	(1) `arr.unshift()`：在数组开头增加一个或多个元素，并返回新长度，多个元素要用展开语法
	(2) `arr.fill(value, start, end)`：以value填充`[start, end]`区间，可以用来创建二维数组，比如`let arr= new Array(row).fill([])`
#### 3. 返回新数组的方法
返回新数组就说明如果你要使用修改后的结果，需要创建一个数组接收它。
- 四颗星
	(1) `arr.concat(otherArr)`：合并其它数组
	(2) `arr.join('-')`：以某种连接符拼接数组元素为字符串，常用的是空格`' '`，即`arr.join(' ')`，跟字符串的`str.split()`是相反的一组运算
	(3) `arr.slice(hegin, end)`：截取数组区间，浅拷贝，注意这是前闭后开区间，即结果不包括`end`对应元素
	
#### 4. 遍历方法
遍历数组是经常会用到的，还有很多其它的有各色各样用处的遍历方法，以下只是经常用到的几个。
- `arr.map()`：传入函数，可以迭代对数组中的每个元素进行对应操作，最后返回操作好的新数组。
- `arr.forEach()`：遍历元素值或索引值，有局限性，不能continue或break。
- `for(let index in arr)`：用索引值遍历数组。
- `for(let value of arr)`：es6新增的遍历迭代对象数值的方法，Map、Set也可以用，这个直接遍历数组元素，而不是索引。

关于这些方法的区别，具体可以看这篇博文：[javascript数组遍历](https://segmentfault.com/a/1190000017394445)
#### 5. 获得索引方法
三颗星
- `arr.indexOf()`：返回某个元素第一次出现的位置，没有该元素返回`-1`
- `arr.lastIndexOf()`：返回某个元素最后一次出现的位置，没有该元素返回`-1`
- `arr.includes(val)`：判断数组中是否有val元素，返回布尔值
## 二、字符串String
字符串的方法比较杂，不能按照像数组一样的块分类，因此这里就按照出现的频率排序总结，最前面的出现的频率高。

- `str.slice(start, end)`：截取字符串某段区域，返回新字符串
- `str.split()`：以指定字符或正则表达式分割字符串
- `str.substr(start)`：返回在指定位置开始的子字符串，也可加第二个参数length，指定子字符串长度
- `str.substring(start, end)`：返回两个下标之间的子字符串

- `str.trim()`：去掉字符串开始和结束的空格
- `str.indexOf()`：返回某个元素第一次出现的位置，没有该元素返回`-1`
- `str.lastIndexOf()`：返回某个元素最后一次出现的位置，没有该元素返回`-1`
- `str.includes(s)`：判断字符串中是否有子字符串s
- `str.charAt(idx)`：返回特定位置的字符
- `str.charCodeAt(idx)`：返回表示给定索引的字符的Unicode的值
- `str.toLowerCase()`：将字符串转换成小写并返回
- `str.toUpperCase()`：将字符串转换成大写并返回


- `str.match(reg)`：使用正则表达式匹配字符串，返回匹配到的数组，关于match和正则表达式的用法，可参看我的另一篇博客：[JavaScript正则表达式](https://blog.csdn.net/weixin_42597880/article/details/100032796)
- `str.replace(reg)`：正则表达式比较字符串，用新的子串替换被匹配的子串
- `str.search(reg)`：对正则表达式和指定字符串进行匹配搜索，返回第一个出现的匹配项的下标
- `str.concat()`：连接两个字符串文本，并返回新的字符串，并不常用，因为连接两个字符串文本可以直接使用`＋`，`str1 + str2`
- `str.endsWith(s)`：判断字符串是否以给定字符串结尾
- `str.startsWith(s)`：判断字符串是否以给定字符串开始


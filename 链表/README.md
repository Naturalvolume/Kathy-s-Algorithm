### 链表是线性表的另一种存储形式，即链式存储结构，链式数据的地址可以是连续的或不连续的。
### 链表的定义方式
单链表只能指向下一个节点，双链表可以向后寻找也可以向前寻找。
```
// 单链表
let Node = function(val, next) {
    this.val = val;     // 存储节点的值
    this.next = next;   // 指向下一个节点的地址
}
// 双向链表
let DoubleNode = function(val, next, prev) {
    this.val = val;     // 存储节点的值
    this.next = next;   // 指向下一个节点的地址
    this.prev = prev;   // 上一个节点的地址
}
```
### 链表的基本方法：增删改查
在list.html中定义了单链表的基本操作，doublelist.html中定义了双向链表的操作。
### 力扣刷题心得
##### 一、双指针法
使用两个指针遍历链表，一般有以下几种使用情况：
 1. 快指针走两步，慢指针走一步：判断链表是否有环、找到链表中点
 2. 快指针先走，到一定判断条件后慢指针再走：找倒数第几个节点
 3. 双指针交叉遍历两个链表：相交链表
[力扣141:环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)
若存在环，快慢节点总会在环中相交
```javascript
var hasCycle = function(head) { 
    if(!head) return false;   
    let slow = fast = head;
    // 进入循环
    while(slow != fast || slow == head) {        
        if(slow && slow.next) {
            slow = slow.next;
        } else {
            return false;
        }
        if(fast && fast.next) {
            fast = fast.next.next;
        } else {
            return false;
        }  
        if(slow == fast) return true;     
    }
};
```
[力扣142:环形链表2](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
快慢指针相遇后，让慢指针重新指向链表头部，快慢指针接下来都走一步。
```javascript
var detectCycle = function(head) { 
    if(!head) return null;   
    let slow = fast = head;
    // 是否有环判断
    while (fast !== slow || slow == head) {
    // fast.next && fast.next.next是为了防止fast的下一个就是null，然后null.next报错的情况
       if (slow.next && fast.next && fast.next.next) {
          slow = slow.next
          fast = fast.next.next
       } else {
          return null
       }
       if(slow==fast) break;
     }

    // 慢指针重新指向头部
    slow = head;
    let index = 0;
    // 两指针每次都走一步
    while(slow != fast) {        
        slow = slow.next;
        fast = fast.next;
        index++;
    }
    return slow;
};
```
[力扣876:链表中点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)
快指针走两步，慢指针走一步，快指针永远是慢指针的两倍
```javascript
var middleNode = function(head) {
    let fast = slow = head;
    while(fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
};
```
[力扣面试题22:倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)
fast先走到第k个节点，然后fast和slow一起向后走，相当于slow比fast少走k个
```javascript
var getKthFromEnd = function(head, k) {
    let slow = head;
    let fast = head;
    while(k--) {
        fast = fast.next;
    }
    while(fast) {
        slow = slow.next;
        fast = fast.next;
    }
    return slow;
};
```
[力扣160：相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)
双指针分别指向一个链表，一个链表遍历完后遍历两一个链表，即双指针交叉遍历两个链表，两链表相遇时所走的步长一样。
```javascript
var getIntersectionNode = function(headA, headB) {
    var PA = headA;
    var PB = headB;
    if(headA == null || headB == null) 
        return null;
    while(PA != PB) {
        PA = PA == null ? headB : PA.next;
        PB = PB == null ? headA : PB.next;
    }
    return PA
};
```

##### 二、创建假链表
当会修改原链表头部时，创建新链表的下一个节点指向原链表。防止因链表头部改变造成返回不了原链表。

```javascript
let dummy = new ListNode();
dummy.next = head;

// 返回
return dummy.next;
```
[力扣61:旋转链表](https://leetcode-cn.com/problems/rotate-list/)

```javascript
var rotateRight = function(head, k) {
    // 特判
    if(head == null || head.next == null) return head;
    let dummy = new ListNode(0);
    dummy.next = head;
    // 创建两个指针
    let last = head;    //快指针
    let pre = head;     // 慢指针
    // 利用快指针找到倒数第k个数
    while(k--) {
        last = last.next;
        // 若k大于链表的长度，就要将指针重新指向头部
        if(!last) {
            last=head;
        }
    }
    // last指针一定要指向最后一个数
    // 两个指针同时向前走
    while(last.next) {
        last = last.next;
        pre = pre.next;
    }
    // 交换
    last.next = head; 
    dummy.next = pre.next;
    pre.next = null;
    return dummy.next;
};
```
##### 三、递归和迭代(while)
在链表中用递归和迭代的地方是非常多的，一般迭代(while)和递归可以互相转换。


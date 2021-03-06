# 题目描述

反转一个单链表。

**示例**:

``` c
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

# 解法1：迭代

这里只需要

1. 设置一个变量不停的维护当前节点，`current = head.next`。
2. 设置初始化时将head节点指向null，让原本的head变为尾节点，下一步将当前节点指向head
3. 然后通过临时变量，将当前节点赋值给head。进入下一次循环


``` js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
const reverseList = function(head) {
    if (head === null || head.next === null) { // 链表为空或只有一个节点时，不用反转
        return head;
    }
    let current = head.next;
    head.next = null; // 让原本的head变为尾节点
    let temp; // 临时指针
    while (current) {
        temp = current.next;
        current.next = head;
        head = current;
        current = temp;
    }
    return head;
};
```

# 解法2：递归

从《JS实现单向链表》和《JS实现双向链表》的文档中可以发现链表的的数据结构特点就是一层一层的嵌套着，所以可以用递归来处理，此处的递归类似于**剥洋葱模型**。

``` js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
const reverseList = function(head) {
    if (head === null || head.next === null) {
        return head;
    }

    const new_head = reverseList(head.next); // 反转后的头节点
    head.next.next = head; // 将反转后的链表的尾节点与当前节点相连
    head.next = null;
    return new_head;
};
```
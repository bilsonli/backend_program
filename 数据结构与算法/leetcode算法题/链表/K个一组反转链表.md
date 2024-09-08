# K个一组反转链表

## 解法一
1) 找出每一组的头尾节点  
2) 反转该组链表(返回新的头尾节点，此时链没有断开)  
3) 链接pre节点，并且更新pre和cur节点(尾节点没有断开)  

```
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseKGroup(head *ListNode, k int) *ListNode {
    dummyHead := &ListNode{Next:head}
    pre := dummyHead
    cur := head
    for cur != nil {
        tail := pre
        for i := 0; i < k; i++ {
            tail = tail.Next
            if tail == nil {
                return dummyHead.Next
            }
        }

        next := tail.Next
        tmpHead, tmpTail := reverse(cur, tail)
        pre.Next = tmpHead
        pre = tmpTail
        cur = next
    }

    return dummyHead.Next
}

func reverse(head, tail *ListNode) (*ListNode, *ListNode) {
    prev := tail.Next
    cur := head
    for prev != tail {
        next := cur.Next

        cur.Next = prev
        prev = cur
        cur = next
    }

    return tail, head
}
```

## 解法二
1) 找出每一组的头尾节点
2) 断链  
3) 反转该组链表(返回新的头尾节点，此时链已经断开)  
4) 重新链接原来的剩下部分的节点  
5) 链接pre节点，并且更新pre和cur节点     
```
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseKGroup(head *ListNode, k int) *ListNode {
    dummyHead := &ListNode{Next:head}
    pre := dummyHead
    cur := pre.Next
    for cur != nil {
        tail := pre
        for i := 0; i < k; i++ {
            tail = tail.Next
            if tail == nil {
                return dummyHead.Next
            }
        }


        next := tail.Next
        tail.Next = nil //断链
        newHead := reverse(cur)
        cur.Next = next //重新连接,新的尾节点就是当前的头节点cur，头节点即是原来的尾节点
        pre.Next = newHead

        pre = cur
        cur = next
    }

    return dummyHead.Next
}

func reverse(head *ListNode) *ListNode {
    pre := (*ListNode)(nil)
    cur := head
    for cur != nil {
        next := cur.Next

        cur.Next = pre
        pre = cur
        cur = next
    }

    return pre
}
```

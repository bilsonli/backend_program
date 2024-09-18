# 删除链表的第N个结点

## 方法一,求长度再删除

```
func removeNthFromEnd(head *ListNode, n int) *ListNode {
    dummyhead := &ListNode{Next:head}
    length := 0
    for cur := head; cur != nil; cur = cur.Next {
        length++
    }

    pre := dummyhead
    for i := 0; pre != nil && i < length - n; {
        pre, i = pre.Next, i + 1 
    }

    pre.Next = pre.Next.Next
    return dummyhead.Next
}
```

## 方法二，快慢指针

```
func removeNthFromEnd(head *ListNode, n int) *ListNode {
    dummyHead := &ListNode{Next:head}
    fast, slow := head, dummyHead
    for i := 0; i < n; i++ {
        fast = fast.Next
    }

    for fast != nil {
        fast = fast.Next
        slow = slow.Next
    }

    slow.Next = slow.Next.Next

    return dummyHead.Next
}
```

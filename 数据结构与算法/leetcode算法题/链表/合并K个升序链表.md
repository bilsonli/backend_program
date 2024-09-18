# 合并K个升序链表

## 分治法

```
func mergeKLists(lists []*ListNode) *ListNode {
    if len(lists) == 0 {
        return nil
    }

    if len(lists) == 1 {
        return lists[0]
    }

    return mergeTwoList(mergeKLists(lists[:len(lists)/2]), mergeKLists(lists[len(lists)/2:]))
}

func mergeTwoList(l1, l2 *ListNode) *ListNode {
    if l1 == nil {
        return l2
    }

    if l2 == nil {
        return l1
    }

    dummyHead := &ListNode{}
    pre := dummyHead
    tmp1, tmp2 := l1, l2
    for ;tmp1 != nil && tmp2 != nil; {
        if tmp1.Val < tmp2.Val {
            pre.Next = tmp1
            tmp1 = tmp1.Next
        } else {
            pre.Next = tmp2
            tmp2 = tmp2.Next
        }

        pre = pre.Next
    }

    pre.Next = tmp1
    if tmp2 != nil {
        pre.Next = tmp2
    }

    return dummyHead.Next
}
```

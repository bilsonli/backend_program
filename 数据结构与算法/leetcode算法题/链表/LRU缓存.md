# lru缓存

```
type LRUCache struct {
    capacity int
    value map[int]*Node
    head *Node
    tail *Node
}

type Node struct {
    key int
    val int
    pre *Node
    next *Node
}


func Constructor(capacity int) LRUCache {
    lru := LRUCache{capacity:capacity}
    lru.value = make(map[int]*Node)
    lru.head = &Node{}
    lru.tail = &Node{}

    lru.head.next = lru.tail
    lru.tail.pre = lru.head

    return lru
}


func (this *LRUCache) Get(key int) int {
    node, ok := this.value[key]
    if !ok {
        return -1
    }

    //连接前后结点
    node.pre.next = node.next
    node.next.pre = node.pre

    //插入首结点之后
    next := this.head.next
    this.head.next = node
    node.pre = this.head
    node.next = next

    //更新首结点后的结点前缀
    next.pre = node

    return node.val
}


func (this *LRUCache) Put(key int, value int)  {
    node, ok := this.value[key]
    if !ok {
        node = &Node{key:key, val:value}
        this.value[key] = node
    } else {
        node.val = value

        //连接前后结点
        node.pre.next = node.next
        node.next.pre = node.pre
    }

    //插入到首结点之后
    next := this.head.next
    node.pre = this.head
    node.next = next
    this.head.next = node
    next.pre = node

    if len(this.value) <= this.capacity {
        return
    }

    //淘汰尾结点
    lastNode := this.tail.pre
    lastNode.pre.next = lastNode.next
    lastNode.next.pre = lastNode.pre
    delete(this.value, lastNode.key)
}
```

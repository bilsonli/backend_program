# 最小堆
```
func findKthLargest(nums []int, k int) int {
    tmpIntHeap := &IntHeap{}
    heap.Init(tmpIntHeap)
    for _, value := range nums {
        if tmpIntHeap.Len() < k {
            heap.Push(tmpIntHeap, value)
        } else {
            if tmpIntHeap.Top() < value {
                heap.Pop(tmpIntHeap)
                heap.Push(tmpIntHeap, value)
            }
        }
    }

    return heap.Pop(tmpIntHeap).(int)
}

type IntHeap []int

func (ih IntHeap) Swap(i, j int) {
    ih[i], ih[j] = ih[j], ih[i]
}

func (ih *IntHeap) Push(x any) {
    //TODO implement me
    *ih = append(*ih, x.(int))
}

func (ih *IntHeap) Pop() any {
    //TODO implement me
    tmp := (*ih)[len(*ih)-1]
    (*ih) = (*ih)[:len(*ih)-1]
    return tmp
}

func (ih IntHeap) Less(i, j int) bool {
    return ih[i] < ih[j]
}

func (ih IntHeap) Len() int {
    return len(ih)
}

func (ih IntHeap) Top() int {
    return ih[0]
}
```

# N叉树的最大高度

```
func maxDepth(root *Node) int {
    if root == nil {
        return 0
    }

    depth := 1
    for _, child := range root.Children {
        depth = max(depth, maxDepth(child) + 1)
    }

    return depth
}

func max(a, b int) int {
    if a > b {
        return a
    }

    return b
}
```

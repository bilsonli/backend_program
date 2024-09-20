# Once 用法
保障函数只执行一次

## 原理
```
type Once struct {
	done atomic.Uint32
	m    Mutex
}

func (o *Once) Do(f func()) {
    // 思考题：为什么这里不用 cas 来判断？(无法保证f执行完毕)
    if o.done.Load() == 0 {
		// Outlined slow-path to allow inlining of the fast-path.
		o.doSlow(f)
	}
}

func (o *Once) doSlow(f func()) {
    o.m.Lock()
    defer o.m.Unlock()
    if o.done.Load() == 0 {
        // 思考题：为什么这里用 defer 来加计数？(个人理解需要等f执行完毕之后才加1)
		defer o.done.Store(1)
		f()
	}
}
```

## 为什么不用cas
cas无法保证f执行完毕，会出现中间态  

## 应用场景
1）初始化  
2）单例模式  
3）关闭channel  

## 坑点
1）不要拷贝一个 sync.Once 使用或作为参数传递，然后去执行 Do，值传递时 done 会归0，无法起到限制一次的效果。
2）不要在 Do 的 f 中嵌套调用 Do（死锁）



## 参考
https://juejin.cn/post/7088305487753510925
https://juejin.cn/post/6940667949350912030


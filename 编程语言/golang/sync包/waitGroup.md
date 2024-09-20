# 用法
控制协程同步的，等待一组协退出

# 原理
```
type WaitGroup struct {
	noCopy noCopy

	state atomic.Uint64 // high 32 bits are counter, low 32 bits are waiter count.
	sema  uint32
}
```
其中state字段前32位为counter，代表未调用done的协程数量；后32位为waiter，代表调用了wait的协程数量  

# 过程  
1、当调用 WaitGroup.Add(n) 时，counter 将会自增: counter += n  
2、当调用 WaitGroup.Wait() 时，会将 waiter++。同时调用 runtime_Semacquire(semap), 增加信号量，并挂起当前 goroutine。  
3、当调用 WaitGroup.Done() 时，将会 counter--。如果自减后的 counter 等于 0，说明 WaitGroup 的等待过程已经结束，则需要调用 runtime_Semrelease 释放信号量，唤醒正在 WaitGroup.Wait 的 goroutine  

## 参考资料
https://juejin.cn/post/7120548108966035470

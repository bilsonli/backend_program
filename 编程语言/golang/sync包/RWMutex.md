# 用法
读写锁  

# 原理
```
type RWMutex struct {
	w           Mutex  // held if there are pending writers
	writerSem   uint32 // semaphore for writers to wait for completing readers
	readerSem   uint32 // semaphore for readers to wait for completing writers
	readerCount int32  // number of pending readers
	readerWait  int32  // number of departing readers
}
```

writerSem: 写阻塞等待的信号量，最后一个读者释放锁时，会释放该信号量；  
readerSem: 读阻塞的协程等待的信号量，持有写锁的协程释放锁后，会释放该信号量；  
readerCount: 拿到锁的 readers 数量（个人理解：已持有读锁和在等待读锁信号量的协程数）  
readerWait: 正在写阻塞时的 readers 数量（加写锁时已持有读锁的数量）。  

## 参考资料
https://juejin.cn/post/7091236529959338015



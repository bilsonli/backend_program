# 用法
互斥锁  

# 原理
```
type Mutex struct {
    state int32 
    sema  uint32
}

const (
	mutexLocked = 1 << iota // mutex is locked
	mutexWoken
	mutexStarving
	mutexWaiterShift = iota
)
```

![image](https://github.com/user-attachments/assets/735498b3-f708-4a62-9f3f-22417f9ce394)

解释：  
Locked: 表示该 Mutex 是否已经被锁定，0表示没有锁定，1表示已经被锁定；  
Woken: 表示是否有协程已经被唤醒，0表示没有协程唤醒，1表示已经有协程唤醒，正在加锁过程中；  
Starving: 表示该 Mutex 是否处于饥饿状态，0表示没有饥饿，1表示饥饿状态，说明有协程阻塞了超过1ms；  

# 饥饿模式和正常模式
正常模式：获取锁失败，不会立即转入阻塞，而是进入自旋（一般4次）  
饥饿模式：如果某个协程被唤醒后，获取锁失败，并且判断当前阻塞时间超过1ms，则将模式切为饥饿模式(不启动自旋，等待信号)  


# woken作用
Woken 状态用于加锁和解锁过程中的通信。比如，同一时刻，两个协程一个在加锁，一个在解锁，在加锁的协程可能在自旋过程中，此时把 Woken 标记为 1，用于通知解锁协程不必释放信号量，类似知会一下对方，不用释放了，我马上就拿到锁了。  

# 自旋条件
1）锁被占用    
2）正常模式  
3）自旋次数小于4  
4）多核  
5）逻辑处理器大于1  
6）P的本地可运行队列为空  

## 参考资料
https://juejin.cn/post/7086756462059323429
https://www.cnblogs.com/zqwlai/p/17268981.html

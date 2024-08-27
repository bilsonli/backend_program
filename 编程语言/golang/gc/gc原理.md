# 参考链接
https://community.apinto.com/d/34057-golang-gc

# 名词解释
## root对象
根对象是指赋值器不需要通过其他对象就可以直接访问到的对象，通过Root对象, 可以追踪到其他存活的对象。常见的root对象有：  
全局变量：程序在编译期就能确定的那些存在于程序整个生命周期的变量。  
执行栈：每个 goroutine (包括main函数)都拥有自己的执行栈，这些执行栈上包含栈上的变量及堆内存指针。【堆内存指针即在gorouine中申请或者引用了在堆内存的变量  

# v1.3版本
## 标记清除法
1、开启STW，停止程序的运行，图中是本次GC涉及到的root节点和相关对象。
![image](https://github.com/user-attachments/assets/12c2aa8d-c525-4227-9f1e-9f331b145f46)

2、从根节点出发，标记所有可达对象。
![image](https://github.com/user-attachments/assets/0ac95124-22a8-41fd-b5fb-cd08cbcc4009)

3、停止STW，然后回收所有未被标记的对象
![image](https://github.com/user-attachments/assets/f43ff209-8044-4088-ac9a-e0786544fe97)

# v1.5版本
## 三色标记法
![image](https://github.com/user-attachments/assets/c8c58451-bc08-4b1a-a438-ff9444efdd6e)
![image](https://github.com/user-attachments/assets/422b01dc-cbca-48e1-a12d-a9f1f7b7d6e7)
![image](https://github.com/user-attachments/assets/b3db463c-bc74-4b9d-97a4-7497d433b1ac)
![image](https://github.com/user-attachments/assets/2b5ef553-af71-49e6-ae71-169fc31328af)
![image](https://github.com/user-attachments/assets/74e22206-e209-405e-b99c-7aac8d49ccdf)

## 强弱三色不变式
#### 强三色不变式
规则：不允许黑色对象引用白色对象
#### 弱三色不变式
规则：黑色对象可以引用白色对象，但是白色对象的上游必须存在灰色对象

#### 插入写屏障（go1.5采用的）：
规则：当一个对象引用另外一个对象时，将另外一个对象标记为灰色
弊端：栈上重新进行一次stw，然后再rescan一次

#### 删除写屏障
规则：在删除引用时，如果被删除引用的对象自身为灰色或者白色，那么被标记为灰色  
弊端：回收精度低

### v1.8 混合写屏障
GC刚开始的时候，会将栈上的可达对象全部标记为黑色  
GC期间，任何在栈上新创建的对象，均为黑色   
堆上被删除的对象标记为灰色  
堆上新添加的对象标记为灰色  

# 总结
Golang v1.3之前采用传统采取标记-清除法，需要STW，暂停整个程序的运行。
在v1.5版本中，引入了三色标记法和插入写屏障机制，其中插入写屏障机制只在堆内存中生效。但在标记过程中，最后需要对栈进行STW。
在v1.8版本中结合删除写屏障机制，推出了混合屏障机制，屏障限制只在堆内存中生效。避免了最后节点对栈进行STW的问题，提升了GC效率

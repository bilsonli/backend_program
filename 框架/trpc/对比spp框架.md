# 对比
1）trpc框架支持pb（默认）、jce、json等多种协议，spp貌似只支持支持jce协议  
2）trpc采用单进程多协程模型，每个链接起一个携程来处理；而spp是采用ctrl+proxy+多worker多进程模式，proxy负责收发消息，而worker负责处理业务逻辑,ctrl负责监控proxy和worker进程  
3）spp proxy采用epoll多路复用模型，而trpc是死循环accept + 多协程模型  

# 番外篇

* **为什么需要锁？**

1.多任务环境中才需要

2.任务都需要对同一资源进行写操作

3.对资源的访问时互斥的

> 1.竞争锁：任务通过竞争获取锁才能对资源进行操作
>
> 2.占有锁：当有一个任务对资源更新时
>
> 3.任务阻塞：其他任务都不可以对这个资源进行操作
>
> 4.锁的释放：直到该任务完成


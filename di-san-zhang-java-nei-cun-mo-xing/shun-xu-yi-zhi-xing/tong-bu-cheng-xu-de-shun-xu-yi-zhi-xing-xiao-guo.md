# 同步程序的顺序一致性效果

下面，对前面的示例程序ReorderExample用锁来同步，看看正确同步的程序如何具有顺序一致性。

请看下面的示例代码：

```text
class SynchronizedExample {
    int a = 0;
    boolean flag = false;
    public synchronized void writer() { // 获取锁
        a = 1;
        flag = true;
    } // 释放锁
    public synchronized void reader() { // 获取锁
        if (flag) {
            int i = a;
……
        } // 释放锁
    }
}
```

在上面示例代码中，假设A线程执行writer\(\)方法后，B线程执行reader\(\)方法。这是一个正确同步的多线程程序。根据JMM规范，该程序的执行结果将与该程序在顺序一致性模型中的执行结果相同。下面是该程序在两个内存模型中的执行时序对比图，如图1所示。

顺序一致性模型中，所有操作完全按程序的顺序串行执行。而在JMM中，临界区内的代码可以重排序（但JMM不允许临界区内的代码“逸出”到临界区之外，那样会破坏监视器的语义）。JMM会在退出临界区和进入临界区这两个关键时间点做一些特别处理，使得线程在这两个时间点具有与顺序一致性模型相同的内存视图（具体细节后文会说明）。虽然线程A在临界区内做了重排序，但由于监视器互斥执行的特性，这里的线程B根本无法“观察”到线程A在临界区内的重排序。这种重排序既提高了执行效率，又没有改变程序的执行结果。

图1

![](../../.gitbook/assets/import-3-3.png)

从这里我们可以看到，JMM在具体实现上的基本方针为：在不改变（正确同步的）程序执行结果的前提下，尽可能地为编译器和处理器的优化打开方便之门。




# ThreadLocal关键字

ThreadLocal，即线程变量，是一个以ThreadLocal对象为键、任意对象为值的存储结构。这

代码清单1　Profiler.java

```
public class Profiler {
    // 第一次get()方法调用时会进行初始化（如果set方法没有调用），每个线程会调用一次
    private static final ThreadLocal<Long> TIME_THREADLOCAL = new ThreadLocal<Long>() {
        protected Long initialValue() {
            return System.currentTimeMillis();
        }
    };
    public static final void begin() {
        TIME_THREADLOCAL.set(System.currentTimeMillis());
    }
    public static final long end() {
        return System.currentTimeMillis() - TIME_THREADLOCAL.get();
    }
    public static void main(String[] args) throws Exception {
        Profiler.begin();
        TimeUnit.SECONDS.sleep(1);
        System.out.println("Cost: " + Profiler.end() + " mills");
    }
}
```

输出结果如下所示。

Cost: 1001 mills

Profiler可以被复用在方法调用耗时统计的功能上，在方法的入口前执行begin\(\)方法，在

先关文档参考：

百度百科：[https://baike.baidu.com/item/ThreadLocal/4915311?fr=aladdin](https://baike.baidu.com/item/ThreadLocal/4915311?fr=aladdin)



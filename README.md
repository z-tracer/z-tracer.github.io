## z-tracer

z-tracer是一个专为嵌入式系统设计的分布式linux性能检测工具。嵌入式系统通常cpu能力和内存大小有限，不适合进行数据分析和数据存储。z-tracer通过设备->服务器的分布式方式，将数据分析和处理移到性能更高的服务器端，减轻设备压力。可以同时监控多个设备。

### 系统概况
系统概况用来根据系统资源的总体使用情况，负载较小，监控的内容包括：
1. cpu利用率
2. 整体内存使用情况
3. cpu平均负载
4. 硬中断
5. 软中断
6. 进程上下文切换
7. 进程创建频率
8. 处于run和block状态的进程数
9. 进程+线程总数
10. cpu使用总计数
![image](http://z-tracer.github.io/img/summary.png)

### CPU监控
cpu监控子项以功能模块的方式来监控和分析对cpu的使用情况

**CPU**
cpu以逻辑cpu为单位，记录每个cpu上的利用率情况
![image](http://z-tracer.github.io/img/cpu_cpus.png)

**硬中断**
同样是以逻辑cpu为单位进行记录
![image](http://z-tracer.github.io/img/cpu_interrupts.png)

**软中断**
也是以逻辑cpu为单位记录
![image](http://z-tracer.github.io/img/cpu_softirqs.png)

**系统调用**
用于系统每时每刻都在进行，数据量会非常大，所以采取了手动采集方式，用来分析当前系统调用的次数和分布情况（采样时间不要太长！）。
![image](http://z-tracer.github.io/img/syscalls.png)
系统调用按两种方式进行了统计：
1. 按系统调用名
2. 按进程pid
当点击系统调用对应的表格时，会弹出该系统调用是由哪些进程发起的，每个进程调用的次数
![image](http://z-tracer.github.io/img/syscalls_bysyscall.png)
同样当点击进程pid对应的表格，会弹出该进程所发生的系统调用类型统计，以及每种系统调用的次数
![image](http://z-tracer.github.io/img/syscalls_bypid.png)
另外，还可以通过制定pid或者系统调用名称的方式来捕获特定的数据，以减小数据量

**信号**
信号用来跟踪当前系统中所发生的信号事件，也是通过手动模式触发，相对于系统调用，它的数据量会小很多。
![image](http://z-tracer.github.io/img/signal.png)
当点击表格里的某个进程时，就会显示出与此进程相关的信号
![image](http://z-tracer.github.io/img/signal_pid.png)

### 内存监控

### 任务监控
任务监控是以进程为单位进行分析
**关系图**
关系图能够显示整个系统中所有进程和线程的父子关系，让我们对整个系统中的进程有个一全面的认识
![image](http://z-tracer.github.io/img/pstree.svg)
在这里进程按照是否含有线程有三种样式：
1. 含有线程的进程是一个矩形：
![image](http://z-tracer.github.io/img/pstree_hasthread.svg)
2. 不含线程的进程是一个椭圆形：
![image](http://z-tracer.github.io/img/pstree_nothread.svg)
3. 线程本身由小圆点代替：
![image](http://z-tracer.github.io/img/pstree_thread.svg)
同时进程也会按照不同的调度属性分配不同的颜色


### 函数监控


### PERF监控





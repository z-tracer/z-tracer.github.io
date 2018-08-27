# z-tracer

## 目录
* [系统概况](#系统概况)
* [CPU监控](#CPU监控)
* [内存监控](#内存监控)
* [任务监控](#任务监控)
* [函数监控](#函数监控)
* [PERF监控](#PERF监控)

***
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
cpu监控子项以功能模块的方式来监控和分析对cpu的使用情况<br>

**CPU**<br>
cpu以逻辑cpu为单位，记录每个cpu上的利用率情况<br>
![image](http://z-tracer.github.io/img/cpu_cpus.png)<br>

**硬中断**<br>
同样是以逻辑cpu为单位进行记录<br>
![image](http://z-tracer.github.io/img/cpu_interrupts.png)<br>

**软中断**<br>
也是以逻辑cpu为单位记录<br>
![image](http://z-tracer.github.io/img/cpu_softirqs.png)<br>

**系统调用**<br>
由于系统每时每刻都在进行，数据量会非常大，所以采取了手动采集方式，用来分析当前系统调用的次数和分布情况（采样时间不要太长！）。<br>
![image](http://z-tracer.github.io/img/syscalls.png)<br>
系统调用按两种方式进行了统计：<br>
1. 按系统调用名<br>
2. 按进程pid<br>
当点击系统调用对应的表格时，会弹出该系统调用是由哪些进程发起的，每个进程调用的次数<br>
![image](http://z-tracer.github.io/img/syscalls_bysyscall.png)<br>
同样当点击进程pid对应的表格，会弹出该进程所发生的系统调用类型统计，以及每种系统调用的次数<br>
![image](http://z-tracer.github.io/img/syscalls_bypid.png)<br>
另外，还可以通过指定pid或者系统调用名称的方式来捕获特定的数据，以减小数据量。

**信号**<br>
信号用来跟踪当前系统中所发生的信号事件，也是通过手动模式触发，相对于系统调用，它的数据量会小很多。<br>
![image](http://z-tracer.github.io/img/signal.png)<br>
当点击表格里的某个进程时，就会显示出与此进程相关的信号<br>
![image](http://z-tracer.github.io/img/signal_pid.png)<br>

### 内存监控

### 任务监控
任务监控是以进程或者线程为单位进行分析<br>
**关系图**<br>
关系图能够显示整个系统中所有进程和线程的父子关系，让我们对整个系统中的进程有个一全面的认识<br>
![image](http://z-tracer.github.io/img/pstree.svg)<br>
在这里进程按照是否含有线程有三种样式：<br>
1. 含有线程的进程是一个矩形：<br>
![image](http://z-tracer.github.io/img/pstree_hasthread.png)<br>
2. 不含线程的进程是一个椭圆形：<br>
![image](http://z-tracer.github.io/img/pstree_nothread.png)<br>
3. 线程本身由小圆点代替：<br>
![image](http://z-tracer.github.io/img/pstree_thread.png)<br>
同时进程也会按照不同的调度属性分配不同的颜色<br>

**进程监控**<br>
进程监控以进程为单位，监控进程各自资源的使用情况，包括：<br>
1. 进程占用cpu情况（总cpu和每个cpu都会统计）
2. 进程占用内存情况（只统计最近有变化的进程）
3. 进程切换，包括自愿切换和非自愿切换
4. 进程缺页，包括主缺页和次缺页
![image](http://z-tracer.github.io/img/process_monitor.png)<br>

**进程/线程信息**<br>
分别以进程/线程为单位，显示详细的进程状态信息。
![image](http://z-tracer.github.io/img/process_info.png)<br>

同时通过树图的方式显示自启动以来进程运行总时间的分布情况：<br>
![image](http://z-tracer.github.io/img/process_cpu.png)<br>
如果想知道最近一段时间哪些进程运行的时间最长，可以点击**查看最近最繁忙进程**来检测<br>

每个进程占用的物理内存也是通过树图的方式直观的显示出来<br>
![image](http://z-tracer.github.io/img/process_mem.png)<br>
如果想知道最近哪些进程在申请内存，哪些在释放内存，可以点击**查看最近内存变化**来检测<br>

另外点击进程列表中的pid可以显示并跟踪单个进程<br>
![image](http://z-tracer.github.io/img/task.png)<br>
会显示与进程相关的详细信息：<br>
1. 命令行
2. 环境变量
3. 打开的文件描述符
4. 进程信息
5. 父子关系
6. 信号设置
7. 地址空间映射
8. 虚拟内存分布<br>

同时会对进程进行监控：<br>
1. 虚拟内存大小监控
2. 物理内存大小监控
3. cpu使用监控（包括内核态和用户态）

### 函数监控
函数监控用来分析一个函数的调用和运行情况，分为callee和caller。<br>
**callee**<br>
callee用于分析一个函数的内部运行情况，可以找出该函数内部的调用路径，以及最耗时的子函数（目前只支持内核函数）。<br>
![image](http://z-tracer.github.io/img/function_callee.png)<br>
* 函数执行时间分布:以热图的方式记录函数的执行时间。<br>
* 调用次数分布：统计每个进程调用该函数的总次数。<br>
* 运行时间分布：统计每个进程该函数的总运行时间。<br>
同时以列表的形式记录每个进程中该函数运行的最大时间，最小时间和平均时间<br>
最后通过热图的方式能够直观的发现最耗时的函数路径<br>
通过点击**查看流程图**可以查看函数运行的流程图，方便理解函数的执行过程。<br>
![image](http://z-tracer.github.io/img/functree.svg)<br>

**caller**<br>
caller用于分析函数被调用的情况，用于感知都是谁在调用此函数。同时也支持对函数的参数进行跟踪<br>
![image](http://z-tracer.github.io/img/function_caller.png)<br>
要想显示函数可跟踪的参数，需要在编译的时候加上-g参数。当然也可以直接指定cpu寄存器。但是不同的平台cpu的传参和寄存器的命名方式不一样，需要参考相应的手册。<br>

### PERF监控
perf监控移植了flamescope，通过设置参数来记录cpu的运行情况，可设置的参数包括：<br>
![image](http://z-tracer.github.io/img/perf_setting.png)<br>
高级参数需要用户熟悉perf工具，请参考对应版本的perf工具。<br>
采集完成之后会以热图的方式显示该段时间内cpu的运行情况。cpu越繁忙颜色越深<br>
![image](http://z-tracer.github.io/img/perf_heatmap.png)<br>
可以通过点击鼠标来选择其中的一段时间，这样就会加载该段时间的火焰图<br>
![image](http://z-tracer.github.io/img/perf_flame.png)<br>
通过火焰图就能知道这段时间cpu都在干什么。


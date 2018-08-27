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


### 内存监控


### 任务监控


### 函数监控


### PERF监控

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```



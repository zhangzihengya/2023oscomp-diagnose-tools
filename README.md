# proj208-performance-and-diagnosis-tool

## 项目介绍

项目名称：高负载下性能分析与异常诊断工具

项目描述：高负载情况下，用户态常用的性能工具常常会失效，这时一个具有高性能、高可靠性 ，同时对系统影响低、准确性强的性能分析和异常诊断工具就显得尤为重要。

预期目标：

- 编写性能工具，采集有关数据，输出火焰图。
- 能够在高负载下进行压力测试，并可靠地完成性能监测和异常分析。
- 在高负载场景下，对工具进程测试，寻找可优化的部分。

## 团队介绍

指导导师：陈莉君教授、谢瑞莲老师

项目导师：谢宝友（国科础石）

团队成员：

| 姓名   | 成员分工                                                     |
| ------ | ------------------------------------------------------------ |
| 张子恒 | 项目整体规划、编写不同业务进程画像及确定异常时间点功能、整合项目文档 |
| 刘冰   | 开源工具diagnose的复现及优化、工具的压力测试、编写相应工作的文档 |
| 南帅波 | 编写负载变化趋势图像功能、整合工具、编写相应工作的文档        |

## 阶段性技术报告

详见：[阶段性技术报告](https://gitlab.eduxiji.net/202311664111382/project1466467-176202/-/blob/main/阶段性技术报告.md)

## 区域赛技术报告

详见：[区域赛技术报告](https://gitlab.eduxiji.net/202311664111382/project1466467-176202/-/blob/main/区域赛技术报告.md)

## 项目规划

| 时间                  | 任务                                               | 完成情况 |
| --------------------- | -------------------------------------------------- | -------- |
| 2023.5.15 - 2023.5.25 | 完成工具在Linux高版本上的复现以及代码上的优化，并有文档记录       | 已完成   |
|                       | 完成扩充异常进程画像及负载变化功能，并有文档记录       | 已完成   |
| 2023.5.26 - 2023.5.30 | 完成扩充异常时间点确定功能及工具的整合，并有文件记录   | 已完成   |
| 2023.5.31 - 2023.6.4  | 完成工具在高负载场景下的基本性能测试，并有文档记录 | 已完成   |
| 2023.6.4   - 2023.6.7 | 完成区域赛文档的汇总，并提交作品                   | 已完成   |
| 2023.6.8   - 2023.7.1 | 完成工具在网络方面功能的实现，并有文档记录         |          |
| 2023.7.2 - 2023.7.25  | 完成工具在高负载场景下的性能测试，并有文档记录     |          |
| 2023.7.26 - 2023.8.10 | 完成工具在高负载场景下的优化，并有文档记录         |          |
| 2023.8.11 - 2023.8.15 | 完成国赛文档的汇总，并提交作品                     |          |

## 会议记录

### 2023.4.9 确定选题

会议目的：进行操作系统大赛的选题

会议内容：根据我们自身对Linux操作系统的熟悉程度和擅长领域，我们决定从性能和诊断工具的方向出发，最终选定了《proj208-performance-and-diagnosis-tool》这一赛题

下一步计划：进行项目调研，一周后进行汇总，选择工具的开发技术，并和项目导师取得联系

### 2023.4.16 暂定开发技术为内核模块

会议目的：进行项目调研的汇总以及开发工具技术的选择

会议内容：经过汇总筛选，我们确定了内核模块和eBPF技术作为重点研究对象，并发现了赛题导师开源的diagnosis-tool工具，决定进行复现和学习，但还未联系到项目导师，暂定采用内核模块的方法进行开发

下一步计划：刘冰负责复现该工具，张子恒和南帅波负责对这个工具的各个模块进行学习

### 2023.4.27 尝试转向eBPF技术

会议目的：对近期学习情况进行一个反馈

会议内容：刘冰这块已经基本完成了赛题导师开源工具的复现，张子恒和南帅波在赛题导师开源工具学习过程中无不感叹这个工具的强大以及代码编写的巧妙，因此也感到很迷茫，如果做内核模块的话，又该如何进行优化和完善呢？于是我们动摇了，决定去做eBPF方向

下一步计划：张子恒负责编写利用eBPF技术去监控一分钟平均负载值的程序；刘冰负责编写利用eBPF技术去遍历所有进程内核态堆栈的程序；南帅波负责编写利用eBPF技术去遍历所有进程用户态堆栈的程序

### 2023.5.4 eBPF开发中的问题

会议目的：解决在编写eBPF程序过程中遇到的问题

会议内容：刘冰和南帅波遇到的问题是虽然利用eBPF 的迭代器技术可以实现遍历打印进程的内核态堆栈，但是无法通过地址找到用户态堆栈名称。张子恒遇到的问题是由于负载的avenrun数组变量是全局变量，那么也就无法通过挂载函数来实现读取这一变量中的值，想到的解决办法是通过在/proc/kallsyms文件中读取avenrun变量的地址，然后通过用户态程序传入到内核态程序读到这一变量值，这也就意味着每次都要通过用户空间程序的读取，但这在高负载环境下是非常不友好的。针对这些问题，我们讨论后始终找不到一个好的解决办法，因此决定寻求老师们的帮助。

下一步计划：找陈莉君老师去探讨利用eBPF技术去实现这一工具的可行性；找谢宝友老师进行沟通，确定具体的开发方案

### 2023.5.10 确定内核模块的具体开发方案

会议目的：探讨具体开发方案

会议内容：在找陈老师进行沟通后，我们进行了内核模块和eBPF技术的对比，发现内核模块突出的是性能而eBPF突出的是安全，而根据赛题要求的是开发一个高负载下性能分析与异常诊断工具，可见对于这个工具来说性能相对来说是更重要的，因此我们决定去做内核模块这个方向，并在和谢老师商讨后确定了具体地开发方案。

下一步计划：刘冰继续去完善diagnosis-tool工具中的复现；张子恒和南帅波负责对已复现模块进行优化，以及横向扩展

### 2023.5.20 开发过程中遇到的问题及解决

会议目的：解决在开发过程中遇到的问题

会议内容：经过了一段时间的工具开发，我们遇到了种种问题，在这次会议中，我们按照先后顺序提出这些问题，针对每个问题都展开了讨论，并制定了解决方法，这些问题主要分为以下几类：运行过程中的错误、数据采集与处理、开源工具diagnose-tools的内部逻辑。通过这次会议，我们解决了很多问题，极大地提高了开发效率。

下一步计划：针对会上提到的解决方法，各自进行问题的解决，如仍遇到问题再及时讨论。

### 2023.6.2    部署区赛最后的任务

会议目的：部署区赛最后的任务

会议内容：尽管我们的工具在整体上已经基本完成，但每个人的工作部分尚未整合，因此工具的完成情况有些零散，并且我们不确定是否还有未考虑到的方面。经过讨论，我们将工作大致划分为三个部分：区域赛技术报告的撰写、工具的整合以及工具的压力测试。我们计划同时进行这三个部分，并由每个人负责其中一部分的工作。

下一步计划：张子恒主要负责区域赛技术报告的撰写、南帅波主要负责工具的整合、刘冰继续负责压测部分，后期根据完成情况，再相互配合。

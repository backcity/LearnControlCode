学习节奏：建议稳定在每周 12–18 小时（工作日每天 1–2 小时，周末 4–6 小时）。若可以全职，则把每周目标放大 2–3 倍。
记录与复盘：每周写一篇 300–600 字的学习/实践日志（放到 GitHub 项目 wiki 或 weekly-notes.md），每月做一次回顾并把里程碑提交到 tag/release。
代码托管与 CI：把每个重要项目做成独立 repo（或 monorepo 下子目录），包含 README、TODO、测试、CI（GitHub Actions），并写部署/运行脚本。
硬件环境：准备一块 Raspberry Pi（建议 Pi 4，4GB 以上），一台 x86 开发机，若做 CAN/CANopen，准备 USB-CAN 转换器（如 Peak、Kvaser 或 cheap USB2CAN），以及一块小的工业总线仿真板（可选）。
考核方式：每月一个可运行的 demo/报告 + GitHub PR + README；每季度请一位同行 review（可以是同事或开源社区）。
下面是按月的具体细化（每月分 4 周，必要时第 5 周作为缓冲/复盘）：

第 1 月：深入 C# 基础，理解 CLR、GC

周目标
第 1 周：复习 C# 基础语法、类型系统、编译/运行流程（IL、JIT）。
第 2 周：学习 CLR 组件（CLR、JIT、类型加载、程序集）、异常处理、元数据。
第 3 周：深入 GC 工作原理（代际回收、Pinned、Large Object Heap）、触发条件。
第 4 周：用 dotnet 写 demo：内存分配/释放统计、强引用/弱引用、观察 GC。
资源
《CLR via C#》、Microsoft docs（CLR、垃圾回收）
dotTrace 或 Visual Studio Diagnostic Tools
实践项目（交付物）
Demo：一个 C# 控制台程序，持续分配对象并记录内存扫描与 GC 事件（输出 csv），并在 README 里解释 GC 行为。
验收标准
能准确解释代际 GC 的某一次回收日志；用工具捕获并分析一次 GC pause。
第 2 月：C 语言基础、指针与内存管理

周目标
第 1 周：C 基础语法、编译链（gcc）、Makefile。
第 2 周：指针、数组、字符串、结构体、内存布局。
第 3 周：动态内存管理（malloc/free）、内存泄露和 UB（valgrind）。
第 4 周：在 Raspberry Pi 上交叉/本地编译并运行简单程序。
资源
《C Primer Plus》/K&R、valgrind 文档、gcc toolchain docs
实践项目
在 Raspberry Pi 上写 C 程序：实现数组操作、字符串处理、简单内存池，并用 valgrind 检查。
验收标准
在 ARM 上可交叉编译并运行，valgrind 无 leak（或记录并修复至少两个内存错误）。
第 3 月：掌握 Linux 基础与交叉编译

周目标
第 1 周：Linux 常用命令、系统结构（内核、用户空间）、权限。
第 2 周：工具链（gcc、gdb、strace、ltrace）、交叉编译原理。
第 3 周：设置交叉编译工具链（arm-none-eabi / gcc-arm-linux-gnueabihf）。
第 4 周：把上月的 C 程序交叉编译并在 Raspberry Pi 上运行；调试（gdbserver）。
资源
《鸟哥的 Linux 私房菜》、Linaro toolchain 文档
实践项目
完成交叉编译脚本，写部署脚本（scp + ssh 执行），记录常见问题。
验收标准
可以在主机上交叉编译并用 gdbserver 远程调试。
第 4 月：C# 并发编程（Task、线程池、锁）

周目标
第 1 周：线程模型、Task vs Thread、async/await 原理。
第 2 周：线程池、Synchronization primitives（lock、Monitor、Semaphore、Mutex、ReaderWriterLockSlim）。
第 3 周：并发设计模式（producer-consumer、blocking collection、工作调度）。
第 4 周：实现并发日志服务（采集 + 写文件 + 回滚/轮转）。
资源
《C# 并发编程实战》、Microsoft Threading docs、TPL docs
实践项目
多线程日志服务：高吞吐写入、文件切换、测试并发写入一致性（单测）。
验收标准
日志服务在高并发下无数据丢失/死锁；包含单元测试和压力测试报告。
第 5 月：Linux 并发与系统编程

周目标
第 1 周：Linux 进程/线程、fork、exec、信号、IPC（pipe、socket、shared memory）。
第 2 周：epoll/select、非阻塞 IO、IO 复用。
第 3 周：多进程/多线程服务器架构（线程池、事件驱动）。
第 4 周：实现 socket 服务器，支持并发客户端（使用 epoll 或 libuv）。
资源
《Linux 高性能服务器编程》、man pages
实践项目
用 C/C++ 实现一个能处理并发连接的 echo 或简单协议服务器，并测并发性能。
验收标准
支持 N=1000 并发连接（受硬件限制），并写性能测试报告（latency/QPS）。
第 6 月：理解实时系统（PREEMPT_RT 基础）

周目标
第 1 周：实时系统概念（soft/hard real-time）、调度策略（SCHED_FIFO, SCHED_RR）。
第 2 周：PREEMPT_RT 概念、补丁、配置内核参数。
第 3 周：在 Raspberry Pi 上应用 PREEMPT_RT（或使用 real-time enabled image）。
第 4 周：用 cyclictest 测试延迟并用 htop/ps 查看线程优先级。
资源
PREEMPT_RT Wiki、cyclictest 文档、rt-tests
实践项目
在 Pi 上做延迟测试（不同负载下记录 max/jitter），编写报告并给出调优建议（CPU isolation, irqbalance）。
验收标准
能给出调优后 jitter 改进的数据（表格/图表）。
第 7 月：工业协议（EtherCAT 基础）

周目标
第 1 周：理解现场总线与 EtherCAT 基本概念、拓扑。
第 2 周：学习 EtherCAT 从站/主站模型，PDO/SDO。
第 3 周：阅读 SOEM 项目，搭建开发环境。
第 4 周：跑通 SOEM 的 slaveinfo demo，读写从站参数。
资源
SOEM 源码与文档、EtherCAT 技术规范概览
实践项目
在你能访问的 EtherCAT 从站（或模拟器）上运行 demo，记录抓包（Wireshark + EtherCAT dissector）。
验收标准
成功使用 SOEM 读写至少一个从站寄存器，并在 README 里写操作步骤与抓包分析。
第 8 月：Modbus / CANopen 协议

周目标
第 1 周：Modbus 基本功能码和帧结构、RTU/TCP。
第 2 周：libmodbus 使用，写主站/从站 Demo。
第 3 周：CAN 基础、CANopen 概念（PDO、SDO、NMT），查看 CANopenNode 或其他实现。
第 4 周：搭建 USB-CAN 并调试 CANopen 示例。
资源
libmodbus 文档、CANopenNode、SocketCAN 文档
实践项目
Modbus 主站 Demo（读取寄存器 + 可视化），CANopen 简单主站/从站交互 demo。
验收标准
能在真实或仿真设备上读写寄存器并能解释报文。
第 9 月：控制算法（PID、滤波器）

周目标
第 1 周：控制理论基础，PID 控制器结构与调参方法（Ziegler–Nichols）。
第 2 周：滤波器（低通、高通、卡尔曼滤波简介）。
第 3 周：用 Python/Matlab 做 PID 仿真、绘图和调参。
第 4 周：把控制算法移植到 C#（或 C），与上面硬件接口联调。
资源
《自动控制原理》、Python control 库、Matlab/Octave 教程
实践项目
PID 控制仿真（joystick 或虚拟 plant），后移植成 C# 服务并接收来自传感器/模拟器的数据。
验收标准
仿真曲线可视化（step response），移植后能在 1 个 demo 中稳定控制目标变量。
第 10 月：软件工程与架构设计

周目标
第 1 周：设计原则（SOLID、分层架构、依赖注入）。
第 2 周：模块化、接口设计、抽象与实现分离。
第 3 周：单元测试、集成测试、mock、测试覆盖率。
第 4 周：对已有小项目重构，增加接口分层和单元测试。
资源
《代码整洁之道》、xUnit/NUnit、Moq、SonarCloud docs
实践项目
对之前某个服务重构：拆出接口、写单元测试、CI 验证，生成覆盖率报告。
验收标准
项目具有清晰模块边界，测试覆盖率 > 60%（按实际情况设定），CI 能自动通过。
第 11 月：性能优化方法

周目标
第 1 周：性能分析工具（perf、top、htop、dotTrace）、CPU/IO/内存瓶颈判断。
第 2 周：常见优化方法（算法优化、内存池、避免锁争用）。
第 3 周：用 perf/dtrace 分析程序热点。
第 4 周：对某个服务做分析并优化，记录 before/after。
资源
《性能之巅》、perf 教程、dotTrace docs
实践项目
选择一个 CPU 或 IO 密集模块，用 perf 测并实现至少一项优化，记录性能指标。
验收标准
CPU 占用或响应时间有显著下降（如 20%+），并把分析报告写到 repo。
第 12 月：CI/CD 与系统优化

周目标
第 1 周：学习 GitHub Actions、构建脚本、交叉编译 pipeline。
第 2 周：自动化测试、部署脚本、artifact 发布。
第 3 周：系统级优化（taskset、isolcpus、cgroups）。
第 4 周：把全年项目做成自动化构建 + ARM 部署并运行自动化测试。
资源
GitHub Actions docs、systemd、cgroups v2 文档
实践项目
为关键项目写 GitHub Actions：CI（build、test）、CD（deploy 到 Pi），并写一份部署说明。
验收标准
每次 push 能触发 CI 并在 Pi 上部署成功（或在模拟环境完成自动化测试）。
作品集与 GitHub 组织建议（交付清单）

每月交付至少一个可运行的 demo（binary 或 docker 镜像）+ README + 测试/压测报告。
最终作品集应包含：
memory-gc-demo（第 1 月）
c-raspi-utils（第 2-3 月）
concurrent-logger（第 4 月）
linux-epoll-server（第 5 月）
rt-latency-tests（第 6 月）
ethercat-soem-playground（第 7 月）
modbus-can-demos（第 8 月）
control-algorithms（第 9 月）
refactor-project-with-tests（第 10 月）
perf-optimizations（第 11 月）
ci-cd-deployments（第 12 月）
每个项目都应包含：README、如何构建、如何运行、测试/bench 数据、License（MIT or Apache 2.0）。
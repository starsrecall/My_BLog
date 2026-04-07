[课程首页](https://missing.csail.mit.edu/) 
[课程列表](https://missing.csail.mit.edu/2026/) [往届内容](https://missing.csail.mit.edu/past/) [关于课程](https://missing.csail.mit.edu/about/)

# 调试与性能分析（Debugging and Profiling）
编程有一条黄金法则：代码不会按你的预期运行，只会严格执行你编写的逻辑。弥合预期与实际行为的差距有时是非常困难的工作。本讲我们将介绍处理缺陷代码、高资源消耗代码的实用技术：**调试（Debugging）**与**性能分析（Profiling）**。

---

## 调试（Debugging）
### Printf调试与日志
> “最有效的调试工具仍然是缜密的思考，加上精心放置的打印语句。”——布莱恩·克尼汉（Brian Kernighan），《Unix入门指南》
调试程序的第一种思路是在你发现问题的位置附近添加打印语句，反复调整打印位置直到获取足够信息，定位问题根源。
第二种更成熟的方案是在程序中使用**日志（Logging）**代替临时添加的打印语句。日志本质是「更规范的打印」，通常通过日志框架实现，内置支持以下能力：
*   支持将全部/部分日志输出到不同位置（文件、远程服务等）
*   支持设置日志严重级别（如INFO、DEBUG、WARN、ERROR等），可以按需过滤输出内容
*   支持结构化日志存储，日志关联的上下文数据可以在事后方便地提取
日志语句通常是你在编码阶段就可以主动添加的，这样需要调试时所需的数据可能已经存在！如果你用打印语句定位并修复了问题，在删除这些打印前，最好把它们转换成规范的日志语句——这样未来出现类似问题时，不需要修改代码就有现成的诊断信息。

> **第三方程序日志**：很多程序支持`-v`或`--verbose`参数输出更详细的运行信息，非常适合排查命令执行失败的原因，部分程序还支持重复添加该参数输出更细节的信息。调试服务（数据库、Web服务器等）问题时可以查看它们的日志，Linux系统的服务日志通常位于`/var/log/`目录；systemd管理的服务可以用`journalctl -u <服务名>`查看日志。第三方库可以检查是否支持通过环境变量或配置开启调试日志。

### 调试器（Debuggers）
打印调试适合你知道需要打印什么信息、且可以轻松修改重跑代码的场景。当你不确定需要什么信息、Bug仅在难以复现的条件下触发、或是修改重启程序的成本很高（启动时间长、需要重建复杂状态等）时，调试器就会发挥巨大价值。
调试器是可以让你在程序运行时与其交互的工具，支持以下能力：
*   程序执行到指定行时暂停
*   逐行/逐指令执行代码
*   程序崩溃后检查变量的值
*   满足指定条件时暂停执行
*   更多高级功能
大多数编程语言都支持（或自带）某种形式的调试器。通用性最强的是**通用调试器**，比如[`gdb`](https://www.gnu.org/software/gdb/)（GNU调试器）和[`lldb`](https://lldb.llvm.org/)（LLVM调试器），可以调试任意原生二进制程序。很多语言还有专属调试器，和语言 runtime 集成更紧密，比如Python的`pdb`、Java的`jdb`。
`gdb`是C、C++、Rust等编译型语言的事实标准调试器，几乎可以探查任意进程的当前机器状态：寄存器、栈、程序计数器等。
常用GDB命令：
*   `run` - 启动程序
*   `b {函数名}` 或 `b {文件名}:{行号}` - 设置断点
*   `c` - 继续执行
*   `step` / `next` / `finish` - 步入函数 / 步过当前行 / 步出当前函数
*   `p {变量名}` - 打印变量的值
*   `bt` - 显示回溯（调用栈）
*   `watch {表达式}` - 表达式值变化时触发断点
> 可以使用GDB的TUI模式（启动时加`-tui`参数，或在GDB内按`Ctrl-x a`）获得分屏视图，同时显示源代码和命令提示符。

#### 录制重放调试（Record-Replay Debugging）
最让人头疼的Bug是**海森堡Bug（Heisenbug）**：这类Bug在你尝试观测时会消失或改变行为。竞态条件、时序依赖Bug、仅在特定系统条件下出现的问题都属于这类。传统调试方法通常对这类问题无效，因为重新运行程序时行为已经改变（比如打印语句会拖慢代码速度，导致竞态条件不再触发）。
**录制重放调试**通过记录程序的完整执行过程，支持你按确定性逻辑无限次重放执行过程，完美解决了这个问题。更强大的是，你可以**反向执行**程序，精准定位问题出现的位置。
[rr](https://rr-project.org/)是Linux平台的强大录制重放工具，可以记录程序执行过程，并支持带完整调试能力的确定性重放。它和GDB兼容，你不需要额外学习新的交互接口。
基础用法：
```bash
# 录制程序执行过程
rr record ./my_program

# 重放录制内容（会自动打开GDB）
rr replay
```
真正的魔力在重放阶段：由于执行过程是完全确定的，你可以使用**反向调试**命令：
*   `reverse-continue`（缩写`rc`） - 反向运行直到遇到断点
*   `reverse-step`（缩写`rs`） - 反向逐行执行
*   `reverse-next`（缩写`rn`） - 反向逐行执行，跳过函数调用
*   `reverse-finish` - 反向运行直到进入当前函数
这是非常强大的调试能力。假设程序出现崩溃，你不需要猜测Bug位置、设置断点，只需要：
1.  运行到崩溃位置
2.  检查被破坏的状态
3.  给被破坏的变量设置观察点
4.  执行`reverse-continue`，直接定位到变量被篡改的精确位置

**rr适用场景**：
*   间歇性失败的不稳定测试
*   竞态条件和多线程Bug
*   难以复现的崩溃
*   所有你希望「回到过去」排查的Bug
> 注意：rr仅支持Linux，且需要硬件性能计数器支持。在不暴露性能计数器的虚拟机（比如大多数AWS EC2实例）中无法使用，也不支持GPU访问。macOS用户可以尝试[Warpspeed](https://warpspeed.dev/)。
> **rr与并发**：由于rr会确定性地记录执行过程，它会序列化线程调度。如果竞态条件依赖特定的时序，可能在rr录制时不会触发。但rr仍然适合调试竞态问题——只要你捕获到了一次失败的运行，就可以可靠地重放它，只是可能需要多次尝试录制才能捕获到间歇性Bug。对于非并发相关的Bug，rr的优势最为明显：你可以完全复现执行过程，用反向调试快速定位内存篡改问题。

### 系统调用追踪（System Call Tracing）
有时你需要理解程序和操作系统的交互逻辑。程序通过[系统调用（System Call）](https://en.wikipedia.org/wiki/System_call)向内核请求服务，比如打开文件、分配内存、创建进程等。追踪这些系统调用可以揭示程序挂起的原因、尝试访问的文件、或是等待资源的耗时。

#### strace（Linux）与dtruss（macOS）
[`strace`](https://www.man7.org/linux/man-pages/man1/strace.1.html)可以观测程序执行的每一个系统调用：
```bash
# 追踪所有系统调用
strace ./my_program

# 仅追踪文件相关的调用
strace -e trace=file ./my_program

# 追踪子进程（对于会启动其他程序的程序非常重要）
strace -f ./my_program

# 追踪正在运行的进程
strace -p <PID>

# 显示调用耗时信息
strace -T ./my_program
```
> macOS和BSD系统可以用[`dtruss`](https://www.manpagez.com/man/1/dtruss/)（基于`dtrace`封装）实现类似功能。
> 想深入学习`strace`可以参考Julia Evans的优秀[strace zine](https://jvns.ca/strace-zine-unfolded.pdf)。

#### bpftrace与eBPF
[eBPF](https://ebpf.io/)（扩展伯克利包过滤器）是Linux的强大技术，支持在内核中运行沙箱化的程序。[`bpftrace`](https://github.com/iovisor/bpftrace)提供了编写eBPF程序的高层语法，你可以在内核中运行任意自定义逻辑，表达能力极强（不过语法类似awk，略显笨拙）。最常见的用途是观测系统调用，包括聚合统计（调用次数、延迟统计等）、检查甚至过滤系统调用参数。
```bash
# 系统级追踪文件打开操作（实时打印）
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_openat { printf("%s %s\n", comm, str(args->filename)); }'

# 按名称统计系统调用次数（按Ctrl-C后打印汇总）
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_* { @[probe] = count(); }'
```
你也可以用[`bcc`](https://github.com/iovisor/bcc)工具链直接用C编写eBPF程序，它自带了很多实用工具，比如`biosnoop`用于打印磁盘操作的延迟分布，`opensnoop`用于打印所有被打开的文件。
`strace`的优势是简单易上手，当你需要更低的追踪开销、追踪内核函数、做任何形式的聚合统计时，应该选择`bpftrace`。注意`bpftrace`需要root权限运行，且默认监控整个内核而非单个进程，你可以通过命令名或PID过滤目标程序：
```bash
# 按命令名过滤（按Ctrl-C后打印汇总）
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_* /comm == "bash"/ { @[probe] = count(); }'

# 用-c参数追踪从启动开始的指定命令（cpid是子进程PID）
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_* /pid == cpid/ { @[probe] = count(); }' -c 'ls -la'
```
`-c`参数会运行指定命令并将`cpid`设为其PID，适合从程序启动就开始追踪的场景。被追踪的命令退出后，bpftrace会打印聚合结果。

#### 网络调试
针对网络问题，[`tcpdump`](https://www.man7.org/linux/man-pages/man1/tcpdump.1.html)和[Wireshark](https://www.wireshark.org/)可以捕获并分析网络数据包：
```bash
# 捕获80端口的数据包
sudo tcpdump -i any port 80

# 捕获并保存到文件，供Wireshark分析
sudo tcpdump -i any -w capture.pcap
```
对于HTTPS流量，加密会让tcpdump的作用有限，此时可以用[mitmproxy](https://mitmproxy.org/)作为拦截代理，解密并查看加密流量。调试Web应用的HTTPS请求最简单的方式是用浏览器开发者工具的网络面板，可以直接看到解密后的请求/响应数据、请求头和耗时信息。

### 内存调试
内存类Bug（缓冲区溢出、释放后使用、内存泄漏）是最危险、最难调试的问题之一，它们通常不会立即崩溃，而是会破坏内存，导致很久之后才出现问题。

#### 运行时错误检测工具（Sanitizers）
发现内存Bug的一种方案是使用**Sanitizer（运行时错误检测工具）**，这是编译器的特性，可以给代码插桩，在运行时检测错误。比如应用最广泛的**地址消毒器（AddressSanitizer, ASan）**可以检测：
*   缓冲区溢出（栈、堆、全局变量）
*   释放后使用（Use-after-free）
*   返回后使用（Use-after-return）
*   内存泄漏
```bash
# 编译时启用地址消毒器
gcc -fsanitize=address -g program.c -o program
./program
```
还有很多实用的Sanitizer：
*   **线程消毒器（ThreadSanitizer, TSan）**：检测多线程代码中的数据竞争，编译参数`-fsanitize=thread`
*   **内存消毒器（MemorySanitizer, MSan）**：检测读取未初始化内存的问题，编译参数`-fsanitize=memory`
*   **未定义行为消毒器（UndefinedBehaviorSanitizer, UBSan）**：检测整数溢出等未定义行为，编译参数`-fsanitize=undefined`
Sanitizer需要重新编译代码，但运行速度足够快，可以在CI流水线和日常开发中使用。

#### Valgrind：无法重新编译时的选择
[Valgrind](https://valgrind.org/)通过类似虚拟机的环境运行程序来检测内存错误，比Sanitizer慢，但不需要重新编译代码：
```bash
valgrind --leak-check=full ./my_program
```
Valgrind适用场景：
*   没有源代码
*   无法重新编译（比如第三方库）
*   需要Sanitizer不具备的特定工具
Valgrind实际上是一个非常强大的受控执行环境，我们后面讲性能分析时还会用到它的能力。

### AI辅助调试
大语言模型已经成为非常实用的调试助手，它们擅长的调试任务可以很好地补充传统工具的能力。
**LLM的优势场景**：
*   **解释晦涩的错误信息**：编译器错误（尤其是C++模板、Rust借用检查器的错误）通常非常难懂，LLM可以把它们翻译成通俗的语言，并给出修复建议。
*   **跨语言/抽象边界调试**：如果你调试的问题跨越多个语言（比如C语言库的Bug通过Python绑定暴露出来），LLM可以帮助你在不同层级间导航。它们特别擅长理解FFI边界、构建系统问题、跨语言调试（比如：我的程序报错，但我认为问题出在依赖库的Bug上）。
*   **关联症状与根因**：「我的程序功能正常，但内存占用是预期的10倍」这类模糊的症状，LLM可以帮助你排查，给出可能的原因和检查方向。
*   **分析崩溃转储和栈追踪**：粘贴栈追踪信息，询问可能的原因。
> **调试符号注意事项**：要获得有意义的栈追踪和调试信息，需要确保二进制文件（以及所有链接的库）编译时包含调试符号（`-g`编译参数）。调试信息通常以DWARF格式存储。此外，编译时保留帧指针（`-fno-omit-frame-pointer`）可以让栈追踪更可靠，尤其对性能分析工具来说。没有这些信息，栈追踪可能只会显示内存地址，或是不完整。这个问题对C++、Rust等原生编译语言的影响比Python、Java更大。

**需要注意的局限性**：
*   LLM可能会生成听起来合理但完全错误的解释
*   它们可能会给出掩盖而非修复Bug的建议
*   永远要用实际调试工具验证LLM的建议
*   它们是理解代码的补充，而非替代品
> 这里的用法和[开发环境](https://missing.csail.mit.edu/2026/development-environment/#ai-powered-development)一讲中提到的通用AI编码能力不同，这里特指将LLM用作调试助手。

---

## 性能分析（Profiling）
即使你的代码功能符合预期，如果它运行时消耗了所有CPU或内存，仍然是不合格的。算法课通常会教大O notation，但不会教如何找到程序的**热点（Hot Spot，消耗资源最多的部分）**。由于[过早优化是万恶之源](https://wiki.c2.com/?PrematureOptimization)，你需要学习性能分析和监控工具，它们可以帮助你理解程序哪部分消耗了最多的时间/资源，让你可以针对性地优化。

### 计时
最简单的性能测量方式是计时。很多场景下，打印代码两点之间的耗时就足够了。
但挂钟时间可能会有误导性，因为你的计算机可能同时运行其他进程，或是程序在等待外部事件。`time`命令可以区分**真实时间（Real）**、**用户态CPU时间（User）**和**内核态CPU时间（Sys）**：
*   **Real**：从启动到结束的挂钟时间，包括等待时间
*   **User**：CPU运行用户代码的时间
*   **Sys**：CPU运行内核代码的时间
```bash
$ time curl https://missing.csail.mit.edu &> /dev/null
real	0m0.272s
user	0m0.079s
sys	    0m0.028s
```
这个例子中请求的真实耗时接近300毫秒，但CPU时间只有107毫秒（user+sys），剩余时间都在等待网络响应。

### 资源监控
分析程序性能的第一步通常是理解它的实际资源消耗情况。程序在资源受限的时候通常会变慢。
*   **通用监控**：[`htop`](https://htop.dev/)是`top`的增强版本，可以显示当前运行进程的各类统计信息。常用快捷键：`<F6>`排序进程，`t`显示树状层级，`h`切换显示线程。还有[`btop`](https://github.com/aristocratos/btop)可以监控更多维度的指标。
*   **I/O操作监控**：[`iotop`](https://www.man7.org/linux/man-pages/man8/iotop.8.html)显示实时I/O使用```
信息。
*   **内存使用监控**：[`free`](https://www.man7.org/linux/man-pages/man1/free.1.html)显示系统总可用、已用内存量。
*   **打开文件检查**：[`lsof`](https://www.man7.org/linux/man-pages/man8/lsof.8.html)列出进程打开的所有文件相关信息，非常适合排查哪个进程占用了特定文件。
*   **网络连接监控**：[`ss`](https://www.man7.org/linux/man-pages/man8/ss.8.html)可以监控网络连接，常见使用场景是查找占用指定端口的进程：`ss -tlnp | grep :8080`。
*   **网络用量监控**：[`nethogs`](https://github.com/raboof/nethogs)和[`iftop`](https://pdw.ex-parrot.com/iftop/)是优秀的交互式命令行工具，可以分别按进程、按连接监控网络用量。

### 性能数据可视化
人类从图表中识别模式的速度远快于从数字表格中。分析性能时，绘制数据图表通常可以暴露出原始数据中看不到的趋势、峰值和异常。

**让数据可被绘图**：添加调试用的打印或日志语句时，尽量考虑让输出格式可以方便后续绘图。CSV格式的时间戳+数值（`1705012345,42.5`）远比描述性句子更容易绘图。JSON结构的日志也可以用极少的工作量解析并绘图。换句话说，要用[整洁的格式](https://vita.had.co.nz/papers/tidy-data.pdf)记录数据。

**用gnuplot快速绘图**：针对简单的命令行绘图需求，[`gnuplot`](http://www.gnuplot.info/)可以直接从数据文件生成图表：
```bash
# 绘制包含时间戳、值的简单CSV文件
gnuplot -e "set datafile separator ','; plot 'latency.csv' using 1:2 with lines"
```

**用matplotlib和ggplot2做探索性分析**：如果需要更深入的分析，Python的[`matplotlib`](https://matplotlib.org/)和R的[`ggplot2`](https://ggplot2.tidyverse.org/)支持迭代式的数据探索。和一次性绘图不同，这些工具可以让你快速切片、转换数据，验证假设。ggplot2的分面图功能尤其强大——你可以按类别将单个数据集拆分到多个子图中（比如按接口或时间段拆分请求延迟），挖掘出原本隐藏的规律。

**典型用例**：
*   绘制请求延迟随时间的变化曲线，可以发现原始百分位数据掩盖的周期性变慢（垃圾回收、定时任务、流量规律等）
*   可视化随数据量增长的插入耗时，可以暴露出算法复杂度问题——比如向量插入的耗时图会在底层数组扩容时出现特征性的峰值
*   按不同维度（请求类型、用户群体、服务器）拆分指标，通常会发现「全系统范围」的问题实际上只出现在某一类场景中

### CPU性能分析器
大多数时候人们提到的「性能分析器」指的是CPU性能分析器，主要分为两类：
*   **追踪型分析器（Tracing Profiler）**：记录程序的每一次函数调用
*   **采样型分析器（Sampling Profiler）**：周期性（通常每毫秒）采样程序的调用栈
采样型分析器的开销更低，通常更适合生产环境使用。

#### perf：采样分析器
[`perf`](https://www.man7.org/linux/man-pages/man1/perf.1.html)是Linux的标准性能分析工具，不需要重新编译程序就可以分析任意进程：
`perf stat`可以快速给出时间消耗的概览：
```bash
$ perf stat ./slow_program

 Performance counter stats for './slow_program':

         3,210.45 msec task-clock                #    0.998 CPUs utilized
               12      context-switches          #    3.738 /sec
                0      cpu-migrations            #    0.000 /sec
              156      page-faults               #   48.587 /sec
   12,345,678,901      cycles                    #    3.845 GHz
    9,876,543,210      instructions              #    0.80  insn per cycle
    1,234,567,890      branches                  #  384.532 M/sec
       12,345,678      branch-misses             #    1.00% of all branches
```
真实程序的分析器输出会包含大量信息，人类是视觉动物，非常不擅长阅读大量数字。[火焰图（Flame Graph）](https://www.brendangregg.com/flamegraphs.html)是一种可视化方式，可以极大降低性能数据的理解成本。

火焰图的Y轴显示函数调用的层级，X轴显示每个函数消耗的时间占比。它是交互式的，你可以点击放大查看程序特定部分的耗时。
[![火焰图示例](https://www.brendangregg.com/FlameGraphs/cpu-bash-flamegraph.svg)](https://www.brendangregg.com/FlameGraphs/cpu-bash-flamegraph.svg)

从`perf`数据生成火焰图的方法：
```bash
# 录制性能数据
perf record -g ./my_program

# 生成火焰图（需要安装flamegraph脚本）
perf script | stackcollapse-perf.pl | flamegraph.pl > flamegraph.svg
```
> 可以使用[Speedscope](https://www.speedscope.app/)这款基于网页的交互式火焰图查看器，或是[Perfetto](https://perfetto.dev/)做全面的系统级分析。

#### Valgrind的Callgrind：追踪型分析器
[`callgrind`](https://valgrind.org/docs/manual/cl-manual.html)是记录程序调用历史和指令计数的分析工具。和采样型分析器不同，它提供精确的调用次数，可以展示调用者和被调用者之间的关系：
```bash
# 用callgrind运行程序
valgrind --tool=callgrind ./my_program

# 用callgrind_annotate（文本界面）或kcachegrind（图形界面）分析结果
callgrind_annotate callgrind.out.<pid>
kcachegrind callgrind.out.<pid>
```
Callgrind比采样型分析器慢，但可以提供精确的调用计数，如果需要还可以模拟缓存行为（加`--cache-sim=yes`参数）。

> 如果你使用特定编程语言，可能有更专用的分析器。比如Python有[`cProfile`](https://docs.python.org/3/library/profile.html)和[`py-spy`](https://github.com/benfred/py-spy)，Go有[`go tool pprof`](https://pkg.go.dev/cmd/pprof)，Rust有[`cargo-flamegraph`](https://github.com/flamegraph-rs/flamegraph)（实际上它支持任意编译型程序！）。

### 内存性能分析器
内存分析器可以帮助你理解程序随时间的内存使用情况，定位内存泄漏。
#### Valgrind的Massif
[`massif`](https://valgrind.org/docs/manual/ms-manual.html)用于分析堆内存使用：
```bash
valgrind --tool=massif ./my_program
ms_print massif.out.<pid>
```
它会展示堆内存随时间的使用情况，帮助识别内存泄漏和过度分配问题。
> 对于Python，[`memory-profiler`](https://pypi.org/project/memory-profiler/)可以提供逐行的内存使用信息。

### 基准测试（Benchmarking）
当你需要比较不同实现或工具的性能时，[`hyperfine`](https://github.com/sharkdp/hyperfine)是非常优秀的命令行程序基准测试工具：
```bash
$ hyperfine --warmup 3 'fd -e jpg' 'find . -iname "*.jpg"'
Benchmark #1: fd -e jpg
  Time (mean ± σ):      51.4 ms ±   2.9 ms    [User: 121.0 ms, System: 160.5 ms]
  Range (min … max):    44.2 ms …  60.1 ms    56 runs

Benchmark #2: find . -iname "*.jpg"
  Time (mean ± σ):      1.126 s ±  0.101 s    [User: 141.1 ms, System: 956.1 ms]
  Range (min … max):    0.975 s …  1.287 s    10 runs

Summary
  'fd -e jpg' ran
   21.89 ± 2.33 times faster than 'find . -iname "*.jpg"'
```
> 针对Web开发，浏览器开发者工具包含非常优秀的分析器，可以参考[Firefox Profiler](https://profiler.firefox.com/docs/)和[Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools/rendering-tools)的文档。

---

## 练习题
### 调试部分
1.  **调试排序算法**：以下伪代码实现了归并排序但包含Bug，用你熟悉的语言实现它，然后用调试器（gdb、lldb、pdb或IDE自带调试器）找到并修复Bug。
    ```
    function merge_sort(arr):
        if length(arr) <= 1:
            return arr
        mid = length(arr) / 2
        left = merge_sort(arr[0..mid])
        right = merge_sort(arr[mid..end])
        return merge(left, right)
    
    function merge(left, right):
        result = []
        i = 0, j = 0
        while i < length(left) AND j < length(right):
            if left[i] <= right[j]:
                append result, left[i]
                i = i + 1
            else:
                append result, right[i]
                j = j + 1
        append remaining elements from left and right
        return result
    ```
    测试用例：`merge_sort([3, 1, 4, 1, 5, 9, 2, 6])`应该返回`[1, 1, 2, 3, 4, 5, 6, 9]`。使用断点并单步执行merge函数，找到选择错误元素的位置。
2.  安装[`rr`](https://rr-project.org/)，用反向调试找到内存篡改Bug。将以下代码保存为`corruption.c`：
    ```c
    #include <stdio.h>
    typedef struct {
        int id;
        int scores[3];
    } Student;
    
    Student students[2];
    
    void init() {
        students[0].id = 1001;
        students[0].scores[0] = 85;
        students[0].scores[1] = 92;
        students[0].scores[2] = 78;
    
        students[1].id = 1002;
        students[1].scores[0] = 90;
        students[1].scores[1] = 88;
        students[1].scores[2] = 95;
    }
    
    void curve_scores(int student_idx, int curve) {
        for (int i = 0; i < 4; i++) {
            students[student_idx].scores[i] += curve;
        }
    }
    
    int main() {
        init();
        printf("=== Initial state ===\n");
        printf("Student 0: id=%d\n", students[0].id);
        printf("Student 1: id=%d\n", students[1].id);
    
        curve_scores(0, 5);
    
        printf("\n=== After curving ===\n");
        printf("Student 0: id=%d\n", students[0].id);
        printf("Student 1: id=%d\n", students[1].id);
    
        if (students[1].id != 1002) {
            printf("\nERROR: Student 1's ID was corrupted! Expected 1002, got %d\n",
                   students[1].id);
            return 1;
        }
        return 0;
    }
    ```
    用`gcc -g corruption.c -o corruption`编译并运行，你会发现Student 1的ID被篡改了，但篡改发生在一个仅操作Student 0的函数中。用`rr record ./corruption`和`rr replay`定位问题，给`students[1].id`设置观察点，在篡改发生后执行`reverse-continue`，找到覆盖该值的精确代码行。
3.  用AddressSanitizer调试内存错误。将以下代码保存为`uaf.c`：
    ```c
    #include <stdlib.h>
    #include <string.h>
    #include <stdio.h>
    int main() {
        char *greeting = malloc(32);
        strcpy(greeting, "Hello, world!");
        printf("%s\n", greeting);
    
        free(greeting);
    
        greeting[0] = 'J';
        printf("%s\n", greeting);
    
        return 0;
    }
    ```
    首先不启用Sanitizer编译运行：`gcc uaf.c -o uaf && ./uaf`，它可能看起来运行正常。现在用AddressSanitizer编译：`gcc -fsanitize=address -g uaf.c -o uaf && ./uaf`，阅读错误报告，ASan找到了什么Bug？修复它发现的问题。
4.  用`strace`（Linux）或`dtruss`（macOS）追踪`ls -l`这类命令的系统调用，它执行了哪些系统调用？尝试追踪更复杂的程序，看看它打开了哪些文件。
5.  用LLM辅助调试晦涩的错误信息：复制一段编译器错误（尤其是C++模板或Rust的错误），让LLM解释并给出修复建议。也可以尝试把`strace`或AddressSanitizer的输出输入给LLM，看看它的分析结果。

### 性能分析部分
1.  用`perf stat`获取你选择的某个程序的基础性能统计，每个计数器的含义是什么？
2.  用`perf record`做性能分析。将以下代码保存为`slow.c`：
    ```c
    #include <math.h>
    #include <stdio.h>
    double slow_computation(int n) {
        double result = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < 1000; j++) {
                result += sin(i * j) * cos(i + j);
            }
        }
        return result;
    }
    
    int main() {
        double r = 0;
        for (int i = 0; i < 100; i++) {
            r += slow_computation(1000);
        }
        printf("Result: %f\n", r);
        return 0;
    }
    ```
    带调试符号编译：`gcc -g -O2 slow.c -o slow -lm`。运行`perf record -g ./slow`，然后用`perf report`查看时间消耗在哪部分。尝试用flamegraph脚本生成火焰图。
3.  用`hyperfine`基准测试同一任务的两种不同实现（比如`find` vs `fd`、`grep` vs `ripgrep`，或是你自己写的两个版本的代码）。
4.  运行资源密集型程序时用`htop`监控系统状态。尝试用`taskset`限制进程可以使用的CPU核心：`taskset --cpu-list 0,2 stress -c 3`，为什么`stress`没有用到三个CPU核心？
5.  一个常见问题是你想要监听的端口已经被其他进程占用，学习如何找到该进程：首先执行`python -m http.server 4444`在4444端口启动一个极简Web服务器，在另一个终端运行`ss -tlnp | grep 4444`找到对应进程，用`kill <PID>`终止它。

---

* * *
[编辑此页面](https://github.com/missing-semester/missing-semester/blob/master/_2026/debugging-profiling.md)
本内容采用[CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)协议授权。
本文转自 [https://missing.csail.mit.edu/2026/debugging-profiling/](https://missing.csail.mit.edu/2026/debugging-profiling/)，如有侵权，请联系删除。

---

【核心术语对照表】
| 英文原文 | 标准译法 | 概念说明 |
|----------|----------|----------|
| Debugging | 调试 | 定位、修复程序中错误的过程，是软件开发的核心环节之一。 |
| Profiling | 性能分析/剖面分析 | 采集程序运行时的资源消耗数据（CPU、内存、I/O等），定位性能瓶颈的过程。 |
| Heisenbug | 海森堡Bug | 一类在观测时会改变行为的Bug，得名于量子力学中的海森堡不确定性原理，常见于竞态条件、时序依赖问题。 |
| Breakpoint | 断点 | 调试器的核心功能，程序运行到指定位置时会暂停执行，供开发者检查状态。 | Watchpoint | 观察点/数据断点 | 调试器功能，当指定变量/表达式的值发生变化时触发断点，适合排查内存篡改问题。 |
| Backtrace (Call Stack) | 回溯/调用栈 | 程序执行到某一位置时的函数调用层级关系，是定位崩溃、分析性能的核心信息。 |
| Context Switch | 上下文切换 | CPU从一个进程/线程切换到另一个进程/线程的操作，频繁上下文切换会导致性能下降。 |
| Record-Replay Debugging | 录制重放调试 | 一种调试技术，通过记录程序完整执行过程，支持确定性重放甚至反向执行，解决难复现的Bug。 |
| System Call | 系统调用 | 用户态程序向操作系统内核请求服务的接口，是用户态和内核态交互的唯一方式。 |
| Sanitizer | 运行时错误检测工具/消毒器 | 编译器插桩工具，在程序运行时检测内存错误、数据竞争、未定义行为等问题，常见如ASan、TSan。 |
| AddressSanitizer (ASan) | 地址消毒器 | 最常用的Sanitizer工具，可检测缓冲区溢出、释放后使用、内存泄漏等常见内存问题，性能开销仅约2倍。 |
| eBPF | 扩展伯克利包过滤器 | Linux内核特性，允许在内核中运行沙箱化程序，是当前系统观测、性能分析领域的主流技术。 |
| Flame Graph | 火焰图 | 由Brendan Gregg发明的性能数据可视化方式，直观展示函数调用栈的时间占比，是性能分析的标配工具。 |
| Sampling Profiler | 采样型性能分析器 | 周期性采样程序调用栈的分析工具，开销低，适合生产环境使用，代表工具如Linux perf。 |
| Tracing Profiler | 追踪型性能分析器 | 记录所有函数调用的分析工具，数据精确但开销高，适合线下深度分析，代表工具如Callgrind。 |
| Benchmarking | 基准测试 | 通过标准化的测试方法，比较不同实现/工具的性能表现的过程，核心是保证测试条件的一致性。 |

---

【难句解析】
1. 原文：A golden rule in programming is that code does not do what you expect it to do, but what you tell it to do. Bridging that gap can sometimes be a quite difficult feat.
   译文：编程有一条黄金法则：代码不会按你的预期运行，只会严格执行你编写的逻辑。弥合预期与实际行为的差距有时是非常困难的工作。
   解析：这是软件工程领域的经典名言，`what you tell it to do`直译是「你告诉它做的事」，结合技术语境意译为「你编写的逻辑」，更符合开发者的认知；`feat`原指「功绩」，这里意译为「工作」更符合中文技术文档的表达习惯。
2. 原文：Some of the most frustrating bugs are _Heisenbugs_: bugs that seem to disappear or change behavior when you try to observe them. Race conditions, timing-dependent bugs, and issues that only appear under certain system conditions fall into this category.
   译文：最让人头疼的Bug是**海森堡Bug（Heisenbug）**：这类Bug在你尝试观测时会消失或改变行为。竞态条件、时序依赖Bug、仅在特定系统条件下出现的问题都属于这类。
   解析：`Heisenbug`是计算机领域的经典术语，得名于量子力学中的海森堡不确定性原理（观测行为本身会影响被观测对象），翻译时保留原名并补充命名背景说明，帮助读者理解术语的由来；多层定语从句拆解为符合中文逻辑的短句，避免生硬的翻译腔。
3. 原文：Where `strace` is useful because it’s easy to “just get up and running”, `bpftrace` is what you should reach for when you need lower overhead, want to trace through kernel functions, need to do any kind of aggregation, etc.
   译文：`strace`的优势是简单易上手，当你需要更低的追踪开销、追踪内核函数、做任何形式的聚合统计时，应该选择`bpftrace`。
   解析：原句用`where`引导对比关系，直译会非常晦涩，翻译时调整为「A的优势是XX，当需要XX时应该选择B」的对比结构，符合中文表达习惯；`get up and running`是常用技术短语，意译为「易上手」更通顺。
4. 原文：Algorithms classes often teach big _O_ notation but not how to find hot spots in your programs. Since [premature optimization is the root of all evil](https://wiki.c2.com/?PrematureOptimization), you should learn about profilers and monitoring tools.
   译文：算法课通常会教大O复杂度，但不会教如何找到程序的**热点**（消耗资源最多的部分）。由于[过早优化是万恶之源](https://wiki.c2.com/?PrematureOptimization)，你需要学习性能分析和监控工具。
   解析：`big O notation`是算法领域的标准术语「大O复杂度」；`hot spots`技术语境下译为「热点」，补充括号说明含义方便新手理解；`premature optimization is the root of all evil`是编程界的经典名言，直译即可，保留原文链接供读者扩展阅读。

---

【背景补充说明】
本讲内容是工业界排查线上问题的核心技能栈，大多数计算机专业课程不会系统教授这些工具，但掌握后可以把问题排查效率提升数倍：
1.  工具普及度：`strace`、`htop`、Sanitizer是开发者的日常必备工具，`perf`、eBPF、`rr`是中高级工程师排查复杂问题的利器，几乎所有互联网公司的技术栈都会包含这些工具。
2.  火焰图由Linux性能优化专家Brendan Gregg发明，现在已经成为性能分析的行业标准，几乎所有云厂商的监控平台都内置了火焰图功能。Speedscope是当前最流行的本地火焰图查看工具，不需要安装，直接打开网页上传性能数据即可交互查看。
3.  Sanitizer工具链由Google开发，现在已经被集成到GCC、Clang、Rustc等主流编译器中，是当前内存问题排查的首选方案，性能开销仅为Valgrind的1/5，很多公司的CI流水线会默认开启ASan、TSan做自动化错误检测。
4.  `rr`工具由Mozilla开发，最初用于调试Firefox浏览器的复杂多线程Bug，现在已经成为录制重放调试的事实标准，对于间歇性崩溃、竞态条件这类传统调试方法难以定位的问题，`rr`可以极大提升排查效率。
5.  eBPF是近十年Linux内核最具革命性的特性之一，不需要修改内核代码就可以扩展内核功能，现在已经被广泛应用于系统观测、网络、安全等领域，`bpftrace`是eBPF的入门工具，适合快速编写自定义观测脚本。
6.  学习建议：新手可以先从简单工具入手，先学会用`htop`查看进程资源、用`strace`查看系统调用、用Sanitizer排查内存问题，再逐步学习`perf`、火焰图、eBPF等进阶工具，最好结合自己实际遇到的Bug和性能问题练习，效果远好于纯粹的命令记忆。
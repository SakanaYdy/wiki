---
title: 进程管理
icon: material/progress-tag
---

## 查看进程 ps

`ps` (process status) 命令用于查看当前系统中的进程信息。

基本命令：

```bash
# 查看当前所有进程
ps

# -e: every process
# -f: full format
ps -ef
```

常用选项：

```bash
# 查看指定用户的进程
ps -u <username>

# 查看指定 PID 的进程信息
ps -p <PID>

# 显示进程树
ps -ejH
```

动态查看工具：

```bash
# 系统自带，默认按 CPU 排序，交互式查看
top

# 增强版 top，支持鼠标操作、垂直滚动和进程树
htop

# 更现代的替代品，界面美观，支持 GPU 监控
btop
```

## 进程与文件 lsof

[lsof](https://github.com/lsof-org/lsof) (list open files) 可以列出系统中所有打开的文件，常用来定位进程与文件、端口之间的关联。

基本命令：

```bash
# 查看占用指定端口的进程
lsof -i :<port>

# 查看指定进程名打开的所有文件
lsof -c <process_name>

# 查看指定 PID 打开的所有文件
lsof -p <PID>
```

## 终止进程 kill

kill 是 Shell 内置的命令，用于控制进程（主要是终止进程）。

基本命令：

```bash
kill -<sig_tag> <PID>
```

控制标签有 64 种，可以使用以下命令打印：

```bash
kill -l

# 输出

#  1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
#  6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
# 11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
# 16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
# 21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
# 26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
# 31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
# 38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
# 43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
# 48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
# 53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
# 58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
# 63) SIGRTMAX-1  64) SIGRTMAX
```

常见的 Ctrl+C 

```bash
# 申请终止
kill <PID>

# Ctrl+C 表示
kill -2 <PID>

# 最强力的终止
kill -9 <PID>
```

## 调试进程 gdb

参考 [GDB 官网](https://www.gnu.org/software/gdb/)。常用的如下：

开始运行：

```bash
r  # 即 run
```

设置断点：

```bash
break <line_num>
```

运行到下一个断点：

```bash
c  # 即 continue
```

## 练习：gdb 实战

一、创建 `fork.c` 文件

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() {
    /* fork another process */
    pid_t  pid;
    pid = fork();

    if (pid < 0) {
        /* error occurred */
        fprintf(stderr, "Fork Failed");
        exit(-1);
    } else if (pid == 0) {
        /* child process */
        printf("This is child process, pid=%d\n", getpid());
        execlp("/bin/ls", "ls", NULL);
        printf("Child process finished\n"); /*这句话不会被打印，除非execlp调用未成功*/
    } else {
        /* parent process */
        /* parent will wait for the child to complete */
        printf("This is parent process, pid=%d\n", getpid());
        wait (NULL);
        printf ("Child Complete\n");
        exit(0);
    }
}
```

![创建文件](https://cdn.dwj601.cn/images/202410081813738.png)

这段程序首先通过调用 `fork()` 函数创建一个子进程，并通过 `pid` 信息来判断当前进程是父进程还是子进程。在并发的逻辑下，执行哪一个进程的逻辑是未知的。

二、编译运行 `fork.c` 文件

![编译运行](https://cdn.dwj601.cn/images/202410081807870.png)

从上述运行结果可以看出：并发时，首先执行父进程的逻辑，然后才执行子进程的逻辑。

三、gdb 调试

在 fork 创建子进程后追踪子进程：

```bash
gdb fork
set follow-fork-mode child
catch exec
```

![追踪子进程](https://cdn.dwj601.cn/images/202410081947476.png)

运行到第一个断点时分别观察父进程 1510168 和子进程 1510171：

![父进程 1510168](https://cdn.dwj601.cn/images/202410081948301.png)

![子进程 1510171](https://cdn.dwj601.cn/images/202410081949586.png)

运行到第二个断点时观察子进程 1510171：

![运行到第二个断点时观察子进程 1510171](https://cdn.dwj601.cn/images/202410081951803.png)

从上述子进程的追踪结果可以看出，在父进程结束之后，子进程成功执行了 `pid == 0` 的逻辑并开始调用 `ls` 工具。

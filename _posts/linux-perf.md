---
title: very simple perf guide
date: 2020-04-09
categories: linux tool
---
# mesuring process performance using perf

## 1. what is perf?
>perf (sometimes called perf_events or perf tools, originally Performance Counters for Linux, PCL) is a performance analyzing tool in Linux, available from Linux kernel version 2.6.31 in 2009. Userspace controlling utility, named perf, is accessed from the command line and provides a number of subcommands; it is capable of statistical profiling of the entire system (both kernel and userland code).  
It supports hardware performance counters, tracepoints, software performance counters (e.g. hrtimer), and dynamic probes (for example, kprobes or uprobes). In 2012, two IBM engineers recognized perf (along with OProfile) as one of the two most commonly used performance counter profiling tools on Linux. [from wikipedia](https://en.wikipedia.org/wiki/Perf_(Linux))

### Simply speaking, we can use perf to measure various things about CPU, Memory related to performance, make benchmark, and visualize the result.
### where to learn perf: https://perf.wiki.kernel.org/index.php/Main_Page, http://www.brendangregg.com/blog/2017-05-09/cpu-utilization-is-wrong.html

## Simple steps to use perf
### perf build & install on linux
1. Download source code
2. make & make install

## perf build & install on webOS
0. Since, I don't know how to configure bb file with git repositary, I just set the local destination of the perf source code. It would be better to config the bb file using git repositary.
1. make webos-local.conf under build-starfish/
2. bitbake lib32-perf
3. copy the build result to the nfsroot

```console
perf record -p <pid> -F 1000 sleep 10
```
## 4. running perf
* with the root authority run,

* explaination : record process number \<pid\> with the sampling rate 1000 for 10 seconds.
* then run,
* perf report
* by this way you will see the record of modules or functions runned on the pid.

## to be updated
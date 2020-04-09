---
title: very simple perf guide
date: 2020-04-09
categories: linux tool
---
# mesuring process performance using perf (not finished)

## 1. what is perf?
>perf (sometimes called perf_events or perf tools, originally Performance Counters for Linux, PCL) is a performance analyzing tool in Linux, available from Linux kernel version 2.6.31 in 2009. Userspace controlling utility, named perf, is accessed from the command line and provides a number of subcommands; it is capable of statistical profiling of the entire system (both kernel and userland code).  
It supports hardware performance counters, tracepoints, software performance counters (e.g. hrtimer), and dynamic probes (for example, kprobes or uprobes). In 2012, two IBM engineers recognized perf (along with OProfile) as one of the two most commonly used performance counter profiling tools on Linux. [from wikipedia](https://en.wikipedia.org/wiki/Perf_(Linux))

### In simple words, we can use perf to measure various things about CPU, Memory related to performance, make benchmark, or visualize the result.
### recommended sites to learn to use perf: [perf wiki](https://perf.wiki.kernel.org/index.php/Main_Page), [some netflix programmer's hompage(?)](http://www.brendangregg.com/blog/2017-05-09/cpu-utilization-is-wrong.html)

### In this page, I'm going to show **very** simple step of perf install on ubuntu, build perf on yocto project (webOS), installing and running perf on board, to measure performance of specific process inside the webOS

## 2. perf build & install on linux
1. Download source code
2. make & make install

## 3. perf build & install on webOS
0. Since, I don't know how to configure bb file with git repositary, I just set the local destination of the perf source code. It would be better to config the bb file using git repositary.
1. make webos-local.conf under build-starfish/
2. bitbake lib32-perf
3. copy the build result to the nfsroot

## 4. running perf
* with the root authority run,
* perf record -p \<pid\> -F 1000 sleep 10
* explaination : record process number \<pid\> with the sampling rate 1000 for 10 seconds.
* then run,
* perf report
* by this way you will see the record of modules or functions runned on the pid.

## to be updated later

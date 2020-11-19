---
title: linux-perf를 활용한 webOS 특정 프로세스 성능 측정 튜토리얼(업데이트중)
date: 2020-05-07 23:51:30 -0400
categories: jekyll update
---

# linux-perf를 활용한 webOS 특정 프로세스 성능 측정 튜토리얼(업데이트중)

## perf란?
Linux kernel version 2.6.31, 2009부터 지원하는 CLI 기반의 성능 측정 툴
2012년 IBM 엔지니어들 사이에서 리눅스 시스템 프로파일링시 가장 보편적으로 사용되는 tool로 뽑혔다고 한다.
 https://en.wikipedia.org/wiki/Perf_(Linux)

쉽게 말해 리눅스 시스템의 성능과 관련된 각종 수치를 측정하고 분석하는데 perf를 활용할 수 있다.
### perf사용법을 익히는데 참고할만한 사이트: [perf wiki](https://perf.wiki.kernel.org/index.php/Main_Page), [어떤 넷플릭스 프로그래머의 홈페이지](http://www.brendangregg.com/blog/2017-05-09/cpu-utilization-is-wrong.html), [레드햇 perf 가이드 페이지](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/developer_guide/perf)

## (ubuntu 18.04 기준) perf 설치하기
OS의 커널 버전을 확인한다.
```bash
$ uname -r
```
출력결과가 4.15.0-99-generic 라고 한다면,
```bash
$ cd /usr/src/linux-source-4.15.0/tools/perf/
```
소스코드가 없다면 https://www.kernel.org/ 에서 내려받는다.

소스코드를 빌드한다.
```bash
$ make;make install
```
설치가 잘 되었는지 확인한다.
```bash
$ cd ~
~$ perf
```
usage: perf [--version] [--help] [OPTIONS] COMMAND [ARGS] .... 가 보인다면 설치완료

## perf를 사용하여 실행중인 프로세스 성능 측정하기
예시 : '2020'프로세스에서 발생하는 이벤트를 1000/s sampling rate으로 10초간 프로파일링 한다면.
```bash
$ sudo perf record -p <pid> -F 1000 sleep 10
```
명령 실행결과로 perf.data 파일이 생성된다. 분석결과를 열기위하여 아래의 명령어를 실행한다.
```bash
$ sudo perf report
```
## webOS에서 perf 활용하기 
oe local build (흔히 'build-starfish/' 라고 써있는듯) 디렉토리 아래에 webos-local.conf파일을 작성한다.
```bash
$ cd build-starfish
$ vi webos-local.conf
```
아래와 같이 webos-local.conf파일을 작성하거나 수정한다.
```
INHERIT += "externalsrc"
EXTERNALSRC_pn-perf = "/home/hs/workspace/linux-src/linux-source-4.15.0/tools/perf"
```
타겟보드 모델에 맞춰 빌드환경을 셋팅한다. 타겟보드가 o20 이라면,
```bash
$ ./mcf -p 2 -b 2 o20
$ source oe-init-build-env                   
```
perf를 타겟보드 사양으로 빌드한다.  
```
$ bitbake lib32-perf
```
빌드결과물을 nfs root로 카피한다.
```bash
$ cp build-starfish/BUILD/sysroots-components/o20/lib32-perf/* -r your-nfs-root-dir/
```
이후의 과정은  [perf를 사용하여 실행중인 프로세스 성능 측정하기](#perf를-사용하여-실행중인-프로세스-성능-측정하기)와 동일하다.

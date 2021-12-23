---
layout: single
title:  "Using DTrace in System Performance Analysis and Monitoring"
category: "study"
last_modified_at: "2021-12-22"
---

All Linux distros provide numerous tracer tools for measuring system performance and debugging, such as `strace/ptrace`(per process) and `perf` (system-wide profiling). However, sometimes we have already had target suspicion on which component of the system is malfunctioning or damaging the performance. Profiling may be thorough, but also has high overhead for some events that occur frequently (`perf sched latency` for scheduling, for example). In this case, probing maybe more helpful, since it only trigger a program when an event happens under certain circumstances. Linux provides `dtrace` to initiate probes and D language to write probes that we want to use.

There are two ways of using `dtrace`. The first one, which is simpler, is one-liner. The format of command is:
```bash
dtrace -n "<code for the probes>"
```
We will talk about the syntax of D programming language later in the blog. Another method is using script in D language. It looks really similar to C:
```c
#!/usr/bin/dtrace -s

#pragma D option quiet

dtrace:::BEGIN
{
    printf("Start tracing...\n");
}

<code for probes>

dtrace:::END
{
    printf("End tracing.");
}

```
After writing the script, simply run this as a program will return the tracing result, since we've already intepret it as running a dtrace session in the first line.

The basic D language syntax has three components: probe name, predicate, and action. The probe name is defined as following:
```
provider:module:function:probe_name
```
For example
```bash
pid$target:libc:malloc:entry
```
inspecting the situation when user call `malloc` in library `libc`. `pid` is the name of the provider and `entry` means it check when kernel enter the function. Predicate is the condition of running the action. For example, `/execname == "mysql"/` means the action code should only be executed when the name of the process equals "mysql". Finally, the code for action is very similar to C, but with some pre-defined aggregate functions and variabled. For example

```bash
syscall::read:entry {
    @[execname, 'nbytes'] = quantize(arg0);
}
```
check the request size of system call `read` for each process and print a distribution for each process.

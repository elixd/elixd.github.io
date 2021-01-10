---
title: "Tuning Linux for Snapiness / Responsiveness"
date: "2019-11-22"
---

#### Preface

Main causes of low responsiveness are:

- processor ques
- I/O ques
- bad prioritization

#### Reduce unneccesary I/O

Tune your swappiness and cache pressure settings to avoid page faults when using your foreground applications. It's a trade-off of performance vs responsiness.

Install The Great Suspender for Chromiun to reduce RAM usage. Or Auto Tab Discard for Firefox. This will reduce the need to use swap. Or (surprise-surprise) buy more RAM.

Use iotop to see if there are processes eating the IO.

#### Improve scheduling / priotitization

Install the Linux-Zen or Liquorix kernels (depending on your distro) to have access to the better MuQSS process scheduler and BFQ I/O schedulers. If you're using an SSD you will want to check if you're using NOOP or Deadline I/O schedulers instead. Also check for TRIM support.

Reduce priority of processes like Dropbox and Tracker (configure them to only run when you are not using the system). It can be done using Ananicy.

Use htop to see if there processes competing for processor time

### Links

[https://www.akitaonrails.com/2017/01/17/optimizing-linux-for-slow-computers](https://www.akitaonrails.com/2017/01/17/optimizing-linux-for-slow-computers)

[https://askubuntu.com/questions/126664/why-choose-a-low-latency-kernel-over-a-generic-or-realtime-one](https://askubuntu.com/questions/126664/why-choose-a-low-latency-kernel-over-a-generic-or-realtime-one)

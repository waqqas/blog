---
published: false
title: "DPDK: 5 Minute Setup Guide"
cover_image: "https://raw.githubusercontent.com/waqqas/blog/master/blog-posts/dpdk-five-minute-setup-guide/assets/cover.jpg"
description: "Get up and running with DPDK in 5 minutes"
tags: dpdk
series: dpdk
canonical_url:
---

Overview
---

This post gives a quick listing of commands to get DPDK to run on a new machine

Getting the source code
---

- `git clone https://github.com/DPDK/dpdk.git`

Building DPDK
---

- `git checkout tags/v19.08`
- `make config T=x86_64-native-linuxapp-gcc`
- `make`

Reserve huge pages memory
---

- `mkdir -p /mnt/huge`
-  `mount -t hugetlbfs nodev /mnt/huge`
-  `echo 64 > /sys/devices/system/node/node0/hugepages/hugepages-2048kB/nr_hugepages`

Bind interface to DPDK
---

- `cd usertools`
- `./dpdk-setup.sh`


References
---

- [DPDK Quick-start guide](http://core.dpdk.org/doc/quick-start/)
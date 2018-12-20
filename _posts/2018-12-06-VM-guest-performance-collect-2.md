---
title: "VM guest API with Python"
date: 2018-12-06 19:10:00 -0400
categories: VMcollection
---

## intro
저번 포스팅과 이어서 VM guest 의 정보(CPU, Memory, HardDisk 사용량 등) 을 가져올수 있도록 여러가지를 찾아 보았습니다.

## KVM(qemu)
먼저 KVM(qumu)는 libvirt 를 통하여 대부분의 정보를 수집할수 있습니다. 


## VMware ESXI
Vmware ESXI 는 pyVim 프레임워크를 사용하여 데이터를 수집할수 있습니다.


## XenServer
XenServer는 자체적으로 제공하는 Xenapi 를 통하여 데이터를 수집할수 있습니다.


## outro
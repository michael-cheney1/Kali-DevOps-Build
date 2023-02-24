# Kali-DevOps-Build
Building your standard Kali environment using Ansible

# Introduction

The purpose of this guide is to share with you my automated Kali build. Anyone who has used Kali for some time has no doubt broken it multiple times and been faced with the prospect of having to rebuild a machine that you have spent countless hours perfecting, for it to fail at the wrong time. Many people utilise Virtual Machines and snapshots to be able to have a **known good** build that they can simply clone. I have also used this approach in the past however updating to the latest build can become arduous and can occasionally break things.  So to simplify the process you can use automation then if you add packages / repositories / download files later it's relatively straight forward to add these items into an Ansible template especially when you have some examples to work from. 

# Guide

## Summary

Normally this is the long part of any guide or tutorial a whole bunch of complicated technical steps that if you follow exactly right you may just get something to work at the end. The TLDR of this is basically clone repo, run script get nice things. 

## Pre-requisites

A base installation of Kali Linux. For this guide I downloaded the latest virtual machine from https://www.kali.org/get-kali/#kali-virtual-machines. You can also use an ISO if you're still a mouse enthusiast and like clicking things but we are trying to break that habit, as a colleague once said to me "The robot works for us, we don't work for the robot". I used the virtual machine https://cdimage.kali.org/kali-2022.4/kali-linux-2022.4-vmware-amd64.7z and imported it into VMware Workstation 15 Pro version 15.5.0 build-14665864. 

## Process

1. Git 


---
layout: post
title: "Using ECMP for Flow Guidance???"
date: 2026-10-08
paper_title: "Unlocking ECMP Programmability for Precise Traffic Control"
paper_authors: "Yadong Liu, Tencent; Yunming Xiao, University of Michigan; Xuan Zhang, Weizhen Dang, Huihui Liu, Xiang Li, and Zekun He, Tencent; Jilong Wang, Tsinghua University; Aleksandar Kuzmanovic, Northwestern University; Ang Chen, University of Michigan; Congcong Miao, Tencent"
paper_venue: "NSDI 2025"
paper_url: "https://www.usenix.org/conference/nsdi25/presentation/liu-yadong"
week: 3
tags: [routing, traffic, fallback]
---

## Key Idea
ECMP typically uses a hash value of the packet headers to route it randomly based on multiple available links. 
This is a problem for when you want precise control over the flow of traffic; such as detecting a connection issue 
before the network does and rerouting traffic through known working links. Major networks can use proprietary routers
to achieve such precise traffic flow, but this ability is generally unavailable in older/consumer hardware. 

The team here figured out how to utilize the 'ECMP-groups' feature, which is generally unused in most networks,
to implement such level of programmatic traffic engineering. Encoding the ability for a packet to specify what 
ECMP-group to use via 'IPv4-header DSCP field'. This allowed the traffic to set their own DSCP header for the 
routing behavior desired. This requires reprogramming the switches in the network to utilize ECMP-group which 
a configuration that encodes this programming, but it allow it to be backward compatible on legacy hardware 
and with IPv4. This also does not affect unaware traffic or handing the packets off to an unaware router. 

## Critique


## Connections


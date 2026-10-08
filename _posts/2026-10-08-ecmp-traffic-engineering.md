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

Calling their new method P-ECMP, the team here figured out how to utilize the 'ECMP-groups' feature to specify what
route to take on each packet. ECMP-group is generally unused in most datacenter networks, however most hardware supports
it, making the idea compatible with Tencent's existing hardware. 
To implement such level of programmatic traffic engineering, the desired path for a packet to take is encoded in the
via 'IPv4-header DSCP field'. This allowed the application traffic to set their own DSCP header for the 
routing behavior desired. This requires reprogramming the switches in the network to utilize ECMP-group which 
a configuration that encodes this programming, but it allow it to be backward compatible on legacy hardware 
and with IPv4. This also does not affect unaware traffic or handing the packets off to an unaware router. 

## Critique
How does this compare to proprietary hardware, or alternative solutions. 
Their deployment and testing was mostly done in simulator. While DSCP was not used by Tencent who was developing
P-ECMP, it might be used by others. One potentially major issue is the limited programability with 6 bits of DSCP
header to use, only a offset can be specified not a full network path which would take 24 bits. The current solution 
just specifies a way to ask for a different ECMP route (offset from route index 0), which works well for recovering 
from discovered bad routes. 

## Connections


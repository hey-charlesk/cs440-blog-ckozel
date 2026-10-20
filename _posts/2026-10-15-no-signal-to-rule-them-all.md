---
layout: post
title: "Don't Bother with Advanced Data Center Congestion Control"
date: 2026-10-15
paper_title: "No Signal to Rule Them All: A Systematic Analysis of In-Network Congestion Signals"
paper_authors: "S. McClure, N. Dukkipati, S. Ratnasamy, S."
paper_venue: "NINeS 2026"
paper_url: "https://drops.dagstuhl.de/storage/01oasics/oasics-vol139-nines2026/OASIcs.NINeS.2026.12/OASIcs.NINeS.2026.12.pdf"
week: 4
tags: [traffic, congestion_control, simulation]
---

## Key Idea
In a data center network where you have access to router level congestion signals (not just end-to-end signals)
can you do better than default TCP congestion control? In order to test this researchers collected sample traffic
from and assorted congestion signals including "queue" (num of packets in a routers queue), and "link"
(router utilization percentage). There signals would only be available where you control the 
entire network, the paper asks if you can implement a better congestion control algorithm (CCA) given these signals for 
use in data centers where the whole network can swap algorithms. To test this the authors expanded 'Remy' to R+, a tool
for learning CCAs based on sample traffic and network topology. It give you a close to optimal routing algorithm
based on that historic data. R+ outputs are not intended to be used as the production routing algorithm, but are 
intended to approximate the optimal routing algorithm for that traffic given those signals. 

Ultimately the results showed only 3% improvement on average over TCP Reno, within the margin or error. While 
some tests greater improvements in specific cases, overall the improvements were considered negligible. 
This indicates that there is no significant benefit to optimizing data center CCAs.

## Critique
The Remy/R+ approach assumes that the output of the tool is a near optimal routing algorithm for the given data.
This makes all the results contingent on the optimality of Remy/R+. The networks they ran simulations on seems too
simple with just 2 senders. 

## Connections

This is the second paper here that lives in the [ns-3](https://www.nsnam.org/) simulator. 
[P-ECMP](/cs440-blog-ckozel/2026/10/08/ecmp-traffic-engineering/) ran most of its evaluation in ns-3
before Tencent deployed one failover use case to production. Here ns-3 is the everything. R+ generates
candidate CCAs, then each is tested on 200 network configurations in ns-3 and the best wins. 

[Raha](/cs440-blog-ckozel/2026/10/06/Raha-Fault-Tolerance/) is also tagged simulation but is not ns-3. It models the network 
through MetaOpt's convex optimization rather than pushing packets through a simulated topology.


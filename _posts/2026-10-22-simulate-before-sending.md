---
layout: post
title: "Delaying Packets Leads to Lower Compleation Times!?"
date: 2026-10-22
paper_title: "Simulate Before Sending: Rethinking Transport in Datacenter Networks"
paper_authors: "D. Straussman, I. Keslassy, A. Shpiner, L. Liss"
paper_venue: "NINeS 2026"
paper_url: "https://drops.dagstuhl.de/entities/document/10.4230/OASIcs.NINeS.2026.19"
week: 5
tags: [simulation, ai, data-center]
---

## Key Idea

In data centers, particularly for AI workloads, packet loss can lead to significant collective completion times (CCT)
of distributed computations. Non-commodity hardware solutions exist but at a steep cost. Their solution is DcSim, which 
creates two queues on all switches, a small one for sim packets and a large one for (real) data packets. Sim packets
are first send out, then ACK'ed before sending data packets. The idea being if everyone on the network is using this
system, if a sim packet is ACK'ed then data packets should reach their destination. Overall this lead to near-zero
loss, and better CCT compared to the next best approach. 

## Critique

I'm pretty skeptical of this approach as it seems conceptually very inefficient. Needing to do a full RTT before
every data packet, doubling packets on the network, makes me cautious to accept the results. Even if sim packets
as small, there must be some fixed cost per packet. Also the delay of one RTT, cumulative an additional RTT on a large
dataset is negligible, but what about small traffic? Since everything in the network must use sim-then-data for 
the sim assumptions to hold, this approach is likely to be inefficient for small traffic. 

There organization of problems/solutions/issues-with-solutions is confusing in this paper. First a "shadow
simulation network" is described, sounding an awful lot like DcSim, only to call it "... appealing, but not practical". 
Then in 3.1 were told theorem 1 does not hold in a general data center networks with more than one switch. It goes on 
to say this means zero-loss is not deterministic, but can be practically near-zero. Feels like whiplash going back
and forth on the goodness of discussed ideas. 

## Connections
More network simulator usage, except here they use the "dcPIM simulator", where 
[P-ECMP](/cs440-blog-ckozel/2026/10/08/ecmp-traffic-engineering/) and
[P-ECMP](/cs440-blog-ckozel/2026/10/15/no-signal-to-rule-them-all/) used NS-3.
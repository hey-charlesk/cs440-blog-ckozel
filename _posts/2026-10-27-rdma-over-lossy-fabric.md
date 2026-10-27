---
layout: post
title: "RDMA Over Lossy Fabric"
date: 2026-10-27
paper_title: "Revisiting RDMA Reliability for Lossy Fabrics"
paper_authors: "W. Li, X. Liu, Y. Zhang, Z. Wang, W. Gu, T. Qian, G. Zeng, S. Ren, X. Huang, Z. Ren, B. Liu, J. Zhang, K. Chen, B. Liu (HKUST / Huawei)"
paper_venue: "SIGCOMM 2025"
paper_url: "https://doi.org/10.1145/3718958.3750480"
week: 5
tags: [rdma, data-center, reliability, ai, load-balancing]
---

## Key Idea
RDMA allows other computers on the network to access another systems memory without that systems CPU. Stock RDMA 
NICs use Go-Back-N to handle loss, making any data dropped very inefficient. To get lossless networking PFC is used
however PFC does not allow maximum network utilization and can cause deadlock. DCP is the solution presented which
create a high priority path for packet headers. The idea is that if a data buffer is full, the data gets dropped
but the header still makes it through. This way the receiver knows to data was lost and to ask the sender again. Since
the header-only packets are so small and prioritized above data, its very unlikely (though not guaranteed) that header
data is dropped. When testing in simulation (NS-3) AI collectives finished 38-61% faster.

## Critique

The packet counter trick only works if header-only packets are never lost, so this optimization is not guaranteed to work.
DCP on its own performed poorly on incast tests, it needs DCQCN congestion control to perform well. This performance
gain is best seen in AI workflows. 

## Connections

Same problem as [DcSim](/cs440-blog-ckozel/2026/10/22/simulate-before-sending/) attempts to solve, except
here it recovers from loss, rather than trying to avoid it entirely. Also used NS-3 simulator.

[No Signal to Rule Them All](/cs440-blog-ckozel/2026/10/15/no-signal-to-rule-them-all/) examined how
in-network signals don't help congestion control much.
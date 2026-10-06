---
layout: post
title: "Raha - Analyzing Fault Tolerance"
date: 2026-10-06
paper_title: "Raha: A General Tool to Analyze WAN Degradation"
paper_authors: "B. Arzani, S. Taheri, P. Namyar, R. Beckett, S. K. Kakarla, E. Jalilipour"
paper_venue: "SIGCOMM 2025"
paper_url: "https://dl.acm.org/doi/10.1145/3718958.3754348"
week: 2
tags: [architecture, simulation, testing]
---

## Key Idea

Microsoft engineers built Raha after an earthquake caused an extended outage in Africa. 
Multiple simultaneous failures and cascading failures lead to network degradation worse
than their prior analysis predicted. This was built on top of the existing MetaOpt tool,
which needed the problem transformed so that it is all convex, unlike an actual network. 
Compared to the old tooling Raha predicted at least 2x more degradation failures. 
Raha then can suggest where to add redundant links to best pickup slack in a failure.

## Critique
Part of the better performance compared to old methods was limiting the existing methods to
2 failures max, giving Raha an advantage. I cannot tell if reworking the problem to solve
all convex traffic is a brilliant idea or a mistake. Clearly it works since it was the 
paper hinges on but I did not follow that math.


## Connections

This is the fault-tolerance detection / solving which can prevent 
[partial reachability from islands and peninsulas](/2026/10/01/partial-reachability/). Baltra et al. measure
the Internet once its broken into fragmented paths. Raha tries to find the fragmentation before it happens.

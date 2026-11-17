---
layout: post
title: "Doing Congestion Control with Cellular Devices"
date: 2026-11-17
paper_title: "BISCAY: Practical Radio KPI Driven Congestion Control for Mobile Networks"
paper_authors: "J. Larrea, T. Shreedhar, A. Niemi, A. Sefiane, M. K. Marina (Revelare Networks / TU Delft / University of Edinburgh / Imperial College London / NVIDIA)"
paper_venue: "NINeS 2026"
paper_url: "https://drops.dagstuhl.de/entities/document/10.4230/OASIcs.NINeS.2026.15"
week: 8
tags: [congestion-control, wireless, cellular, transport, latency]
---

## Key Idea

Doing congestion control with cellular devices is challenging because of their tendency to move between towers
causing a lag between the actual conditions and measured CC stats. You either end up overestimating and queueing 
packets or underestimating and underutilizing the network capacity.

Authors propose BISCAY, a new congestion control algorithm which utilizes lower level radio metrics to estimate the
available network bandwidth. An existing user space interface for radio metrics exists called MobileInsight however
this provides data on the second level. BISCAY leverages a debugging protocol of Qualcom modems to get 10ms granularity
metrics. 

Using these lower level metrics at a more frequent rate shows much better performance in testing. In simulations 
against 8 other CCAs, BISCAY keeps about the same throughput while cutting delay. Compared to BBR, BISCAY  throughput 
was 1.03x with average delay down 58%. Compared to CUBIC, throughput was 0.96x with average delay down 99%. In a physical
test on a Pixel 5 BISCAY got 4.6% more throughput than BBR and CUBIC with average delay 46% lower than BBR. 
The granularity of sampling is key. When BISCAY's sampling rate is slowed up to 100ms it still performs better than BBR, 
with 1000ms sampling it performs worse than BBR/MobileInsight.

## Critique

One of the available metrics is "bandwidth grant" which has a 95% correlation with their calculated bandwidth. This
value is reported by the cell tower every 100ms, it's based on current network conditions and the data the modem reports 
it has to send. Since this has such a close correlation to their calculated value, it's reported on the same order of
magnitude they discovered was the upper limit for performance gains. Using this just seems like a simpler solution,
and offloads the complexity to the cell tower. Since this value is based in part on reported bytes in the modem queue,
it might need adequate queued packets to get the highest granted bytes. I think the solution should have been focused
around this value, as its the max bandwidth the model is allowed to use. Fall back to traditional CCAs when bandwidth 
grand exceeds packet-measured bandwidth implying a non-cellular bottleneck. 

## Connections

Project github: [github.com/netsys-edinburgh/BISCAY](https://github.com/netsys-edinburgh/BISCAY)

[Don't Bother with Advanced Data Center Congestion Control](/cs440-blog-ckozel/2026/10/15/no-signal-to-rule-them-all/)
Unlike this paper where they found listening to additional signals did not improve performance meaningfully,
here listening to lower level signals at a more frequent rate greatly improves performance.

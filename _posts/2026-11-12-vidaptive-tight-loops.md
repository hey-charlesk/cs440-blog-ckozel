---
layout: post
title: "Send Garbage, Improve Performance"
date: 2026-11-12
paper_title: "Tight Loops, Smooth Streams: Responsive Congestion Control for Real-Time Video"
paper_authors: "P. Karimi, S. Fouladi, V. Sivaraman, M. Alizadeh (MIT / Microsoft Research / Amazon)"
paper_venue: "NINeS 2026"
paper_url: "https://drops.dagstuhl.de/entities/document/10.4230/OASIcs.NINeS.2026.9"
week: 8
tags: [congestion-control, real-time-video, webrtc, transport, latency]
---

## Key Idea

Video calls are challenging to apply network level congestion control to for a few reasons. The real time nature of
frame generation and latency sensitive nature contrast bulk data sending where there is always some data queued to
send until a transfer is complete. Also the video encoder will output frames at a set bitrate, which needs to be
adjusted to match the network conditions. Since a drop in bandwidth utilizing the link fully would result in video
frame delay, todays algorithms operate safely under maximum utilization. One funny issue is when frames come out lower
than the target bitrate this is then a poor signal indicator since you no longer know if the network is supporting
your target bitrate for smaller frames. 

Vidaptive is the solution the authors create to address these problems. Vidaptive sits inbetween the video encoder
and congestion control algorithm and makes the CCA treat the content more like 'backlogged' content. To accomplish this
it uses padding packets queued up behind the CCA so it always has something to pull from. The encoder rate is based
on the service time of a frame, how long it takes the first packet to be acked to the last packet of that frame to
be acked. 

Compared to GCC as the baseline for todays video calls, they implemented Vidaptive within WebRTC to profile it.
Their results were: 1.5x the video bitrate of GCC, VMAF (frame quality index) 39 → 53.5, and P95 frame latency 
of 3941 ms → 1695 ms. Median latency was worse 48 ms → 65 ms attributed to the padding being in the queue. So 
this helps achieve better quality at the cost of latency/framerate.

## Critique

It feels bad to be 'wasting' bandwidth on padding packets to always be measuring the network conditions. Best case
(P=33) showed about 30% of the video traffic was padding. If live streaming is a significant portion of the network
traffic this could hurt everyone. However I suspect it's a small enough portion that it's not a huge problem.

Audio not discussed, which is a key part of video call quality preception. However since its a significantly smaller
portion of the calls bandwidth efficient compression matters less.

## Connections

[Delaying Packets Leads to Lower Compleation Times!?](/cs440-blog-ckozel/2026/10/22/simulate-before-sending/) has a similarly non-intuitive ideas of doing more work to
get better performance. Similar to DcSIM simulating packet arrival, padding data helps simulate the target bitrate
is actually achievable on the network all the time. Both spending bandwidth on packets nobody wants in exchange for
information.

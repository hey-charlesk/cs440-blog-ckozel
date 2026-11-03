---
layout: post
title: "Wow Carbon Aware Routing, What a clever idea!"
date: 2026-11-03
paper_title: "Exploring the Benefits of Carbon-Aware Routing"
paper_authors: "S. El-Zahr, P. Gunning, N. Zilberman (University of Oxford / BT Research & Network Strategy)"
paper_venue: "CoNEXT 2023"
paper_url: "https://doi.org/10.1145/3629165"
week: 6
tags: [routing, traffic-engineering, carbon, energy, link-state, ns3]
---

## Key Idea
Initially I thought this topic was kind of silly, trying to save energy by manipulating routing. However in theory if 
it was 'free' to route via more green nodes, and that savings was significant why not implement such a solution to
reduce global carbon emissions? Here an outstanding job is done analyzing the cost of running routers and calculating
their load vs idle power consumption. In what seems like an impossible task to measure this on existing hardware authors
note the possibility of using smart power supplies already on the network to measure consumption. A database matching 
profiled components could compensate for others. 

They propose CATE, Carbon Aware Traffic Engineering, which operates on the intra-AS level, where Link-State algorithm
typically determines how packets are routed. Its reasonably feasible that a AS network manager could profile their
network and deploy such an algorithm to route optimizing carbon emissions. 

Note Carbon vs. Energy aware, since carbon emissions are ultimately the real negative to avoid, components are also
location-aware to local power generation emissions. So inefficient hardware could be favored if its powered by more
agree generation methods. Really clever stuff, the authors really did great working considering different factors 
going into the problem, even time of day difference in power generation methods. 

## Critique
Does it matter? Should we put time and effort and risk network outages to do this??? No probably not. 
Best estimates found CATE would reduce networking carbon by 18%. All Information and Communications Technology in
2010 accounted for 2% of global carbon. That includes all datacenter-related emissions, networking is a tiny
part of that. But its great to have the answer and tremendous work done to implement such a strategy if it was
deemed worthwhile. 
78908;..

## Connections

The [ns-3](https://www.nsnam.org/) simulator strikes again. This is the fourth paper evaluated in it, after
[P-ECMP](/cs440-blog-ckozel/2026/10/08/ecmp-traffic-engineering/),
[No Signal to Rule Them All](/cs440-blog-ckozel/2026/10/15/no-signal-to-rule-them-all/), and
[RDMA Over Lossy Fabric](/cs440-blog-ckozel/2026/10/27/rdma-over-lossy-fabric/).

[Using ECMP for Flow Guidance???](/cs440-blog-ckozel/2026/10/08/ecmp-traffic-engineering/) also steers traffic by manipulating the routing layer. 

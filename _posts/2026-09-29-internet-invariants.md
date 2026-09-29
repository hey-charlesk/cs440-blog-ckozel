---
layout: post
title: "Fractal Turtles All The Way Down..."
date: 2026-09-29
paper_title: "There is More to Internet Invariants Than Meets the Eye"
paper_authors: "C. Misa, W. Willinger, R. Durairajan, R. Rejaie"
paper_venue: "NINeS 2026"
paper_url: "https://nines-conference.org/papers/p022-Misa.pdf"
week: 2
tags: [architecture, invariants, traffic, self-similarity]
---

## Key Idea
C. Misa et al. examine an interesting phenomenon of the internet: patterns in traffic are self-similar 
regardless of the scale examined. Zooming into spikes in packets yields similar shaped patterns once adjusted
for magnitude/time. This extends into the IPv4 address space, where traffic is seen to be divided amongst addresses
in a 'multifractal' pattern (clustered around certain address blocks, fractally consistent as you go into smaller blocks).
The hypothesis is that generally the allocation systems have this pattern: the higher level allocator does not know
how many addresses/what traffic pattern the lower level consumer will use them for. This pattern arises in Highly
Optimized Tolerance (HOT) systems, where future usage is unknown. These systems arise for dealing with heavy-tailed
distributions of traffic. Consumers of addresses also follow this pattern, Amazon/Comcast/etc. contributing to the
multifractal patterns observed. By examining these phenomenon we can better generate synthetic test data or fingerprint 
communication patterns.

## Critique



## Connections

[-empty-]